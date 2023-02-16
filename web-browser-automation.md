---
title: Web Browser Automation
description: Web Browser Automation with Laravel Dusk
---

# Web Browser Automation

Using the `app:install` Artisan command you can install the `console-dusk` component:
```bash
php <your-app-name> app:install console-dusk
```

The Console Dusk allows the usage of Laravel Dusk in Artisan commands. However, in Laravel Zero
you can use Laravel Dusk for web tasks that should be automated. Let's take a look at the usage:
```php
class VisitLaravelZeroCommand extends Command
{
    public function handle(): void
    {
        $this->browse(function ($browser) {
            $browser->visit('https://laravel-zero.com')
                ->assertSee('Laravel Zero');
        });
    }
}
```

Output example:

<img src="https://github.com/nunomaduro/laravel-console-dusk/raw/master/docs/example.gif" class="md:w-4/5 md:mx-auto">

Get more details: [https://github.com/nunomaduro/laravel-console-dusk](https://github.com/nunomaduro/laravel-console-dusk).
