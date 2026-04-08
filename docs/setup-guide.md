# Setup Guide

Detailed reference for file structure, MCP server patterns, and common pitfalls. Read this when you need to understand how things work under the hood — or when something breaks.

---

## File Structure

> **Note:** Paths below use `~/` (Mac/Linux). On Windows, the Claude Code config lives in `%USERPROFILE%\.claude\` (e.g. `C:\Users\YourName\.claude\`). The structure inside is the same.

This is what a working Claude Code setup looks like:

```
~/.claude/
├── settings.json          ← Global settings (hooks, plugins, effort level)
├── settings.local.json    ← Permissions (auto-allow tools)
├── CLAUDE.md              ← Global instructions
├── skills/                ← Your slash commands (markdown files)
│   └── code-review.md
├── mcp-servers/           ← Local MCP server installations
│   ├── hubspot/
│   │   ├── .env           ← API keys (never commit this)
│   │   └── run.sh         ← Startup script
│   ├── loom/
│   │   ├── .env
│   │   └── run.sh
│   └── my-custom-server/
│       ├── .env
│       ├── run.sh
│       ├── package.json   ← Dependencies
│       └── node_modules/  ← Installed packages
└── projects/
    └── {project}/
        └── memory/        ← Per-project memory
            ├── MEMORY.md
            └── *.md
```

---

## MCP Server Patterns

There are three ways to set up an MCP server.

### Pattern 1: Remote servers (easiest)

Use `@anthropic/mcp-remote` with a URL. Auth happens via browser popup. No local files needed.

```json
"atlassian": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.atlassian.com/v1/sse"]
}
```

Best for: Jira, Confluence, Figma, Slack, and other services that offer a hosted MCP endpoint.

### Pattern 2: Local Node servers

A folder in `~/.claude/mcp-servers/` with a run script and `.env` file.

**run.sh:**
```bash
#!/bin/bash
set -e
SCRIPT_DIR="$(cd "$(dirname "$0")" && pwd)"
set -a
source "$SCRIPT_DIR/.env"
set +a
exec npx -y @hubspot/mcp-server
```

**.env:**
```bash
HUBSPOT_ACCESS_TOKEN=your-token-here
```

**settings.json:**
```json
"hubspot": {
  "command": "bash",
  "args": ["/path/to/.claude/mcp-servers/hubspot/run.sh"]
}
```

Best for: servers that need API keys and run via npm/npx.

### Pattern 3: Python-based servers

Some servers are written in Python and need a virtual environment.

**Directory structure:**
```
mcp-servers/my-python-server/
├── .env
├── .venv/           ← Python virtual environment
├── run.sh
├── requirements.txt
└── src/
```

**run.sh:**
```bash
#!/bin/bash
set -e
SCRIPT_DIR="$(cd "$(dirname "$0")" && pwd)"
set -a
source "$SCRIPT_DIR/.env"
set +a
exec "$SCRIPT_DIR/.venv/bin/python" -m my_server
```

Best for: servers from GitHub repos that use Python (Aircall, Skyscanner, custom servers).

---

## Common Pitfalls

**Missing packages**
If an MCP server does not start, it is almost always a missing npm or pip dependency. Check the server's README for install instructions.

**Wrong PATH**
Node-based servers need `npx` to be findable. If it fails, add the full path in your run script:
```bash
# Mac/Linux
exec /usr/local/bin/npx -y @some/mcp-server

# Or find it with: which npx
```

**Permissions on .env files (Mac/Linux)**
Keep API keys secure:
```bash
chmod 600 ~/.claude/mcp-servers/*/.env
chmod 700 ~/.claude/mcp-servers/*/run.sh
```
On Windows, `.env` files are not executable by default, so this is less of a concern. Just make sure they are not committed to git (the `.gitignore` in this repo handles that).

**The `.mcp.json` file**
Some setups use `~/.mcp.json` instead of `settings.json` for server configs. Both work. Pick one and stick with it.

**Auth expiring**
Some MCP servers (especially MS365, Teams) have sessions that expire. If a tool stops working, re-authenticate.

---

## The Best Approach: Let Claude Do It

Instead of manually creating folders, writing run scripts, and debugging PATH issues:

```
Set up the Atlassian MCP server for me. I want it as a local installation
in ~/.claude/mcp-servers/ with a run.sh and .env file.
```

Claude will create the directory, write the run script, install packages, configure auth, and add it to your settings. If something breaks, it will debug it too.
