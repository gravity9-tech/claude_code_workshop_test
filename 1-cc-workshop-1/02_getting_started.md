# Workshop 02: Getting Started with Claude Code

**Duration: ~8 minutes**

## What You'll Learn

- Initialize Claude Code in a project using `/init`
- Understand the generated CLAUDE.md memory file
- Basic commands and permission model

---

## Task 1: Initialize with `/init`

The `/init` command creates a `CLAUDE.md` file - Claude's memory for your project.

In Claude Code, type:

```
/init
```

### What Happens

Claude will:
1. Analyze your project structure
2. Read configuration files (requirements.txt, pyproject.toml, etc.)
3. Identify your tech stack (FastAPI, Python, etc.)
4. Generate a `CLAUDE.md` with project context

---

## Task 2: Explore the Generated CLAUDE.md

Open the generated `CLAUDE.md` file in your editor.

You'll see sections like:
- Project overview
- Tech stack
- Common commands (run, test, lint)
- Project structure

**Why this matters**: Every new Claude session reads this file first, so Claude immediately understands your project. 

---

## Task 3: Understanding Permissions

Claude Code uses a permission system:

| Operation | Permission |
|-----------|------------|
| Reading files | Usually automatic |
| Writing/editing files | Requires approval |
| Running commands | Requires approval |
| MCP tool calls | Requires approval |

Try it - ask Claude to:
```
Find all the API endpoints in this project and list them
```

This runs in the **main context** using Claude's built-in Explore agent to search and analyze your codebase efficiently.

Then try a write operation:
```
Add a comment to the top of app/main.py
```

Notice how Claude asks permission before making changes.

---

## Task 4: Explore the Codebase

Ask Claude to help you understand the project:

```
Explain how the shopping basket API works in this project
```

```
What models are defined and what fields do they have?
```

```
Show me the available API routes
```

This exploration will be useful later when we create tickets for new features.

---

## Next Up

Continue to: [03_jira_mcp.md](./03_jira_mcp.md) - Set up Jira integration
