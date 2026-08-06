---
title: Configuration
description: The `config` folder
---

# Configuration

The `config` folder should contain your application config files. Files in this folder
are automatically registered as configuration files. As example, if you create an `config/bar.php`,
you can access it using `config('bar')`.

The configuration files `config/app.php` and `config/commands.php` are used internally by the
framework, and can not be removed.

The `config/app.php` contains information related to your application:

| Property  | Description
| ------------- | -------------
| name  | This value is the name of your application
| version  | This value determines the "version" your application is currently running in.
| env  | This value determines the "environment" your application is currently running in.

The default command of your application contains a list of commands. That list of commands
can be configured using `config/commands.php`:

| Property  | Description
| ------------- | -------------
| default  | The default application command when no command name is provided.
| paths  | The "paths" that should be loaded by the console's kernel.
| add  | Here you may specify which commands classes you wish to include.
| hidden  | Adds the provided commands, but make them hidden.
| remove  | Removes the list of commands provided.

<a name="disabling-default-component-providers"></a>
### Disabling default component providers

The Database, Log, and Queue components support disabling auto-loading for their
default service provider to allow the use of a custom provider.

To disable the default service provider for a component, set the `useDefaultProvider` value
to `false` in the configuration file. You can then add your custom `ServiceProvider` class
to the `app.providers` configuration array.

### Disabling Developer Commands in PHAR Build

To remove the developer centric commands from the application when being used in PHAR you can modify `config/commands.php`

```php
 'remove' => Phar::running() ? [
        Illuminate\Database\Console\Migrations\FreshCommand::class,
        Illuminate\Database\Console\Migrations\InstallCommand::class,
        Illuminate\Database\Console\Migrations\MigrateCommand::class,
        Illuminate\Database\Console\Migrations\RefreshCommand::class,
        Illuminate\Database\Console\Migrations\ResetCommand::class,
        Illuminate\Database\Console\Migrations\RollbackCommand::class,
        Illuminate\Database\Console\Migrations\StatusCommand::class,
        Illuminate\Database\Console\Migrations\MigrateMakeCommand::class,
        Illuminate\Database\Console\Seeds\SeedCommand::class,
        Illuminate\Database\Console\WipeCommand::class,
        Illuminate\Database\Console\Factories\FactoryMakeCommand::class,
        Illuminate\Foundation\Console\ModelMakeCommand::class,
        Illuminate\Database\Console\Seeds\SeederMakeCommand::class,
        LaravelZero\Framework\Commands\MakeCommand::class,
        LaravelZero\Framework\Commands\RenameCommand::class,
        LaravelZero\Framework\Commands\StubPublishCommand::class,
        LaravelZero\Framework\Commands\BuildCommand::class,
        LaravelZero\Framework\Commands\InstallCommand::class,
    ] : [],
```
