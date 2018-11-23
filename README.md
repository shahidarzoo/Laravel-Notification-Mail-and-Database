# LaravelNotification By Mial and Database
## First Create an account 
[https://pusher.com/](https://pusher.com/)

### BY Mail
### Run the artisan commond
```php

php artisan make:notification BookingNotification
```
### Past the keys in your env
```php
env
PUSHER_APP_ID=4234324324
PUSHER_APP_KEY=***************
PUSHER_APP_SECRET=*******************
```

# Model 
use this in your model from where you want to send notification
```php
use Illuminate\Notifications\Notifiable;
class User extends Authenticatable
{
    use Notifiable;
}
```
# App\Notification folder
```php
namespace App\Notifications;

use Illuminate\Bus\Queueable;
use Illuminate\Notifications\Notification;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Notifications\Messages\MailMessage;

class BookingNotification extends Notification
{
    use Queueable;

    /**
     * Create a new notification instance.
     *
     * @return void
     */
    private $user = '';
    protected $message; 
    public function __construct($user, $message)
    {
        $this->user = $user;
        $this->message = $message;
    }

    /**
     * Get the notification's delivery channels.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function via($notifiable)
    {
        return ['mail','database'];
    }

    /**
     * Get the mail representation of the notification.
     *
     * @param  mixed  $notifiable
     * @return \Illuminate\Notifications\Messages\MailMessage
     */
    public function toMail($notifiable)
    {
        return (new MailMessage)
	    ->line('Hi '.$this->user)
	    ->line($this->message)
	    ->action('Welcome to Fastinternetcable', url('http://dev.fastinternetcable.com'))
	    ->line('Thank you for using our application!');
    }

    /**
     * Get the array representation of the notification.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function toArray($notifiable)
    {
        return [
            'data' => 'New Booking has been created'
        ];
    }
}


```
# Controller

```php
use App\Notifications\BookingNotification;
public function index()
{
	$user = App\User::first();
	$message = "New Booking Created";
	$this->sendNotification($user, $message);
}

private function sendNotification($user, $message)
{
	try 
	{
	    $user->notify(new BookingNotification($user->name, $message));
	} 
	catch (Exception $exception) 
	{
	    echo $exception->getMessage();
	}
}

```
# Database Notifications
### Run artisan commond
```php
php artisan notifications:table

php artisan migrate
php artisan make:notification TaskCompletedNotification
```
# Routs
```php
use App\Notifications\TaskCompletedNotification;
Route::get('/', function () {

	$user = App\User::first();
	$user->notify(new TaskCompletedNotification);
    return view('welcome');
});

Route::get('/markAsRead', function(){
	auth()->user()->unreadNotifications->markAsRead();
	return redirect()->back();
})->name('markAsRead');

```

# Views

```html
<li class="dropdown">
<a  class="nav-link dropdown-toggle" href="#" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
<i class="fa fa-bell"></i>
@if(auth()->user()->unReadNotifications->count())
<span class="badge badge-light">{{ auth()->user()->unReadNotifications->count() }}</span>
@endif
</a>

<ul class="dropdown-menu">
<li><a style="margin-left: 27px;" href="{{route('markAsRead')}}">Mark all as read</a></li>
@foreach(auth()->user()->unReadNotifications as $notify)
<li class="dropdown-item" href="#">
<a href="#">{{$notify->data['data']}}</a>
</li>
@endforeach
</ul>
</li>

```
# App\Notification TaskCompletedNotification
```php

namespace App\Notifications;

use Illuminate\Bus\Queueable;
use Illuminate\Notifications\Notification;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Notifications\Messages\MailMessage;

class TaskCompletedNotification extends Notification
{
    use Queueable;

    /**
     * Create a new notification instance.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }

    /**
     * Get the notification's delivery channels.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function via($notifiable)
    {
        return ['database'];
    }

    /**
     * Get the mail representation of the notification.
     *
     * @param  mixed  $notifiable
     * @return \Illuminate\Notifications\Messages\MailMessage
     */
    public function toMail($notifiable)
    {
        return (new MailMessage)
                    ->line('The introduction to the notification.')
                    ->action('Notification Action', url('/'))
                    ->line('Thank you for using our application!');
    }

    /**
     * Get the array representation of the notification.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function toArray($notifiable)
    {
        return [
            'data' => 'This is my first notification',
        ];
    }
}


```
