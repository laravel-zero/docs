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

## Using views in production

 In order to use blade view in production, a `view.php` file must be added in `config` directory to specify the path where the compiled view should be stored.

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
 ]
 ```

 You also need to add the `resources` directory in the `box.json` file to include it in the PHAR file will be compiled:
 ```json
 "directories": [
     "app",
     "bootstrap",
     "config",
     "vendor",
     "resources"
 ],
 ```

Full details on using the View component is available on the [main Laravel documentation](https://laravel.com/docs/views).
