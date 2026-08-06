---
title: Database
description: Eloquent ORM, migrations, factories, and seeders in your console application
---

# Database

- [Introduction](#introduction)
- [Installation](#installation)
- [Running Queries](#running-queries)
- [Eloquent](#eloquent)
- [Migrations, Factories, and Seeders](#migrations-factories-and-seeders)
- [Redis](#redis)
- [Building Your Application](#building-your-application)
    - [Including the Database Directory](#including-the-database-directory)
    - [Choosing a Database Location](#choosing-a-database-location)

<a name="introduction"></a>
## Introduction

The `database` component brings Laravel's [database layer](https://laravel.com/docs/database) — the query builder, [Eloquent](https://laravel.com/docs/eloquent), migrations, factories, and seeders — to your console application. It works exactly as it does in Laravel.

<a name="installation"></a>
## Installation

You may install the `database` component using the `app:install` Artisan command:

```shell
php application app:install database
```

In addition to requiring `illuminate/database` and `fakerphp/faker`, the installer will scaffold a `database` directory containing an empty SQLite database, a `migrations` directory, a `factories` directory, and a `seeders` directory. A `config/database.php` configuration file will also be created, with the `sqlite` connection configured as the default.

<a name="running-queries"></a>
## Running Queries

Once the component is installed, you may run queries using the `DB` facade:

```php
use Illuminate\Support\Facades\DB;

DB::table('movies')->insert([
    'title' => 'The Empire Strikes Back',
]);

$movies = DB::table('movies')->get();
```

<a name="eloquent"></a>
## Eloquent

Eloquent models work like a breeze in a Laravel Zero environment. Models are typically placed within your application's `app/Models` directory, and may be generated using the `make:model` Artisan command:

```shell
php application make:model Movie
```

```php
use App\Models\Movie;

$movie = Movie::create([
    'title' => 'The Empire Strikes Back',
]);

$movies = Movie::where('year', '>', 1980)->get();
```

<a name="migrations-factories-and-seeders"></a>
## Migrations, Factories, and Seeders

Laravel's [migrations](https://laravel.com/docs/migrations), [factories](https://laravel.com/docs/eloquent-factories), and [seeders](https://laravel.com/docs/seeding) are all included, along with the Artisan commands you would expect:

```shell
php application make:migration create_movies_table

php application migrate

php application make:factory MovieFactory

php application make:seeder MovieSeeder

php application db:seed
```

<a name="redis"></a>
## Redis

Laravel Zero also provides a component for [Redis](https://redis.io), an in-memory data structure store. To get started, install the `redis` component:

```shell
php application app:install redis
```

Once the component is installed, configure your Redis connections within your `config/database.php` configuration file, and you may use the `Redis` facade:

```php
use Illuminate\Support\Facades\Redis;

Redis::set('name', 'Daniel LaRusso');

$name = Redis::get('name');
```

Consult the [Redis documentation](https://laravel.com/docs/redis) on the Laravel website for full details.

<a name="building-your-application"></a>
## Building Your Application

<a name="including-the-database-directory"></a>
### Including the Database Directory

The `database` directory is not included in a [PHAR build](/docs/distribute-as-a-phar-archive) by default. If your application relies on migrations, factories, or seeders at runtime, add the directory to the `directories` section of your `box.json` file:

```json
"directories": [
    "app",
    "bootstrap",
    "config",
    "database",
    "vendor"
],
```

<a name="choosing-a-database-location"></a>
### Choosing a Database Location

Files inside a PHAR archive are read-only, so a SQLite database cannot live within your build. By default, the `sqlite` connection points at `database_path('database.sqlite')`, which resolves to a path inside the archive.

Instead, you should store the database somewhere on the user's machine. A "dot" directory inside the user's home directory is a good choice:

```diff
// config/database.php...

'connections' => [
    'sqlite' => [
        'driver' => 'sqlite',
        'url' => env('DB_URL'),
-       'database' => database_path('database.sqlite'),
+       'database' => $_SERVER['HOME'].'/.movie-cli/database.sqlite',
        'prefix' => '',
        'foreign_key_constraints' => env('DB_FOREIGN_KEYS', true),
    ],
],
```

> **Warning:** This file will not exist when a user first installs your application. You should either create and migrate it on demand, or provide an `install` command that does so.
