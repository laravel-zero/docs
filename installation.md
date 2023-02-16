---
title: Installation
description: Create a new Laravel Zero project
---

# Installation

> **Requires [PHP 8.1+](https://php.net/releases)**

Laravel Zero utilizes [Composer 2](https://getcomposer.org) to manage its dependencies. So, before using Laravel Zero, make sure you have Composer installed on your machine.

<a name="via-composer-create-project"></a>
#### Via Composer Create-Project

You may install Laravel Zero by issuing the Composer `create-project` command in your terminal:

```bash
composer create-project --prefer-dist laravel-zero/laravel-zero movie-cli
```

You will then need to run the `app:rename` command to rename your project:

```bash
php application app:rename [movie-cli]
```

<a name="via-the-laravel-zero-installer"></a>
#### Via the Laravel Zero Installer (DEPRECATED)

> Note: This method is deprecated, although it will continue to work.

If you would like to use the Laravel Zero installer, install it using Composer:

```bash
composer global require "laravel-zero/installer"
```

Make sure to place composer's system-wide vendor bin directory in your `$PATH` so the Laravel Zero executable can be located by your system. This directory exists in different locations based on your operating system. On macOS and GNU/Linux distributions, it's `$HOME/.composer/vendor/bin`.

Once installed, the `laravel-zero new` command will create a fresh Laravel Zero installation in the directory you specify. For instance, `laravel-zero new movie-cli` will create a directory named `movie-cli` containing a fresh Laravel Zero installation with all of Laravel Zero's dependencies already installed:

```bash
laravel-zero new movie-cli
```

On versions v2.5.0 and later, the command will also automatically rename your app.
