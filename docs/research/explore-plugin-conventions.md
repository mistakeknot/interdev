# Claude Code Plugin Conventions — Analysis of Simple Companion Plugins

**Research Date:** 2025-02-14  
**Analyzed Plugins:** interform, interslack, interdev (3 simple 1-skill companion plugins)  
**Purpose:** Establish conventions for new simple plugins (1 skill, 0 hooks, 0 agents, 0 commands)

## Executive Summary

All three analyzed plugins follow an identical minimal structure:
- **Single skill** at `./skills/<name>/SKILL.md`
- **Single plugin.json** at `./.claude-plugin/plugin.json`
- **Minimal CLAUDE.md** with overview and design decisions
- **Shared version management** via `/Interverse/scripts/interbump.sh`
- **Marketplace registry** with standardized entry format

This document captures the exact conventions to replicate when building simple companion plugins.

---

## 1. Plugin Structure Overview

```
plugin-root/
├── .claude-plugin/
│   └── plugin.json           # Plugin manifest (mandatory)
├── .git/                     # Per-plugin git repo (each plugin independent)
├── .gitignore                # Minimal standard
├── scripts/
│   └── bump-version.sh       # Thin wrapper → interbump.sh
├── skills/
│   └── <skill-name>/
│       └── SKILL.md          # Single skill definition
├── CLAUDE.md                 # Quick reference docs
└── [docs/]                   # Optional: research, notes
```

### Directory Layout Details

**Three plugins analyzed showed identical structure:**
- `interform`: 6 items (plugin.json, CLAUDE.md, .gitignore, scripts/, skills/, .git/)
- `interslack`: Same 6 items
- `interdev`: Same 6 items + optional `docs/` for research/notes

**Key pattern:** Minimal surface area. No README.md (marketplace provides description), no package.json (Python or shell skills only), no hooks/ or commands/ or agents/ directories.

---

## 2. plugin.json Schema & Field Requirements

### Complete Schema

```json
{
  "name": "plugin-name",
  "version": "0.1.0",
  "description": "One-line description for marketplace listing",
  "author": { "name": "MK", "email": "mistakeknot@vibeguider.org" },
  "repository": "https://github.com/mistakeknot/plugin-name",
  "license": "MIT",
  "keywords": ["tag1", "tag2", "tag3"],
  "skills": ["./skills/skill-name"]
}
```

### Field Analysis

| Field | Type | Required | Example | Notes |
|-------|------|----------|---------|-------|
| `name` | string | YES | `"interform"` | Lowercase, matches directory name and marketplace entry |
| `version` | string | YES | `"0.1.0"` | Semver X.Y.Z; must match marketplace.json |
| `description` | string | YES | `"Design patterns..."` | 1-2 sentences; appears in marketplace |
| `author` | object | YES | `{"name":"MK","email":"..."}` | Must include both name and email |
| `repository` | string | YES | `"https://github.com/mistakeknot/..."` | Full HTTP URL; GitHub convention |
| `license` | string | YES | `"MIT"` | License identifier |
| `keywords` | array | YES | `["design","ui"]` | 3-5 tags for marketplace discovery |
| `skills` | array | YES | `["./skills/skill-name"]` | Relative path(s) to skill directories |

### Observed Values (interform, interslack, interdev)

**All three use identical conventions:**
- **Author:** MK (mistakeknot@vibeguider.org) — same person
- **License:** MIT — universal choice
- **Version:** All start at 0.1.0 — safe initial version
- **Repository URL:** `https://github.com/mistakeknot/<plugin-name>.git` — HTTPS with .git suffix
- **Keywords:** 3-5 tags, lowercase, hyphenated where multi-word (e.g., "developer-tools", "tool-discovery")

### No Optional Fields Used

Analyzed plugins do NOT include:
- `commands` array
- `hooks` object
- `agents` array
- `mcp_servers` array
- `homepage` field

**Pattern:** Only include what's needed. Empty arrays/objects are omitted.

---

## 3. SKILL.md Format and Conventions

### YAML Frontmatter

All SKILL.md files start with frontmatter:

```yaml
---
name: skill-name
description: Use when asked to... (action trigger description)
[user-invocable: boolean]
[allowed-tools: Bash(pattern:*), tool2:*]
---
```

### Frontmatter Fields

| Field | Type | Required | Example | Notes |
|-------|------|----------|---------|-------|
| `name` | string | YES | `"distinctive-design"` | Lowercase, hyphenated; matches skill directory |
| `description` | string | YES | `"Use when asked to send..."` | Should start with "Use when" — Claude's trigger for skill invocation |
| `user-invocable` | boolean | NO | `false` | Default: true (omit if true). Set false for internal skills not user-facing |
| `allowed-tools` | string | NO | `"Bash(slackcli:*, curl:*)"` | Comma-separated list of allowed tool + command patterns. Example: `Bash(slackcli:*, curl:*), Read(*), Write(*)` |

