# Setting Up CLAUDE.md

CLAUDE.md is the instruction layer — always loaded, always present. It's the single most impactful file you can create.

## Where It Lives

| Location | Scope | Loads when |
|----------|-------|-----------|
| `~/.claude/CLAUDE.md` | All your projects | Every session |
| `./CLAUDE.md` | This project (team-shared) | Every session in this project |
| `./.claude/CLAUDE.md` | Same as above (alternative path) | Every session in this project |
| `./subdir/CLAUDE.md` | Subdirectory only | On-demand when files in subdir are read |

## What to Include

**Do include:**
- Project architecture (3-5 sentences max)
- Key directories and what lives in them
- Coding conventions that differ from general best practices
- Build/test/lint commands
- "Never do X" rules
- Pointers to important files: `See @src/types/index.ts for domain types`

**Don't include:**
- Entire file contents (use `@path` pointers instead)
- Generic best practices the AI already knows
- Information that changes frequently
- Everything — keep it under 200 lines

## Pointer Architecture

Don't describe. Point.

```markdown
## Architecture
- API routes: @src/api/routes/
- Shared types: @src/types/index.ts
- Auth middleware: @src/middleware/auth.ts
- Test helpers: @tests/helpers/
```

The AI reads the referenced files when needed, not upfront. This is the difference between a 50K token context dump and a 2K token navigation guide.

## Example

```markdown
# Project: Acme API

## Stack
Node.js, Express, TypeScript, PostgreSQL, Jest

## Structure
- src/api/ — Route handlers, grouped by domain
- src/services/ — Business logic, one file per domain
- src/middleware/ — Express middleware (auth, validation, errors)
- src/types/ — Shared TypeScript types (@src/types/index.ts)

## Conventions
- All API responses use the wrapper in @src/utils/response.ts
- Errors go through @src/middleware/errorHandler.ts — never catch and swallow
- Database queries live in src/services/, never in route handlers
- Tests mirror src/ structure in tests/

## Commands
- `npm test` — run all tests
- `npm run test:watch` — watch mode
- `npm run lint` — eslint + prettier

## Rules
- Never use `any` type
- Keep responses concise — no verbose explanations
```
