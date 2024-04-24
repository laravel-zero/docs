---
title: Installation
description: Create a new Laravel Zero project
---

# Installation

> **Requires:**
>- [PHP 8.2+](https://php.net/releases)
>- [**`sodium`** PHP extension](https://php.net/manual/book.sodium.php) (required by [Box](https://github.com/box-project/box)) - if you are bundling to a Phar

Laravel Zero utilizes [Composer](https://getcomposer.org) to manage its dependencies. So, before using Laravel Zero, make sure you have Composer installed on your machine.

You may install Laravel Zero by issuing the Composer `create-project` command in your terminal:

```bash
composer create-project --prefer-dist laravel-zero/laravel-zero movie-cli
```

You will then need to run the `app:rename` command to rename your project:

```bash
php application app:rename [movie-cli]
```
