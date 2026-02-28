# interdev — Vision and Philosophy

**Version:** 0.1.0
**Last updated:** 2026-02-28

## What interdev Is

interdev is developer tooling for Claude Code: two skills that give agents authoritative knowledge of the platform they run on. The `working-with-claude-code` skill delivers 37 curated reference documents scraped from docs.claude.com — covering plugins, hooks, MCP, skills, CLI, settings, integrations, and security — as agent-readable memory rather than raw web search results. The `mcp-cli` skill documents on-demand MCP server interaction via the mcptools CLI, enforcing a discovery-first workflow (tools → resources → prompts → call) before any tool invocation.

These two skills are the complete scope. interdev has no agents, hooks, or commands. Skill authoring was extracted to interskill; plugin development to interplug. What remains is intentionally narrow: know the platform, use MCP safely.

## Why This Exists

Demarch's agents build Demarch using Claude Code. For that loop to work, agents need accurate, reliable knowledge of how Claude Code behaves — its configuration format, hook lifecycle, plugin API, MCP protocol, and CLI semantics. Guessing or fetching live docs mid-session is expensive and unreliable. interdev solves this by making official documentation a first-class artifact: versioned, curated, locally available, and updatable on demand. Documentation quality directly determines agent output quality. The 37 reference docs are the product.

## Design Principles

1. **Documentation as memory, not scaffolding.** The reference library is not background material — it is the primary deliverable. Agents that read it answer Claude Code questions correctly; agents that skip it guess and hallucinate.

2. **Discovery before invocation.** The mcp-cli skill enforces structural discipline: inspect tool schemas before calling them. This is not style guidance — it prevents parameter mismatches and tool misuse that are otherwise invisible until failure.

3. **Strong defaults over raw search.** Curated, locally stored docs outperform live web fetching: no network dependency, no parsing overhead, no hallucination from partial content. The `update_docs.js` script keeps defaults fresh when Claude Code ships new features.

4. **Composition made interdev smaller.** interdev was extracted from Clavain, then extracted interskill and interplug. Each split tightened scope. The current two-skill surface area is the correct abstraction, not a minimum viable placeholder.

5. **Self-building is the design constraint.** If Demarch's agents can't use interdev to understand Claude Code, the skill is broken. Every reference doc must be agent-readable, and the coverage must be complete enough that no Claude Code question requires leaving the skill.

## Scope

**Does:** Official Claude Code reference (37 docs across all feature areas), on-demand MCP server interaction via mcptools CLI, update script to refresh docs from docs.claude.com.

**Does not do:** Skill authoring guidance (interskill), plugin development workflows (interplug), Clavain engineering patterns (Clavain), agent orchestration (Intercore), or MCP server hosting.

## Direction

- Reduce friction when MCP tool metadata drifts from server implementations (IDEV-N1, P1).
- Add Codex-friendly bootstrap recipes for common local developer toolchains (IDEV-N3).
- Explore self-healing tooling profiles that detect and recover from environment breakages without manual intervention.
