# Workshop 01: Understanding the Context Window

**Duration: ~5 minutes**

## What You'll Learn

- What a context window is and why it matters
- How Claude Code manages conversation context
- Tools to monitor and manage your context usage

---

## What is a Context Window?

The **context window** is the total amount of text Claude can "see" and remember during a conversation. Think of it as Claude's working memory — everything you've said, every file Claude has read, and every response generated lives in this window.

### Size Matters

Claude's standard context window is **200,000 tokens** (approximately 150,000 words). To put that in perspective:

- **~500 pages** of a typical book
- **~50 average-sized source code files**
- **The entire Lord of the Rings trilogy** could fit with room to spare

---

## The Main Context Window

When you start a Claude Code session, everything goes into the **main context window**:

```
┌─────────────────────────────────────────────┐
│           Main Context Window               │
├─────────────────────────────────────────────┤
│ • Your messages                             │
│ • Claude's responses                        │
│ • File contents Claude reads                │
│ • Command outputs                           │
│ • Tool results (MCP servers, etc.)          │
│ • System instructions                       │
└─────────────────────────────────────────────┘
```

Every interaction adds to this window. A simple question? A few hundred tokens. Reading a large file? Thousands of tokens. Over a long session, it fills up.

---

## Managing Your Context

### Check Your Usage: `/stats`

See how much context (and money) you've used:

```bash
/stats
```

This shows:
- Tokens used in the current session
- Estimated cost
- Context utilization

### View Context Details: `/context`

Get a detailed breakdown of what's in your context window:

```bash
/context
```

This shows:
- How much space each part of the conversation takes
- Which files or tool results are using the most tokens
- Helps identify what to clear or compact

### Clear Everything: `/clear`

Start fresh with an empty context:

```bash
/clear
```

Use this when:
- Starting a new, unrelated task
- Context is cluttered with old information
- You want a clean slate

**Note:** This erases all conversation history — Claude won't remember anything from before.

### Compact Context: `/compact`

Summarize and compress your conversation:

```bash
/compact
```

This:
- Keeps important context (what you're working on)
- Removes verbose details (full file contents, long outputs)
- Lets you continue without losing track of your task

Use `/compact` when:
- You're mid-task but running low on context
- You want to continue but don't need every detail
- The conversation has accumulated a lot of noise

---

## Try It Out

1. Start a Claude Code session in your pandora-demo directory
2. Have a brief conversation
3. Run `/stats` to see your usage
4. Try `/context` to see what's taking up space
5. Try `/compact` to see how it summarizes
6. Run `/stats` again to see the difference

---

## Checkpoint

You now understand:

- [ ] What a context window is (Claude's working memory)
- [ ] That 200K tokens ≈ 500 pages of text
- [ ] Agents get their own separate context windows
- [ ] How to check usage with `/stats` and `/context`
- [ ] How to manage context with `/clear` and `/compact`

---

## Next Up

Continue to: [02_getting_started.md](./02_getting_started.md) - Start using Claude Code
