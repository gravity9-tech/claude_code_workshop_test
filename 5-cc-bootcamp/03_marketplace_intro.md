# Module 3: Understanding Plugins & the Marketplace

**Duration: ~20 minutes**
**Format:** Presentation + Discussion

---

## What You'll Learn

- What a plugin is and how it packages skills, agents, and commands into a shareable unit
- The problem plugins solve — why copy-pasting `.claude/` folders doesn't scale
- How the three-tier marketplace organises plugins across organisation, business line, and team levels
- The difference between building components (Module 2) and distributing them (this module)
- How plugins connect to the two key repos: the target project and the marketplace

---

## 1. The Distribution Problem (5 min)

### What We Built So Far

In Module 2, you created a complete feature implementation lifecycle inside a single project:

```
tea-store-demo/.claude/
├── agents/
│   ├── planner.md
│   ├── tdd-implementer.md
│   └── code-reviewer.md
├── commands/
│   └── implement-feature.md
└── skills/
    ├── create-ticket/
    ├── expand-ticket/
    ├── tdd-workflow/
    ├── create-branch/
    └── commit-and-merge/
```

This works perfectly — for one project, on one machine.

### But What Happens Next?

Think about the real-world scenarios:

| Scenario | Problem |
|----------|---------|
| A colleague wants to use your `/implement-feature` workflow | Copy-paste the entire `.claude/` folder? Which files? What about dependencies? |
| Another team wants the TDD workflow but not the Jira integration | Cherry-pick individual files from your folder structure? |
| You improve the `code-reviewer` agent | Manually update every project that has a copy? |
| 27 teams each build their own version | 27 divergent implementations of the same pattern |
| A new developer joins | "Download these 12 files from Slack and put them in the right folders" |

Copy-pasting files between repos is how tribal knowledge becomes stale knowledge. It works once, then drifts.

### The Core Insight

> **Building** components is a solved problem — you did it in Module 2. **Distributing** components across teams and projects is the unsolved problem. Plugins and the marketplace solve it.

---

## 2. What Is a Plugin? (5 min)

A **plugin** is a packaged collection of related skills, agents, commands, and MCP configurations that can be installed into any Claude Code project with a single command.

### Components vs Plugins

| | Components (Module 2) | Plugin |
|---|---|---|
| **What it is** | Individual files in `.claude/` | A packaged bundle of related components |
| **How it's shared** | Copy files manually | Install with a single command |
| **Versioning** | None — whatever's on disk | Versioned releases via Git tags |
| **Dependencies** | Implicit — you just know what's needed | Declared in a manifest |
| **Updates** | Manual — overwrite files | Pull latest version |
| **Scope** | One project | Any project that installs it |

### What's Inside a Plugin?

A plugin bundles everything needed for a capability into a single installable unit:

```
feature-lifecycle-plugin/
├── plugin.json              ← Manifest: name, version, description, dependencies
├── skills/
│   ├── create-ticket/
│   │   └── SKILL.md
│   ├── expand-ticket/
│   │   └── SKILL.md
│   ├── tdd-workflow/
│   │   └── SKILL.md
│   ├── create-branch/
│   │   └── SKILL.md
│   └── commit-and-merge/
│       └── SKILL.md
├── agents/
│   ├── planner.md
│   ├── tdd-implementer.md
│   └── code-reviewer.md
└── commands/
    └── implement-feature.md
```

The `plugin.json` manifest declares:
- **What** the plugin provides (skills, agents, commands)
- **What** it requires (MCP servers, other plugins)
- **Metadata** (name, version, author, description)

### The Mental Model

Think of it like package managers you already know:

| Concept | npm | pip | Claude Code Marketplace |
|---------|-----|-----|------------------------|
| **Package** | npm package | Python package | Plugin |
| **Manifest** | `package.json` | `pyproject.toml` | `plugin.json` |
| **Registry** | npmjs.com | PyPI | Marketplace repo (Git) |
| **Install** | `npm install` | `pip install` | Install from marketplace |
| **Dependencies** | `dependencies: {}` | `requires: []` | MCP servers, other plugins |

The key difference: Claude Code plugins are **Git-based**. The marketplace is a Git repository, and plugins are installed from it. No external registry needed.

---

## 3. What Is the Marketplace? (5 min)

The **marketplace** is a Git repository that serves as a catalogue of available plugins. It's not a hosted service — it's a repo your organisation controls.

### Why a Git Repo?

