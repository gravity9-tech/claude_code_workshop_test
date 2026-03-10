# The Context Engineering Stack

Context Engineering is a layered system. Each layer serves a different purpose and loads at a different time.

```
┌─────────────────────────────────┐
│  Navigation Layer               │  Know WHERE to look
│  Codemaps, DeepWiki, indexes    │
├─────────────────────────────────┤
│  Instruction Layer              │  Know HOW to work
│  CLAUDE.md, project conventions │
├─────────────────────────────────┤
│  Conditional Layer              │  Know WHEN rules apply
│  rules/, path-scoped files      │
├─────────────────────────────────┤
│  Learned Layer                  │  Know WHAT was discovered
│  Auto-memory, session history   │
├─────────────────────────────────┤
│  Task Layer                     │  Know WHAT to do
│  Your prompt                    │
└─────────────────────────────────┘
```

## Layer Breakdown

### Navigation Layer — Prevents Token Drain at the source
Pre-built indexes of your codebase structure. The AI consults a map before reading files. Eliminates full-codebase scans.

### Instruction Layer — Prevents Drift
Always-loaded project conventions. Architecture overview, coding standards, "never do X" rules. This is your constitution.

### Conditional Layer — Context efficiency
Rules that load only when relevant. Frontend rules load when touching React files. API rules load when touching endpoints. Keeps the base context lean.

### Learned Layer — Session continuity
What the AI has discovered across sessions. Build commands, debugging patterns, preferences. Supplementary to instructions — fills gaps, not foundations.

### Task Layer — Your prompt
With the other layers in place, this can be simple. "Fix the login bug" is enough when the AI already has the map, the rules, and the memory.

## The Principle

**Each layer should reduce the burden on the layer below it.** Good navigation means fewer instructions needed. Good instructions mean simpler prompts. The stack works together.

---

**Next**: [Implementation Guide →](../04-implementation-guide/01-claude-md.md)
