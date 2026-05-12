# Coree Cursor Integration

[Coree](https://github.com/coree-ai/coree) provides persistent memory and code intelligence for AI agents. This repository contains the MCP server configuration and context file for integrating Coree into [Cursor](https://cursor.com).

## Features

- **Persistent Memory**: Stores decisions, architectural discoveries, and gotchas across sessions.
- **Code Intelligence**: Hybrid search over source code and git history.

## Installation

Cursor does not have a distributable plugin format for MCP servers. Installation
requires two manual steps.

### 1. Add the MCP server config

**Project scope** - create or edit `.cursor/mcp.json` in your project root:

```json
{
  "mcpServers": {
    "coree": {
      "command": "npx",
      "args": ["--yes", "@coree-ai/coree@0.13.0", "serve"]
    }
  }
}
```

**User scope** - create or edit `~/.cursor/mcp.json` to install for all projects.
The format is identical.

After adding the config, restart Cursor or reload the window. The coree MCP server
will appear in Cursor's MCP panel (Settings > MCP). Enable it if not already active.

### 2. Add the context file to your project

Copy `.cursorrules` from this repository into your project root:

```bash
curl -fsSL https://raw.githubusercontent.com/coree-ai/cursor/main/.cursorrules \
  -o .cursorrules
```

This tells Cursor's agent when and how to use the coree tools. If you already have
a `.cursorrules` file, append the contents.

## Environment variables

If you use coree's remote sync (Turso), add env vars to the MCP config:

```json
{
  "mcpServers": {
    "coree": {
      "command": "npx",
      "args": ["--yes", "@coree-ai/coree@0.13.0", "serve"],
      "env": {
        "COREE__MEMORY__REMOTE_AUTH_TOKEN": "your-token",
        "COREE__MEMORY__REMOTE_URL": "libsql://your-db.turso.io"
      }
    }
  }
}
```

## Verify

Open a Cursor Agent session and ask:

```
call the diagnose tool and show me the output
```

The `diagnose` MCP tool reports server state, database status, and any
initialisation errors.

## Usage

Once configured, Coree provides MCP tools. Ask Cursor to search your codebase
or memories:

```
search for how the indexing works
```

See [.cursorrules](./.cursorrules) for detailed usage guidelines.