### Content Structure

After frontmatter, SKILL.md contains free-form Markdown:

1. **Title section** — "# Skill Name: Detailed Description" (or just free markdown)
2. **When to use** — Context and trigger conditions
3. **Installation/Setup** — Prerequisites, environment setup
4. **How to use** — Step-by-step workflows, examples
5. **Common patterns** — Recipes and best practices
6. **Troubleshooting** — FAQ and error handling

### Analyzed Examples

**interform / distinctive-design:**
- `user-invocable: false` — This is a design *guidance* skill for Claude, not a user action skill
- No `allowed-tools` — Generates prose/design thinking, not commands
- Content: Design thinking process → aesthetic guidelines → anti-patterns → implementation advice
- 42 lines total (concise)

**interslack / slack-messaging:**
- `user-invocable: false` — Internal skill, user doesn't invoke directly
- `allowed-tools: "Bash(slackcli:*, curl:*)"` — Restricts to slackcli CLI and curl
- Content: Installation → authentication → message operations → testing → troubleshooting
- 143 lines (comprehensive reference)

**interdev / mcp-cli:**
- (Default `user-invocable: true`) — Omitted
- (No `allowed-tools`) — Omitted, allows general use
- Content: When to use → prerequisites → discovery workflow → tool calls → auth → common servers → best practices → troubleshooting
- 376 lines (extensive tutorial + reference)

### Key Pattern: Frontmatter Concision

All three frontmatters are minimal:
- `interform`: 2 fields (name, description)
- `interslack`: 4 fields (name, description, user-invocable, allowed-tools)
- `interdev`: 2 fields (name, description)

**Pattern:** Include only what differs from defaults.

---

## 4. CLAUDE.md Format

### Structure (All Three Follow Identical Pattern)

```markdown
# plugin-name

One-line description from marketplace.

## Overview

1 skill, 0 agents, 0 commands, 0 hooks. Companion plugin for Clavain.

## Quick Commands

\`\`\`bash
python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"  # Manifest check
ls skills/*/SKILL.md | wc -l  # Should be 1
\`\`\`

## Design Decisions (Do Not Re-Ask)

- **Decision 1** — Rationale and context
- **Decision 2** — Rationale and context
- **Decision 3** — Rationale and context
```

### Analysis

| Section | Purpose | interform | interslack | interdev |
|---------|---------|-----------|-----------|----------|
| Title + tagline | Quick ID | ✓ | ✓ | ✓ |
| Overview | Manifest summary | ✓ (1s, 0a, 0c, 0h) | ✓ (1s, 0a, 0c, 0h) | ✓ (1s, 0a, 0c, 0h) |
| Quick Commands | Integrity checks | ✓ (manifest + skill count) | ✓ (manifest + skill count) | ✓ (manifest + skill count) |
| Design Decisions | Architecture rationale | ✓ (3 decisions) | ✓ (3 decisions) | ✓ (3 decisions) |
| Length | Brevity | ~21 lines | ~21 lines | ~21 lines |

### Design Decisions Content

**interform:**
- Anti-AI-slop aesthetic — bold, intentional design over generic templates
- Covers web, TUI, native, and print mediums
- Extracted from Clavain — domain-specific design skill, not core engineering

**interslack:**
- Uses slackcli (shaharia-lab/slackcli) for CLI-based Slack access
- Browser session tokens (xoxc + xoxd), no Slack app creation required
- Extracted from Clavain — domain-specific integration, not core engineering

**interdev:**
- Uses mcptools CLI for on-demand MCP server interaction
- Discovery-first workflow: tools → resources → prompts → call
- Extracted from Clavain — developer tooling concern, not core engineering

**Pattern:** 
- Decisions explain *why* each choice was made
- Often reference origin (extracted from Clavain)
- Focus on constraints and trade-offs, not implementation details

---

## 5. .gitignore Contents

### Standard Content (All Three Identical)

```
node_modules/
.claude/
*.log
```

### Analysis

| Entry | Reason | Applies To |
|-------|--------|-----------|
| `node_modules/` | Node package cache (excluded even if no package.json) | Generic safety |
| `.claude/` | Claude Code project-specific settings (DO NOT commit) | User session data |
| `*.log` | Runtime logs (DO NOT commit) | Transient debugging artifacts |

**Pattern:** Minimal, universal rules. No language-specific ignores because these are pure Bash/Python skill plugins.

---

## 6. Version Management: scripts/bump-version.sh

### File Content (Identical in All Three)

```bash
#!/bin/bash
# Thin wrapper — delegates to shared interbump.sh
SHARED="$(cd "$(dirname "$0")/../../.." && pwd)/scripts/interbump.sh"
[ -f "$SHARED" ] || { echo "Error: interbump.sh not found at $SHARED" >&2; exit 1; }
exec "$SHARED" "$@"
```

