# agent-skills
A home for all of my agents' skills 🛖

[![License: Hippocratic 3.0](https://img.shields.io/badge/License-Hippocratic_3.0-lightgrey.svg)](https://firstdonoharm.dev)

---

## Overview

**Agent-skills** is a collection of specialized skills that extend Claude's capabilities. These skills enable Claude to perform specific tasks more effectively—from shopping research to custom automation. You can load these skills into your Claude environment (web, API, or desktop app) to unlock powerful new features.

---

## For Non-Developers: How to Use These Skills

Skills are plain text files that Claude Code reads automatically when you work inside this repository. No plugins, no configuration — just open the repo in Claude Code and the skills are live.

### What you need

- A Claude account (free or paid) at [claude.ai](https://claude.ai)
- [Claude Code](https://claude.ai/code) — the desktop app, web app, or CLI

### Option A: Use skills from this repo (project scope)

Skills are active whenever Claude Code is open inside this folder.

1. **Download this repository** — click the green **Code** button on GitHub, then **Download ZIP**, and unzip it somewhere on your computer.
2. **Open Claude Code** — go to [claude.ai/code](https://claude.ai/code) in your browser, or launch the desktop app.
3. **Open the folder** — use "Open folder" (desktop app) or start a new session pointed at the unzipped directory.
4. **Start chatting** — Claude will automatically pick up and use any skill that matches your request. No extra steps needed.

### Option B: Make skills available everywhere (personal scope)

Copy a skill folder into your personal Claude skills directory so it works in any project, not just this one.

1. Complete step 1 from Option A to get the files.
2. Create the directory `~/.claude/skills/` if it doesn't already exist.
3. Copy the skill folder you want — for example:
   ```
   ~/.claude/skills/shopping-research/
   ```
   The folder must contain the `SKILL.md` file at the top level.
4. Restart Claude Code. The skill is now available in every session.

### Current available skills

| Skill | What it does | Example prompts |
|---|---|---|
| **Shopping Research** | Finds the best all-in price (product + tax + shipping) across major retailers | "Find me the best deal on AirPods Pro" · "Compare prices for a 65-inch 4K TV" · "Where should I buy running shoes?" |

### How skills get used

Skills activate automatically — Claude reads each skill's description and applies it when your request matches. You can also invoke any skill explicitly by typing `/skill-name` (e.g., `/shopping-research find me a laptop deal`).

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

The project is organized as follows:

```
agent-skills/
├── .claude/
│   └── skills/
│       └── shopping-research/
│           ├── SKILL.md          # Skill definition and behavior
│           └── references/        # Reference documents used by the skill
│               ├── sources.md
│               ├── tax-rates.md
│               └── tariff-flags.md
├── CLAUDE.md                      # Guidance for Claude Code
├── .gitignore                     # Git ignore rules
└── README.md
```

Each skill lives in `.claude/skills/<skill-name>/` with:
- **SKILL.md** - The skill's definition, behavior rules, and instructions
- **references/** - Optional supporting documents the skill uses

### Step 3: Load a Skill into Claude Code

Skills are loaded automatically — no CLI flags or import commands needed. Just open Claude Code in a directory that contains `.claude/skills/` and all skills in that directory are active.

For personal scope (available in every project), copy the skill folder to `~/.claude/skills/<skill-name>/`.

### Step 4: Review Skill Documentation

Each skill's behavior is defined in its `SKILL.md` file. Open `.claude/skills/<skill-name>/SKILL.md` to understand what the skill does, how it processes requests, and any behavior rules.

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
   mkdir -p .claude/skills/my-skill/references
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
- The markdown body contains the actual instructions Claude follows
- These instructions tell Claude how to behave and what steps to take

**References**
- Skills can include supporting reference files in the `references/` directory
- Claude uses these to provide accurate, consistent information
- Examples: lookup tables, API documentation, lists of sources

**Availability**
- Skills are available whenever you use Claude Code
- Claude automatically offers to use relevant skills when appropriate
- You maintain control—you always approve skill usage

### Troubleshooting

**Skill not appearing in Claude Code?**
- Make sure your `SKILL.md` file is in the correct location: `.claude/skills/<skill-name>/`
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
