---
title: Distribute as a PHAR Archive
description: Build a standalone PHAR archive to ease the distribution of your application
---

# Distribute as a PHAR Archive

- [Introduction](#introduction)
- [Building Your Application](#building-your-application)
    - [Non-Interactive Builds](#non-interactive-builds)
    - [Configuring the Build](#configuring-the-build)
    - [Debugging a Failed Build](#debugging-a-failed-build)
- [What Changes in a Build](#what-changes-in-a-build)
- [Distributing via Packagist](#distributing-via-packagist)
- [Self Updating](#self-updating)
    - [Update Strategies](#update-strategies)

<a name="introduction"></a>
## Introduction

Every Laravel Zero project may be compiled into a standalone [PHAR archive](https://php.net/manual/en/book.phar.php) — a single file containing all of your application's code and its dependencies. Anyone with PHP installed may then run it, without cloning your repository or running `composer install`.

Laravel Zero uses [Box](https://github.com/box-project/box) to provide fast application bundling. Box is included with the framework, so there is nothing to install.

> **Note:** If you would like to distribute your application to people who don't have PHP at all, take a look at [building a single executable binary](/docs/distribute-as-a-single-executable-binary).

<a name="building-your-application"></a>
## Building Your Application

You may compile your application using the `app:build` Artisan command:

```shell
php application app:build movie-cli
```

You will be asked for a build version, which will be baked into the archive as the value of your `app.version` configuration option. Once the build finishes, the archive is placed in your project's `builds` directory, ready to be executed:

```shell
./builds/movie-cli
```

Or, on Windows:

```shell
C:\application\path> php builds\movie-cli
```

<a name="non-interactive-builds"></a>
### Non-Interactive Builds

When building from a script or a continuous integration pipeline, you will not want to be asked for the build version. You may provide it upfront using the `--build-version` option:

```shell
php application app:build movie-cli --build-version=1.0.0
```

The build process is also subject to a timeout of 300 seconds. Larger applications may need more time, which you may grant using the `--timeout` option. Pass `0` to disable the timeout entirely:

```shell
php application app:build movie-cli --timeout=600
```

<a name="configuring-the-build"></a>
### Configuring the Build

The contents of the archive are determined by the `box.json` file at the root of your project. A fresh application includes the `app`, `bootstrap`, `config`, and `vendor` directories:

```json
{
    "chmod": "0755",
    "directories": [
        "app",
        "bootstrap",
        "config",
        "vendor"
    ],
    "files": [
        "composer.json"
    ],
    "exclude-composer-files": false,
    "compression": "GZ"
}
```

If your application relies on other directories at runtime — such as `database` or `resources` — you should add them to the `directories` array. Consult the [Box configuration documentation](https://github.com/box-project/box/blob/master/doc/configuration.md) for the full list of available options.

<a name="debugging-a-failed-build"></a>
### Debugging a Failed Build

Builds fail for a number of reasons, but a typical pitfall is a missing `ext-*` dependency in the PHP installation performing the build — for example, when building inside a Docker container or a CI runner.

Running the build with increased verbosity will surface the output of Box:

```shell
php application app:build movie-cli -v
```

If that isn't enough, you may invoke Box directly with its `--debug` option:

```shell
./vendor/laravel-zero/framework/bin/box compile --working-dir=/project/path --config=/project/path/box.json --debug
```

<a name="what-changes-in-a-build"></a>
## What Changes in a Build

Two things are true of your application inside an archive that are not true during development, and both affect how you write your commands:

- **The environment becomes `production`.** Development commands such as `app:build`, `app:install`, `make:command`, and `test` are no longer registered. You may remove additional commands using the `remove` option of your [`config/commands.php`](/docs/configuration#removing-development-commands-from-a-build) configuration file.
- **The filesystem becomes read-only.** Nothing inside the archive may be written to. This affects [writing files](/docs/filesystem#writing-files-from-a-build), [log files](/docs/logging#logging-from-a-build), [compiled Blade views](/docs/view#using-views-in-a-build), and [SQLite databases](/docs/database#choosing-a-database-location).

If the [`dotenv`](/docs/environment-variables) component is installed, your application will also load a `.env` file placed in the same directory as the archive, which allows the people using your application to configure it without rebuilding.

<a name="distributing-via-packagist"></a>
## Distributing via Packagist

To distribute your application via [Packagist](https://packagist.org), so that it may be installed with `composer global require`, you need to make a few changes to your `composer.json` and `box.json` files.

Within your `composer.json` file, move the `laravel-zero/framework` dependency — along with any other dependency already bundled in your archive — from `require` to `require-dev`, leaving the supported PHP versions and required extensions in place. Then, point the `bin` entry at your build:

```diff
-   "require": {
-       "laravel-zero/framework": "^13.0"
-   },
+   "require-dev": {
+       "laravel-zero/framework": "^13.0"
+   },

-   "bin": ["movie-cli"]
+   "bin": ["builds/movie-cli"]
```

Within your `box.json` file, you should add:

```json
"exclude-dev-files": false,
```

The reason for these changes is that Composer installs all non-development dependencies by default. Those dependencies are already contained within the archive, so converting them to development dependencies means Composer will skip them altogether — which also ensures they can't conflict with other globally installed packages.

Finally, build your application once more, and you are ready to publish it:

```shell
php application app:build movie-cli
```

```shell
composer global require your-vendor/movie-cli
```

<a name="self-updating"></a>
## Self Updating

The `self-update` component adds a `self-update` command to your built application, which downloads the latest version from your repository if one is available. You may install it using the `app:install` Artisan command:

```shell
php application app:install self-update
```

Once the component is installed, the people using your application may update it in place:

```shell
./movie-cli self-update
```

<a name="update-strategies"></a>
### Update Strategies

The self-updater uses "strategies" to determine where a new version should be downloaded from. Laravel Zero ships with three:

| Strategy | Description |
| -------- | ----------- |
| `LaravelZero\Framework\Components\Updater\Strategy\GithubStrategy` | Downloads the archive from a `builds/` directory in the GitHub repository. This is the default. |
| `LaravelZero\Framework\Components\Updater\Strategy\GithubReleasesStrategy` | Downloads the archive from GitHub release assets. |
| `LaravelZero\Framework\Components\Updater\Strategy\GitlabStrategy` | Downloads the archive from a `builds/` directory in the GitLab repository. |

To choose a different strategy, first publish the component's configuration file:

```shell
php application vendor:publish --provider="LaravelZero\Framework\Components\Updater\Provider"
```

Then, update the `strategy` option within your `config/updater.php` configuration file:

```php
use LaravelZero\Framework\Components\Updater\Strategy\GithubReleasesStrategy;

'strategy' => GithubReleasesStrategy::class,
```

You are also free to write a strategy of your own. Custom strategies must implement the [`StrategyInterface`](https://github.com/laravel-zero/framework/blob/master/src/Components/Updater/Strategy/StrategyInterface.php) interface.
