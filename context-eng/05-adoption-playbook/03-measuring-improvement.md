# Measuring Improvement

Context Engineering should produce measurable results. Here's what to track.

## Token Usage

**Before**: Note your average token usage per task.
**After**: Compare after implementing CLAUDE.md + rules.

Key metrics:
- Tokens per task (should decrease)
- Files read per task (should decrease sharply)
- Context window compactions per session (fewer = better)

## Drift Indicators

- PR review comments about "doesn't match our pattern" (should decrease)
- AI-generated code that requires style/convention fixes (should decrease)
- Duplicate utility creation (should stop)

## Qualitative

- Can you use simpler prompts and get correct results?
- Does the AI find the right files on first try?
- Are responses focused and actionable?

## The Feedback Loop

```
Measure → Identify leaks → Add rule/instruction → Measure again
```

Common pattern: "The AI keeps using X pattern instead of Y" → Add to CLAUDE.md → Problem solved permanently.

This is the advantage over prompt engineering. A fix in CLAUDE.md applies to every future session, for every team member. One fix, permanent impact.