### How It Works

1. **Thin wrapper pattern:** Each plugin's `scripts/bump-version.sh` just delegates to the shared `/root/projects/Interverse/scripts/interbump.sh`
2. **Locates shared script:** Walks up 3 directories from `scripts/` to reach monorepo root
3. **Delegates fully:** `exec` passes all arguments to shared script without modification

### Shared interbump.sh Capabilities

The centralized `/root/projects/Interverse/scripts/interbump.sh` handles:

1. **Version validation:** Checks semver format (X.Y.Z)
2. **Auto-discovery:** Finds all version files:
   - `.claude-plugin/plugin.json` (required)
   - `pyproject.toml`, `package.json`, `server/package.json` (if present)
   - `agent-rig.json`, `docs/PRD.md` (if present)
3. **Marketplace lookup:** Walks up to find `/infra/marketplace/.claude-plugin/marketplace.json`
4. **Plugin validation:** Ensures plugin exists in marketplace
5. **Atomic updates:** Uses jq (JSON) or sed (TOML/MD) to update all files
6. **Post-bump hook:** Runs `scripts/post-bump.sh` if it exists (for pre-commit work)
7. **Git operations:**
   - Commits version files in plugin repo
   - Pushes plugin repo
   - Commits marketplace.json in marketplace repo
   - Pushes marketplace repo
8. **Cache symlink bridging:** Creates aliases for old version → new version in cache directories

### Usage

```bash
# Manual bump from plugin directory
cd /root/projects/Interverse/plugins/my-plugin
./scripts/bump-version.sh 0.2.0

# Or via /interpub command (if installed)
/interpub:release 0.2.0
```

### Key Invariant

**All version locations must stay in sync:** If .claude-plugin/plugin.json says 0.2.0 but marketplace.json says 0.1.0, the cache symlinks break and plugins fail to load.

---

## 7. Marketplace Registration

### Entry Format in marketplace.json

```json
{
  "name": "plugin-name",
  "source": {
    "source": "url",
    "url": "https://github.com/mistakeknot/plugin-name.git"
  },
  "description": "Short description for marketplace listing",
  "version": "0.1.0",
  "keywords": ["tag1", "tag2", "tag3"],
  "strict": true
}
```

### Analysis of Examined Entries

**interform entry:**
```json
{
  "name": "interform",
  "source": { "source": "url", "url": "https://github.com/mistakeknot/interform.git" },
  "description": "Design patterns and visual quality for Claude Code — distinctive, production-grade interfaces.",
  "version": "0.1.0",
  "keywords": ["design", "ui", "ux", "visual", "interface"],
  "strict": true
}
```

**interslack entry:**
```json
{
  "name": "interslack",
  "source": { "source": "url", "url": "https://github.com/mistakeknot/interslack.git" },
  "description": "Slack integration for Claude Code — send messages, read channels, test integrations.",
  "version": "0.1.0",
  "keywords": ["slack", "messaging", "integration", "communication"],
  "strict": true
}
```

**interdev entry:**
```json
{
  "name": "interdev",
  "source": { "source": "url", "url": "https://github.com/mistakeknot/interdev.git" },
  "description": "Developer tooling for Claude Code — MCP CLI interaction and tool discovery.",
  "version": "0.1.0",
  "keywords": ["mcp", "cli", "developer-tools", "tool-discovery"],
  "strict": true
}
```

### Field Requirements

| Field | Constraint | Pattern |
|-------|-----------|---------|
| `name` | Must match plugin.json name | lowercase |
| `source.source` | Always `"url"` | Literal string "url" |
| `source.url` | Full GitHub HTTPS URL with .git | `https://github.com/mistakeknot/<name>.git` |
| `description` | 1-2 sentences, matches marketplace listing | Similar to plugin.json but slightly expanded |
| `version` | Must match plugin.json version | Semver X.Y.Z |
| `keywords` | 3-5 tags for discovery | Lowercase, hyphenated if multi-word |
| `strict` | Always `true` | Enables validation |

### Version Sync Requirement

**Critical:** marketplace.json version MUST match `.claude-plugin/plugin.json` version. The `interbump.sh` script enforces this automatically.

---

## 8. Complete Example Structure

Here's the minimal directory tree for a new simple plugin (`interexample`):

