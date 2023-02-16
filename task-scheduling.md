---
title: Task Scheduling
description: Scheduler allows you to fluently and expressively define your command schedule
---

# Task Scheduling

Laravel Zero ships with the [Task Scheduling](https://laravel.com/docs/scheduling) system of
Laravel. To use the scheduler, you need to periodically execute your application, and for that you
need to add the following Cron entry to your server:

```
* * * * * php /path-to-your-project/your-app-name schedule:run >> /dev/null 2>&1
```

You may define all of your scheduled tasks in the `schedule` method of the Artisan command:

```php
public function schedule(Schedule $schedule): void
{
    $schedule->command(static::class)->everyMinute();
}
```

<a name="viewing-scheduled-tasks"></a>
### Viewing scheduled tasks

The `schedule:list` command can be used to view a list of tasks that are scheduled in your application as follows:

```shell
php <your-app-name> schedule:list
```
