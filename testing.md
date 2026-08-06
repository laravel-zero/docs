---
title: Testing
description: Testing your console application with Pest
---

# Testing

- [Introduction](#introduction)
- [Creating Tests](#creating-tests)
- [Running Tests](#running-tests)
- [Testing Commands](#testing-commands)
    - [Testing Input and Output](#testing-input-and-output)
    - [Asserting Commands Were Called](#asserting-commands-were-called)

<a name="introduction"></a>
## Introduction

Laravel Zero is built with testing in mind. Every application ships with a `tests` directory containing a [Pest](https://pestphp.com) test suite, a `Feature` suite that boots the full application, and a `Unit` suite for everything else.

Your test cases extend `Tests\TestCase`, which itself extends `LaravelZero\Framework\Testing\TestCase`. This gives you the same testing experience you are used to in Laravel, adapted to the console.

<a name="creating-tests"></a>
## Creating Tests

To create a new test case, use the `make:test` Artisan command. By default, tests will be placed in the `tests/Feature` directory:

```shell
php application make:test SendEmailsTest
```

If you would like to create a test within the `tests/Unit` directory, you may use the `--unit` option:

```shell
php application make:test SendEmailsTest --unit
```

<a name="running-tests"></a>
## Running Tests

You may run your tests using the `test` Artisan command:

```shell
php application test
```

Any argument that may be passed to Pest may also be passed to the `test` command:

```shell
php application test --filter=inspire
```

Of course, you may also invoke Pest directly:

```shell
./vendor/bin/pest
```

> **Note:** The `test` command is only registered when your application is not running in the `production` environment, so it will never appear in a build of your application.

<a name="testing-commands"></a>
## Testing Commands

<a name="testing-input-and-output"></a>
### Testing Input and Output

Laravel Zero provides Laravel's `artisan` test method, which allows you to easily test your commands. Using the `expectsOutput`, `expectsQuestion`, `expectsConfirmation`, and `assertExitCode` methods, you may write expressive tests for your commands' entire interaction with the user:

```php
it('inspires artisans', function () {
    $this->artisan('inspire')
        ->expectsOutput('Simplicity is the ultimate sophistication.')
        ->assertExitCode(0);
});
```

Interactive commands may be tested by describing the answers the user provides:

```php
it('creates a user', function () {
    $this->artisan('user:create')
        ->expectsQuestion('What is your name?', 'Taylor')
        ->expectsConfirmation('Do you wish to continue?', 'yes')
        ->expectsOutput('User created successfully.')
        ->assertSuccessful();
});
```

For a complete list of the available assertions, consult the [console tests](https://laravel.com/docs/console-tests) documentation on the Laravel website.

<a name="asserting-commands-were-called"></a>
### Asserting Commands Were Called

Laravel Zero records every command executed through the `call` and `callSilently` methods. This allows you to assert that a command dispatched the commands you expected, without asserting on their output:

```php
it('migrates the database before importing', function () {
    $this->artisan('movies:import');

    $this->assertCommandCalled('migrate', ['--force' => true]);
});
```

The inverse assertion is available through the `assertCommandNotCalled` method:

```php
$this->assertCommandNotCalled('migrate:fresh');
```
