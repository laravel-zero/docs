---
title: Distribute as a Single Executable Binary
description: Build a self-contained binary that runs without PHP installed on the target system
---

# Distribute as a Single Executable Binary

- [Introduction](#introduction)
- [Installation](#installation)
- [Building the Binary](#building-the-binary)
- [Running the Binary](#running-the-binary)
- [Considerations](#considerations)

<a name="introduction"></a>
## Introduction

A [PHAR archive](/docs/distribute-as-a-phar-archive) still requires PHP to be installed on the machine that runs it. When you are distributing your application to people who may not have PHP — or may have the wrong version — you may instead bundle your application into a standalone executable binary with the PHP runtime embedded in it.

Laravel Zero uses [PHPacker](https://phpacker.dev) to create these self-contained binaries from your PHAR archives.

<a name="installation"></a>
## Installation

First, add PHPacker to your project as a development dependency:

```shell
composer require phpacker/phpacker --dev
```

<a name="building-the-binary"></a>
## Building the Binary

PHPacker works from a PHAR archive, so you should build one first. The build name must have the `.phar` extension:

```shell
php application app:build movie-cli.phar
```

Once the archive is ready in your `builds` directory, create binaries for every supported platform:

```shell
./vendor/bin/phpacker build --src=./builds/movie-cli.phar --php=8.4 all
```

This command builds binaries with PHP 8.4 embedded for:

- **macOS** — `arm64` and `x64`
- **Linux** — `arm64` and `x64`
- **Windows** — `x64`

The resulting binaries are placed in the `./builds/build` directory, organized by platform. For platform-specific builds and additional configuration options, consult the [PHPacker documentation](https://phpacker.dev/docs/getting-started/).

<a name="running-the-binary"></a>
## Running the Binary

The binary may be executed directly, with no other dependencies:

```shell
./builds/build/mac/mac-arm
```

Or, on Linux:

```shell
./builds/build/linux/linux-x64
```

Or, on Windows:

```shell
C:\application\path> builds\build\windows\windows-x64.exe
```

<a name="considerations"></a>
## Considerations

- Binaries are considerably larger than PHAR archives, since they embed the PHP runtime.
- Building requires network access, as the PHP binaries are downloaded during the build.
- Some PHP extensions may not be available in the embedded runtime. If your application depends on one, verify it is present before distributing.
- Everything that is true of a PHAR build is also true here: the environment is `production`, and the bundled filesystem is read-only. See [what changes in a build](/docs/distribute-as-a-phar-archive#what-changes-in-a-build).
