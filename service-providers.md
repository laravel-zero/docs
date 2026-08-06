---
title: Service Providers
description: The central place to bootstrap your console application
---

# Service Providers

- [Introduction](#introduction)
- [Writing Service Providers](#writing-service-providers)
    - [The Register Method](#the-register-method)
    - [The Boot Method](#the-boot-method)
- [Registering Providers](#registering-providers)

<a name="introduction"></a>
## Introduction

Service providers are the central place of all application bootstrapping. Your own application, as well as all of Laravel Zero's core services, are bootstrapped via service providers.

Service providers live in your application's `app/Providers` directory. A fresh installation contains a single `AppServiceProvider` class, which is a fine place to register your application's own bindings. As your application grows, you are free to create additional providers to keep related bootstrapping logic together.

<a name="writing-service-providers"></a>
## Writing Service Providers

All service providers extend the `Illuminate\Support\ServiceProvider` class and typically contain a `register` and a `boot` method.

<a name="the-register-method"></a>
### The Register Method

Within the `register` method, you should **only bind things into the [service container](https://laravel.com/docs/container)**. You should never attempt to register any event listeners, commands, or any other piece of functionality within the `register` method — otherwise, you may accidentally use a service that is provided by a service provider which has not loaded yet.

Service providers are the best way to specify that a concrete implementation should be bound to a contract or interface:

```php
use App\Contracts\MovieRepository;
use App\Repositories\TmdbMovieRepository;

public function register(): void
{
    $this->app->singleton(MovieRepository::class, function ($app) {
        return new TmdbMovieRepository(config('services.tmdb.key'));
    });
}
```

This is useful because it allows the rest of your application to depend on the contract instead of the implementation. The container will resolve the concrete class for you whenever the contract is type-hinted:

```php
public function handle(MovieRepository $movies): void
{
    $this->table(['Title'], $movies->popular());
}
```

Of course, you may also resolve the implementation manually from the container:

```php
$movies = app(MovieRepository::class);
```

<a name="the-boot-method"></a>
### The Boot Method

The `boot` method is called after all other service providers have been registered, meaning you have access to all other services that have been registered by the framework. This is where you should register event listeners, macros, or adjust configuration values at runtime:

```php
use Illuminate\Support\Facades\Event;

public function boot(): void
{
    Event::listen(function (MovieWasImported $event) {
        // ...
    });
}
```

You may type-hint dependencies for your service provider's `boot` method. The service container will automatically inject any dependencies you need:

```php
public function boot(MovieRepository $movies): void
{
    // ...
}
```

<a name="registering-providers"></a>
## Registering Providers

All service providers are registered within your application's `bootstrap/providers.php` file. This file returns an array of your application's service provider class names:

```php
<?php

use App\Providers\AppServiceProvider;
use App\Providers\MovieServiceProvider;

return [
    AppServiceProvider::class,
    MovieServiceProvider::class,
];
```

Since Laravel Zero does not use package auto-discovery, service providers offered by third-party packages must be registered in this file as well.
