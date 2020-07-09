# Events & Listeners & Mails & Queues & Jobs:

## Fire event in controller

```
event(new NewUserHasRegisteredEvent($user_data));
```

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

## Mail

```
php artisan make:mail WelcomeRegisterUserInCourseWithDetailsEmail
```

```
we can also add implements ShouldQueue if we need

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

## EventServiceProvider

```
php artisan event:generate // to auto generate Listeners, create them automatics
```

```
NewUserHasRegisteredEvent::class => [
    \App\Listeners\WelcomeRegisterUserInCourseWithDetailsEmailListener::class,
    // \App\Listeners\NotifyAdminThatNewUserRegisteredListener::class,
],
```

## .env

```
QUEUE_CONNECTION=database
```

## Queues & Jobs

```
php artisan queue:table

php artisan migrate

php artisan queue:work

php artisan queue:work > storage/logs/jobs.log &

jobs -l

KILL 19690
```
