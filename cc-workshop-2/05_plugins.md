# Workshop 05: Creating Plugins

**Duration: ~8 minutes**

## What You'll Learn

- Plugin structure and components
- How to create a simple plugin
- Publishing plugins for others

---

## What is a Plugin?

A **plugin** is a packaged extension containing any combination of:

| Component | Purpose |
|-----------|---------|
| Commands | Slash commands (`/deploy`, `/test`) |
| Skills | Domain expertise |
| Agents | Specialized subagents |
| Hooks | Event automation |
| MCP Servers | External integrations |

---

## Plugin Structure

```
my-plugin/
├── plugin.json          # Plugin manifest (required)
├── commands/
│   └── my-command.md
├── skills/
│   └── my-skill/
│       └── SKILL.md
├── agents/
│   └── my-agent.md
└── README.md
```

---

## Plugin Manifest

Create `plugin.json`:

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "What this plugin does",
  "author": "your-name",
  "components": {
    "commands": ["commands/my-command.md"],
    "skills": ["skills/my-skill"],
    "agents": ["agents/my-agent.md"]
  }
}
```

### Manifest Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Plugin identifier (lowercase, hyphens) |
| `version` | Yes | Semantic version (1.0.0) |
| `description` | Yes | Brief description |
| `author` | No | Creator name or org |
| `components` | Yes | What the plugin includes |

---

## Task 1: Create Plugin Directory

```bash
mkdir -p my-plugin/commands
mkdir -p my-plugin/skills/greeting
```

---

## Task 2: Create Plugin Manifest

Create `my-plugin/plugin.json`:

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "A simple greeting plugin",
  "components": {
    "commands": ["commands/hello.md"],
    "skills": ["skills/greeting"]
  }
}
```

---

## Task 3: Add a Command

Create `my-plugin/commands/hello.md`:

```markdown
---
description: Say hello with a custom message
arguments:
  - name: name
    description: Name to greet
    required: false
---

Greet the user. If a name is provided, use it. Otherwise say "Hello, friend!"
```

---

## Task 4: Add a Skill

Create `my-plugin/skills/greeting/SKILL.md`:

```markdown
---
name: greeting
description: Provides friendly greeting expertise
allowed-tools: Read
---

# Greeting Skill

You are an expert at crafting warm, personalized greetings.

## Guidelines

- Keep greetings brief and friendly
- Match the tone to the context
- Include the person's name when known
```

---

## Installing Local Plugins

Test your plugin locally:

```bash
/plugin install ./my-plugin
```

---

## Publishing Plugins

Share your plugin:

1. **GitHub** — Push to a public repo
2. **Install via repo** — `/plugin install owner/repo`

```bash
# Others can install with:
/plugin install your-username/my-plugin
```

---

## Your Structure

```
my-plugin/
├── plugin.json
├── commands/
│   └── hello.md
└── skills/
    └── greeting/
        └── SKILL.md
```

---

## Checkpoint

- [ ] Created plugin directory structure
- [ ] Created `plugin.json` manifest
- [ ] Added at least one command or skill
- [ ] Understand how to install local plugins

---

## Next Up

Continue to: [06_workflow_automation.md](./06_workflow_automation.md) - Multi-step command workflows
