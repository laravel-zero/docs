---
title: Distribute as a single executable binary
description: Package your entire project into a single executable binary file that can be easily distributed and run on target systems without requiring separate installation of dependencies or runtime environments.
---

# Distribute as a single executable binary

Transform your Laravel Zero project into a standalone executable binary that runs on any target system without requiring
PHP, Composer, or any other dependencies to be pre-installed.

This approach uses [PHPacker](https://phpacker.dev) to create self-contained binaries from your PHAR archives.

## Quick Start Guide

Let's walk through the complete process from creating a new Laravel Zero project to building distributable binaries.

### Step 1: Create a new Laravel Zero project

```bash
composer create-project --prefer-dist laravel-zero/laravel-zero zero-cli
cd zero-cli
```

### Step 2: Build the PHAR archive

```bash
php application app:build zero-cli.phar
```

> **Important**
> The artifact name must have the `.phar` extension for PHPacker to work correctly.

After building, you'll find your PHAR archive in the `builds` folder:

```
./builds/
└── zero-cli.phar
```

### Step 3: Install PHPacker

Add PHPacker as a development dependency:

```bash
composer require phpacker/phpacker --dev
```

### Step 4: Build executable binaries

Create binaries for all supported platforms:

```bash
./vendor/bin/phpacker build --src=./builds/zero-cli.phar --php=8.4 all
```

This command builds binaries with PHP 8.4 embedded for:

- **macOS**: arm64 and x64 architectures
- **Linux**: arm64 and x64 architectures
- **Windows**: x64 architecture

For more build options and platform-specific builds, check
the [PHPacker documentation](https://phpacker.dev/docs/getting-started/).

### Step 5: Verify the build

Check the generated binaries in the `./builds/build` folder:

```
./builds/
├── build/
│   ├── linux/
│   │   ├── linux-arm
│   │   └── linux-x64
│   ├── mac/
│   │   ├── mac-arm
│   │   └── mac-x64
│   └── windows/
│       └── windows-x64.exe
└── zero-cli.phar
```

## Testing Your Binary

Test the binary by copying it to a different location and running it:

**For macOS with Apple Silicon (arm64):**

```bash
cp ./builds/build/mac/mac-arm ~/zero-cli
cd ~
./zero-cli
```

**For Linux x64:**

```bash
cp ./builds/build/linux/linux-x64 ~/zero-cli
cd ~
./zero-cli
```

**For Windows x64:**

```bash
copy .\builds\build\windows\windows-x64.exe %USERPROFILE%\zero-cli.exe
cd %USERPROFILE%
zero-cli.exe
```

## Benefits

✅ **Zero Dependencies**: No need for PHP or Composer on target systems  
✅ **Easy Distribution**: Single file deployment  
✅ **Cross-Platform**: Support for macOS, Linux, and Windows  
✅ **Self-Contained**: All dependencies bundled within the binary  
✅ **Version Control**: Embed specific PHP versions for consistency

## Considerations

- Binary files are larger than PHAR archives due to embedded PHP runtime
- Build process requires network access to download PHP binaries
- Some PHP extensions may not be available in the embedded runtime

## Resources

- [PHPacker Official Website](https://phpacker.dev)
- [PHPacker Documentation](https://phpacker.dev/docs/getting-started/)
- [PHPacker GitHub Repository](https://github.com/phpacker/phpacker)

---

Here you go! Distributing your Laravel Zero application as a single executable binary, is as easy as copying the
file to the target system, and running it. Enjoy!
