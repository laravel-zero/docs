---
title: Send Desktop Notifications
description: Notify your users when a long running command finishes
---

# Send Desktop Notifications

- [Introduction](#introduction)
- [Sending Notifications](#sending-notifications)
- [Platform Notes](#platform-notes)

<a name="introduction"></a>
## Introduction

Long running commands are usually started and then forgotten about. Laravel Zero provides a `notify` method that lets your command tap the user on the shoulder when the work is done, using their operating system's native notification center. There is no component to install.

<a name="sending-notifications"></a>
## Sending Notifications

The `notify` method accepts a title, a body, and an optional path to an icon:

```php
$this->notify('Hello Web Artisan', 'Love beautiful..', 'icon.png');
```

The code above will generate the following notification on your desktop:

<img src="https://raw.githubusercontent.com/nunomaduro/laravel-desktop-notifier/stable/docs/icon.png" class="md:w-4/5 md:mx-auto">

<a name="platform-notes"></a>
## Platform Notes

**On macOS**, displaying an icon requires the [terminal-notifier](https://github.com/julienXX/terminal-notifier) command line tool. It may be installed via [Homebrew](https://brew.sh):

```shell
brew install terminal-notifier
```

> **Note:** Behind the scenes, the `notify` method uses the [`nunomaduro/laravel-desktop-notifier`](https://github.com/nunomaduro/laravel-desktop-notifier) package, which is included with every Laravel Zero application.
