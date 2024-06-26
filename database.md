---
title: Database
description: Database
---

# Database

Using the `app:install` Artisan command you can install the `database` component:
```bash
php <your-app-name> app:install database
```

As you might have already guessed, it is Laravel's [Eloquent](https://laravel.com/docs/eloquent) component
that works like a breeze in the Laravel Zero environment too.

Usage:

```php
use Illuminate\Support\Facades\DB;

DB::table('users')->insert(
    ['email' => 'enunomaduro@gmail.com']
);

$users = DB::table('users')->get();
```

Laravel [Database Migrations](https://laravel.com/docs/migrations), [database factories](https://laravel.com/docs/database-testing#writing-factories), and [Database Seeding](https://laravel.com/docs/seeding) features are also included.

<a name="redis"></a>
## Redis

Laravel Zero also provides a component for [Redis](https://redis.io), an in-memory data-structure store.

To get started with using Redis for fast data storage or caching, install the Component with:

```bash
php <your-app-name> app:install redis
```

You can then use the Redis facade after configuring Redis in your `config/database.php`.

```php
use Illuminate\Support\Facades\Redis;

Redis::set('full_name', 'Daniel LaRusso');
Redis::get('full_name');
```

Check the [Laravel documentation](https://laravel.com/docs/8.x/redis) for full details of using Redis.

<a name="note-on-phar-builds"></a>
## Note on PHAR builds

The `database` directory isn't included in Laravel Zero standalone PHAR builds by default.

If you are using the Database components migration, factory, or seeder functionality make sure to add the directory to your [Box configurations](https://github.com/laravel-zero/laravel-zero/blob/master/box.json) `directories` section.
