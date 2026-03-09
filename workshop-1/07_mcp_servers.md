# Workshop 07: MCP Servers

**Duration: ~15 minutes**

## What You'll Learn

- What MCP (Model Context Protocol) is
- How to add a local MCP server (Playwright)
- How to add a remote MCP server (Atlassian/Jira)
- Managing MCP servers with `/mcp`

---

## What is MCP?

**Model Context Protocol (MCP)** is an open standard for connecting AI tools to external services. MCP servers give Claude new capabilities — tools it can call to interact with databases, APIs, browsers, and more.

```
┌─────────────┐     MCP Protocol     ┌─────────────────┐
│ Claude Code │ ◄──────────────────► │   MCP Server    │
└─────────────┘                      │  (Playwright,   │
                                     │    Jira, etc.)  │
                                     ├─────────────────┤
                                     │  Provides tools │
                                     │  Claude can use │
                                     └─────────────────┘
```

Think of MCP servers as **plugins** that extend what Claude can do.

---

## Two Types of MCP Servers

| Type | Transport | Where it runs | Auth |
|------|-----------|---------------|------|
| **Local (stdio)** | Standard I/O | Your machine | None needed |
| **Remote (HTTP)** | HTTP/SSE | Cloud service | Usually OAuth |

You'll set up one of each.

---

## Part 1: Local MCP — Playwright

The Playwright MCP server gives Claude the ability to interact with web browsers — navigate pages, click elements, fill forms, take screenshots, and more. This is perfect for testing our Tea Store app.

### Task 1: Add the Playwright MCP Server

```bash
claude mcp add --transport stdio playwright -- npx -y @anthropic-ai/mcp-playwright@latest
```

This registers a **local** MCP server called `playwright` that launches a browser Claude can control.

### Task 2: Verify and Test

Restart Claude Code (or start a new session), then check:

```bash
/mcp
```

You should see `playwright` listed with its tools (browser_navigate, browser_click, browser_screenshot, etc.).

Make sure your Tea Store app is running (`./start.sh | ./start.bat` in the project directory), then test:

```
Navigate to http://localhost:4321 and take a screenshot
```

```
Click on the "Green" category filter and tell me what products are shown
```

```
Add "Dragon Well Green Tea" to the cart and take a screenshot of the cart sidebar
```

Claude uses the Playwright MCP tools to interact with the live application in a real browser — useful for visual verification and end-to-end testing.

---

## Part 2: Remote MCP — Atlassian Jira

The Atlassian MCP server connects Claude to your Jira instance via OAuth. No API tokens to manage.

### Task 3: Add the Atlassian MCP Server

```bash
claude mcp add --transport http atlassian https://mcp.atlassian.com/v1/sse
```

### Task 4: Authenticate with OAuth

Start a Claude Code session and run:

```bash
/mcp
```

Select `atlassian` and follow the browser authentication:

1. A browser window opens
2. Log in with your Atlassian account
3. Grant the requested permissions
4. Return to Claude Code

Authentication tokens are stored securely and refresh automatically.

### Task 5: Test the Jira Connection

```
List 2 projects I have access to in Jira
```

If successful, you'll see your Jira projects. Now try:

```
Show me open issues in {YOUR_PROJECT_KEY}
```

```
What's the status of {TICKET-ID}?
```

```
Create a task in {YOUR_PROJECT_KEY}: "Test MCP integration from workshop"
```

---

## Managing MCP Servers

| Command | Description |
|---------|-------------|
| `/mcp` | View status and authenticate (inside Claude Code) |
| `claude mcp list` | List all configured servers |
| `claude mcp get <name>` | Show details for a server |
| `claude mcp remove <name>` | Remove a server |

### MCP Scopes

When adding servers, use `--scope` to control where the config is stored:

| Scope | Flag | Stored in | Use for |
|-------|------|-----------|---------|
| Local | `--scope local` (default) | `~/.claude.json` | Personal, this project |
| Project | `--scope project` | `.mcp.json` (git) | Shared with team |
| User | `--scope user` | `~/.claude.json` | Personal, all projects |

For team-shared servers, use `--scope project` to create a `.mcp.json` that gets checked into git.

---

## Clean Up

Remove the Playwright server if you want:

```bash
claude mcp remove playwright
```

Keep the Atlassian MCP — you'll use it in the next modules.

---

## Key Takeaways

- **MCP servers** extend Claude with new tools (browsers, APIs, databases)
- **Local servers** (like Playwright) run on your machine, no auth needed
- **Remote servers** (like Jira) connect to cloud services via OAuth
- Use **`--scope project`** to share MCP configs with your team
- **`/mcp`** to check status and authenticate

---

Continue to: [08_skills.md](./08_skills.md)
