# The 3R Framework

Every AI interaction spends tokens in three places. Optimise each one.

## Request — What Goes In

The files, instructions, and context the AI receives before it starts working.

**Unoptimised**: AI reads 50 files to understand a 3-file change.
**Optimised**: A codemap points it to the 3 files. CLAUDE.md explains the architecture. Rules load conditionally based on what it touches.

**Levers**: Codemaps, CLAUDE.md, pointer architecture, scoped rules.

## Reasoning — How It Thinks

The internal processing the model does to understand the problem and form a solution.

**Unoptimised**: Vague context forces broad exploration — the AI considers patterns from its training data instead of yours.
**Optimised**: Clear constraints and conventions narrow the reasoning space. The AI thinks about *your* solution, not *a* solution.

**Levers**: Architecture docs, explicit conventions, file boundary definitions.

## Response — What Comes Back

The output tokens returned to you.

**Unoptimised**: Long, hedging responses with restated context and unnecessary explanation.
**Optimised**: Concise, targeted output. Every line earns its place.

**Levers**: Output instructions in CLAUDE.md, response format rules.

## The Cascade

```
Bad Request → Wasted Reasoning → Bloated Response
    ↑                                      |
    └──── feeds back into next turn ───────┘
```

In multi-turn sessions, the Response becomes part of the next Request. Bloated output pollutes future context. The 3 Rs compound — for better or worse.

**Fix Request first. The rest follows.**
