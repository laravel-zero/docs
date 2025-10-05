---
title: Distribute as a PHAR archive
description: Build a standalone PHAR archive to ease the deployment or distribution of your project
---

# Distribute as a PHAR archive

Your Laravel Zero project, by default, allows you to build a standalone PHAR archive to ease the deployment or distribution of your project.
```bash
php <your-app-name> app:build <your-build-name>
```

The build will provide a single PHAR archive, ready to use, containing all the code of your project and its dependencies. You will then be able to execute it directly:
```bash
./builds/<your-build-name>
```

or on Windows:
```bash
C:\application\path> php builds\<your-build-name>
```

We use [`humbug/box`](https://github.com/box-project/box) to provide fast application bundling. In order to configure your build, you should take a look at the file `box.json`.

Please check the box documentation to understand all options: [github.com/box-project/box/blob/master/doc/configuration.md](https://github.com/box-project/box/blob/master/doc/configuration.md).

<a name="debug-information"></a>
### Debug information

#### Failing build?
If your build is failing, it can be due to multiple things. A typical pitfall is missing `ext-*` dependencies in PHP (e.g. When running build in Docker container/CI)

Two commands that can be helpful to you to figure out what your problem is, are:  

```shell
# -v for Verbose
php <your-app-name> app:build <your-build-name> -v

# This does the compiling, so can be used if -v isn't helping you. 
./vendor/laravel-zero/framework/bin/box compile --working-dir=/project/path --config=/project/path/box.json --debug 
```

<a name="distribute-via-packagist"></a>
## Distribute via Packagist

To distribute your application via [Packagist](https://packagist.org) you will need to make some changes to your `composer.json` & `box.json` files.

In your `composer.json` file you will need to move the `laravel-zero/framework` dependency as well as any other custom dependencies from `require` to `require-dev`, excluding the supported PHP versions and required extensions.

You will also need to change the bin path to point to your build.
```diff
-   "require": [
-       "laravel-zero/framework": "x.x"
-   ]
+   "require-dev": [
+       "laravel-zero/framework": "x.x"
+   ]

-   "bin": ["<your-app-name>"]
+   "bin": ["builds/<your-app-name>"]
```

In your `box.json` file you should add:
```
"exclude-dev-files": false,
```

The reason for the above is that Composer will install all non-development dependencies by default, but these are already contained within the PHAR archive, thanks to the above change in `box.json`. By converting them to development dependencies, Composer will skip them altogether, which also ensures that they won't conflict with other globally-installed dependencies.

Now you will need to build your application again with:
```bash
php <your-app-name> app:build <your-build-name>
```

And you are ready to distribute your package via [Packagist](https://packagist.org)  and install it by running
```
composer global require <your-app-name>
```


<a name="non-interactive-build"></a>
## Non-interactive build

When you build you get asked about build version, in case you want to skip this step you can provide the build version as an option:
```bash
php your-app-name app:build --build-version=<your-build-version>
```

<a name="self-update"></a>
## Self update

Using the `app:install` Artisan command you can install the `self-update` component:
```bash
php <your-app-name> app:install self-update
```

This component will add an Artisan `self-update` command to your built application. This command
will try to download the latest version from GitHub, if available.

<a name="custom-update-strategies"></a>
#### Custom update strategies

The self-updater supports custom "strategies" to configure how the application is updated. By default it uses the `GithubStrategy` which will try to download the PHAR binary from a `builds/` directory in the GitHub source repository.

Custom strategies must implement the following [`StrategyInterface` interface](https://github.com/laravel-zero/framework/blob/master/src/Components/Updater/Strategy/StrategyInterface.php).

By default, a few strategies are provided in Laravel Zero:

- Download the PHAR file from the `builds/` directory on GitHub:  
  `LaravelZero\Framework\Components\Updater\Strategy\GithubStrategy`
- Download the PHAR file from GitHub releases assets:  
  `LaravelZero\Framework\Components\Updater\Strategy\GithubReleasesStrategy`
- Download the PHAR file from the `builds/` directory on GitLab:  
  `LaravelZero\Framework\Components\Updater\Strategy\GitlabStrategy`

To use a custom strategy, first publish the config using:

```bash
php <your-app-name> vendor:publish --provider "LaravelZero\Framework\Components\Updater\Provider"
```

Then update the `updater.strategy` value in the configuration file to use the required class name.

<a name="environment-variables"></a>
## Environment Variables

If the `dotenv` component is installed, you can place a `.env` file in the same
folder as the build application to make Laravel Zero load environment variables from
that same file.

<a name="database"></a>
## Database

To use SQLite in your standalone PHAR application, you need to tell Laravel Zero where to place the database in a production environment.

Similar to Laravel, this is configured in `config/database.php` under the `connections.sqlite.database` key. By default this is set to `database_path('database.sqlite')` which resolves to `<project>/database/database.sqlite`. Since we can't modify files within the project once the PHAR is built, we need to store this somewhere on the users computer. A good choice for this would be to create a "dot" folder inside your users home folder. For example:

```diff
// config/database.php
'connections' => [
  'sqlite' => [
      'driver' => 'sqlite',
      'url' => env('DATABASE_URL'),
-     'database' => database_path('database.sqlite'),
+     'database' => $_SERVER['HOME'] . '/.your-project-name/database.sqlite',
      'prefix' => '',
      'foreign_key_constraints' => env('DB_FOREIGN_KEYS', true),
  ],
]
```

In this case it would tell Laravel to use the database at `/Users/<username>/.your-project-name/database.sqlite` (for macOS).

It is important to note that this file will not exist upon installation of your app so you will either need to ensure it exists and is migrated before using the database or provide an `install` command which creates the database and migrates it.
