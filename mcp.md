---
title: MCP
description: Expose your console application to AI agents as an MCP server
---

# MCP

- [Introduction](#introduction)
- [Installation](#installation)
- [Creating Servers](#creating-servers)
- [Registering Servers](#registering-servers)
- [Creating Tools](#creating-tools)
- [Inspecting Your Server](#inspecting-your-server)

<a name="introduction"></a>
## Introduction

The [Model Context Protocol](https://modelcontextprotocol.io) (MCP) is a standard that allows AI agents — such as Claude Code, Cursor, and ChatGPT — to discover and use the capabilities of your application. The `mcp` component brings [Laravel MCP](https://laravel.com/docs/mcp) to Laravel Zero, allowing you to expose your console application's tools, prompts, and resources to any MCP client.

Console applications are a natural fit for MCP: your application is already a single binary that an agent can start and talk to over standard input and output.

<a name="installation"></a>
## Installation

You may install the `mcp` component using the `app:install` Artisan command:

```shell
php application app:install mcp
```

The installer will require `laravel/mcp` and publish a `bootstrap/ai.php` route file, which is where your servers are registered.

<a name="creating-servers"></a>
## Creating Servers

An MCP server is the entry point an agent connects to. You may create one using the `make:mcp-server` Artisan command:

```shell
php application make:mcp-server MovieServer
```

The generated class lives in `app/Mcp/Servers` and describes your server along with the tools, resources, and prompts it offers:

```php
<?php

namespace App\Mcp\Servers;

use App\Mcp\Tools\SearchMovies;
use Laravel\Mcp\Server;

class MovieServer extends Server
{
    /**
     * The MCP server's name.
     */
    protected string $name = 'Movie Server';

    /**
     * The MCP server's instructions for the LLM.
     */
    protected string $instructions = 'This server provides access to a movie catalog.';

    /**
     * The tools registered with this MCP server.
     *
     * @var array<int, class-string<\Laravel\Mcp\Server\Tool>>
     */
    protected array $tools = [
        SearchMovies::class,
    ];
}
```

<a name="registering-servers"></a>
## Registering Servers

Servers are registered within your application's `bootstrap/ai.php` file. Since console applications communicate over standard input and output, you should register your servers using the `local` method:

```php
<?php

use Laravel\Mcp\Facades\Mcp;

Mcp::local('movies', \App\Mcp\Servers\MovieServer::class);
```

Once registered, an agent may start the server by invoking your application:

```shell
php application mcp:start movies
```

<a name="creating-tools"></a>
## Creating Tools

Tools are the actions an agent may take. You may generate one using the `make:mcp-tool` Artisan command:

```shell
php application make:mcp-tool SearchMovies
```

```php
<?php

namespace App\Mcp\Tools;

use App\Models\Movie;
use Illuminate\JsonSchema\JsonSchema;
use Laravel\Mcp\Request;
use Laravel\Mcp\Response;
use Laravel\Mcp\Server\Tool;

class SearchMovies extends Tool
{
    /**
     * The tool's description.
     */
    protected string $description = 'Search the movie catalog by title.';

    /**
     * Handle the tool request.
     */
    public function handle(Request $request): Response
    {
        $movies = Movie::where('title', 'like', "%{$request->get('title')}%")->get();

        return Response::json($movies);
    }

    /**
     * Get the tool's input schema.
     *
     * @return array<string, JsonSchema>
     */
    public function schema(JsonSchema $schema): array
    {
        return [
            'title' => $schema->string()
                ->description('The title to search for.')
                ->required(),
        ];
    }
}
```

Prompts and resources may be generated in the same way, using the `make:mcp-prompt` and `make:mcp-resource` commands.

<a name="inspecting-your-server"></a>
## Inspecting Your Server

Laravel MCP includes the official [MCP Inspector](https://github.com/modelcontextprotocol/inspector), which allows you to test your server and browse its capabilities interactively:

```shell
php application mcp:inspector movies
```

> **Note:** The `mcp:inspector` command is only registered when your application is not running in the `production` environment, so it will not appear in a build of your application.

For everything else — authentication, validation, pagination, and streaming responses — consult the [Laravel MCP documentation](https://laravel.com/docs/mcp) on the Laravel website.
