---
title: Send desktop notifications
description: Send desktop notifications
---

# Send desktop notifications

Laravel Zero provides a `notify` method that gives the power of sending desktop notifications from your Artisan commands:

```php
$this->notify("Hello Web Artisan", "Love beautiful..", "icon.png");
```

The code above will generate the following notification in your desktop:

<img src="https://raw.githubusercontent.com/nunomaduro/laravel-desktop-notifier/stable/docs/icon.png" class="md:w-4/5 md:mx-auto">

Get more details at: [github.com/nunomaduro/laravel-desktop-notifier](https://github.com/nunomaduro/laravel-desktop-notifier)

<a name="notes"></a>
### Notes

**On macOS**, to use an icon for the notification, you will need to install the [terminal-notifier](https://github.com/julienXX/terminal-notifier) command-line tool.

This can be installed via [RubyGems](https://rubygems.org/gems/terminal-notifier) with `gem install terminal-notifier`, or via [Homebrew](https://brew.sh) with `brew install terminal-notifier`.
