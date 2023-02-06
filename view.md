---
title: View
description: View Support for Laravel Zero
---

# View

Using the `app:install` Artisan command you can install the `view` component:

```bash
php <your-app-name> app:install view
```

<a name="usage"></a>
#### Usage

The `View` component allows to render HTML views and take advantage of the blade template engine.

As usual, the usage is similar to Laravel:
```php
use Illuminate\Support\Facades\View;

View::make('view.name', ['foo' => 'bar']);
view('view-name', ['foo' => 'bar']);
```

<a name="views-in-production"></a>
## Using views in production

 In order to use Blade views in production, a `view.php` file must be added in the `config` directory to specify the path where the compiled views should be stored.

 For example:
 ```php
 <?php

 return [
     'paths' => [
         resource_path('views'),
     ],
    'compiled' => \Phar::running()
        ? getcwd()
        : env('VIEW_COMPILED_PATH', realpath(storage_path('framework/views'))),
 ];
 ```
 
 An alternative to using the current working directory is to use the system temporary directory with `sys_get_temp_dir()`, but any path can be specified, such as a custom location in the user's home directory.

The `resources` directory must also be added to the `box.json` file to include it in the compiled PHAR file:
 ```json
 "directories": [
     // ...
     "resources"
 ],
 ```

Full details on using the View component is available on the [main Laravel documentation](https://laravel.com/docs/views).