| Approach | Problem |
|----------|---------|
| Share plugins via Slack/email | No versioning, no discoverability, no structure |
| Central hosted service | Overhead to build and maintain, dependency on external infra |
| **Git repository** | **Version control built-in, PRs for review, branch protection, your existing CI/CD** |

Your organisation already knows Git. The marketplace uses that existing muscle memory.

### The Three-Tier Structure

Plugins are organised into three tiers based on their scope:

```
┌──────────────────────────────────────────────────────────────────┐
│                         MARKETPLACE                               │
│                                                                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │                Organisation Level                           │  │
│  │  Plugins shared across ALL business lines and teams         │  │
│  │                                                             │  │
│  │  • feature-lifecycle     (Jira → Plan → TDD → Review)      │  │
│  │  • git-conventions       (branching, commit standards)      │  │
│  │  • tdd-workflow          (red-green-refactor patterns)      │  │
│  │  • code-review-standards (quality gates)                    │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │               Business Line Level                           │  │
│  │  Plugins shared across teams within a business line         │  │
│  │                                                             │  │
│  │  ┌─────────────────────┐  ┌──────────────────────────────┐ │  │
│  │  │ Online              │  │ Digital Commerce             │ │  │
│  │  │ • react-patterns    │  │ • dotnet-conventions         │ │  │
│  │  │ • node-api-standards│  │ • apex-standards             │ │  │
│  │  │ • playwright-e2e    │  │ • lightning-components       │ │  │
│  │  └─────────────────────┘  └──────────────────────────────┘ │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │                    Team Level                               │  │
│  │  Plugins specific to an individual team's conventions       │  │
│  │                                                             │  │
│  │  • checkout-api-patterns  (Team Alpha — checkout service)   │  │
│  │  • crm-integration       (Team Beta — CRM connectors)      │  │
│  │  • billing-conventions    (Team Gamma — billing domain)     │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

**Organisation level** — Plugins that apply to every business line and every team regardless of tech stack. The feature lifecycle plugin you built in Module 2 belongs here: every team creates tickets, plans work, and reviews code.

**Business line level** — Plugins shared across all teams within a business line. A business line like Online might have several teams that all use React and Node — they share frontend patterns, API design standards, and E2E testing conventions. Another business line like Digital Commerce might share .NET and Salesforce conventions. These plugins are too specific for the entire organisation but too broad for a single team.

**Team level** — Plugins that encode an individual team's specific conventions, domain patterns, and service-specific standards. These are the most granular — a checkout service team's API patterns differ from a billing team's, even if both sit within the same business line.

### How Tiers Compose

A developer's Claude Code environment is the sum of all three tiers:

```
┌─────────────────────────────────────────────┐
│           Developer's Environment            │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │ Organisation plugins (installed)       │  │
│  │ • feature-lifecycle                    │  │
│  │ • git-conventions                      │  │
│  └────────────────────────────────────────┘  │
│                    +                         │
│  ┌────────────────────────────────────────┐  │
│  │ Business line plugins (installed)      │  │
│  │ • react-patterns                       │  │
│  │ • node-api-standards                   │  │
│  └────────────────────────────────────────┘  │
│                    +                         │
│  ┌────────────────────────────────────────┐  │
│  │ Team plugins (installed)               │  │
│  │ • checkout-api-patterns                │  │
│  └────────────────────────────────────────┘  │
│                    =                         │
│  Claude that knows org standards, business   │
│  line patterns, AND team-specific conventions│
└─────────────────────────────────────────────┘
```

No single developer needs to configure all of this from scratch. The marketplace provides it.

---

## 4. The Two Repos (3 min)

The plugin workflow involves two repositories:

```
┌─────────────────────────────┐
│  claude_code_workshop       │  ← Target project (Tea Shop)
│  (install plugins here,     │     Where plugins get consumed
│   test them in real use)    │
└──────────────▲──────────────┘
               │ install
               │
