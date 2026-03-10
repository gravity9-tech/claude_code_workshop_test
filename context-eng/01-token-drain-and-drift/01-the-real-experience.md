# What Developers Actually Experience

You plug an AI agent into your codebase. It reads dozens of files, thinks for a while, and returns something that looks right — until you realise it ignored your project's patterns, invented a utility that already exists, or restructured something nobody asked for.

Meanwhile, your token bill climbs.

This isn't an AI problem. It's a **context problem**. The AI had access to everything and understood nothing.

## The Symptoms

- AI reads 500 files when it needed 3
- Generated code works but doesn't match your conventions
- Repeated sessions re-discover the same things
- Responses are verbose, generic, or off-target
- Token costs scale with codebase size, not task complexity

## The Root Cause

Developers treat AI like a search engine — ask a question, get an answer. But AI agents don't search. They **read**. Without guidance on *what* to read and *how* your team builds, they default to reading everything and guessing the rest.

The result has a name: **Token Drain & Drift**.
