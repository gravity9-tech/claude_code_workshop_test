# Workshop 02: MCP Servers & Playwright Testing

**Duration: ~12 minutes**

## What You'll Learn

- What MCP (Model Context Protocol) is
- How to add MCP servers to Claude Code
- Playwright MCP tools for browser automation
- Set up the foundation for basket testing (used in next workshop)

---

## What is MCP?

**Model Context Protocol (MCP)** is an open standard for connecting AI tools to external services. MCP servers give Claude Code new capabilities:

- Browser automation (Playwright)
- Database access
- API integrations
- File system operations
- And much more

Think of MCP servers as **plugins** that extend what Claude can do.

---

## How MCP Works

```
┌─────────────┐     MCP Protocol     ┌─────────────────┐
│ Claude Code │ ◄──────────────────► │   MCP Server    │
└─────────────┘                      │  (Playwright)   │
                                     └────────┬────────┘
                                              │
                                              ▼
                                     ┌─────────────────┐
                                     │    Browser      │
                                     └─────────────────┘
```

MCP servers run as separate processes and communicate with Claude Code via:
- **stdio** - Local process (most common)
- **SSE** - Server-sent events
- **HTTP** - Remote servers

---

## Task 1: Check Current MCP Servers

First, see what MCP servers are available:

```bash
claude mcp list
```

This shows all configured MCP servers.

---

## Task 2: Add Playwright MCP Server

Add the Playwright MCP server to your project:

```bash
claude mcp add playwright --scope project -- npx @playwright/mcp@latest
```

**Flags explained:**
- `playwright` - Name for this server
- `--scope project` - Saves to `.mcp.json` (shared with team)
- `--` - Separates claude args from the command
- `npx @playwright/mcp@latest` - The official Microsoft Playwright MCP server

### Important: Restart Claude Code

After adding the MCP server, you need to:

1. **Start (or restart) Claude Code:**
   ```bash
   claude
   ```

2. **Accept the MCP server** when prompted. You'll see a permission prompt asking to allow the new MCP server - accept it.

3. **Verify with `/mcp`** inside Claude Code to confirm the server is connected.

### Scope Options

| Scope | Location | Use Case |
|-------|----------|----------|
| `local` | `~/.claude.json` | Personal, not shared |
| `project` | `.mcp.json` | Team-shared, version controlled |
| `user` | `~/.claude.json` | Personal, all projects |

---

## Task 3: Verify Installation

Check the server was added in the Terminal:

```bash
claude mcp list
```

You should see `playwright` in the list.

Get details:

```bash
claude mcp get playwright
```

---

## Task 4: Understanding the .mcp.json File

After adding with `--scope project`, a `.mcp.json` file is created:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

This file should be committed to version control so your team shares the same MCP setup.

---

## Task 5: Start the Application

Before testing, start the Pandora application:

**Terminal 1:**
```bash
python main.py
```

The app runs at `http://localhost:8000`

---

## Task 6: Playwright MCP Tools

The Playwright MCP server provides these tools:

| Tool | Description |
|------|-------------|
| `browser_navigate` | Navigate to a URL |
| `browser_screenshot` | Capture screenshot (full page or viewport) |
| `browser_click` | Click an element by selector or text |
| `browser_type` | Type text into an input field |
| `browser_hover` | Hover over an element |
| `browser_select_option` | Select dropdown option |
| `browser_get_text` | Extract text from elements |
| `browser_wait_for` | Wait for element or condition |
| `browser_evaluate` | Run JavaScript in the page |
| `browser_resize` | Change viewport size |

**Next step:** In the next workshop, we'll create a `/test-basket` slash command that uses these tools to test shopping basket functionality.

---

## Task 7: Useful MCP Commands

```bash
# List all MCP servers
claude mcp list

# Get server details
claude mcp get playwright

# Remove a server
claude mcp remove playwright

# Check status in Claude Code
/mcp
```

---

## Playwright MCP Capabilities

Beyond individual tools, Playwright MCP enables:

| Capability | Example Use Case |
|------------|------------------|
| User flows | Complete checkout process |
| Visual testing | Screenshot comparisons |
| Form validation | Test input error states |
| Responsive testing | Verify mobile/desktop layouts |
| State verification | Check basket totals, counts |

---

## Your .mcp.json

After this workshop, your project should have:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

---

## Checkpoint

- [ ] Added Playwright MCP server
- [ ] Verified with `claude mcp list`
- [ ] Understand the Playwright MCP tools
- [ ] Application running at http://localhost:8000

---

## Troubleshooting

**Server not starting:**
```bash
# Check if npx works
npx @playwright/mcp@latest --help
```

**App not running:**
```bash
# Make sure the app is started first
python main.py
```

**MCP tools not appearing:**
```
# In Claude Code, check MCP status
/mcp
```

---

## What You've Learned

1. **MCP** extends Claude Code with external capabilities
2. **Playwright MCP** provides browser automation tools (`browser_click`, `browser_get_text`, etc.)
3. **Project scope** shares config with your team via `.mcp.json`
4. **Next:** We'll use these tools in a slash command to test the shopping basket

---

## Next Up

Continue to: [04_slash_commands.md](./04_slash_commands.md) - Create the `/test-basket` command
