# Workshop 01: Understanding the Context Window

**Duration: ~10 minutes**

## What You'll Learn

- What a context window is and why it's your most important resource
- How every action consumes tokens
- What happens when the context fills up

---

## What is a Context Window?

The **context window** is everything Claude can "see" during a conversation. Think of it as working memory — every message, file read, and command output lives here.

```
┌─────────────────────────────────────────────┐
│           Context Window (~200K tokens)      │
├─────────────────────────────────────────────┤
│ • System instructions & CLAUDE.md           │
│ • Your messages                             │
│ • Claude's responses                        │
│ • Every file Claude reads                   │
│ • Every command output                      │
│ • MCP tool results                          │
└─────────────────────────────────────────────┘
```

**200,000 tokens** sounds like a lot (~150,000 words), but it fills fast. A single large file can be thousands of tokens. A debugging session with multiple file reads and command outputs can consume tens of thousands.

---

## Why This Matters

Context isn't just about running out of space. **Claude's performance degrades as context fills up.** A half-full context window means Claude may:

- Miss instructions you gave earlier
- Make more mistakes
- Forget decisions from earlier in the conversation
- Ignore parts of your CLAUDE.md

This is the single most important concept in this workshop. Every technique you'll learn is ultimately about **using context efficiently**.

---

## What Costs Tokens?

| Action | Approximate Token Cost |
|--------|----------------------|
| Short message from you | 50-200 tokens |
| Claude's response | 200-2,000 tokens |
| Reading a small file | 500-2,000 tokens |
| Reading a large file | 5,000-20,000 tokens |
| Bash command output | 100-10,000 tokens |
| MCP tool result | 500-25,000 tokens |

Every interaction adds up. A 30-minute session exploring a codebase can easily use 50-100K tokens — half the window.

---

## Task 1: Check Your Context Usage

Open Claude Code in your project and run:

```bash
/context
```

This shows a breakdown of what's consuming your context window. Note the total usage.

Now read a file:

```
Read the README.md in this project
```

Run `/context` again. Notice how the usage increased — that file is now in your context.

---

## Task 2: See Context Fill Up

Try a few more actions and check `/context` after each:

```
What files are in the src/ directory?
```

```
Show me the package.json
```

```
/context
```

Watch the numbers climb. Every question and every file read stays in the window.

---

## Key Takeaway

Every action has a token cost. The goal isn't to minimize usage — it's to keep context **relevant**. In the next module you'll learn practical techniques to manage this.

---

Continue to: [02_token_management.md](./02_token_management.md)
