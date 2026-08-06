---
title: HTTP Client
description: An expressive HTTP client for your console application
---

# HTTP Client

- [Introduction](#introduction)
- [Installation](#installation)
- [Making Requests](#making-requests)
- [Testing](#testing)

<a name="introduction"></a>
## Introduction

Console applications frequently talk to APIs. The `http` component brings Laravel's expressive, minimal API around the [Guzzle HTTP client](https://github.com/guzzle/guzzle) to your application, allowing you to quickly make outgoing HTTP requests.

<a name="installation"></a>
## Installation

You may install the `http` component using the `app:install` Artisan command:

```shell
php application app:install http
```

<a name="making-requests"></a>
## Making Requests

To make requests, you may use the `get`, `post`, `put`, `patch`, and `delete` methods provided by the `Http` facade:

```php
use Illuminate\Support\Facades\Http;

$response = Http::get('https://api.themoviedb.org/3/movie/popular');

$response = Http::post('https://example.com/movies', [
    'title' => 'The Empire Strikes Back',
]);
```

The `get` method returns an instance of `Illuminate\Http\Client\Response`, which provides a variety of methods that may be used to inspect the response:

```php
$response->json();
$response->status();
$response->successful();
$response->failed();
```

Of course, the full fluent API is available — headers, authentication, retries, timeouts, and concurrent requests:

```php
$response = Http::withToken($token)
    ->timeout(10)
    ->retry(3, 100)
    ->get('https://api.themoviedb.org/3/movie/popular');
```

<a name="testing"></a>
## Testing

The HTTP client allows you to fake responses, which is invaluable when testing a command that talks to a third-party API:

```php
Http::fake([
    'api.themoviedb.org/*' => Http::response(['results' => []], 200),
]);

$this->artisan('movies:import')->assertSuccessful();
```

Full details on using the HTTP client are available in the [HTTP client documentation](https://laravel.com/docs/http-client) on the Laravel website.
