# Anti-Patterns

What not to do.

## The Kitchen Sink CLAUDE.md
Dumping everything into CLAUDE.md — 500+ lines of rules, full file contents, entire API docs. More context ≠ better context. Keep it under 200 lines and use pointers.

## The Empty Project
No CLAUDE.md, no rules, relying entirely on prompts. Every session starts from zero. The AI scans broadly, drifts freely, and you pay for it every time.

## Copy-Paste Conventions
Pasting entire files into CLAUDE.md instead of using `@path` pointers. This duplicates content, wastes tokens, and drifts from the actual source.

## Stale Rules
Rules that described the project 6 months ago. Outdated instructions are worse than no instructions — they cause confident drift.

## Over-Scoping Rules
Path-scoped rules with patterns so broad they match everything. If a rule applies to `**/*`, it should be in CLAUDE.md, not rules/.

## Ignoring Auto-Memory
Never checking what the AI has learned. Auto-memory can accumulate incorrect assumptions. Review it periodically, promote good learnings to CLAUDE.md, delete bad ones.

## Verbose Output Tolerance
Not setting output rules, then skimming AI responses. Every unread token is waste. If you're skimming, your output rules are too loose.

## The Fix Pattern

Every anti-pattern follows the same fix:
1. Notice the waste (drain or drift)
2. Identify which R is leaking
3. Add the minimum context to fix it
4. Verify the fix persists across sessions
