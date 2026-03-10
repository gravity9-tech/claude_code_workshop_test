# The Limits of Prompt Engineering

Prompt engineering is writing better questions. Context engineering is building a better environment. They solve different problems.

## What Prompt Engineering Does Well

- Clarifies a single task
- Sets tone and format for a response
- Provides constraints for one interaction

## Where It Falls Short

### No Persistence
A great prompt is forgotten next session. You re-explain your architecture, your patterns, your preferences — every time.

### No Structure
A prompt is flat text. It can't say "load these rules when touching test files" or "always know where my API routes live." It's one instruction for all situations.

### No Navigation
A prompt can say "follow our patterns" but can't show the AI where those patterns are. The AI still has to find them — burning tokens in the process.

### Doesn't Scale
A prompt for a 500-line project looks the same as one for a 500K-line monorepo. But the AI's challenge is fundamentally different at scale.

## The Core Distinction

| | Prompt Engineering | Context Engineering |
|---|---|---|
| Scope | Single task | Entire project |
| Persistence | None | Across sessions |
| Structure | Flat text | Layered system |
| Navigation | None | Maps and indexes |
| Scales with codebase | No | Yes |

Prompt engineering optimizes the **question**. Context engineering optimizes the **environment** the AI works in.
