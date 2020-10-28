---
title: Installation
description: Create a new Laravel Zero project
---

# Installation

> **Requires [PHP 7.3+](https://php.net/releases)**

Laravel Zero utilizes [Composer](https://getcomposer.org) to manage its dependencies. So, before using Laravel Zero, make sure you have Composer installed on your machine.

<a name="via-the-laravel-zero-installer"></a>
#### Via the Laravel Zero Installer

First, download the Laravel Zero installer using Composer:

```bash
composer global require "laravel-zero/installer"
```

Make sure to place composer's system-wide vendor bin directory in your `$PATH` so the laravel Zero executable can be located by your system. This directory exists in different locations based on your operating system. On macOS and GNU/Linux distributions, it's `$HOME/.composer/vendor/bin`.

Once installed, the `laravel-zero new` command will create a fresh Laravel Zero installation in the directory you specify. For instance, `laravel-zero new movie-cli` will create a directory named `movie-cli` containing a fresh Laravel Zero installation with all of Laravel Zero's dependencies already installed:

```bash
laravel-zero new movie-cli
```

<a name="via-composer-create-project"></a>
#### Via Composer Create-Project

Alternatively, you may also install Laravel Zero by issuing the Composer `create-project` command in your terminal:

```bash
composer create-project --prefer-dist laravel-zero/laravel-zero movie-cli
```

You will then need to run the `app:rename` command to rename your project:

```bash
php application app:rename [movie-cli]
```
