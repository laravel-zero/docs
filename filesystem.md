---
title: Filesystem
description: Interacting with the local filesystem and cloud storage
---

# Filesystem

- [Introduction](#introduction)
- [The Storage Facade](#the-storage-facade)
- [The File Facade](#the-file-facade)
- [Writing Files From a Build](#writing-files-from-a-build)
    - [Using the Storage Facade](#using-the-storage-facade)
    - [Using the File Facade](#using-the-file-facade)

<a name="introduction"></a>
## Introduction

Laravel Zero ships with Laravel's [filesystem](https://laravel.com/docs/filesystem) component, powered by the [Flysystem](https://github.com/thephpleague/flysystem) PHP package. It provides simple drivers for working with local filesystems, SFTP, and Amazon S3 — there is no component to install.

By default, Laravel Zero configures a single `local` disk rooted at your application's `storage/app` directory.

<a name="the-storage-facade"></a>
## The Storage Facade

The `Storage` facade may be used to interact with any of your configured disks:

```php
use Illuminate\Support\Facades\Storage;

Storage::put('reminders.txt', 'Task 1');

$contents = Storage::get('reminders.txt');
```

If you would like to configure additional disks — for example, an Amazon S3 disk — create a `config/filesystems.php` configuration file in your application. Its structure is identical to the one used by Laravel.

<a name="the-file-facade"></a>
## The File Facade

When you need to work with absolute paths on the machine running your application, rather than with a configured disk, the `File` facade is often a better fit:

```php
use Illuminate\Support\Facades\File;

File::put('/path/to/reminders.txt', 'Task 1');

$contents = File::get('/path/to/reminders.txt');
```

<a name="writing-files-from-a-build"></a>
## Writing Files From a Build

Once your application has been compiled into a [PHAR archive](/docs/distribute-as-a-phar-archive), you can no longer add, update, or delete files inside of it. Since the default `local` disk is rooted at `storage/app` — a path within the archive — writing to it will fail.

Instead, you should write to a location on the user's machine, such as the current working directory or a dedicated directory inside their home directory.

<a name="using-the-storage-facade"></a>
### Using the Storage Facade

To keep using the `Storage` facade, create a `config/filesystems.php` configuration file that roots the `local` disk outside of your build:

```php
<?php

return [
    'default' => 'local',

    'disks' => [
        'local' => [
            'driver' => 'local',
            'root' => \Phar::running()
                ? getcwd()
                : storage_path('app'),
        ],
    ],
];
```

The `Storage` facade may then be used exactly as before:

```php
Storage::put('reminders.txt', 'Task 1');
```

> **Note:** The current working directory is only one option. The system temporary directory, via `sys_get_temp_dir()`, or a "dot" directory inside the user's home directory are both good choices as well.

<a name="using-the-file-facade"></a>
### Using the File Facade

The `File` facade already works with absolute paths, so it requires no configuration. You only need to build the path yourself:

```php
File::put(getcwd().'/reminders.txt', 'Task 1');
```
