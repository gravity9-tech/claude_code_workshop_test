# Using Auto-Memory

Auto-memory is the learned layer. The AI accumulates knowledge across sessions — build commands, debugging patterns, your preferences.

## How It Works

- Lives in `~/.claude/projects/<project>/memory/`
- `MEMORY.md` — first 200 lines loaded every session
- Topic files (e.g., `debugging.md`, `patterns.md`) — read on demand
- Claude manages it automatically, but you can edit it directly

## What Gets Remembered

- Build/test commands that work for your project
- Debugging patterns discovered during sessions
- Your correction patterns ("I told it to keep output concise — it remembers")
- File locations and project structure insights

## MEMORY.md vs Topic Files

Keep `MEMORY.md` as a concise index (under 200 lines). Move details to topic files.

```
memory/
├── MEMORY.md          # Index — loaded at startup
├── debugging.md       # Detailed debugging notes
├── conventions.md     # Patterns learned from corrections
└── commands.md        # Build/test/deploy commands
```

## The Relationship to CLAUDE.md

| CLAUDE.md | Auto-Memory |
|-----------|-------------|
| You write it | AI maintains it |
| Team-shared (version controlled) | Machine-local |
| Authoritative — the source of truth | Supplementary — fills gaps |
| Wins on conflicts | Defers to CLAUDE.md |

## When to Use Each

- **Stable conventions** → CLAUDE.md (you control it, team shares it)
- **Discovered patterns** → Auto-memory (AI learns and retains them)
- **Personal preferences** → Auto-memory or `~/.claude/CLAUDE.md`

## Managing It

- `/memory` command to view and browse
- Edit files directly — they're plain markdown
- Delete stale entries when projects evolve
