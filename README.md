# interdev

Developer tooling for Claude Code, Codex, and Kimi Code: the meta-plugin for building plugins.

## What this does

interdev provides MCP CLI interaction patterns and Claude Code reference documentation. For skill authoring, see [interskill](https://github.com/mistakeknot/interskill). For plugin development, see [interplug](https://github.com/mistakeknot/interplug).

The MCP CLI skill is particularly useful: it provides a discovery-first workflow for interacting with MCP servers via the mcptools CLI. Instead of guessing at tool names, you walk through tools → resources → prompts → call.

## Installation

First, add the [interagency marketplace](https://github.com/mistakeknot/interagency-marketplace) (one-time setup):

```bash
/plugin marketplace add mistakeknot/interagency-marketplace
```

Then install the plugin:

```bash
/plugin install interdev
```

## Skills

- **mcp-cli**: On-demand MCP server interaction via mcptools CLI
- **working-with-claude-code**: Claude Code CLI reference and patterns

## Companion Plugins

- **interskill**: Skill authoring (create + audit)
- **interplug**: Plugin development (create + validate + troubleshoot)

## Usage

```
/interdev:mcp-cli
/interdev:working-with-claude-code
```
