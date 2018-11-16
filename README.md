# LaravelNotification

# Model 
```php
First run the commond
php artisan make:notification Booking

use this in your model from where you want to send notification
use Illuminate\Notifications\Notifiable;
class User extends Authenticatable
{
    use Notifiable;
}
```
#App\Notification folder
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
