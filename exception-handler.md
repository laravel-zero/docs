---
title: Exception Handler
description: Managing which exceptions to report
---

# Introduction

When you start a new Laravel Zero project, error and exception handling is part of the core. 

The `Illuminate\Foundation\Exceptions\Handler` class is where all exceptions triggered by your application are logged and rendered back to the user. 

> If you would like to view the internals, this class is part of the [`laravel-zero/foundation`](https://github.com/laravel-zero/foundation) package.

<a name="custom-exception-handler"></a>
# Custom Exception Handler

When working with Laravel Zero you might want to prevent some exceptions to be reported to the end user.  

As `Illuminate\Foundation\Exceptions\Handler` is part of the core you can't just modify it to fit your needs. Your modifications would be overwritten with every update of Laravel Zero.

Below we'll describe how you can create your own handler so you can add your custom logic.

<a name="creating-the-handler-class"></a>
#### Creating The Handler Class

Inside your `app` folder, create a new folder `Exceptions`. Inside that folder, create a file called `Handler.php`. This will be our new Handler in the next few minutes. 

Copy the content below and paste it in your newly created file. 
    
```php 
<?php

namespace App\Exceptions;

use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;
use Throwable;

class Handler extends ExceptionHandler
{
    /**
     * A list of the exception types that are not reported.
     *
     * @var array
     */
    protected $dontReport = [
        // \Illuminate\Database\Eloquent\ModelNotFoundException::class,
    ];

    /**
     * Register the exception handling callbacks for the application.
     *
     * @return void
     */
    public function register()
    {
        $this->reportable(function (Throwable $e) {
            //
        });
    }
}
```
As you can see we are just extending the default handler here.

> The above content might feel familiar to those using Laravel.  
It's a trimmed down version of the [`App\Exceptions\Handler`](https://github.com/laravel/laravel/blob/master/app/Exceptions/Handler.php) of Laravel.

See [the official Laravel documentation](https://laravel.com/docs/errors#reporting-exceptions) for more details.

<a name="replacing-the-default-handler"></a>
#### Replacing The Default Handler

In the file `bootstrap/app.php` look for the following code.

```php 
$app->singleton(
    Illuminate\Contracts\Debug\ExceptionHandler::class,
    Illuminate\Foundation\Exceptions\Handler::class
);
```

Replace the `Illuminate\Foundation\Exceptions\Handlers::class` with your newly created `App\Exceptions\Handler:class`.

```php
$app->singleton(
    Illuminate\Contracts\Debug\ExceptionHandler::class,
    App\Exceptions\Handler::class
);
```

> **Congratulations!**  
You can now start handling exceptions in your new `Handler.php` file.

#### Ignoring Exceptions

The `$dontReport` property of the exception handler contains an array of exception types that will not be logged. For example, if you would not like to report the `RuntimeException`, you would add it to the `dontReport` array.

```php
protected $dontReport = [
    \Symfony\Component\Console\Exception\RuntimeException::class,
];
```

This however results in `RuntimeException` not being reported at all. More fine grained control can be achieved by updating the `register` function.

The following would prevent the *Not enough arguments* exception to be reported, but any other `RuntimeException` would still be reported.

```php
public function register(Throwable $exception)
{
    $this->reportable(function (RuntimeException $e) {
        if (Str::contains($e->getMessage(), ['Not enough arguments'])) {
            return false;
        }
    });
}
```

If you would like to disable the reporting only when running from the built phar file you could use the [`\Phar::running()`](https://php.net/manual/en/phar.running.php) function.

```php
if (\Phar::running()) {
    $this->reportable(function (RuntimeException $e) {
        if (Str::contains($e->getMessage(), ['Not enough arguments'])) {
            return false;
        }
    });
}
```
