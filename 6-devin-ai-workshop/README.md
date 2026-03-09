# Devin AI Workshop: Onboarding for Engineering & Product Teams

**Total Duration: ~110 min**

A hands-on guide to getting started with Devin AI — the autonomous AI software engineer. Learn how to delegate tasks, write effective prompts, and integrate Devin into your team's workflow.

## Prerequisites

- Devin AI invitation (check your Pandora email for the invite)
- Access to [app.devin.ai](https://app.devin.ai)
- GitHub account with access to the workshop repository:
  [PandoraJewelry/ai_workshop](https://github.com/PandoraJewelry/ai_workshop)
- Jira Cloud account with access to your team's project

## Workshop Modules

| Module | Topic | Duration |
|--------|-------|----------|
| 00 | [Introduction to Devin AI](./00_introduction.md) | 15 min |
| 01 | [Getting Started](./01_getting_started.md) | 20 min |
| 02 | [DeepWiki & Ask Devin](./02_deepwiki_ask_devin.md) | 15 min |
| 03 | [Effective Prompting](./03_effective_prompting.md) | 15 min |
| 04 | [Playbooks & Workflows](./04_playbooks_workflows.md) | 10 min |
| 05 | [Integrations — Jira, GitHub & MCP](./05_integrations.md) | 15 min |
| 06 | [Session Management & Working at Scale](./06_session_management.md) | 10 min |
| 07 | [Best Practices & Playbook](./07_best_practices_playbook.md) | 10 min |
| 08 | [Action Plan & Expectations](./08_action_plan_and_expectations.md) | 10 min |

## Learning Path

```
Introduction ──► Getting Started ──► DeepWiki & Ask Devin
                  (setup repo,        (understand code,
                   knowledge,          plan before you
                   secrets)            execute)
                                            │
                                            ▼
                                   Effective Prompting
                                   (task, context,
                                    requirements,
                                    success criteria)
                                            │
                        ┌───────────────────┼───────────────────┐
                        ▼                   ▼                   ▼
               ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
               │  Playbooks   │    │ Integrations │    │   Session    │
               │  & Workflows │    │ Jira, GitHub │    │  Management  │
               │              │    │ & MCP        │    │  & Scale     │
               └──────┬───────┘    └──────┬───────┘    └──────┬───────┘
                      └───────────────────┼───────────────────┘
                                          ▼
                                 Best Practices &
                                    Playbook
                                          │
                                          ▼
                               ┌────────────────────┐
                               │   Action Plan &    │
                               │   Expectations     │
                               │  (prove value,     │
                               │   guardrails,      │
                               │   next steps)      │
                               └────────────────────┘
```

---

## What You'll Build

Throughout this workshop, you'll work with the **Tea Store** project — an e-commerce application with a Python/FastAPI backend and React/TypeScript frontend. You'll:

- Set up Devin's environment and connect the repository
- Use DeepWiki and Ask Devin to explore the codebase
- Write well-structured prompts for real tasks (search filter, test coverage, new endpoints)
- Create a reusable Playbook for adding API endpoints
- Create a Jira ticket, trigger Devin's analysis, and start a session with Playwright testing
- Run parallel sessions and review PRs

---

## Key Concepts at a Glance

```
┌─────────────────────────────────────────────────────────────────┐
│                     Devin AI Workflow                           │
│                                                                 │
│  ┌───────────┐    ┌──────────────┐     ┌───────────────────┐    │
│  │   Wiki    │───►│  Ask Devin   │────►│     Session       │    │
│  │           │    │              │     │                   │    │
│  │ Understand│    │ Plan & scope │     │ Devin executes,   │    │
│  │ the code  │    │ the task     │     │ tests, opens PR   │    │
│  └───────────┘    └──────────────┘     └───────────────────┘    │
│                                                                 │
│  Supported by:                                                  │
│  ┌────────────┐ ┌────────────┐ ┌──────────┐ ┌──────────────┐    │
│  │ Knowledge  │ │ Playbooks  │ │ Secrets  │ │ Integrations │    │
│  │ (org-wide  │ │ (reusable  │ │ (creds)  │ │ (Jira, MCP,  │    │
│  │  standards)│ │  prompts)  │ │          │ │  GitHub)     │    │
│  └────────────┘ └────────────┘ └──────────┘ └──────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

---

## Troubleshooting

**Can't access Devin:**
- Check your Pandora email for the invitation link
- Ensure you're signing in at [app.devin.ai](https://app.devin.ai) with your Pandora account
- Contact your team lead if the invitation hasn't arrived

**Repository not visible:**
- Go to Settings > Integrations and verify GitHub is connected
- Check that your GitHub account has access to `PandoraJewelry/ai_workshop`

**Jira integration not working:**
- Ensure the dedicated Devin Jira account has been created and connected (if not you can use your account for this workshop)
- Verify the "Devin" label exists in your Jira project
- Check Settings > Integrations for connection status

**Devin session stuck or unresponsive:**
- Check the session status in the Devin dashboard
- Review the terminal output in the workspace for errors
- Start a new session rather than nudging a stuck one

## Help

- [Devin Documentation](https://docs.devin.ai)
- [Devin Support](mailto:support@cognition.ai)

---

Start with [00_introduction.md](./00_introduction.md)
