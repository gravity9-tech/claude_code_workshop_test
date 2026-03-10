# Output Discipline

If you don't read every line the AI produces, those tokens were wasted. Output discipline is optimising the Response R.

## Why It Matters

1. **Output tokens cost money** — often more than input tokens
2. **Context pollution** — in multi-turn sessions, verbose responses eat future capacity
3. **Attention waste** — unread output is zero-value output
4. **The cascade** — bloated responses feed back as bloated context in the next turn

## How to Set It

Add output rules to CLAUDE.md:

```markdown
## Output Rules
- Keep responses concise. Every line must earn its place.
- No filler, no restating the question, no unnecessary preamble.
- Lead with the answer, not the reasoning.
- Use tables and lists over paragraphs where possible.
```

Or as a rule file (`.claude/rules/output.md`):

```markdown
# Response Format
- Be concise. Short sentences. No padding.
- Code over explanation when possible.
- Only explain what isn't self-evident.
```

## In Auto-Memory

You can also tell the AI directly: "Always keep responses concise." It remembers via auto-memory and applies it in future sessions.

## The Test

> If a developer can't read and digest the response in under 3 minutes, it's too long.
