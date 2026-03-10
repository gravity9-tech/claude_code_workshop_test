# The Scale Problem

Token Drain & Drift don't matter much on a 500-line project. They become critical at scale.

## Why Larger Codebases Suffer More

- **More files to scan** — without guidance, AI explores broadly
- **More patterns to miss** — conventions are implicit, spread across hundreds of files
- **More duplication risk** — existing utilities are buried in deep directory trees
- **Context window pressure** — large codebases can exhaust the window before the AI reaches the task

## The Paradox

The codebases that need AI the most — large, complex, hard to navigate — are the ones where AI performs worst without structure.

Small projects work fine with raw prompting. Enterprise codebases need Context Engineering.

---

**Next**: [Why Prompt Engineering Isn't Enough →](../02-why-prompt-engineering-isnt-enough/01-prompt-engineering-limits.md)
