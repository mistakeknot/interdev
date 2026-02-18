# interdev

Developer tooling for Claude Code — the meta-plugin for building plugins.

## What This Does

interdev bundles everything you need for Claude Code plugin development into one place: MCP CLI interaction patterns, skill authoring guidance, plugin lifecycle management, and 37 reference documents covering Claude Code internals.

The MCP CLI skill is particularly useful — it provides a discovery-first workflow for interacting with MCP servers via the mcptools CLI. Instead of guessing at tool names, you walk through tools → resources → prompts → call.

## Installation

```bash
/plugin install interdev
```

## Skills

- **mcp-cli** — On-demand MCP server interaction via mcptools CLI
- **working-with-claude-code** — Claude Code CLI reference and patterns
- **developing-claude-code-plugins** — Plugin creation, testing, and release
- **create-agent-skills** — Agent and skill authoring guidance
- **writing-skills** — SKILL.md structure and best practices

## Usage

```
"create a new Claude Code plugin"
"how do I add an MCP server to my plugin?"
"write a skill for code review"
```

Or invoke skills directly when you know what you need:

```
/interdev:mcp-cli
/interdev:writing-skills
```
