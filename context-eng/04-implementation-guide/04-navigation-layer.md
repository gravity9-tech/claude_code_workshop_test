# Building a Navigation Layer

The navigation layer stops Token Drain at the source. Instead of scanning the codebase, the AI consults a map first.

## The Two-Phase Pattern

1. **Navigate** (cheap) — check the index, find relevant files
2. **Fetch** (expensive) — read only what's needed

Without navigation: AI reads 50 files → uses 3.
With navigation: AI reads the map → reads 3 files.

## Approaches

### Hand-Maintained Architecture Doc
Simplest approach. Add a structure section to CLAUDE.md with pointers.

```markdown
## Key Files
- Entry point: @src/index.ts
- Routes: @src/api/routes/ (one file per domain)
- Business logic: @src/services/
- Database: @src/db/queries/ (raw SQL, no ORM)
- Types: @src/types/index.ts
```

**Pros**: No tooling needed, version controlled, always accurate if maintained.
**Cons**: Manual upkeep, can drift from reality.

### Codemaps (Automated)
Tools like Aider's repo-map use tree-sitter to extract function signatures, classes, and exports into a lightweight index.

**Pros**: Auto-generated, always current, covers the full codebase.
**Cons**: Requires tooling setup.

### DeepWiki
Generates wiki-style documentation from code. Pre-computes relationships so the AI reads summaries instead of source.

**Pros**: Rich semantic context, explains relationships.
**Cons**: Heavier setup, may generate stale docs.

### Hybrid (Recommended)
- CLAUDE.md has a hand-maintained architecture overview
- A pre-session hook generates a codemap
- Detailed docs exist for complex subsystems

## Integration with Claude Code

- **CLAUDE.md pointers**: Zero setup, immediate value
- **Hooks**: Run codemap generation at session start
- **MCP servers**: Expose a live index as a queryable tool
- **Skills**: `/map` skill to regenerate on demand

## The Rule

If your codebase has more than ~50 files, you need a navigation layer. The AI shouldn't discover your architecture by reading — it should know it before it starts.
