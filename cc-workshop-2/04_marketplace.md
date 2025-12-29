# Workshop 07: Plugins & Marketplaces

**Duration: ~8 minutes**

## What You'll Learn

- What plugins and marketplaces are
- How to discover and install plugins
- Example marketplace to explore

---

## What is a Plugin?

A **plugin** is a packaged extension for Claude Code that can include:

| Component | What it adds |
|-----------|--------------|
| Commands | Slash commands like `/review`, `/deploy` |
| Skills | Domain expertise (APIs, frameworks) |
| Agents | Specialized subagents |
| Hooks | Automation triggers |
| MCP Servers | External tool integrations |

Plugins let you **share and reuse** Claude Code configurations across projects and teams.

---

## What is a Marketplace?

A **marketplace** is a catalog of plugins you can browse and install from:

- **Official marketplace** — Curated plugins from Anthropic
- **Community marketplaces** — Shared by the community on GitHub
- **Team marketplaces** — Private plugins for your organization

---

## Task 1: View Installed Plugins

```bash
# List currently installed plugins
/plugins
```

---

## Task 2: Add a Marketplace

Add the official Anthropic marketplace:

```bash
/plugin marketplace add anthropics/claude-code-plugins
```

Or add a community marketplace:

```bash
/plugin marketplace add owner/repo-name
```

---

## Task 3: Browse Available Plugins

```bash
# List all available plugins from added marketplaces
/plugin list
```

---

## Task 4: Install a Plugin

```bash
# Install from default marketplace
/plugin install plugin-name

# Install from specific marketplace
/plugin install plugin-name@marketplace-name
```

---

## Example: Official Plugin Dev Tools

Install Anthropic's plugin development toolkit:

```bash
/plugin install anthropics/claude-code:plugin-dev
```

This adds:
- `/plugin-dev:create-plugin` — Guided plugin creation
- Plugin validator agent
- Skills for building commands, hooks, MCP servers

---

## Quick Reference

| Action | Command |
|--------|---------|
| View installed plugins | `/plugins` |
| Add marketplace | `/plugin marketplace add owner/repo` |
| List available plugins | `/plugin list` |
| Install plugin | `/plugin install name` |
| Install from marketplace | `/plugin install name@marketplace` |
| Update marketplaces | `/plugin marketplace update` |
| Remove plugin | `/plugin remove name` |

---

## Checkpoint

- [ ] Understand what plugins contain (commands, skills, agents, hooks, MCP)
- [ ] Know how to add a marketplace
- [ ] Know how to browse and install plugins

---

## Next Up

Continue to: [05_plugins.md](./05_plugins.md) - Create your own plugins
