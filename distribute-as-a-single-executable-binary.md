---
title: Distribute as a single executable binary
description: Build a standalone executable binary to distribute your project without requiring PHP or dependencies on target systems
---

# Distribute as a single executable binary

Your Laravel Zero project can be bundled into a standalone executable binary that runs on target systems without requiring PHP, Composer, or any other dependencies to be pre-installed.

We use [PHPacker](https://phpacker.dev) to create self-contained binaries from your PHAR archives.

<a name="installation"></a>
## Installation

First, add PHPacker as a development dependency:
```bash
composer require phpacker/phpacker --dev
```

<a name="building"></a>
## Building

Before creating the binary, you need to build your PHAR archive. The build name must have the `.phar` extension:
```bash
php <your-app-name> app:build <your-build-name>.phar
```

Once your PHAR archive is ready in the `builds` folder, create binaries for all supported platforms:
```bash
./vendor/bin/phpacker build --src=./builds/<your-build-name>.phar --php=8.4 all
```

This command builds binaries with PHP 8.4 embedded for:
- **macOS**: arm64 and x64 architectures
- **Linux**: arm64 and x64 architectures
- **Windows**: x64 architecture

The binaries will be created in the `./builds/build` folder, organized by platform.

For platform-specific builds or additional configuration options, please check the [PHPacker documentation](https://phpacker.dev/docs/getting-started/).

<a name="running"></a>
## Running

You can execute the binary directly:
```bash
./builds/build/mac/mac-arm
```

or on Linux:
```bash
./builds/build/linux/linux-x64
```

or on Windows:
```bash
C:\application\path> builds\build\windows\windows-x64.exe
```

The binary is completely self-contained and can be distributed as a single file without any dependencies.

<a name="considerations"></a>
## Considerations

- Binary files are larger than PHAR archives due to the embedded PHP runtime
- Build process requires network access to download PHP binaries
- Some PHP extensions may not be available in the embedded runtime
