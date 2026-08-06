---
title: Logging
description: Robust logging services for your console application
---

# Logging

- [Introduction](#introduction)
- [Installation](#installation)
- [Writing Log Messages](#writing-log-messages)
- [Logging From a Build](#logging-from-a-build)

<a name="introduction"></a>
## Introduction

To help you learn more about what's happening within your application, Laravel Zero provides a `log` component that brings the robust logging services of Laravel to the console. These services allow you to log messages to files, the system error log, and even to Slack to notify your entire team.

Until the component is installed, the `log` service resolves to a null logger. Log messages are silently discarded, and nothing is written to disk.

<a name="installation"></a>
## Installation

You may install the `log` component using the `app:install` Artisan command:

```shell
php application app:install log
```

The installer will require `illuminate/log` and create a `config/logging.php` configuration file in your application. Within that file, you may configure the log channels your application uses. By default, the `stack` channel writes to `storage/logs/laravel.log`.

<a name="writing-log-messages"></a>
## Writing Log Messages

You may write information to the logs using the `Log` facade. The logger provides the eight logging levels defined in the [RFC 5424 specification](https://tools.ietf.org/html/rfc5424): **emergency**, **alert**, **critical**, **error**, **warning**, **notice**, **info**, and **debug**:

```php
use Illuminate\Support\Facades\Log;

Log::emergency($message);
Log::alert($message);
Log::critical($message);
Log::error($message);
Log::warning($message);
Log::notice($message);
Log::info($message);
Log::debug($message);
```

An array of contextual data may be passed to the log methods. This contextual data will be formatted and displayed with the log message:

```php
Log::info('Movie imported.', ['id' => $movie->id]);
```

For everything else — log channels, custom channels, and contextual information — consult the [logging documentation](https://laravel.com/docs/logging) on the Laravel website.

<a name="logging-from-a-build"></a>
## Logging From a Build

The `storage_path` helper, which determines where log files are written, resolves to a path inside your project. Once your application has been compiled into a [PHAR archive](/docs/distribute-as-a-phar-archive), that path lives inside the archive, which is read-only.

To handle this, you should reconfigure the channel's path at runtime within your `AppServiceProvider`:

```php
/**
 * Bootstrap any application services.
 */
public function boot(): void
{
    config(['logging.channels.single.path' => \Phar::running()
        ? dirname(\Phar::running(false)).'/movie-cli.log'
        : storage_path('logs/movie-cli.log'),
    ]);
}
```

> **Note:** Make sure you configure the path of the channel your application actually uses, as defined by the `default` option of your `config/logging.php` configuration file.
