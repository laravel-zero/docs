---
title: Run Tasks
description: Report the progress of long running operations to your users
---

# Run Tasks

- [Introduction](#introduction)
- [Running Tasks](#running-tasks)
- [Failing Tasks](#failing-tasks)

<a name="introduction"></a>
## Introduction

Console applications spend most of their time performing operations, and the people using them want to know how those operations went. The `task` method, available on every Laravel Zero command, prints the description of a task and then appends a success or failure indicator once the task has finished.

<a name="running-tasks"></a>
## Running Tasks

The `task` method accepts a description and a closure containing the work to be performed:

```php
$this->task('Fetching the movies', function () {
    $this->movies->fetch();
});

$this->task('Storing the movies', function () {
    $this->movies->store();
});
```

While the closure runs, the description is displayed alongside a "loading..." indicator. Once the closure returns, the line is rewritten with the outcome of the task:

<img src="https://raw.githubusercontent.com/nunomaduro/laravel-console-task/master/docs/example.png" class="md:w-4/5 md:mx-auto">

You may customize the text shown while the task is running by passing a third argument:

```php
$this->task('Fetching the movies', fn () => $this->movies->fetch(), 'please wait...');
```

<a name="failing-tasks"></a>
## Failing Tasks

A task is considered successful unless its closure explicitly returns `false`. This allows you to report the outcome of an operation without throwing an exception:

```php
$this->task('Checking the connection', function () {
    return $this->movies->reachable();
});
```

The `task` method itself returns a boolean indicating the result, so you may branch on it:

```php
if (! $this->task('Checking the connection', fn () => $this->movies->reachable())) {
    return 1;
}
```

> **Note:** If the closure throws an exception, the task is marked as failed and the exception is re-thrown, so it will be reported by [Collision](https://github.com/nunomaduro/collision) as usual.

> **Note:** Behind the scenes, the `task` method is provided by the [`nunomaduro/laravel-console-task`](https://github.com/nunomaduro/laravel-console-task) package, which is included with every Laravel Zero application.
