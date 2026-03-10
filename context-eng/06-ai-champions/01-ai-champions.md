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

### How It Works

1. Champion selects packages relevant to their team's stack
2. Team members install via CLI or copy into `.claude/`
3. Champion customises to match team-specific conventions
4. Updates flow through the marketplace as the community evolves them

The marketplace removes the cold-start problem. Teams don't build context from scratch — they start from a proven baseline and adapt.

## Champion Checklist

- [ ] CLAUDE.md created and committed for your domain
- [ ] At least 2-3 path-scoped rules in `.claude/rules/`
- [ ] Navigation layer set up (architecture doc or codemap)
- [ ] Output discipline rules in place
- [ ] Team walked through the 3R Framework
- [ ] Marketplace packages installed and customised
- [ ] Quarterly review scheduled for context maintenance
