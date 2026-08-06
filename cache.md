---
title: Cache
description: Caching data in your console application
---

# Cache

- [Introduction](#introduction)
- [Configuration](#configuration)
- [Cache Usage](#cache-usage)

<a name="introduction"></a>
## Introduction

Laravel's [cache](https://laravel.com/docs/cache) is available in every Laravel Zero application — there is no component to install.

By default, Laravel Zero uses the `array` cache driver. Because the `array` driver stores items in memory, cached values live for the duration of the currently executing command and are discarded when the process exits. This is a sensible default for a console application: the `Cache` facade works out of the box, and nothing is written to disk.

<a name="configuration"></a>
## Configuration

If you would like your cached items to survive between runs, you should publish the cache configuration file:

```shell
php application config:publish cache
```

Then, choose one of the drivers supported by Laravel — such as `file`, `database`, or `redis` — within the `config/cache.php` configuration file:

```php
'default' => env('CACHE_STORE', 'file'),
```

> **Note:** Drivers such as `database` and `redis` require the corresponding [database](/docs/database) component to be installed.

<a name="cache-usage"></a>
## Cache Usage

You may interact with the cache using the `Cache` facade:

```php
use Illuminate\Support\Facades\Cache;

Cache::put('movies', $movies, $seconds = 600);

$movies = Cache::get('movies');
```

The `remember` method is particularly useful in console applications, since it allows you to retrieve an item from the cache, or compute and store it when it is missing:

```php
$movies = Cache::remember('movies', 600, function () {
    return $this->tmdb->popular();
});
```

Full details on the available cache methods are available in the [cache documentation](https://laravel.com/docs/cache) on the Laravel website.
