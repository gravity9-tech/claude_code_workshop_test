# Workshop 05: Exploring the Skill Creator

**Duration: ~8 minutes**

## What You'll Learn

- How to explore a pre-built skill
- Understand how the skill-creator skill works
- Prepare for building your own skills

---

## The Skill Creator

This repository includes a pre-built skill called `skill-creator` that can generate well-structured skills from natural language descriptions.

Take a look at it:

```bash
cat .claude/skills/skill-creator/SKILL.md
```

Notice the structure:
- **Frontmatter** with name, description, and allowed-tools
- **Instructions** that guide Claude step-by-step
- **Examples** showing expected behavior

---

## How the Skill Creator Works

The skill-creator skill:

1. **Parses your request** to understand what the skill should do
2. **Researches best practices** for the skill's domain
3. **Designs metadata** (name, description, required tools)
4. **Writes the SKILL.md** with proper structure
5. **Validates** against a checklist before saving

This meta-skill helps ensure all your custom skills follow best practices.

---

## Anatomy of the Skill Creator

Examine the key sections:

### Frontmatter
```yaml
---
name: skill-creator
description: Creates new skills from natural language descriptions...
---
```

### Instructions
The body contains step-by-step guidance:
- How to interpret user requests
- What research to perform
- How to structure the output
- Validation checklist

### Examples
Shows expected input/output patterns so Claude understands the format.

---

## Try a Simple Test

Test that the skill-creator is working:

```
What skills can you create for me?
```

Claude should recognize the skill-creator and explain its capabilities.

---

## Why Use a Skill Creator?

| Manual Approach | Skill Creator Approach |
|-----------------|------------------------|
| Copy/paste templates | Generates from description |
| Easy to forget sections | Validates against checklist |
| Inconsistent structure | Follows best practices |
| No research | Researches domain conventions |

---

## Checkpoint

You've now:

- [ ] Explored the pre-built skill-creator skill
- [ ] Understand its structure (frontmatter + instructions + examples)
- [ ] Understand what the skill-creator does at each step
- [ ] Tested that Claude recognizes the skill

---

## Key Takeaways

1. **Meta-skills save time** — the skill-creator automates skill creation
2. **Validation matters** — the checklist ensures quality
3. **Research improves output** — domain best practices are incorporated
4. **Skills are composable** — use skills to create more skills

---

## Next Up

Continue to: [06_dev_lifecycle.md](./06_dev_lifecycle.md) - Build Your Dev Lifecycle Skill Suite
