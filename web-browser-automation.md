---
title: Web Browser Automation
description: Automate a real browser from your Artisan commands with Laravel Dusk
---

# Web Browser Automation

- [Introduction](#introduction)
- [Installation](#installation)
- [Browsing From a Command](#browsing-from-a-command)

<a name="introduction"></a>
## Introduction

[Laravel Dusk](https://laravel.com/docs/dusk) provides an expressive, easy-to-use browser automation API. In Laravel, it is used for testing. In a console application, it is a great way to automate web tasks — scraping a page, filling out a form, or taking a screenshot on a schedule.

The `console-dusk` component makes Laravel Dusk available inside your Artisan commands.

<a name="installation"></a>
## Installation

You may install the `console-dusk` component using the `app:install` Artisan command:

```shell
php application app:install console-dusk
```

<a name="browsing-from-a-command"></a>
## Browsing From a Command

Once the component is installed, your commands have access to the `browse` method. It accepts a closure that receives a browser instance:

```php
<?php

namespace App\Commands;

use LaravelZero\Framework\Commands\Command;

class VisitLaravelZeroCommand extends Command
{
    protected $signature = 'visit';

    protected $description = 'Visit the Laravel Zero website';

    public function handle(): void
    {
        $this->browse(function ($browser) {
            $browser->visit('https://laravel-zero.com')
                ->assertSee('Laravel Zero');
        });
    }
}
```

Running the command will drive a real browser:

<img src="https://github.com/nunomaduro/laravel-console-dusk/raw/master/docs/example.gif" class="md:w-4/5 md:mx-auto">

The full browser API — clicking links, filling forms, waiting for elements, and taking screenshots — is documented in the [Dusk documentation](https://laravel.com/docs/dusk) on the Laravel website.

> **Note:** Behind the scenes, this component uses the [`nunomaduro/laravel-console-dusk`](https://github.com/nunomaduro/laravel-console-dusk) package. You will find further details there.
