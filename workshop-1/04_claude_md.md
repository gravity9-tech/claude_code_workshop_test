# Workshop 04: CLAUDE.md & Memory

**Duration: ~15 minutes**

## What You'll Learn

- What CLAUDE.md is and why it matters
- How to generate one with `/init`
- Auto memory — how Claude learns on its own
- Organizing rules with `.claude/rules/`

---

## What is CLAUDE.md?

CLAUDE.md is a markdown file Claude reads at the **start of every session**. It gives Claude persistent context about your project — things it can't figure out by reading code alone.

```
┌─────────────────────────────────────────────┐
│         Every new session loads:             │
├─────────────────────────────────────────────┤
│ 1. System instructions (built-in)           │
│ 2. CLAUDE.md (your project instructions)    │
│ 3. Auto memory (Claude's notes to itself)   │
│ 4. Your first message                       │
└─────────────────────────────────────────────┘
```

Without CLAUDE.md, you'd repeat the same instructions every session: "use pnpm not npm," "run tests with pytest," "our API uses kebab-case URLs."

---

## Where CLAUDE.md Files Live

| Location | Scope | Shared? |
|----------|-------|---------|
| `./CLAUDE.md` | Project (checked into git) | Team via source control |
| `./CLAUDE.local.md` | Project (gitignored) | Just you |
| `~/.claude/CLAUDE.md` | All your projects | Just you |

Project CLAUDE.md is the most important — it's shared with your team and loaded every session.

---

## Task 1: Generate a CLAUDE.md with `/init`

Run this in your project:

```bash
/init
```

Claude analyzes your codebase and generates a starter CLAUDE.md with:
- Build and test commands it detected
- Code patterns and conventions
- Project structure notes

Review the generated file. It's a starting point — you'll refine it next.

---

## Task 2: Refine Your CLAUDE.md

Open the generated `CLAUDE.md` and make it yours. A good CLAUDE.md is **short and specific**. Target under 200 lines.

### What to include

```markdown
# Build & Test
- Run tests: `npm test`
- Lint: `npm run lint`
- Build: `npm run build`

# Code Style
- Use ES modules (import/export), not CommonJS (require)
- Prefer named exports over default exports

# Project Conventions
- API routes live in src/api/
- Tests mirror src/ structure in tests/
- Environment variables are in .env.example
```

### What NOT to include

- Things Claude can figure out by reading code
- Standard language conventions Claude already knows
- Long explanations or tutorials
- Information that changes frequently

**Test:** For each line, ask *"Would removing this cause Claude to make mistakes?"* If not, cut it.

---

## Task 3: Test Your CLAUDE.md

Start a new session (`/clear` or restart Claude Code) and ask:

```
What are the build and test commands for this project?
```

Claude should answer using your CLAUDE.md instructions — not by guessing.

---

## Auto Memory

Auto memory lets Claude take notes for itself across sessions. You don't write these — Claude does, based on corrections and patterns it discovers.

### How it works

- Auto memory is **enabled by default** but the memory file doesn't exist until Claude first writes to it
- Claude decides what's worth saving — build commands, debugging insights, preferences you correct it on
- Once created, notes live in `~/.claude/projects/<project>/memory/MEMORY.md`
- First 200 lines load at the start of every session
- Claude creates topic files for detailed notes

### When does MEMORY.md get created?

The file is **not** created on your first session. Claude only creates it when it encounters something worth remembering. The fastest way to trigger it is to explicitly tell Claude to remember something:

```
Remember: always use pnpm, not npm in this project
```

After this, you'll find the file at `~/.claude/projects/<project>/memory/MEMORY.md`.

### View and manage memory

```bash
/memory
```

This shows all loaded CLAUDE.md files and auto memory. You can open and edit any of them.

---

## Task 4: Teach Claude Something

Tell Claude a preference it should remember:

```
Remember: to run tests use ./test.js, for backend only use cd backend && pytest -v
or
Remember to end each request with "All done!"
```

Now `/clear` and start a fresh session. Ask Claude "how do I run the tests?" — it should answer from memory without reading any files.

---

## Organizing Rules with `.claude/rules/`

For larger projects, split instructions into topic-specific files:

```
.claude/
├── CLAUDE.md              # Main instructions (keep short)
└── rules/
    ├── code-style.md      # Code formatting rules
    ├── testing.md         # Testing conventions
    └── api-design.md      # API patterns
```

Rules can be **scoped to file types** using frontmatter:

```markdown
---
paths:
  - "src/api/**/*.ts"
---

# API Rules
- Use kebab-case for URL paths
- Always include input validation
- Return consistent error formats
```

This rule only loads when Claude works with files matching `src/api/**/*.ts` — keeping context clean.

---

## Key Takeaways

- **`/init`** generates a starter CLAUDE.md — refine from there
- Keep CLAUDE.md **under 200 lines** — it loads every session
- **Auto memory** lets Claude learn without you writing anything
- Use **`.claude/rules/`** to organize and scope instructions

---

Continue to: [05_prompting.md](./05_prompting.md)
