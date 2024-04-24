---
title: Testing
description: The `tests` folder
---

# Testing

The `tests` folder contains your [Pest](https://pestphp.com) tests. By default, Laravel Zero
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
