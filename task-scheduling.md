---
title: Task Scheduling
description: Fluently and expressively define your application's command schedule
---

# Task Scheduling

- [Introduction](#introduction)
- [Defining Schedules](#defining-schedules)
- [Running the Scheduler](#running-the-scheduler)
- [Viewing Scheduled Tasks](#viewing-scheduled-tasks)

<a name="introduction"></a>
## Introduction

Laravel Zero ships with Laravel's [task scheduler](https://laravel.com/docs/scheduling), which offers a fresh approach to managing scheduled tasks on your server. The scheduler allows you to fluently and expressively define your command schedule within your application itself, so only a single cron entry is needed on your server.

<a name="defining-schedules"></a>
## Defining Schedules

Every Laravel Zero command may define its own schedule using the `schedule` method. This keeps the schedule right next to the logic it runs:

```php
use Illuminate\Console\Scheduling\Schedule;

/**
 * Define the command's schedule.
 */
public function schedule(Schedule $schedule): void
{
    $schedule->command(static::class)->everyMinute();
}
```

A variety of schedule frequencies may be assigned to your task:

```php
$schedule->command(static::class)->hourly();

$schedule->command(static::class)->dailyAt('13:00');

$schedule->command(static::class)->weekdays()->hourly();
```

You are also free to schedule closures or operating system commands, and to apply any of the scheduler's other options — such as preventing overlaps or running tasks in the background:

```php
$schedule->command(static::class)
    ->everyFiveMinutes()
    ->withoutOverlapping()
    ->runInBackground();
```

For a complete list of the available schedule frequencies and options, consult the [task scheduling documentation](https://laravel.com/docs/scheduling) on the Laravel website.

<a name="running-the-scheduler"></a>
## Running the Scheduler

The scheduler needs to be invoked every minute in order to evaluate your schedule. Add the following cron entry to the server that runs your application:

```
* * * * * php /path-to-your-project/movie-cli schedule:run >> /dev/null 2>&1
```

<a name="viewing-scheduled-tasks"></a>
## Viewing Scheduled Tasks

You may use the `schedule:list` Artisan command to view an overview of the tasks scheduled by your application and the next time each one is due to run:

```shell
php application schedule:list
```
