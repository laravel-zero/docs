---
title: Introduction
description: Laravel Zero is a micro-framework that provides an elegant starting point for your console application
---

# Introduction

- [Meet Laravel Zero](#meet-laravel-zero)
    - [Why Laravel Zero?](#why-laravel-zero)
- [What's Included](#whats-included)
- [Next Steps](#next-steps)

<a name="meet-laravel-zero"></a>
## Meet Laravel Zero

Laravel Zero is a micro-framework that provides an elegant starting point for your console application. It is an **unofficial** and customized version of [Laravel](https://laravel.com), optimized for building command line applications.

Laravel Zero was created by, and is maintained by, [Nuno Maduro](https://github.com/nunomaduro). It is built on top of the latest release of Laravel, the most popular PHP web framework. Because of that, you are free to take advantage of Laravel's core features — the service container, Eloquent, the scheduler, and more — while enjoying a minimal application structure and a lightweight, fast-booting experience.

Laravel Zero is **100% open source**, so you're free to dig through the source to see exactly how it works. See something that could be improved? Send us a [pull request on GitHub](https://github.com/laravel-zero).

<a name="why-laravel-zero"></a>
### Why Laravel Zero?

A console application does not need a router, a session store, or a view layer. Laravel Zero ships with none of them. What you get instead is the console — and a framework that stays out of your way.

#### A Familiar Framework

If you know Laravel, you already know Laravel Zero. Commands, service providers, configuration files, facades, and the service container all behave exactly as you would expect. Your existing knowledge — and the [Laravel documentation](https://laravel.com/docs) — carries over directly.

#### A Minimal Framework

A fresh Laravel Zero application contains a handful of files: a command, a service provider, and two configuration files. Everything else is optional. Need a database? Install the `database` component. Need Blade views? Install the `view` component. You only ship what you actually use.

#### A Distributable Framework

Console applications are meant to be handed to other people. Laravel Zero can compile your entire project — dependencies included — into a single [PHAR archive](/docs/distribute-as-a-phar-archive), or into a [standalone executable binary](/docs/distribute-as-a-single-executable-binary) that runs on machines without PHP installed at all.

<a name="whats-included"></a>
## What's Included

Out of the box, every Laravel Zero application includes:

| Feature | Description |
| ------- | ----------- |
| [Commands](/docs/commands) | Artisan commands, arguments, options, and command I/O. |
| [Laravel Prompts](https://laravel.com/docs/prompts) | Beautiful, user-friendly forms for your console application. |
| [Termwind](https://github.com/nunomaduro/termwind) | Style your command output using Tailwind CSS-like syntax. |
| [Collision](https://github.com/nunomaduro/collision) | Beautiful error reporting for your console application. |
| [Task Scheduling](/docs/task-scheduling) | Fluently define your application's command schedule. |
| [Testing](/docs/testing) | A [Pest](https://pestphp.com) test suite, ready to go. |
| [Build](/docs/distribute-as-a-phar-archive) | Compile your application into a single file executable. |

Everything else — [databases](/docs/database), [queues](/docs/queues), [logging](/docs/logging), [Blade views](/docs/view), [MCP servers](/docs/mcp), and more — is available as an optional component that may be installed with a single command.

<a name="next-steps"></a>
## Next Steps

Now that you know what Laravel Zero is, let's get you building:

- [Installation](/docs/installation) — create your first application.
- [Commands](/docs/commands) — write the commands that do the work.
- [Configuration](/docs/configuration) — configure your application and its list of commands.
- [Distribution](/docs/distribute-as-a-phar-archive) — ship your application to the world.
