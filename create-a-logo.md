---
title: Create a logo
description: Create a logo
---

# Create a logo

Using the `app:install` Artisan command you can install the `logo` component:
```bash
php <your-app-name> app:install logo
```

Just after installation, if you run `php <your-app-name>` your application will contain
a ASCII logo:

<img src="https://raw.githubusercontent.com/laravel-zero/docs/master/images/logo.png" class="md:w-4/5 md:mx-auto" >

This command will install dependencies needed and publishes a config file under `config/logo.php`.

<a name="using-a-different-font"></a>
### Using a different font

Under the hood the `logo` component uses the [`laminas/laminas-text`](https://github.com/laminas/laminas-text) package which renders text using fonts called "figlets".

By default Laravel Zero uses the `big.flf` FIGlet file by Glenn Chappell. Additional FIGlet files can be downloaded from [figlet.org](http://www.figlet.org/fontdb.cgi) or created using FIGlet editing software.

Once a font has been downloaded, the `logo.font` value can be set in the config to provide the full path to the FIGlet file.

```diff
// config/logo.php
-  'font' => \LaravelZero\Framework\Components\Logo\FigletString::DEFAULT_FONT,
+  'font' => resource_path('fonts/doom.flf'),
```

For more details, check out the [Laminas docs](https://docs.laminas.dev/laminas-text/figlet) on FIGlets.

<a name="customising-the-logo-text"></a>
### Customising the logo text

By default, Laravel Zero will use the `app.name` configuration value for the logo text. However, this can be overridden in the `config/logo.php` file.

You can change the `logo.name` configuration value to any other text, and this will then be used for the logo.
