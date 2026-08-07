---
title: Tinker REPL
description: Interact with your console application from an interactive shell
---

# Tinker REPL

- [Introduction](#introduction)
- [Installation](#installation)
- [Usage](#usage)

<a name="introduction"></a>
## Introduction

[Tinker](https://github.com/laravel/tinker) is a powerful REPL, powered by the [PsySH](https://github.com/bobthecow/psysh) package. It allows you to interact with your entire application on the command line, including your Eloquent models, jobs, events, and services.

Tinker is made available to Laravel Zero applications by [Jorge González](https://github.com/scrubmx) through the [`intonate/tinker-zero`](https://github.com/intonate/tinker-zero) package.

<a name="installation"></a>
## Installation

You may install Tinker via Composer:

```shell
composer require intonate/tinker-zero
```

Since Laravel Zero does not use package auto-discovery, you should then register the package's service provider within your application's `bootstrap/providers.php` file:

```php
return [
    App\Providers\AppServiceProvider::class,
    Intonate\TinkerZero\TinkerZeroServiceProvider::class,
];
```

<a name="usage"></a>
## Usage

To enter the Tinker environment, run the `tinker` Artisan command:

```shell
php application tinker
```

Further details, including how to hide the command from your application's list of commands, are available in the [package's documentation](https://github.com/intonate/tinker-zero).