```
interexample/
├── .claude-plugin/
│   └── plugin.json
│       {
│         "name": "interexample",
│         "version": "0.1.0",
│         "description": "Example plugin for simple companion use",
│         "author": { "name": "MK", "email": "mistakeknot@vibeguider.org" },
│         "repository": "https://github.com/mistakeknot/interexample",
│         "license": "MIT",
│         "keywords": ["example", "companion", "simple"],
│         "skills": ["./skills/example-skill"]
│       }
├── .gitignore
│   node_modules/
│   .claude/
│   *.log
├── CLAUDE.md
│   # interexample
│   
│   Example plugin for simple companion use.
│   
│   ## Overview
│   
│   1 skill, 0 agents, 0 commands, 0 hooks. Companion plugin for Clavain.
│   
│   ## Quick Commands
│   
│   ```bash
│   python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"  # Manifest check
│   ls skills/*/SKILL.md | wc -l  # Should be 1
│   ```
│   
│   ## Design Decisions (Do Not Re-Ask)
│   
│   - **Decision 1** — Rationale
│   - **Decision 2** — Rationale
│   - **Decision 3** — Rationale
├── scripts/
│   └── bump-version.sh
│       #!/bin/bash
│       SHARED="$(cd "$(dirname "$0")/../../.." && pwd)/scripts/interbump.sh"
│       [ -f "$SHARED" ] || { echo "Error: interbump.sh not found at $SHARED" >&2; exit 1; }
│       exec "$SHARED" "$@"
└── skills/
    └── example-skill/
        └── SKILL.md
            ---
            name: example-skill
            description: Use when asked to... (action trigger)
            ---
            
            # Skill: Example Skill
            
            [Content describing the skill...]
```

### Checklist for New Plugin Creation

- [ ] Create `.claude-plugin/plugin.json` with all required fields
- [ ] Create `skills/<skill-name>/SKILL.md` with frontmatter and content
- [ ] Create `CLAUDE.md` with overview, quick commands, and design decisions
- [ ] Create `.gitignore` with standard entries (node_modules/, .claude/, *.log)
- [ ] Create `scripts/bump-version.sh` as thin wrapper to interbump.sh
- [ ] Create `.git` directory (initialize git repo)
- [ ] Add entry to `/root/projects/Interverse/infra/marketplace/.claude-plugin/marketplace.json`
- [ ] Test manifest validation: `python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"`
- [ ] Test skill count: `ls skills/*/SKILL.md | wc -l` (should be 1)

---

## 9. Key Lessons for Simple Plugins

### What To Include

1. ✓ Single `.claude-plugin/plugin.json` with all 7 required fields
2. ✓ One skill directory with `SKILL.md` (frontmatter + content)
3. ✓ Minimal `CLAUDE.md` (~20 lines) with overview and 3 design decisions
4. ✓ Standard `.gitignore` (3 entries)
5. ✓ Thin `scripts/bump-version.sh` wrapper (6 lines)
6. ✓ Entry in marketplace.json with 7 required fields

### What NOT To Include

- ✗ No package.json (unless Node.js skill)
- ✗ No pyproject.toml (unless Python package)
- ✗ No commands/ directory
- ✗ No hooks/ directory
- ✗ No agents/ directory
- ✗ No README.md (marketplace provides the description)
- ✗ No docs/ directory (optional only for research/notes)

### Version Numbering

- Start at `0.1.0` for initial release
- Use semver strictly (X.Y.Z)
- Always keep `.claude-plugin/plugin.json` and `marketplace.json` versions in sync
- Use `scripts/bump-version.sh` to ensure sync

### Skill YAML Frontmatter

```yaml
---
name: skill-name
description: Use when asked to...
[user-invocable: false]  # Only if not user-facing
[allowed-tools: Bash(pattern:*), ...]  # Only if restricting tools
---
```

### Design Decisions in CLAUDE.md

Should explain:
- Why this external library/service (not built-in)
- Why this architecture/approach
- Why this is a companion plugin, not core

---

## Appendix: File Paths and References

### Analyzed Plugin Locations

- `/root/projects/Interverse/plugins/interform/`
- `/root/projects/Interverse/plugins/interslack/`
- `/root/projects/Interverse/plugins/interdev/`

### Key Infrastructure Files

- `/root/projects/Interverse/scripts/interbump.sh` — Shared version management
- `/root/projects/Interverse/infra/marketplace/.claude-plugin/marketplace.json` — Central registry

### External Repositories

All three plugins hosted at:
- `https://github.com/mistakeknot/interform.git`
- `https://github.com/mistakeknot/interslack.git`
- `https://github.com/mistakeknot/interdev.git`

---

## Summary

Simple companion plugins in this monorepo follow a strict, minimal convention:
1. Single skill at `skills/<name>/SKILL.md`
2. Plugin manifest at `.claude-plugin/plugin.json` (7 required fields)
3. CLAUDE.md (~20 lines) with overview and 3 design decisions
4. Standard .gitignore (3 entries)
5. Version management via `scripts/bump-version.sh` (thin wrapper)
6. Marketplace entry with 7 required fields (kept in sync automatically)

All three analyzed plugins (interform, interslack, interdev) follow these conventions identically, making them ideal templates for new simple companion plugins.
