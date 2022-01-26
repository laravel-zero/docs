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

Full details on using the View component is available on the [main Laravel documentation](https://laravel.com/docs/views).
