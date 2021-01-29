---
title: Cache
description: Configuring a custom cache driver
---

# Cache

By default, Laravel Zero [uses the `array` driver](https://github.com/laravel-zero/framework/blob/master/src/Providers/Cache/CacheServiceProvider.php) for caching. This means that using the `Cache` facade will work out of the box for the remainder of a commands process.

You can configure one or more custom cache drivers for your application by creating a new `config/cache.php` file.

For example, if you wanted to use the file driver, you could add the following:

```php
<?php

return [
    'default' => 'file',

    'stores' => [
        'file' => [
            'driver' => 'file',
            'path' => storage_path('framework/cache/data'),
        ],
    ],
];
```

> Note, this will not work in a compiled app as the storage path in the Phar cannot be accessed.
> See the [Filesystem docs](https://laravel-zero.com/docs/filesystem#production) for more details.

Once implemented, you can then use the `Cache` facade as with a standard Laravel application.

```php
use Illuminate\Support\Facades\Cache;

Cache::put('full_name', 'Johnny Lawrence');
Cache::get('full_name');
```

Full details on using the Cache drivers are available on the [main Laravel documentation](https://laravel.com/docs/cache).
