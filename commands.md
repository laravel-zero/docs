---
title: Commands
description: Writing the commands that power your console application
---

# Commands

- [Introduction](#introduction)
- [Writing Commands](#writing-commands)
    - [Generating Commands](#generating-commands)
    - [Command Structure](#command-structure)
    - [Exit Codes](#exit-codes)
- [Defining Input Expectations](#defining-input-expectations)
    - [Arguments](#arguments)
    - [Options](#options)
- [Command I/O](#command-io)
    - [Retrieving Input](#retrieving-input)
    - [Prompting for Input](#prompting-for-input)
    - [Writing Output](#writing-output)
    - [Styling Output With Termwind](#styling-output-with-termwind)
- [The Default Command](#the-default-command)
- [Registering Commands](#registering-commands)
- [Programmatically Executing Commands](#programmatically-executing-commands)
- [Scheduling Commands](#scheduling-commands)
- [Stub Customization](#stub-customization)

<a name="introduction"></a>
## Introduction

Commands are the heart of a Laravel Zero application. They are stored in the `app/Commands` directory, and every command placed in that directory is registered with your application automatically — there is no list to maintain.

A fresh application ships with a single example command, `app/Commands/InspireCommand.php`, which you may run like so:

```shell
php application inspire
```

<a name="writing-commands"></a>
## Writing Commands

<a name="generating-commands"></a>
### Generating Commands

To create a new command, you may use the `make:command` Artisan command. This command will create a new command class in the `app/Commands` directory:

```shell
php application make:command SendEmails
```

<a name="command-structure"></a>
### Command Structure

After generating your command, you should define appropriate values for the `signature` and `description` properties of the class. The `signature` property will be used when displaying your command on the list screen, and it also allows you to define [your command's input expectations](#defining-input-expectations). The `handle` method will be called when your command is executed, and it is the place where the logic of your command should live.

Let's take a look at an example command. Note that we are able to request any dependencies we need via the command's `handle` method. The Laravel [service container](https://laravel.com/docs/container) will automatically inject all dependencies that are type-hinted in this method's signature:

```php
<?php

namespace App\Commands;

use App\Support\DripEmailer;
use Illuminate\Console\Scheduling\Schedule;
use LaravelZero\Framework\Commands\Command;

class SendEmails extends Command
{
    /**
     * The signature of the command.
     *
     * @var string
     */
    protected $signature = 'mail:send {user}';

    /**
     * The description of the command.
     *
     * @var string
     */
    protected $description = 'Send a marketing email to a user';

    /**
     * Execute the console command.
     */
    public function handle(DripEmailer $drip): void
    {
        $drip->send($this->argument('user'));

        $this->info('The email was sent successfully.');
    }

    /**
     * Define the command's schedule.
     */
    public function schedule(Schedule $schedule): void
    {
        // $schedule->command(static::class)->everyMinute();
    }
}
```

> **Note:** For greater code reuse, it is good practice to keep your console commands light and let them defer to application services to accomplish their tasks. In the example above, note that we inject a service class to do the "heavy lifting" of sending the emails.

Laravel Zero commands extend `LaravelZero\Framework\Commands\Command`, which itself extends Laravel's own `Illuminate\Console\Command` class. In addition to everything Laravel offers, the base class provides the [`schedule`](#scheduling-commands) method, the [`task`](/docs/run-tasks) method, and the `title` method.

<a name="exit-codes"></a>
### Exit Codes

If nothing is returned from the `handle` method and the command executes successfully, the command will exit with a `0` exit code, indicating success. However, the `handle` method may optionally return an integer to manually specify the command's exit code:

```php
$this->error('Something went wrong.');

return 1;
```

If you would like to "fail" the command from any method within the command, you may utilize the `fail` method. The `fail` method will immediately terminate execution of the command and return an exit code of `1`:

```php
$this->fail('Something went wrong.');
```

<a name="defining-input-expectations"></a>
## Defining Input Expectations

When writing console commands, it is common to gather input from the user through arguments or options. Laravel Zero makes it very convenient to define the input you expect from the user using the `signature` property on your commands. The `signature` property allows you to define the name, arguments, and options for the command in a single, expressive, route-like syntax.

<a name="arguments"></a>
### Arguments

All user supplied arguments and options are wrapped in curly braces. In the following example, the command defines one required argument, `user`:

```php
protected $signature = 'mail:send {user}';
```

You may also make arguments optional or define default values for arguments:

```php
// Optional argument...
protected $signature = 'mail:send {user?}';

// Optional argument with default value...
protected $signature = 'mail:send {user=foo}';
```

<a name="options"></a>
### Options

Options, like arguments, are another form of user input. Options are prefixed by two hyphens (`--`) when they are provided via the command line. There are two types of options: those that receive a value and those that don't. Options that don't receive a value serve as a boolean "switch":

```php
protected $signature = 'mail:send {user} {--queue}';
```

If the user must specify a value for an option, you should suffix the option name with an `=` sign:

```php
protected $signature = 'mail:send {user} {--queue=}';
```

You may assign descriptions to arguments and options by separating the name from the description using a colon. If you need a little extra room to define your command, feel free to spread the definition over multiple lines:

```php
protected $signature = 'mail:send
                        {user : The ID of the user}
                        {--queue : Whether the job should be queued}';
```

For a complete list of the available input syntax — including input arrays, shortcuts, and prompting for missing input — check out the [Defining Input Expectations](https://laravel.com/docs/artisan#defining-input-expectations) documentation on the Laravel website.

<a name="command-io"></a>
## Command I/O

<a name="retrieving-input"></a>
### Retrieving Input

While your command is executing, you will likely need to access the values for the arguments and options accepted by your command. To do so, you may use the `argument` and `option` methods. If an argument or option does not exist, `null` will be returned:

```php
$userId = $this->argument('user');

$queue = $this->option('queue');
```

If you need to retrieve all of the arguments or options as an array, you may call the `arguments` and `options` methods:

```php
$arguments = $this->arguments();

$options = $this->options();
```

<a name="prompting-for-input"></a>
### Prompting for Input

Every Laravel Zero application includes [Laravel Prompts](https://laravel.com/docs/prompts), a PHP package for adding beautiful and user-friendly forms to your command line applications, with browser-like features including placeholder text and validation:

```php
use function Laravel\Prompts\select;
use function Laravel\Prompts\text;

$name = text('What is your name?', required: true);

$role = select('What role should the user have?', [
    'member' => 'Member',
    'admin' => 'Administrator',
]);
```

In addition to Laravel Prompts, the base command class offers the classic `ask`, `secret`, `confirm`, `anticipate`, and `choice` methods:

```php
$name = $this->ask('What is your name?');

$password = $this->secret('What is the password?');

if ($this->confirm('Do you wish to continue?')) {
    // ...
}

$name = $this->choice('What is your name?', ['Taylor', 'Dayle'], $default);
```

<a name="writing-output"></a>
### Writing Output

To send output to the console, you may use the `line`, `info`, `comment`, `question`, `warn`, and `error` methods. Each of these methods will use appropriate ANSI colors for their purpose:

```php
$this->info('The command was successful!');

$this->error('Something went wrong!');
```

If you would like to display a "title" bar above your output, the base command class provides the `title` method:

```php
$this->title('Building process');
```

For long running operations, the `task` method displays the given description alongside a "success" or "failure" indicator once the given closure has finished executing. You may learn more about it in the [running tasks](/docs/run-tasks) documentation:

```php
$this->task('Fetching the movies', function () {
    return true;
});
```

Laravel Zero also gives you access to Laravel's table and progress bar helpers:

```php
$this->table(
    ['Name', 'Email'],
    [['Taylor', 'taylor@laravel.com']],
);

$this->withProgressBar($users, function ($user) {
    $this->performTask($user);
});
```

<a name="styling-output-with-termwind"></a>
### Styling Output With Termwind

Every Laravel Zero application includes [Termwind](https://github.com/nunomaduro/termwind), which allows you to style your command line output using a syntax borrowed from [Tailwind CSS](https://tailwindcss.com). The `render` function accepts an HTML string and renders it to the terminal:

```php
use function Termwind\render;

render(<<<'HTML'
    <div class="py-1 ml-2">
        <div class="px-1 bg-blue-300 text-black">Laravel Zero</div>
        <em class="ml-1">
          Simplicity is the ultimate sophistication.
        </em>
    </div>
HTML);
```

This is exactly how the example `inspire` command included with a fresh application renders its output.

<a name="the-default-command"></a>
## The Default Command

When your application is invoked without a command name, Laravel Zero runs the "default" command. By default, this is a summary of all of the commands available to your application:

```shell
php application
```

Many console applications only do one thing. If that is the case for your application, you may set your own command as the default within the `config/commands.php` configuration file:

```php
'default' => \App\Commands\MovieCommand::class,
```

> **Note:** When you set your own default command, arguments and options provided on the command line will be proxied to it, so `php movie-cli "Star Wars"` will behave exactly as you would expect.

<a name="registering-commands"></a>
## Registering Commands

All of the commands within the `app/Commands` directory are automatically registered with your application. However, you are free to instruct Laravel Zero to scan other directories for commands using the `paths` option of your `config/commands.php` configuration file:

```php
'paths' => [
    app_path('Commands'),
    app_path('Console'),
],
```

You may also register individual command classes — for example, commands provided by a third-party package — using the `add` option:

```php
'add' => [
    \Vendor\Package\Commands\ExampleCommand::class,
],
```

Consult the [configuration documentation](/docs/configuration) to learn how to hide or remove commands from your application's command list.

<a name="programmatically-executing-commands"></a>
## Programmatically Executing Commands

Sometimes you may wish to execute a command from within another command. You may do so using the `call` method. This method accepts the command name and an array of command arguments or options:

```php
$exitCode = $this->call('mail:send', [
    'user' => 1, '--queue' => 'default',
]);
```

If you would like to call another console command and suppress all of its output, you may use the `callSilently` method:

```php
$this->callSilently('mail:send', [
    'user' => 1,
]);
```

You may also execute commands from outside of a command class using the `Artisan` facade:

```php
use Illuminate\Support\Facades\Artisan;

Artisan::call('mail:send', ['user' => 1]);
```

> **Note:** Every command executed via `call` and `callSilently` is recorded by the framework, which allows you to assert against it in your tests using the `assertCommandCalled` method. See the [testing documentation](/docs/testing) for more details.

<a name="scheduling-commands"></a>
## Scheduling Commands

In addition to Laravel's standard command features, Laravel Zero commands offer a `schedule` method. This method allows a command to define its own schedule, right next to the logic it schedules:

```php
public function schedule(Schedule $schedule): void
{
    $schedule->command(static::class)->everyMinute();
}
```

Head over to the [task scheduling](/docs/task-scheduling) documentation to learn more.

<a name="stub-customization"></a>
## Stub Customization

The `make:command` command uses "stub" files that are populated with values to generate the command class. You may customize the generated file by publishing the stubs to your project:

```shell
php application stub:publish
```

The published stub will be placed in a `stubs` directory at the root of your application. Any changes you make to `stubs/console.stub` will be reflected the next time you generate a command.
