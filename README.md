# agent-skills
A home for all of my agents' skills 🛖

[![License: Hippocratic 3.0](https://img.shields.io/badge/License-Hippocratic_3.0-lightgrey.svg)](https://firstdonoharm.dev)

---

## Overview

**Agent-skills** is a collection of specialized skills for **Claude Code, GitHub Copilot CLI, and other agents** that read the same skill layouts. They help models perform focused tasks more effectively—from shopping research to custom automation. You can load them in a project (this repo) or copy skill folders into your personal config so they apply everywhere.

### Layout (canonical `.agents/`)

This repo follows the same idea as [francisfuzz/dotfiles](https://github.com/francisfuzz/dotfiles): **`.agents/skills/`** is the single source of truth for skill definitions. [Claude Code](https://claude.ai/code) loads them through `.claude/skills` → `.agents/skills`; [GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-skills) can use `.github/skills` → `.agents/skills`. Agent-wide instructions live in **`AGENTS.md`**; **`CLAUDE.md`** is a symlink to that file for tools that only read `CLAUDE.md`.

Copilot’s docs also treat **`.agents/skills`** as a supported project path on its own, so skills stay discoverable even if **symlinks are missing** (for example after **Download ZIP** on some systems).

### Bookmarks and old GitHub links

If you saved links to skills under **`/.claude/skills/`** on GitHub, those paths no longer exist in this tree. Use **[`.agents/skills/`](https://github.com/francisfuzz/agent-skills/tree/main/.agents/skills)** instead (same folders and files; only the path changed).

---

## For Non-Developers: How to Use These Skills

Skills are plain text files that **Claude Code** (and compatible tools) read when you work inside this repository. No plugins, no configuration — open the repo in your agent and the skills are available.

### What you need

- A Claude account (free or paid) at [claude.ai](https://claude.ai)
- [Claude Code](https://claude.ai/code) — the desktop app, web app, or CLI

### Option A: Use skills from this repo (project scope)

Skills are active whenever Claude Code is open inside this folder.

1. **Get a copy of the repository**
   - **Recommended:** [Clone with Git](https://github.com/francisfuzz/agent-skills) (`git clone …`) so **symlinks** (`.claude/skills`, `.github/skills`, `CLAUDE.md`) are preserved.
   - **Alternative:** GitHub **Download ZIP** — some unzip tools **do not restore symlinks**. If skills seem missing, open or copy from **`.agents/skills/`** directly; [vendor docs](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-skills) list that folder as a valid project skills location alongside `.claude/skills`.
2. **Open Claude Code** — go to [claude.ai/code](https://claude.ai/code) in your browser, or launch the desktop app.
3. **Open the folder** — use "Open folder" (desktop app) or start a new session pointed at the project directory.
4. **Start chatting** — the agent will pick up skills that match your request. No extra steps needed.

### Option B: Make skills available everywhere (personal scope)

Copy a skill folder into your personal Claude skills directory so it works in any project, not just this one.

1. Complete step 1 from Option A to get the files (copy from **`.agents/skills/<skill-name>/`**).
2. Create the directory `~/.claude/skills/` if it doesn't already exist.
3. Copy the skill folder you want — for example:
   ```
   ~/.claude/skills/shopping-research/
   ```
   The folder must contain the `SKILL.md` file at the top level.
4. Restart Claude Code. The skill is now available in every session.

### Current available skills

Browse the [`.agents/skills/` directory](https://github.com/francisfuzz/agent-skills/tree/main/.agents/skills) — each folder is one skill, and its `SKILL.md` describes what it does and when it triggers.

### How skills get used

Skills activate automatically — the agent reads each skill's description and applies it when your request matches. You can also invoke any skill explicitly by typing `/skill-name` (e.g., `/shopping-research find me a laptop deal`).

---

## For Developers: How to Use and Create Skills

If you're a developer who wants to understand, customize, or create skills, this section is for you.

### Prerequisites

- A Claude account
- Git (for cloning the repository)
- Claude Code CLI installed — see [setup docs](https://docs.anthropic.com/en/docs/claude-code/setup) for the current installer (`npm install -g @anthropic-ai/claude-code` is deprecated)

### Step 1: Clone the Repository

```bash
git clone https://github.com/francisfuzz/agent-skills.git
cd agent-skills
```

### Step 2: Understand the Project Structure

The project is organized as follows (every skill uses `SKILL.md` plus optional `references/` or other supporting files):

```
agent-skills/
├── .agents/
│   └── skills/                         # Canonical skill definitions
│       ├── interview/
│       │   └── SKILL.md
│       ├── homeowner-contractor-outreach/
│       │   ├── SKILL.md
│       │   └── references/
│       │       ├── agents.md
│       │       ├── call-script-template.md
│       │       ├── email-template.md
│       │       └── negotiation-template.md
│       └── shopping-research/
│           ├── SKILL.md
│           └── references/
│               ├── sources.md
│               ├── tax-rates.md
│               └── tariff-flags.md
├── .claude/
│   └── skills -> ../.agents/skills     # Symlink: Claude Code discovery
├── .github/
│   └── skills -> ../.agents/skills     # Symlink: Copilot CLI discovery
├── AGENTS.md                           # Guidance for agents (canonical)
├── CLAUDE.md -> AGENTS.md              # Symlink for Claude Code
├── .gitignore
└── README.md
```

You may create **`.claude/settings.local.json`** locally for Claude Code; it is not checked into this repository (often gitignored).

Each skill lives in `.agents/skills/<skill-name>/` with:
- **SKILL.md** - The skill's definition, behavior rules, and instructions
- **references/** - Optional supporting documents the skill uses

### Step 3: Load a Skill into Claude Code

Skills are loaded automatically — no CLI flags or import commands needed. Open Claude Code at the repo root; Claude follows `.claude/skills/` (symlinked to `.agents/skills/`) and loads every skill there.

For personal scope (available in every project), copy the skill folder to `~/.claude/skills/<skill-name>/`.

### Step 4: Review Skill Documentation

Each skill's behavior is defined in its `SKILL.md` file. Open `.agents/skills/<skill-name>/SKILL.md` (or the same path via `.claude/skills/`) to understand what the skill does, how it processes requests, and any behavior rules.

### Step 5: Test the Skill

Start a Claude Code session from the repo root and ask naturally:

```
Find the best price for a MacBook Air 13-inch in my area
```

Claude will recognize the request, apply the shopping-research skill, and return ranked results.

### Creating Your Own Skill

To create a new skill:

1. **Create the directory structure:**
   ```bash
   mkdir -p .agents/skills/my-skill/references
   ```

2. **Create SKILL.md** with the YAML frontmatter and instructions:
   ```markdown
   ---
   name: my-skill
   description: Brief description of what your skill does
   ---

   # My Skill

   ## How It Works

   [Your detailed instructions here]

   ## Behavior Rules

   - Rule 1
   - Rule 2
   ```

3. **Add reference files** (optional) in the `references/` directory if your skill needs supporting data

4. **Open Claude Code** in this repo's root directory — the skill is automatically available

5. **Test thoroughly** before sharing

### Important Concepts

**Skill Definition (SKILL.md)**
- The YAML front matter defines metadata (name, description)
- The markdown body contains the actual instructions the agent follows
- These instructions define behavior and steps

**References**
- Skills can include supporting reference files in the `references/` directory
- Models use these for accurate, consistent information
- Examples: lookup tables, API documentation, lists of sources

**Availability**
- Project skills load when you open this repo in Claude Code (or tools that honor the same paths)
- Agents typically offer relevant skills automatically when appropriate
- You maintain control—you approve skill usage where the product requires it

### Troubleshooting

**Skill not appearing in Claude Code?**
- Make sure your `SKILL.md` file is in the correct location: `.agents/skills/<skill-name>/` (the `.claude/skills` path is a symlink to the same folder)
- If you used **Download ZIP** and symlinks broke, rely on **`.agents/skills/`** or **clone with Git** (see Option A)
- Check that the YAML frontmatter is valid
- Reload Claude Code

**Skill works but behaves unexpectedly?**
- Review the behavior rules in `SKILL.md`
- Check reference files for outdated information
- Clarify instructions in `SKILL.md` if needed

**Want to share a skill?**
- Push your branch with the new skill
- Submit a pull request to the main repository
- Include documentation and test cases

---

## License

This project is licensed under the [Hippocratic License 3.0](https://firstdonoharm.dev)
