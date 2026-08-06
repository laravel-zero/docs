---
title: Configuration
description: Configuring your application, its version, and its list of commands
---

# Configuration

- [Introduction](#introduction)
- [Accessing Configuration Values](#accessing-configuration-values)
- [Application Configuration](#application-configuration)
    - [Application Versioning](#application-versioning)
- [Command Configuration](#command-configuration)
    - [The Default Command](#the-default-command)
    - [Command Paths](#command-paths)
    - [Adding Commands](#adding-commands)
    - [Hiding Commands](#hiding-commands)
    - [Removing Commands](#removing-commands)
    - [Removing Development Commands From a Build](#removing-development-commands-from-a-build)
- [Publishing Configuration Files](#publishing-configuration-files)
- [Disabling Default Component Providers](#disabling-default-component-providers)

<a name="introduction"></a>
## Introduction

All of the configuration files for your application are stored in the `config` directory. Every file placed in that directory is registered as a configuration file automatically. For example, if you create a `config/movies.php` file, its values are immediately available via the `config` helper using the `movies` key.

A fresh application contains two configuration files, `config/app.php` and `config/commands.php`. Both are used internally by the framework and should not be removed.

<a name="accessing-configuration-values"></a>
## Accessing Configuration Values

You may easily access your configuration values using the `Config` facade or the global `config` function from anywhere in your application. Configuration values may be accessed using "dot" syntax, which includes the name of the file and the option you wish to access. A default value may also be specified and will be returned if the configuration option does not exist:

```php
use Illuminate\Support\Facades\Config;

$name = Config::get('app.name');

$name = config('app.name');

$name = config('app.name', 'Application');
```

To set configuration values at runtime, you may invoke the `Config` facade's `set` method or pass an array to the `config` function:

```php
Config::set('app.name', 'Movie CLI');

config(['app.name' => 'Movie CLI']);
```

<a name="application-configuration"></a>
## Application Configuration

The `config/app.php` file contains information about your application itself:

| Option | Description |
| ------ | ----------- |
| `name` | The name of your application. It is displayed on your application's list of commands. |
| `version` | The "version" your application is currently running in. |
| `env` | The "environment" your application is currently running in. May be overridden using the `--env` command line option. |
| `providers` | Additional [service providers](/docs/service-providers) that should be loaded by your application. |

<a name="application-versioning"></a>
### Application Versioning

By default, the `version` option resolves the `git.version` binding, which determines your application's version from the most recent Git tag:

```php
'version' => app('git.version'),
```

If your project is not tracked by Git, or has no tags yet, the version will be reported as `unreleased`. Of course, you are free to hard-code a version string instead:

```php
'version' => '1.0.0',
```

<a name="command-configuration"></a>
## Command Configuration

The list of commands presented by your application may be tailored using the `config/commands.php` configuration file:

| Option | Description |
| ------ | ----------- |
| `default` | The command that runs when no command name is provided. |
| `paths` | The paths that should be scanned by the console kernel for commands. |
| `add` | Individual command classes that should be added to your application. |
| `hidden` | Commands that should be hidden from the list of commands. |
| `remove` | Commands that should be removed from your application entirely. |

<a name="the-default-command"></a>
### The Default Command

Laravel Zero will always run the command specified by the `default` option when no command name is provided. Out of the box, this is a summary of every command your application offers:

```php
use NunoMaduro\LaravelConsoleSummary\SummaryCommand;

'default' => SummaryCommand::class,
```

If your application only does one thing, you should update this value to point to your own command:

```php
'default' => \App\Commands\MovieCommand::class,
```

<a name="command-paths"></a>
### Command Paths

The `paths` option determines the directories that should be scanned by the console kernel. For each path present in the array, the kernel will extract all `Illuminate\Console\Command` based classes and register them with your application:

```php
'paths' => [
    app_path('Commands'),
],
```

<a name="adding-commands"></a>
### Adding Commands

You may want to include a single command class without having to load an entire folder. The `add` option allows you to specify which command classes should be registered with your application:

```php
'add' => [
    \Vendor\Package\Commands\ExampleCommand::class,
],
```

<a name="hiding-commands"></a>
### Hiding Commands

Your application's commands will always be visible on the list of commands. But you can still make them "hidden" by specifying them within the `hidden` option. All hidden commands may still be executed:

```php
'hidden' => [
    \App\Commands\InternalCommand::class,
],
```

<a name="removing-commands"></a>
### Removing Commands

Do you have a service provider that loads a list of commands you don't need? No problem. The `remove` option allows you to specify a list of commands that should not be part of your application at all:

```php
'remove' => [
    \Illuminate\Database\Console\WipeCommand::class,
],
```

<a name="removing-development-commands-from-a-build"></a>
### Removing Development Commands From a Build

Development commands such as `app:build`, `app:install`, and `make:command` are only registered when your application is not running in the `production` environment, so they will not appear in a production build.

Third-party commands, such as the migration commands provided by the `database` component, are not aware of that distinction. To remove them from your [PHAR build](/docs/distribute-as-a-phar-archive) while keeping them available during local development, you may check whether the application is running from within a PHAR archive:

```php
'remove' => Phar::running() ? [
    \Illuminate\Database\Console\Migrations\FreshCommand::class,
    \Illuminate\Database\Console\Migrations\InstallCommand::class,
    \Illuminate\Database\Console\Migrations\MigrateCommand::class,
    \Illuminate\Database\Console\Migrations\MigrateMakeCommand::class,
    \Illuminate\Database\Console\Migrations\RefreshCommand::class,
    \Illuminate\Database\Console\Migrations\ResetCommand::class,
    \Illuminate\Database\Console\Migrations\RollbackCommand::class,
    \Illuminate\Database\Console\Migrations\StatusCommand::class,
    \Illuminate\Database\Console\Seeds\SeedCommand::class,
    \Illuminate\Database\Console\Seeds\SeederMakeCommand::class,
    \Illuminate\Database\Console\Factories\FactoryMakeCommand::class,
    \Illuminate\Database\Console\WipeCommand::class,
    \Illuminate\Foundation\Console\ModelMakeCommand::class,
] : [],
```

<a name="publishing-configuration-files"></a>
## Publishing Configuration Files

Several of Laravel Zero's built-in services provide a sensible default configuration and do not require a configuration file at all. The `cache` service, for example, uses the `array` driver until you tell it otherwise.

If you would like to take control of one of these services, you may publish its configuration file using the `config:publish` command:

```shell
php application config:publish cache
```

<a name="disabling-default-component-providers"></a>
## Disabling Default Component Providers

The [database](/docs/database), [logging](/docs/logging), and [queue](/docs/queues) components each register a default service provider that adapts the component to a console context. If you would like to replace one of them with a provider of your own, set the `useDefaultProvider` option to `false` within the component's configuration file:

```php
// config/database.php...

'useDefaultProvider' => false,
```

Then, register your own service provider class within your application's `bootstrap/providers.php` file.
