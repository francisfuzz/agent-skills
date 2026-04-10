# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository stores personal Claude Code skills — reusable agent behaviors invoked by name during conversations.

## Structure

Skills live in `.claude/skills/<skill-name>/` with a required `SKILL.md` entrypoint. Supporting files (reference data, templates, examples, scripts) go in subdirectories within the skill directory.

```
.claude/skills/
└── <skill-name>/
    ├── SKILL.md          # Required: YAML frontmatter + instructions
    └── references/       # Optional: static lookup data the skill reads
```

## Adding a New Skill

1. Create `.claude/skills/<skill-name>/SKILL.md` with YAML frontmatter:
   ```yaml
   ---
   name: skill-name
   description: >
     When to invoke this skill — written as trigger conditions Claude uses
     to auto-select it.
   ---
   ```
2. Add any reference files the skill needs under `references/`.
3. The skill is automatically available in any Claude Code session in this repo.

## Skill Scope

- **Project scope** (this repo): `.claude/skills/` — applies only here
- **Personal scope** (all projects): `~/.claude/skills/` — applies everywhere
