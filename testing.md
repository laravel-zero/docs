---
title: Testing
description: The `tests` folder
---

# Testing

The `tests` folder contains your [Pest](https://pestphp.com) or [PHPUnit](https://phpunit.de) tests. By default, Laravel Zero
ships with an *Integration* suite written with Pest syntax that can be used as follows:

```php
test('inspiring command', function () {
    $this->artisan('inspiring')
         ->expectsOutput('Simplicity is the ultimate sophistication.')
         ->assertExitCode(0);
});
```

As usual, you can always run your tests using the `pest` command:

```bash
./vendor/bin/pest
```

## Asserting that a command was called

Using the `assertCommandCalled` test helper method you can check if a specific method was called.
This is helpful if you need to check if calling _SomeFirstCommand_ also triggers _AnotherCommand_.

The inverse of this assertion is `assertCommandNotCalled`.

__Note__ that both assertions are "argument-sensitive".

```php
test('migration command', function () {
    $this->artisan('migrate', ['--seed' => true]);

    // Assert that a command was called
    $this->assertCommandCalled('migrate', ['--seed' => true]);
    $this->assertCommandCalled('db:seed');

    // Assert that a command was *NOT* called
    $this->assertCommandNotCalled('migrate', ['--seed' => false]);
});
```

## Using PHPUnit-style tests

Pest has full support for running PHPUnit tests, however if you would rather use PHPUnit directly, you can either run the binary with:

```bash
./vendor/bin/phpunit
```

Or uninstall Pest and install PHPUnit via Composer:

```bash
composer remove pestphp/pest --dev && composer require phpunit/phpunit --dev
```
