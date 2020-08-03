# Events & Listeners & Mails & Queues & Jobs:

## Event

```
php artisan make:event NewUserHasRegisteredEvent
```

```
public $user;

public function __construct($user)
{
    $this->user = $user;
}
```

## Fire event in controller

```
event(new NewUserHasRegisteredEvent($user_data));
```

## Mail

```
php artisan make:mail WelcomeRegisterUserInCourseWithDetailsEmail
```

We can also add implements ShouldQueue if we need

```
protected $user;

public function __construct($user)
{
    $this->user = $user;
}

public function build()
{
    return $this->view('mails.WelcomeRegisterUserInCourseWithDetailsEmail')
        ->subject('codeKids24')
        ->with('user', $this->user);
}
```

## Listener

```
php artisan make:listener WelcomeRegisterUserInCourseWithDetailsEmailListener
```

```
class WelcomeRegisterUserInCourseWithDetailsEmailListener implements ShouldQueue
{
    public function handle($event)
    {
        Mail::to($event->user['email'])->send(new WelcomeRegisterUserInCourseWithDetailsEmail($event->user));
    }
}
```

## EventServiceProvider

To connect Event with Listeners:

```
protected $listen = [
    NewUserHasRegisteredEvent::class => [
        \App\Listeners\WelcomeRegisterUserInCourseWithDetailsEmailListener::class,
        // \App\Listeners\NotifyAdminThatNewUserRegisteredListener::class,
    ],
];
```

If we need to auto generate Listeners, create them automatics:

```
php artisan event:generate
```

## .env

Drivers: "sync", "database", "beanstalkd", "sqs", "redis", "null"

```
QUEUE_CONNECTION=database
```

## Queues & Jobs

```
php artisan queue:table

php artisan migrate

php artisan queue:work

php artisan queue:work & // to run in background

php artisan queue:work > storage/logs/jobs.log &

jobs -l

KILL 19690
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
