# LaravelNotification
## First Create an account https://pusher.com/
### click create new app on leftsite bootom corner on your pusher dashboard then click on App keys

Markup :  Past your keys in you .env file which you have copied form App Keys in you pusher
Markup : env
```php

PUSHER_APP_ID=649978
PUSHER_APP_KEY=a12347a154825569035a
PUSHER_APP_SECRET=4e17b31339fb780de42d

```


```php
First run the commond
php artisan make:notification Booking
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

