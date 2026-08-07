---
title: Contributing
description: How to contribute to Laravel Zero
---

# Contributing

- [Introduction](#introduction)
- [Bug Reports](#bug-reports)
- [Feature Requests](#feature-requests)
- [Which Repository?](#which-repository)
- [Security Vulnerabilities](#security-vulnerabilities)
- [Funding](#funding)
- [License](#license)

<a name="introduction"></a>
## Introduction

Everything we make is 100% open source and developed collaboratively by people from all over the world. Even if you're not a programmer, you can get involved and make a difference — improving the documentation, reporting a bug, or helping someone in an issue thread all count.

<a name="bug-reports"></a>
## Bug Reports

To encourage active collaboration, we strongly encourage pull requests, not just bug reports. Bug reports may also be sent in the form of a pull request containing a failing test.

However, if you file a bug report, your issue should contain a title and a clear description of the issue. You should also include as much relevant information as possible, and a code sample that demonstrates the problem. The goal of a bug report is to make it easy for yourself — and others — to reproduce the bug and develop a fix.

<a name="feature-requests"></a>
## Feature Requests

You may propose new features or improvements to existing Laravel Zero behavior. If you propose a new feature, please be willing to implement at least some of the code that would be needed to complete it.

Remember that Laravel Zero is deliberately minimal. A feature that only a handful of applications need is often better served by a separate package than by the framework itself.

<a name="which-repository"></a>
## Which Repository?

Laravel Zero's source code is managed on GitHub, and is split across a number of repositories:

| Repository | Description |
| ---------- | ----------- |
| [laravel-zero/laravel-zero](https://github.com/laravel-zero/laravel-zero) | The application skeleton created by `composer create-project`. |
| [laravel-zero/framework](https://github.com/laravel-zero/framework) | The framework itself, including all of its components. |
| [laravel-zero/docs](https://github.com/laravel-zero/docs) | This documentation. |
| [laravel-zero/laravel-zero.com](https://github.com/laravel-zero/laravel-zero.com) | The Laravel Zero website. |
| [nunomaduro/collision](https://github.com/nunomaduro/collision) | The error reporting used by every application. |

The full contribution guidelines are available within the [CONTRIBUTING.md](https://github.com/laravel-zero/laravel-zero/blob/master/CONTRIBUTING.md) file, and the [CHANGELOG](https://github.com/laravel-zero/laravel-zero/blob/master/CHANGELOG.md) contains detailed information about every release.

<a name="security-vulnerabilities"></a>
## Security Vulnerabilities

Please don't disclose security-related issues publicly. If you discover a security vulnerability within Laravel Zero, report it privately using one of the following channels:

1. **GitHub private vulnerability reporting** (preferred) — visit the **Security** tab of the [framework repository](https://github.com/laravel-zero/framework/security) and click **"Report a vulnerability"**.
2. **Email** — send the details to Nuno Maduro at [enunomaduro@gmail.com](mailto:enunomaduro@gmail.com).

All security vulnerabilities will be promptly addressed.

<a name="funding"></a>
## Funding

Laravel Zero, [Collision](https://github.com/nunomaduro/collision), and the rest of Nuno Maduro's work have always been released under the MIT license, and will continue to be into the far future. It has cost thousands of hours to develop, test, and support them.

If you find this work valuable, you can show your appreciation by supporting it. The support is used to create new features and to make console development as enjoyable as possible:

- [GitHub Sponsors](https://github.com/sponsors/nunomaduro)
- [Patreon](https://www.patreon.com/nunomaduro)
- [Open Collective](https://opencollective.com/laravel-zero)
- [PayPal](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=66BYDWAT92N6L)

<a name="license"></a>
## License

Laravel Zero is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
