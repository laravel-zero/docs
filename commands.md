---
title: Commands
description: The `app/Commands` folder
---

# Commands

The `app/Commands` folder should contain your application's Artisan commands. By default,
it provides `app/Commands/InspiringCommand.php` as an example command:
```php
namespace App\Commands;

use Illuminate\Console\Scheduling\Schedule;
use LaravelZero\Framework\Commands\Command;

class InspiringCommand extends Command
{
    /**
     * The signature of the command.
     *
     * @var string
     */
    protected $signature = 'inspiring {name=Artisan}';

    /**
     * The description of the command.
     *
     * @var string
     */
    protected $description = 'Display an inspiring quote';

    /**
     * Execute the console command.
     */
    public function handle(): void
    {
        $this->info('Simplicity is the ultimate sophistication.');
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

Of course, you can always create a new command using the `make:command` Artisan command:
```bash
php <your-app-name> make:command <NewCommand>
```

The `signature` property should contain the definition of the input expectations. This is the place
to define how to gather input from the user through arguments or options:
```php
 protected $signature = 'user:create
                        {name : The name of the user (required)}
                        {--age= : The age of the user (optional)}'
```

For more information, check out the [Defining Input Expectations](https://laravel.com/docs/artisan#defining-input-expectations) on the Laravel Documentation.

The `description` property should contain one line description of your command's job. Later, this description is used on the application list of commands.

The `handle` method is the place where the logic of your command should be. This method will be called when your command is executed. Note that we are able to inject any dependencies we need into the `handle` method:
```php
public function handle(Service $service): void
{
    $service->execute('foo');

    $this->info('Operation executed');
}
```

The [Command I/O](https://laravel.com/docs/artisan#command-io) topic in the Laravel Documentation, can help
you to understand how to capture those input expectations and interact with the user using commands
like `line`, `info`, `comment`, `question` and `error` methods.

The `schedule` method allows to define the command's schedule. Please head over to the
topic [Task Scheduling](/docs/task-scheduling) to find more information about this method.
