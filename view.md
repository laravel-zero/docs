---
title: View
description: Rendering Blade templates from your console application
---

# View

- [Introduction](#introduction)
- [Installation](#installation)
- [Rendering Views](#rendering-views)
- [Using Views in a Build](#using-views-in-a-build)

<a name="introduction"></a>
## Introduction

The `view` component allows you to render templates using the [Blade](https://laravel.com/docs/blade) templating engine. This is useful whenever your console application needs to produce something more structured than terminal output — an HTML report, a generated configuration file, or the body of an email.

> **Note:** If all you want is beautifully styled terminal output, you don't need this component. Every Laravel Zero application already includes [Termwind](/docs/commands#styling-output-with-termwind).

<a name="installation"></a>
## Installation

You may install the `view` component using the `app:install` Artisan command:

```shell
php application app:install view
```

The installer will require `illuminate/view`, create the `resources/views` directory where your templates live, create a `config/view.php` configuration file, and prepare `storage/framework/views` for compiled templates.

<a name="rendering-views"></a>
## Rendering Views

Views may be rendered using the `View` facade or the global `view` helper. Both accept the name of the view and an array of data that should be made available to it:

```php
use Illuminate\Support\Facades\View;

$html = View::make('report', ['movies' => $movies])->render();

$html = view('report', ['movies' => $movies])->render();
```

Assuming the view above lives at `resources/views/report.blade.php`:

```php
<ul>
    @foreach ($movies as $movie)
        <li>{{ $movie->title }}</li>
    @endforeach
</ul>
```

Consult the [Blade documentation](https://laravel.com/docs/blade) on the Laravel website for the full templating syntax.

<a name="using-views-in-a-build"></a>
## Using Views in a Build

Blade compiles your templates to plain PHP files and writes them to the path defined by the `compiled` option of your `config/view.php` configuration file. By default, that path is `storage/framework/views`, which lives inside your [PHAR archive](/docs/distribute-as-a-phar-archive) and is therefore read-only.

To handle this, point the `compiled` option at a writable location whenever the application is running from within an archive:

```php
<?php

return [
    'paths' => [
        resource_path('views'),
    ],

    'compiled' => \Phar::running()
        ? sys_get_temp_dir()
        : env('VIEW_COMPILED_PATH', realpath(storage_path('framework/views'))),
];
```

Your templates also need to be part of the build itself. Add the `resources` directory to the `directories` section of your `box.json` file:

```json
"directories": [
    "app",
    "bootstrap",
    "config",
    "resources",
    "vendor"
],
```
