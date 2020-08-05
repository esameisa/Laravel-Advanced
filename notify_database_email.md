# Notification user by email and database record with Queue

## Create Notification and call it

```
php artisan make:notification initNewConnectionEmailNotification
php artisan notifications:table // if we need to store notification in database
php artisan migrate
```

Call method in controller:

\$user => user that need to notify

\$connected_user => data we want to send it with notification if needed

```
$user->notify(new initNewConnectionEmailNotification($connected_user));
```

## Notification/initNewConnectionEmailNotification

implements ShouldQueue if you need to Queue it

```
class initNewConnectionEmailNotification extends Notification implements ShouldQueue
```

```
public $connected_user;

public function __construct($connected_user)
{
    $this->connected_user = $connected_user;
}

public function via($notifiable)
{
    return ['mail', 'database'];
}

public function toMail($notifiable)
{
    return (new MailMessage)
        ->line('Your probably ' . $this->user->name . ' needs to contact with you.')
        ->action('Connect Now', route('website.user.profile', ['user' => $this->user->id]))
        ->line('Thank you for using our application!');
}


public function toDatabase($notifiable)
{
    return [
        'sender_id' => $this->user->id,
        'sender_email' => $this->user->email,
        'message' => $this->user->name . ' send connection request to ' . $notifiable->name
    ];
}
```

## .env

Drivers: "sync", "database", "beanstalkd", "sqs", "redis", "null"

```
QUEUE_CONNECTION=database
```

## Run queue:work on CPanel

Add this code to you App\Console\Kernel in schedule function

```
    protected function schedule(Schedule $schedule)
    {
        $schedule->command('queue:work --tries=3 --daemon > storage/logs/jobs.log')
            ->everyMinute()
            ->withoutOverlapping();
    }
```

And run this code in Cron Jobs on CPanel every 5 min for example.

```
/usr/local/bin/php /home/connqyvr/public_html/artisan schedule:run >> /dev/null 2>&1
```
