---
title: Installation
description: Create your first Laravel Zero application
---

# Installation

- [Requirements](#requirements)
- [Creating an Application](#creating-an-application)
    - [Naming Your Application](#naming-your-application)
- [Directory Structure](#directory-structure)
- [Running Your Application](#running-your-application)
- [Next Steps](#next-steps)

<a name="requirements"></a>
## Requirements

Laravel Zero has a few system requirements. Before creating your first application, make sure that your local machine meets the following:

- PHP 8.3 or greater
- [Composer](https://getcomposer.org)

If you intend to [distribute your application as a PHAR archive](/docs/distribute-as-a-phar-archive), the [`sodium`](https://php.net/manual/book.sodium.php) PHP extension is also required, since it is used by [Box](https://github.com/box-project/box) to compile the archive.

<a name="creating-an-application"></a>
## Creating an Application

Laravel Zero utilizes [Composer](https://getcomposer.org) to manage its dependencies. You may create a new application by issuing the Composer `create-project` command in your terminal:

```shell
composer create-project --prefer-dist laravel-zero/laravel-zero movie-cli
```

<a name="naming-your-application"></a>
### Naming Your Application

A fresh application is executed via a binary named `application`. Once your project has been created, you should run the `app:rename` command to give the binary a name of your own:

```shell
cd movie-cli

php application app:rename movie-cli
```

This command renames the binary at the root of your project and updates the `bin` entry within your `composer.json` file. From this point forward, all of the examples in this documentation that reference `php application` should be invoked using your own application's name instead:

```shell
php movie-cli inspire
```

> **Note:** If you invoke `app:rename` without providing a name, Laravel Zero will prompt you for one.

<a name="directory-structure"></a>
## Directory Structure

Laravel Zero applications are intentionally small. A fresh installation looks like this:

```
├── app
│   ├── Commands
│   │   └── InspireCommand.php
│   └── Providers
│       └── AppServiceProvider.php
├── bootstrap
│   ├── app.php
│   └── providers.php
├── config
│   ├── app.php
│   └── commands.php
├── tests
├── application
├── box.json
└── composer.json
```

| Path | Description |
| ---- | ----------- |
| `app/Commands` | Your application's [commands](/docs/commands). Every command in this directory is registered automatically. |
| `app/Providers` | Your application's [service providers](/docs/service-providers). |
| `bootstrap/app.php` | Creates the application instance. |
| `bootstrap/providers.php` | The list of service providers loaded by your application. |
| `config` | Your application's [configuration files](/docs/configuration). |
| `tests` | Your application's [Pest tests](/docs/testing). |
| `application` | The binary used to invoke your commands. |
| `box.json` | The [Box](https://github.com/box-project/box) configuration used when [building](/docs/distribute-as-a-phar-archive) your application. |

Directories such as `database`, `resources`, and `storage` are not present in a fresh installation. They are created for you when you install the component that needs them.

<a name="running-your-application"></a>
## Running Your Application

To view a list of all of the commands available to your application, run the binary without any arguments:

```shell
php application
```

A fresh application ships with a single example command. Let's run it:

```shell
php application inspire
```

Every command also includes a "help" screen which displays and describes the command's available arguments and options:

```shell
php application help inspire
```

<a name="next-steps"></a>
## Next Steps

Your application is ready. Now, head over to the [commands documentation](/docs/commands) to write your first command, or browse the [configuration documentation](/docs/configuration) to learn how to customize your application's name, version, and command list.
