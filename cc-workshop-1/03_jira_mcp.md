# Workshop 03: Jira MCP Integration

**Duration: ~5 minutes**

## What You'll Learn

- What MCP (Model Context Protocol) is
- How to connect Claude Code to Jira using OAuth
- Verify the connection works

---

## What is MCP?

**Model Context Protocol (MCP)** is an open standard for connecting AI tools to external services. MCP servers give Claude Code new capabilities:

- **Jira** — Create and manage tickets
- **Confluence** — Search and create documentation
- **Playwright** — Browser automation
- **Databases** — Query and modify data
- **GitHub** — Manage repos and PRs

Think of MCP servers as **plugins** that extend what Claude can do.

---

## How MCP Works

```
┌─────────────┐     MCP Protocol     ┌─────────────────┐
│ Claude Code │ ◄──────────────────► │   MCP Server    │
└─────────────┘       (SSE)          │   (Atlassian)   │
                                     ├─────────────────┤
                                     │     Tools:      │
                                     │  • jira_search  │
                                     │  • jira_create  │
                                     │  • jira_update  │
                                     │  • confluence   │
                                     └────────┬────────┘
                                              │ OAuth 2.1
                                              ▼
                                     ┌─────────────────┐
                                     │  Atlassian Cloud│
                                     └─────────────────┘
```

The Official Atlassian MCP uses **OAuth 2.1** — no API tokens to manage. Just log in with your browser.

---

## Task 1: Install the Atlassian MCP Server

Use our meta-skill to set up the Official Atlassian MCP server:

```
/create-jira-mcp setup jira mcp for this project
```

This will:
1. Register the Official Atlassian MCP server
2. Configure it at project scope (creates `.mcp.json`)

After the command completes, **restart Claude Code**.

---

## Task 2: Authenticate with OAuth

After restarting, verify the MCP server is registered:

```
/mcp
```

You should see `atlassian` listed. On first use, a browser window will open for OAuth authentication:

1. Log in with your Atlassian account
2. Grant the requested permissions
3. Return to Claude Code

The authentication happens automatically when you first use a Jira command.

---

## Task 3: Test the Connection

Ask Claude to interact with your Jira:

```
List all projects I have access to in Jira
```

If successful, you'll see your Jira projects listed.

---

## Try It Out

Now that you're connected, try these commands:

```
Show me open bugs in project {Project Name}
```

```
What's the status of {Ticket-ID}?
```

```
Create a task in {Project Name}: "Test MCP integration"
```

---

## Available Tools

The Official Atlassian MCP provides these capabilities:

| Tool | Description |
|------|-------------|
| `searchJiraIssuesUsingJql` | Search issues with JQL |
| `getJiraIssue` | Get issue details |
| `createJiraIssue` | Create new issues |
| `editJiraIssue` | Update existing issues |
| `transitionJiraIssue` | Change issue status |
| `addCommentToJiraIssue` | Add comments |
| `getVisibleJiraProjects` | List accessible projects |
| `searchConfluenceUsingCql` | Search Confluence |
| `getConfluencePage` | Get page content |
| `createConfluencePage` | Create new pages |

---

## Troubleshooting

**OAuth window doesn't open:**
- Check browser popup blockers
- Try a different default browser

**"Needs authentication" status:**
- Run any Jira command to trigger OAuth flow
- Or restart Claude Code and try again

**Can't see your projects:**
- Ensure you have access to the Atlassian Cloud site
- The MCP only works with Atlassian Cloud (*.atlassian.net)
- Self-hosted Jira Server/Data Center is not supported

**Frequent re-authentication:**
- This can happen occasionally with the OAuth flow
- Simply re-run the command to trigger a new login

---

## Next Up

Continue to: [04_skills_intro.md](./04_skills_intro.md) - Understanding Skills
