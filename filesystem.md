---
title: Filesystem
description: Filesystem interaction through Storage of File facade, just like Laravel.
---

# Filesystem

If you want to move files in your system, or to different providers like AWS S3 and Dropbox. By default,
Laravel Zero ships with the [Filesystem](https://laravel.com/docs/filesystem) component of Laravel.

**Note:** By default the root directory is `your-app-name/storage`.

> Writing files after you [build](/docs/build-a-standalone-application) your application is different, check [Writing files in production](#production)

<a name="using-the-storage-facade"></a>
### Using the Storage facade

```php
use Illuminate\Support\Facades\Storage;

Storage::put("reminders.txt", "Task 1");
```

<a name="using-the-file-facade"></a>
### Using the File facade

```php
use Illuminate\Support\Facades\File;

File::put("/path/to/file/reminders.txt", "Task 1");
```

<a name="production"></a>
### Writing files in production

When using the filesystem be aware that when you build the application with `app:build` you create a `.phar` file. You can not add, update or delete files inside the `.phar` file.

> We are currently looking into streamlining the filesystem access. The code below is an example of how you can currently write files from the built application to the current working directory.

<a name="with-the-storage-facade"></a>
#### With the Storage facade

If you want to use the `Storage` facade you will need to use a config file, similar to how Laravel does it.

- Create a file `filesystems.php` in your `config/` directory.
- Copy and paste the following contents. This code will replace the default `your-app-name/storage/app` folder with the current working directory.

```php
<?php

return [
    'default' => 'local',
    'disks' => [
        'local' => [
            'driver' => 'local',
            'root' => getcwd(),
        ],
    ],
];
```
    
- Use the `Storage` facade like you would before.

<a name="with-the-file-facade"></a>
#### With the File facade

Using the `File` facade is just the same as normal

```php 
File::put(getcwd() . "/reminders.txt", "Task 1");
```
