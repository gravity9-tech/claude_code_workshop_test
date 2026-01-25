# Workshop 01: Introduction to Subagents

**Duration: ~10 minutes**

## What You'll Learn

- What subagents are and why they exist
- The three built-in subagents (Explore, Plan, general-purpose)
- How Claude delegates tasks using the Task tool
- Benefits of isolated context and parallel execution

---

## What Are Subagents?

Subagents are **specialized agents** that operate in their own isolated context windows. When Claude needs to perform complex tasks, it can delegate work to subagents instead of doing everything in the main conversation.

```
┌─────────────────────────────────────────────────────────────┐
│                    MAIN CONVERSATION                         │
│                    (your context window)                     │
│                                                              │
│   "Search the codebase for how authentication works"        │
│                                                              │
└─────────────────────────┬───────────────────────────────────┘
                          │ Claude delegates via Task tool
                          ▼
                ┌───────────────────┐
                │   EXPLORE AGENT   │
                │                   │
                │ • Searches files  │
                │ • Reads code      │
                │ • Analyzes        │
                │                   │
                │ (isolated context)│
                └─────────┬─────────┘
                          │
                          ▼
              Summarized results returned
              to main conversation
```

---

## Why Subagents?

| Problem | Solution with Subagents |
|---------|------------------------|
| Context window fills up with search results | Each agent has isolated context |
| Sequential execution is slow | Agents can run in parallel |
| One-size-fits-all tool access | Each agent gets specific tools |
| Information overload in main chat | Only summarized results returned |

---

## Built-in Subagents

Claude Code includes three built-in subagents that Claude automatically uses when appropriate:

### 1. Explore Agent (Read-Only, Fast)

**Purpose**: Search and analyze codebases without making changes

**Tools**: Glob, Grep, Read, WebSearch, WebFetch

**When used**:
- "Find all files that handle authentication"
- "How does the cart functionality work?"
- "Search for API endpoints"

```
User: "How does the payment processing work in this codebase?"

Claude: I'll delegate this to the Explore agent to search the codebase.

[Explore agent searches, reads files, analyzes patterns]

Claude: Based on the exploration, payment processing works like this...
```

### 2. Plan Agent (Research for Planning)

**Purpose**: Gather context before presenting an implementation plan

**Tools**: Read-only tools (Glob, Grep, Read)

**When used**:
- In plan mode when Claude needs to understand the codebase
- Before proposing architectural changes
- When researching existing patterns

### 3. General-Purpose Agent

**Purpose**: Complex, multi-step tasks requiring both exploration and action

**Tools**: All tools except Task (to prevent infinite delegation)

**When used**:
- Tasks requiring both reading and writing
- Complex reasoning with multiple dependent steps
- When other agents are too limited

---

## Observing Subagent Delegation

Let's watch Claude delegate to a subagent.

**Try this prompt:**

```
Search the codebase to understand how the cart functionality works.
Look at the data flow from adding items to checkout.
```

**Watch for:**
1. Claude decides this is a search/exploration task
2. Claude spawns the Explore agent
3. Explore agent searches files, reads code
4. Summarized results return to main conversation
5. Your main context stays clean

---

## The Task Tool

Internally, Claude uses the **Task tool** to spawn subagents:

```javascript
Task({
  prompt: "Search for authentication handling in the codebase",
  subagent_type: "Explore",  // or "Plan", "general-purpose"
  model: "sonnet"            // optional model override
})
```

**Key parameters:**
- `prompt` - What the agent should do
- `subagent_type` - Which agent to use
- `model` - Optional: sonnet, opus, haiku

---

## Benefits Realized

### 1. Context Isolation

```
Main Context                    Agent Context
─────────────                   ─────────────
Your conversation               Search results
remains focused                 File contents
                                Analysis details
         ◄──────────────────────
         Only summary returned
```

### 2. Parallel Execution

```
                    ┌─► Agent A ─┐
Main Context ───────┼─► Agent B ─┼───► Aggregated Results
                    └─► Agent C ─┘

All three agents work simultaneously!
```

### 3. Specialized Tools

| Agent | Tools | Why |
|-------|-------|-----|
| Explore | Read-only | Safe for searching |
| Plan | Read-only | Research without changes |
| General | All tools | Full capability when needed |

---

## Diagram: Complete Subagent Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    MAIN CONVERSATION                         │
│                    (your context window)                     │
└─────────────────────────┬───────────────────────────────────┘
                          │ Task tool
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
    ┌──────────┐    ┌──────────┐    ┌──────────┐
    │ Explore  │    │   Plan   │    │ General  │
    │ Agent    │    │  Agent   │    │ Purpose  │
    │          │    │          │    │  Agent   │
    │ Tools:   │    │ Tools:   │    │ Tools:   │
    │ • Glob   │    │ • Glob   │    │ • All    │
    │ • Grep   │    │ • Grep   │    │   except │
    │ • Read   │    │ • Read   │    │   Task   │
    │ • Web*   │    │          │    │          │
    └────┬─────┘    └────┬─────┘    └────┬─────┘
         │               │               │
         └───────────────┼───────────────┘
                         ▼
              Summarized results back
              to main conversation
```

---

## Key Takeaways

1. **Subagents = Isolated execution** - They have their own context windows
2. **Three built-in agents** - Explore (search), Plan (research), General-purpose (everything)
3. **Task tool** - How Claude spawns subagents
4. **Summarized results** - Only relevant info returns to main context
5. **Parallel capable** - Multiple agents can run simultaneously

---

## Checkpoint

You now understand:

- [ ] What subagents are and why they exist
- [ ] The three built-in subagents and their purposes
- [ ] How the Task tool spawns agents
- [ ] Benefits: isolation, parallelization, specialization

---

## Next Up

Continue to [02_custom_subagents.md](./02_custom_subagents.md) to learn how to create your own custom subagents with AGENT.md files.
