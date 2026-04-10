# agent-skills
A home for all of my agents' skills 🛖

[![License: Hippocratic 3.0](https://img.shields.io/badge/License-Hippocratic_3.0-lightgrey.svg)](https://firstdonoharm.dev)

---

## Overview

**Agent-skills** is a collection of specialized skills that extend Claude's capabilities. These skills enable Claude to perform specific tasks more effectively—from shopping research to custom automation. You can load these skills into your Claude environment (web, API, or desktop app) to unlock powerful new features.

---

## For Non-Developers: How to Use These Skills

If you just want to use these skills with Claude and aren't interested in the technical setup, follow these steps:

### Prerequisites
- A Claude account (free or paid)
- Access to Claude at [claude.ai](https://claude.ai)

### Step 1: Access Claude Code on the Web

1. Go to **[claude.ai/code](https://claude.ai/code)**
2. Sign in with your Claude account if you haven't already
3. You're now in Claude Code, which gives you enhanced capabilities

### Step 2: Load a Skill into Your Session

When you visit Claude Code, you can start a conversation and Claude will suggest relevant skills based on what you're asking for. Here's how to use them:

**Option A: Let Claude Suggest Skills Automatically**
- Simply describe what you want to do (e.g., "find me the best price for a laptop")
- Claude will automatically suggest and offer to use relevant skills
- Click "Accept" when prompted to enable a skill

**Option B: Manually Request a Skill**
- Type your request in Claude Code
- If a skill is available for your task, Claude will recognize it and ask permission to use it
- You can grant access and Claude will use the skill to help you

### Step 3: Use the Skill

Once a skill is enabled, Claude will use it automatically whenever relevant to your request. You don't need to do anything special—just ask Claude what you need!

### Current Available Skills

- **Shopping Research** - Find the best price for any product, compare retailers, and get an all-in cost including tax and shipping

### Example: Using the Shopping Research Skill

You could say any of these to trigger the skill:
- "Find me the best deal on a 65-inch 4K TV"
- "What's the cheapest way to get a blue wool sweater in size M?"
- "Compare prices for AirPods Pro"
- "Where should I buy running shoes online?"

Claude will search multiple retailers, calculate tax and shipping for your location, and show you the top 3 options ranked by total cost.

---

## For Developers: How to Use and Create Skills

If you're a developer who wants to understand, customize, or create skills, this section is for you.

### Prerequisites

- Node.js 18+ installed
- A Claude account
- Git (for cloning the repository)
- Claude Code CLI installed (`npm install -g @anthropic-ai/claude-code`)

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
├── README.md
└── package.json
```

Each skill lives in `.claude/skills/<skill-name>/` with:
- **SKILL.md** - The skill's definition, behavior rules, and instructions
- **references/** - Optional supporting documents the skill uses

### Step 3: Load a Skill into Claude Code

#### Using the Web/Desktop App

1. Open **Claude Code** on [claude.ai/code](https://claude.ai/code) or in the desktop app
2. Look for the **Skills** tab in the sidebar
3. Click **"Add a local skill"** or **"Import from repository"**
4. Point to the skill directory or paste the repository URL
5. The skill will be imported and available in your session

#### Using Claude Code CLI

If you're using the Claude Code CLI:

```bash
# Navigate to the skill directory
cd .claude/skills/shopping-research

# Load the skill in Claude Code
claude-code --load-skill ./SKILL.md
```

### Step 4: Review Skill Documentation

Each skill's behavior and capabilities are defined in its `SKILL.md` file. Open `.claude/skills/<skill-name>/SKILL.md` to understand:
- What the skill does
- How it processes requests
- Important behavior rules
- Any special configurations

### Step 5: Test the Skill

Start a Claude conversation and test the skill:

```
# Test the shopping research skill
claude-code

# Then type:
"Find the best price for a MacBook Air 13-inch in my area"
```

Claude will use the skill to search for prices and recommend the best option.

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

4. **Load it into Claude Code** using the steps above

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
