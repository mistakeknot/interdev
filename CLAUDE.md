# interdev

Developer tooling for Claude Code — MCP CLI interaction and tool discovery.

## Overview

1 skill, 0 agents, 0 commands, 0 hooks. Companion plugin for Clavain.

## Quick Commands

```bash
python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"  # Manifest check
ls skills/*/SKILL.md | wc -l  # Should be 1
```

## Design Decisions (Do Not Re-Ask)

- Uses mcptools CLI for on-demand MCP server interaction
- Discovery-first workflow: tools → resources → prompts → call
- Extracted from Clavain — developer tooling concern, not core engineering
