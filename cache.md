---
title: Cache
description: Configuring a custom cache driver
---

# Cache

By default, Laravel Zero [uses the `array` driver](https://github.com/laravel-zero/framework/blob/master/src/Providers/Cache/CacheServiceProvider.php) for caching. This means that using the `Cache` facade will work out of the box for the remainder of a commands process.

You can configure one or more custom cache drivers for your application by publishing the configuration file:

```bash
php <your-app-name> config:publish cache
```

Full details on using the Cache drivers are available on the [main Laravel documentation](https://laravel.com/docs/cache).
