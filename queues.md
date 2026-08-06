---
title: Queues
description: Defer time consuming tasks with Laravel's unified queue API
---

# Queues

- [Introduction](#introduction)
- [Installation](#installation)
- [Creating Jobs](#creating-jobs)
- [Dispatching Jobs](#dispatching-jobs)
- [Running the Queue Worker](#running-the-queue-worker)
- [Dealing With Failed Jobs](#dealing-with-failed-jobs)

<a name="introduction"></a>
## Introduction

Console applications often need to do things that take a while — importing a large file, calling a slow API, or generating a report. The `queues` component brings Laravel's [queue system](https://laravel.com/docs/queues) to your application, giving you a unified API across a variety of queue backends such as SQS, Redis, and your own database.

<a name="installation"></a>
## Installation

You may install the `queue` component using the `app:install` Artisan command:

```shell
php application app:install queue
```

Because queues store their jobs somewhere, the installer will also install the [database](/docs/database) component for you. Once the installation finishes, a `config/queue.php` configuration file will be present in your application, with the `sync` connection configured as the default.

Before using the `database` connection, you should create the tables that store your queued and failed jobs:

```shell
php application make:queue-table

php application make:queue-failed-table

php application migrate
```

<a name="creating-jobs"></a>
## Creating Jobs

Job classes are typically stored in the `app/Jobs` directory and may be generated using the `make:job` Artisan command:

```shell
php application make:job ImportMovies
```

The generated class implements the `Illuminate\Contracts\Queue\ShouldQueue` interface, indicating to Laravel Zero that the job should be pushed onto the queue to run asynchronously:

```php
<?php

namespace App\Jobs;

use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Queue\Queueable;

class ImportMovies implements ShouldQueue
{
    use Queueable;

    /**
     * Create a new job instance.
     */
    public function __construct(
        public string $path,
    ) {
        //
    }

    /**
     * Execute the job.
     */
    public function handle(): void
    {
        // ...
    }
}
```

<a name="dispatching-jobs"></a>
## Dispatching Jobs

Once you have written your job class, you may dispatch it from within a command using the job's `dispatch` method:

```php
use App\Jobs\ImportMovies;

public function handle(): void
{
    ImportMovies::dispatch($this->argument('path'));

    $this->info('The import has been queued.');
}
```

If you would like to delay the execution of a queued job, you may use the `delay` method:

```php
ImportMovies::dispatch($path)->delay(now()->addMinutes(10));
```

<a name="running-the-queue-worker"></a>
## Running the Queue Worker

Laravel Zero includes the `queue:work` Artisan command, which starts a worker that processes new jobs as they are pushed onto the queue:

```shell
php application queue:work
```

You may also process a single job and exit, which is useful when your worker is driven by an external scheduler:

```shell
php application queue:work --once
```

> **Note:** Remember that queue workers are long-lived processes and store the booted application state in memory. As a result, they will not notice changes in your code base after they have been started. Restart your workers with `queue:restart` after deploying a new build.

<a name="dealing-with-failed-jobs"></a>
## Dealing With Failed Jobs

Jobs that exceed their configured number of attempts are inserted into the `failed_jobs` database table. You may inspect, retry, and delete them with the following commands:

```shell
php application queue:failed

php application queue:retry all

php application queue:forget <id>

php application queue:flush
```

For everything else — job batching, rate limiting, unique jobs, and middleware — consult the [queues documentation](https://laravel.com/docs/queues) on the Laravel website.
