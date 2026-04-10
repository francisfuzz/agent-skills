# AGENTS.md

Configuration and guidance for AI coding agents (Claude Code, Copilot CLI, and compatible tools) when working in this repository.

## Purpose

This repository stores reusable **skills** — agent behaviors invoked by name or matched automatically from each skill’s description.

## Structure

Skills are authored under **`.agents/skills/<skill-name>/`**, the canonical location. Claude Code discovers them via `.claude/skills` → `.agents/skills`; GitHub Copilot CLI can use `.github/skills` → `.agents/skills` (same target).

```
.agents/skills/
└── <skill-name>/
    ├── SKILL.md          # Required: YAML frontmatter + instructions
    └── references/       # Optional: static lookup data the skill reads
```

### Symlinks

| Symlink | Target | Purpose |
|---------|--------|---------|
| `.claude/skills` | `.agents/skills` | Claude Code skill discovery |
| `.github/skills` | `.agents/skills` | Copilot CLI skill discovery |
| `CLAUDE.md` | `AGENTS.md` | Shared agent instructions for tools that read `CLAUDE.md` |

## Adding a New Skill

1. Create `.agents/skills/<skill-name>/SKILL.md` with YAML frontmatter:
   ```yaml
   ---
   name: skill-name
   description: >
     When to invoke this skill — written as trigger conditions the agent uses
     to auto-select it.
   ---
   ```
2. Add any reference files the skill needs under `references/`.
3. The skill is automatically available in any session opened at this repo root (via the symlinks above).

## Pre-PR Checklist for New Skills

Run through this before opening a PR. Catches the most common issues.

**Frontmatter**
- [ ] Only `name` and `description` are present — no other fields (`license`, `metadata`, `requires_tools`, `author`, `version`, etc. are not recognized and will be silently ignored)
- [ ] `name` value matches the directory name exactly (e.g. `name: my-skill` → `.agents/skills/my-skill/`)
- [ ] `description` is under 1024 characters

**Body**
- [ ] Under 500 lines total
- [ ] Every file referenced in the body exists under `references/` (or the appropriate subdirectory)

**Review sources**
- [ ] Validated against [Claude Code docs](https://docs.anthropic.com/en/docs/claude-code) for any tool, MCP, or invocation claims
- [ ] Validated against [agentconfig.org/skills.md](https://agentconfig.org/skills.md) for spec conformance

## Skill Scope

- **Project scope** (this repo): `.agents/skills/` (also visible as `.claude/skills/` and `.github/skills/` via symlinks)
- **Personal scope** (all projects): `~/.claude/skills/` — applies everywhere in Claude Code
