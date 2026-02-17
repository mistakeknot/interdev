# interdev

Developer tooling for Claude Code — MCP CLI interaction, skill authoring, plugin development, and Claude Code reference.

## Overview

5 skills, 0 agents, 0 commands, 0 hooks. Companion plugin for Clavain.

## Skills

| Skill | What it does |
|-------|-------------|
| `mcp-cli` | On-demand MCP server interaction via mcptools CLI |
| `working-with-claude-code` | Official Claude Code documentation reference (37 docs) |
| `developing-claude-code-plugins` | Plugin lifecycle — plan, create, test, release |
| `create-agent-skills` | Expert guidance for SKILL.md authoring |
| `writing-skills` | TDD-adapted skill creation process |

## Quick Commands

```bash
python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"  # Manifest check
ls skills/*/SKILL.md | wc -l  # Should be 5
```

## Design Decisions (Do Not Re-Ask)

- Uses mcptools CLI for on-demand MCP server interaction
- Discovery-first workflow: tools → resources → prompts → call
- Extracted from Clavain — developer tooling concern, not core engineering
