# Drift

Drift is what happens when AI produces code that *works* but doesn't *belong*.

## What Drift Looks Like

- A React component that uses a different state pattern than the rest of your app
- An API endpoint that skips your standard error handling middleware
- A utility function that duplicates one that already exists in `src/utils/`
- Test files structured differently from every other test in the project
- Naming conventions that don't match the codebase

## Why It Happens

The AI doesn't know your conventions. Without explicit guidance, it falls back on general training — best practices from millions of codebases, none of which are yours.

The larger the codebase, the worse drift gets. More files means more noise, less signal, and a higher chance the AI misses the patterns that matter.

## Why It's Dangerous

Drift is subtle. The code passes tests. It might even look cleaner than your existing code. But it introduces inconsistency, and inconsistency compounds:

- New developers (and future AI sessions) see conflicting patterns
- Code reviews catch it sometimes, miss it others
- Over time, the codebase loses its coherence

## Drain Feeds Drift

Token Drain and Drift reinforce each other. When the AI wastes tokens reading irrelevant files, it's less likely to find and follow your actual patterns. The drain *causes* the drift.