┌──────────────┴──────────────┐
│  claude_code_marketplace    │  ← Marketplace repository
│  _demo                      │     Where plugins are packaged,
│  (build, package, publish)  │     published, and discovered
└─────────────────────────────┘
```

| Repo | Role | You'll Use It To |
|------|------|-----------------|
| `claude_code_marketplace_demo` | Marketplace | Package components into plugins and publish them |
| `claude_code_workshop` | Target project | Install plugins from the marketplace and verify they work |

The marketplace repo is the **source of truth** for all shared plugins. The target project is any repo that **consumes** those plugins. In a real-world setup, you'd have one marketplace repo and many target projects installing from it.

---

## 5. Plugin Lifecycle Overview (2 min)

The full lifecycle of a plugin follows four stages:

```
┌───────────┐     ┌───────────┐     ┌───────────┐     ┌───────────┐
│  1. Build │ ──► │ 2. Package│ ──► │ 3. Publish│ ──► │ 4. Install│
│           │     │           │     │           │     │           │
│ Create    │     │ Bundle    │     │ Push to   │     │ Install   │
│ skills,   │     │ into a    │     │ marketplace│    │ in target │
│ agents,   │     │ plugin    │     │ repo      │     │ project   │
│ commands  │     │ with      │     │           │     │           │
│ (Module 2)│     │ manifest  │     │           │     │           │
└───────────┘     └───────────┘     └───────────┘     └───────────┘
      ✅                Module 4          Module 4        Module 4
   (done)             (next module)     (next module)   (next module)
```

**Build** (done in Module 2) — Create the individual components: skills, agents, commands.

**Package** — Bundle related components into a plugin with a `plugin.json` manifest that declares what's included and what's required.

**Publish** — Push the plugin to the marketplace repository so other teams can discover it.

**Install** — Install the plugin into a target project. The components are placed in the correct `.claude/` subdirectories automatically.

You've already completed stage 1. Module 4 walks through stages 2–4.

---

## Key Comparisons

### Everything Side by Side

| | MCP Tools | Skills | Agents | Commands | Plugins |
|---|---|---|---|---|---|
| **What** | API connections | Knowledge files | Isolated executors | Orchestration scripts | Packaged bundles |
| **Scope** | Single action | Single task | Single focused job | Single workflow | Entire capability |
| **Shared via** | `.mcp.json` config | File copy | File copy | File copy | Marketplace install |
| **Contains** | — | `SKILL.md` | `AGENT.md` | `command.md` | All of the above |
| **Versioned** | Server version | No | No | No | Yes (Git tags) |
| **Example** | `jira_create_issue` | `create-ticket` | `planner` | `/implement-feature` | `feature-lifecycle` |

### Building vs Distributing

| Phase | What You Do | Where | Module |
|-------|------------|-------|--------|
| **Build** | Create skills, agents, commands | Target project `.claude/` | Module 2 |
| **Package** | Bundle into a plugin with manifest | Marketplace repo | Module 4 |
| **Publish** | Push to marketplace | Marketplace repo | Module 4 |
| **Install** | Pull into a new project | Target project | Module 4 |
| **Maintain** | Update, version, deprecate | Marketplace repo | Module 7 |

---

## Discussion: Your Marketplace

Before moving to the hands-on packaging in Module 4, think about these questions:

1. **What belongs at the organisation level?**
   Workflows every business line and team uses — ticket creation, branching, code review, deployment.

2. **What's shared across your business line but not the whole organisation?**
   Tech stack conventions, framework patterns, API design standards that apply to all teams in your business line but not to teams in other business lines.

3. **What's specific to your team alone?**
   Domain-specific patterns, service conventions, testing strategies unique to your team's codebase.

4. **What already exists as tribal knowledge?**
   Processes that senior developers "just know" but aren't documented anywhere. These are prime candidates for skills — and deciding which tier they belong to forces you to think about who benefits.

---

## Reference Card: Plugins & Marketplace at a Glance

```
┌──────────────────────────────────────────────────┐
│       PLUGINS & MARKETPLACE AT A GLANCE           │
├──────────────────────────────────────────────────┤
│                                                   │
│  PLUGIN: A packaged bundle of related skills,     │
│  agents, commands, and config — installable        │
│  into any project with a single command            │
│                                                   │
│  MARKETPLACE: A Git repository that catalogues     │
│  available plugins across three tiers              │
│                                                   │
│  THE THREE TIERS:                                  │
│  • Organisation  → Shared across all bus. lines     │
│  • Business Line → Shared across teams in a BL      │
│  • Team          → Specific to one team              │
│                                                   │
│  PLUGIN LIFECYCLE:                                 │
│  Build → Package → Publish → Install → Maintain    │
│                                                   │
│  TWO REPOS:                                        │
│  • claude_code_marketplace    → Registry (publish)  │
│  • claude_code_workshop       → Target (consume)    │
│                                                   │
└──────────────────────────────────────────────────┘
```

---

## Next Up

Continue to: [04_plugins_marketplace_architecture.md](./04_plugins_marketplace_architecture.md) — Package your components into a plugin, publish it to the marketplace, and install it in a fresh project.
