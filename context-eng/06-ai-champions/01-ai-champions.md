# AI Champions

AI Champions are team leads who bridge the gap between AI tooling and team adoption. They don't just use AI well — they make sure their team does too.

```
                              AI Champion
                                  │
                    ┌─────────────┼──────────────┐
                    ▼             ▼              ▼
              ┌──────────┐ ┌──────────┐  ┌──────────────┐
              │ Team     │ │ Team     │  │ Team         │
              │ Member A │ │ Member B │  │ Member C     │
              └────┬─────┘ └────┬─────┘  └──────┬───────┘
                   │            │               │
                   ▼            ▼               ▼
              ┌─────────────────────────────────────────┐
              │         Marketplace                     │
              │                                         │
              │  ┌───────────┐  ┌────────────────────┐  │
              │  │ DeepWiki  │  │ Starter CLAUDE.md  │  │
              │  │ Codemaps  │  │ Rule Packs         │  │
              │  │ Indexing  │  │ Skills Library      │  │
              │  └─────┬─────┘  └────────┬───────────┘  │
              │        │    Download     │              │
              └────────┼─────& Install───┼──────────────┘
                       ▼                 ▼
              ┌─────────────────────────────────────────┐
              │         Project .claude/                 │
              │                                         │
              │  Navigation ──► Memory ──► Rules        │
              │  (Codemaps)    (CLAUDE.md)  (rules/)    │
              │       │            │           │        │
              │       └────────────┼───────────┘        │
              │                    ▼                    │
              │          AI works with focus,           │
              │          not full-codebase scans        │
              └─────────────────────────────────────────┘
```

## The Role

- **Own the context** — Maintain CLAUDE.md and rules/ for their team's domain
- **Spot the waste** — Identify Token Drain & Drift patterns across team members
- **Teach the stack** — Help individuals set up their navigation layer, memory, and rules
- **Curate patterns** — Promote proven auto-memory learnings into shared CLAUDE.md rules
- **Review context, not just code** — PRs that change `.claude/` files get champion review

## How Champions Drive Context Engineering

```
AI Champion
    │
    ├── Sets up team CLAUDE.md + rules/
    │   (Instruction & Conditional layers)
    │
    ├── Establishes navigation layer
    │   (Codemaps, architecture docs, DeepWiki)
    │
    ├── Coaches team on the 3R Framework
    │   (Request, Reasoning, Response discipline)
    │
    └── Evolves context quarterly
        (Prune stale rules, promote learnings)
```

## The Feedback Loop

Champions sit between individual usage and team standards:

```
Team member hits drift → Reports to Champion → Champion adds rule → Entire team benefits
```

One person's problem becomes everyone's fix. This is what makes Context Engineering compound.

## The Marketplace

The **Context Engineering Marketplace** is where team members go to get started fast:

| Available | What It Does |
|-----------|-------------|
| **DeepWiki** | Auto-generates navigable wiki from your codebase — install and index your repo |
| **Starter CLAUDE.md** | Templates per stack (Node/React, Python/Django, Go, etc.) |
| **Rule Packs** | Pre-built rules/ for common domains (API design, testing, security) |
| **Codemap Tools** | Tree-sitter based indexing for your language |
| **Skills Library** | Reusable workflow skills (review, deploy, migrate) |

### Repository

The marketplace is hosted at: **https://github.com/PandoraJewelry/online-claude-code-marketplace**

```
online-claude-code-marketplace/
├── plugins/
│   ├── community/       ← Community contributions go here
│   ├── core/            ← Maintained by the platform team
│   └── integrations/    ← Third-party tool integrations
├── docs/
├── marketplace-browser/
├── schemas/
└── scripts/
```

### Getting Started

```bash
# Clone the marketplace
git clone https://github.com/PandoraJewelry/online-claude-code-marketplace.git

# Browse available plugins
ls plugins/core/
ls plugins/community/

# Copy what you need into your project
cp -r plugins/core/<plugin-name> /path/to/your-project/.claude/
```

### Contributing

Contributions go into `plugins/community/`:

1. Fork the repo
2. Add your plugin to `plugins/community/<your-plugin-name>/`
3. Include a README describing what it does and which stacks it supports
4. Open a PR

The marketplace removes the cold-start problem. Teams don't build context from scratch — they start from a proven baseline and adapt.

## Champion Checklist

- [ ] CLAUDE.md created and committed for your domain
- [ ] At least 2-3 path-scoped rules in `.claude/rules/`
- [ ] Navigation layer set up (architecture doc or codemap)
- [ ] Output discipline rules in place
- [ ] Team walked through the 3R Framework
- [ ] Marketplace packages installed and customised
- [ ] Quarterly review scheduled for context maintenance
