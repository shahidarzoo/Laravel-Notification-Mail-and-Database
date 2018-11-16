# LaravelNotification By Mial and Database
## First Create an account https://pusher.com/
### BY Mail
### Run the artisan commond
```php

php artisan make:notification Booking
```
### Past the keys in your env
```php
env
PUSHER_APP_ID=649978
PUSHER_APP_KEY=a12347a154825569035a
PUSHER_APP_SECRET=4e17b31339fb780de42d
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
<?php

namespace App\Notifications;

use Illuminate\Bus\Queueable;
use Illuminate\Notifications\Notification;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Notifications\Messages\MailMessage;

class Booking extends Notification
{
    use Queueable;

    /**
     * Create a new notification instance.
     *
     * @return void
     */
    protected $my_notification; 
    public function __construct($msg)
    {
         $this->my_notification = $msg;
    }

    /**
     * Get the notification's delivery channels.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function via($notifiable)
    {
        return ['mail'];
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
                   ->line('Welcome '.$this->my_notification)
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
            //
        ];
    }
}

```
# Controller

```php
use App\Notifications\Booking;
$user = App\User::first();
$user->notify(new Newvisit("New Booking has created"));

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
