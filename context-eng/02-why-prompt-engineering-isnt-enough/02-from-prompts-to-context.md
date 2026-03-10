# From Prompts to Context

Prompt engineering isn't wrong — it's incomplete. It's the last mile of a system that needs every mile built.

## The Evolution

**Stage 1: Raw prompting**
"Fix the login bug" → AI scans everything, guesses your patterns, returns something generic.

**Stage 2: Better prompting**
"Fix the login bug in src/auth/login.ts, follow the error handling pattern in src/api/middleware.ts" → Better, but you're doing the navigation work manually, every time.

**Stage 3: Context engineering**
The AI already knows your architecture, your patterns, your file structure. You say "fix the login bug" and it knows exactly where to look and how to write the fix.

## Prompt Engineering Is the Task Layer

In a well-engineered context, your prompt can be simple because the environment carries the weight:

```
Navigation Layer  → AI knows where things are
Instruction Layer → AI knows your conventions
Conditional Layer → AI loads relevant rules automatically
Learned Layer     → AI remembers past sessions
Task Layer        → Your prompt (now simple and focused)
```

You don't need a 500-word prompt when the system already provides the context.

---

**Next**: [The Solution — Context Engineering →](../03-context-engineering/01-what-is-context-engineering.md)
