# Workshop 00: Project Setup

**Duration: ~5 minutes**

## What You'll Do

- Download the Tea Store project
- Understand the project structure
- Install dependencies
- Verify Claude Code installation

---

## Prerequisites

Before starting, ensure you have:

- **Python 3.9+** for the backend
- **Node.js 18+** for the frontend
- **Claude Code** installed (run one of these in your terminal):
  ```bash
  # Option 1: Using npm (all platforms)
  npm install -g @anthropic-ai/claude-code

  # Option 2: Using Homebrew (macOS/Linux only)
  brew install --cask claude-code
  ```
- A code editor (VS Code, Cursor, Vim, etc.)
- **Jira Cloud account** with:
  - A project you can create tickets in
  - An API token (we'll create this in Module 03)

---

## Step 1: Download and Open the Tea Store Project

1. Download the project zip file:
   **https://github.com/gravity9-tech/claude_code_workshop/archive/refs/heads/main.zip**

2. Extract the zip file to a location of your choice

3. Open the extracted folder in your favorite text editor (VS Code, Cursor, Vim, etc.)

---

## Step 2: Verify Claude Code Installation

Open a terminal in your project folder and run:

```bash
claude --version
```

You should see version output like `claude 2.x.x`.

---

## Step 3: Install & Run

In the terminal, run:

```bash
# macOS/Linux
./start.sh

# Windows
start.bat
```

This will install all dependencies and start both servers.

**Access Points:**
| URL | Description |
|-----|-------------|
| http://localhost:4321 | React frontend |
| http://localhost:8765 | FastAPI backend |
| http://localhost:8765/docs | API Docs |

---

## Project Features

The Tea Store app includes:

**Backend API:**
- `GET /api/products` — List products with filters
- `GET /api/products/{id}` — Get single product
- `GET /api/customization-config/{category}` — Customization options
- `GET /health` — Health check

**Frontend Features:**
- Product listing with hero section
- Filtering by category, price, material
- Shopping cart (localStorage)
- Wishlist functionality
- Product customization wizard
- Dark/light theme toggle
- Responsive design

---

## Useful Commands

| Command | Description |
|---------|-------------|
| `./start.sh` | Install deps & start both servers (macOS/Linux) |
| `start.bat` | Install deps & start both servers (Windows) |
| `./test.sh` | Run backend + frontend unit tests |
| `./test.sh --e2e` | Include E2E tests |
| `./test.sh --e2e --headed` | E2E with visible browser |
| `./test.sh --coverage` | Run with coverage reports |

> **Note:** On Windows, use `test.sh` with Git Bash or WSL.

---

## Checkpoint

Before continuing, verify:

- [ ] Tea Store project downloaded and extracted
- [ ] Started the app with `./start.sh` (macOS/Linux) or `start.bat` (Windows)
- [ ] Backend running at http://localhost:8765
- [ ] Frontend running at http://localhost:4321
- [ ] `claude --version` works
- [ ] You have access to a Jira Cloud project

---

## Next Up

Continue to: [01_context_window.md](./01_context_window.md) - Understanding the Context Window
