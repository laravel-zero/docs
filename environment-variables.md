---
title: Environment Variables
description: Loading environment variables from a .env file
---

# Environment Variables

- [Introduction](#introduction)
- [Installation](#installation)
- [Retrieving Environment Variables](#retrieving-environment-variables)
- [Environment Variables in a Build](#environment-variables-in-a-build)

<a name="introduction"></a>
## Introduction

It is often helpful to have different configuration values based on the environment in which your application is running, or to keep credentials out of your source control. To make this a cinch, Laravel Zero uses the [DotEnv](https://github.com/vlucas/phpdotenv) PHP library, which loads environment variables from a `.env` file at the root of your project.

<a name="installation"></a>
## Installation

You may scaffold the required files using the `app:install` Artisan command:

```shell
php application app:install dotenv
```

The installer creates a `.env` file and a matching `.env.example` file at the root of your project, and adds `.env` to your `.gitignore` file.

> **Note:** Your `.env` file should not be committed to source control. Each developer using your application may require a different environment configuration, and committing the file would be a security risk if it contains sensitive credentials. The `.env.example` file, on the other hand, should be committed, so your team can see which variables your application expects.

<a name="retrieving-environment-variables"></a>
## Retrieving Environment Variables

Assuming your `.env` file contains the following:

```
TMDB_KEY=234567
```

You may retrieve the value using the `env` helper. The second argument passed to the `env` function is the "default value", which will be returned if no environment variable exists for the given key:

```php
$key = env('TMDB_KEY');

$key = env('TMDB_KEY', 'default-key');
```

> **Note:** As in Laravel, you should only call the `env` helper from within your [configuration files](/docs/configuration). Elsewhere in your application, read the value through the `config` helper instead.

<a name="environment-variables-in-a-build"></a>
## Environment Variables in a Build

When your application has been compiled into a [PHAR archive](/docs/distribute-as-a-phar-archive), Laravel Zero also looks for a `.env` file placed alongside the archive itself, and loads it on top of any variables that were already defined. This allows the people using your application to configure it without rebuilding anything:

```
.
├── .env
└── movie-cli
```
