# interdev

Developer tooling for Claude Code — MCP CLI interaction and Claude Code reference.

## Overview

2 skills, 0 agents, 0 commands, 0 hooks. Companion plugin for Clavain.

## Skills

| Skill | What it does |
|-------|-------------|
| `mcp-cli` | On-demand MCP server interaction via mcptools CLI |
| `working-with-claude-code` | Official Claude Code documentation reference (37 docs) |

## Companion Plugins

- **interskill** — skill authoring toolkit (extracted from interdev's create-agent-skills + writing-skills)
- **interplug** — plugin development toolkit (extracted from interdev's developing-claude-code-plugins)

## Quick Commands

```bash
python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"  # Manifest check
ls skills/*/SKILL.md | wc -l  # Should be 2
```

## Design Decisions (Do Not Re-Ask)

- Uses mcptools CLI for on-demand MCP server interaction
- Discovery-first workflow: tools → resources → prompts → call
- Extracted from Clavain — developer tooling concern, not core engineering
- Skill authoring extracted to interskill plugin (2026-02-25)
- Plugin development extracted to interplug plugin (2026-02-25)
