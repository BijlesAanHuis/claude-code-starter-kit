# Claude Code Starter Kit

Everything you need to go from zero to a fully connected Claude Code setup.

### TL;DR — 5 minutes to get started

1. `npm install -g @anthropic-ai/claude-code`
2. Tell Claude: *"Set up the GitHub MCP server for me"*
3. Tell Claude: *"Read the repo at github.com/BijlesAanHuis/claude-code-starter-kit and help me set up my environment"*

That is it. Claude reads this guide and walks you through the rest. Everything below explains what each part does and why.

## What is Claude Code?

Claude Code is Anthropic's CLI tool that lets you work with Claude directly in your terminal or IDE. What makes it powerful is not just the AI — it is the setup around it. Connect your tools, teach it your workflow, and it becomes a multiplier for everything you do.

## The Key Insight

> You do not need to configure everything manually.
> Just tell Claude: **"Set up the GitHub MCP server for me"** and it will edit your config, install packages, and test the connection.
>
> This works for almost everything: MCP servers, skills, CLAUDE.md, memory. **Ask Claude to do it.**

---

## Getting Started

### Step 0: Think About Your Tools

Before installing anything, take 10 minutes to list the tools you use daily:

- Where does your code live? (GitHub, GitLab, Bitbucket)
- Where do you track work? (Jira, Linear, Asana, Notion)
- Where do you communicate? (Teams, Slack, Email)
- Where are your designs? (Figma, Storybook)
- Where is your data? (Postgres, BigQuery, Google Sheets, Athena)
- Where are your docs? (Confluence, Notion, Google Docs)
- Where are your meetings recorded? (Fireflies, Loom, Otter)

Each of these likely has an MCP server. The more you connect, the more Claude can do in a single prompt.

**Pro tip:** Install [ActivityWatch](https://activitywatch.net/) (open source, runs locally) and let it track your tool usage for a week. Then look at which sites and apps you use most — those are the ones worth connecting via MCP.

### Step 1: Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Or if you prefer the desktop app: [claude.ai/download](https://claude.ai/download)

**Auth:** When you first run `claude`, it will ask you to log in. If you have a Claude Pro, Max, or Team subscription, you just log in with your account — no API key needed. Only if you want to use the API programmatically do you need a key from [console.anthropic.com](https://console.anthropic.com).

### Step 2: Connect GitHub and Let Claude Help You Set Up

Before doing anything else, set up the GitHub MCP server:

```
Tell Claude: "Set up the GitHub MCP server for me"
```

Once GitHub is connected, give Claude this repo:

```
Read the repo at github.com/BijlesAanHuis/claude-code-starter-kit
and help me set up my Claude Code environment based on the guide.
```

From here, Claude can walk you through every remaining step: creating your CLAUDE.md, adding more MCP servers, installing skills. You do not need to follow this README manually — Claude reads it and does it with you.

### Step 3: Create Your CLAUDE.md

CLAUDE.md is a markdown file that tells Claude how to work with you and your project. It is not about code style — it is about **context**.

There are two levels:
- `~/.claude/CLAUDE.md` — Global. Applies to every conversation.
- `./CLAUDE.md` (in your repo root) — Project-specific. Overrides global.

See the [template](./CLAUDE.md.template) in this repo for a starting point.

#### The Fast Way to Bootstrap Your CLAUDE.md

If you have a meeting recording tool connected (Fireflies, Loom, etc.) or use Teams/Outlook, you can ask Claude:

> "Look through my meeting transcripts and messages from the last year. Based on what you find — my role, my team, how I work, what tools I use, what I care about — write me a CLAUDE.md."

This works surprisingly well. Claude will read through your actual communication patterns and build a profile that reflects how you really work, not how you think you work.

### Step 3: Add MCP Servers

MCP (Model Context Protocol) is an open standard that lets Claude talk to external tools. You plug in a server, Claude gets new capabilities.

**The easy way:**
```
Tell Claude: "Set up the Jira MCP server for me"
```
Claude will edit `~/.claude/settings.json`, install the right packages, and configure auth.

**The manual way:**
Edit `~/.claude/settings.json`:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "your-token-here"
      }
    }
  }
}
```

See [examples/mcp-configs.md](./examples/mcp-configs.md) for configs for 20+ tools.

Browse the official MCP servers repo for reference implementations: **[github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)**

#### The File Structure That Matters

This is where most people get stuck. Claude Code needs a specific directory structure and the right packages installed. Here is what a working setup looks like:

```
~/.claude/
├── settings.json          ← Global settings (hooks, plugins, effort level)
├── settings.local.json    ← Permissions (auto-allow tools)
├── CLAUDE.md              ← Global instructions
├── skills/                ← Your slash commands (markdown files)
│   ├── bug-triage.md
│   ├── ship.md
│   └── code-review.md
├── mcp-servers/           ← Local MCP server installations
│   ├── github/
│   │   ├── .env           ← API keys (never commit this)
│   │   └── run.sh         ← Startup script
│   ├── hubspot/
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

**There are two patterns for MCP servers:**

**Pattern 1: Remote servers (easy)** — Use `@anthropic/mcp-remote` with a URL. Auth happens via browser popup. No local files needed.
```json
"atlassian": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.atlassian.com/v1/sse"]
}
```

**Pattern 2: Local servers (more control)** — A folder in `~/.claude/mcp-servers/` with a run script and `.env` file:
```bash
# ~/.claude/mcp-servers/github/run.sh
#!/bin/bash
set -e
SCRIPT_DIR="$(cd "$(dirname "$0")" && pwd)"
set -a
source "$SCRIPT_DIR/.env"
set +a
exec npx -y @modelcontextprotocol/server-github
```

```bash
# ~/.claude/mcp-servers/github/.env
GITHUB_TOKEN=ghp_your-token-here
```

Then in your config, reference the run script:
```json
"github": {
  "command": "bash",
  "args": ["/path/to/.claude/mcp-servers/github/run.sh"]
}
```

**Pattern 3: Python-based servers** — Some servers need a Python venv:
```
mcp-servers/my-python-server/
├── .env
├── .venv/           ← Python virtual environment
├── run.sh
├── requirements.txt
└── src/
```

The run script activates the venv:
```bash
#!/bin/bash
set -e
SCRIPT_DIR="$(cd "$(dirname "$0")" && pwd)"
set -a
source "$SCRIPT_DIR/.env"
set +a
exec "$SCRIPT_DIR/.venv/bin/python" -m my_server
```

#### Common Pitfalls

1. **Missing packages.** If an MCP server does not start, it is almost always a missing npm/pip dependency. Check the server's README for install instructions.
2. **Wrong PATH.** Node-based servers need `npx` to be findable. If it fails, add the full path to npx in your run script.
3. **Permissions on .env files.** Keep API keys secure: `chmod 600 ~/.claude/mcp-servers/*/.env`
4. **Permissions on run scripts.** Make them executable: `chmod 700 ~/.claude/mcp-servers/*/run.sh`
5. **The `.mcp.json` file.** Some setups use `~/.mcp.json` instead of `settings.json` for server configs. Both work. Pick one and stick with it.

#### The Best Approach: Let Claude Do It

Seriously. Instead of manually creating folders, writing run scripts, and debugging PATH issues:

```
Set up the Atlassian MCP server for me. I want it as a local installation
in ~/.claude/mcp-servers/ with a run.sh and .env file.
```

Claude will create the directory, write the run script, install packages, configure auth, and add it to your settings. If something breaks, it will debug it too.

### Step 4: Add Skills (Slash Commands)

A skill is a reusable prompt saved as a markdown file. You invoke it with `/skill-name`.

Drop markdown files into `~/.claude/skills/` and they become available as slash commands.

See the [skills/](./skills/) folder for starter examples:
- [bug-triage.md](./skills/bug-triage.md) — Triage a bug report across multiple tools
- [ship.md](./skills/ship.md) — Run tests, review, and create a PR
- [code-review.md](./skills/code-review.md) — Structured code review

### Step 5: Try It

```bash
claude "Search my GitHub repos for any open PRs that have been waiting for review for more than 3 days"
```

---

## How It All Fits Together

```
+------------------------------------------+
|  Skills      (your own slash commands)    |
+------------------------------------------+
|  Memory      (persistent context)         |
+------------------------------------------+
|  CLAUDE.md   (project instructions)       |
+------------------------------------------+
|  MCP Servers (tool connections)           |
+------------------------------------------+
|     Claude Code (CLI / IDE / Desktop)     |
+------------------------------------------+
```

**Layer 1: MCP Servers** — Connect Claude to your tools (GitHub, Jira, Figma, databases, etc.)

**Layer 2: CLAUDE.md** — Teach Claude your project context, team, and conventions.

**Layer 3: Memory** — Claude remembers decisions, preferences, and feedback across sessions. File-based, stored in `~/.claude/projects/{project}/memory/`.

**Layer 4: Skills** — Reusable prompts you invoke with `/skill-name`. Build them for your repetitive workflows.

**Auto-Invoke** — Define rules in CLAUDE.md that automatically trigger skills based on what you say:
```markdown
# Auto-Invoke Rules
| Trigger | Skill |
|---------|-------|
| "bug", "broken" | /bug-triage |
| "ship", "deploy" | /ship |
| "how many", "show me data" | /data-query |
```

---

## Prompt Cheat Sheet

See [cheat-sheet.md](./cheat-sheet.md) for a full list. Here are the highlights:

### Multi-Tool Workflows
```
Pull all Jira tickets completed this sprint, cross-reference with GitHub PRs,
and create a summary in Google Sheets.
```

### Automated Triage
```
There is a bug where users cannot log in after resetting their password.
Search Jira for existing tickets, check GitHub for recent auth changes,
and create a ticket if one does not exist.
```

### Data Analysis
```
Query the database for signups by month for the last 6 months,
compare with the same period last year, and put the results in a spreadsheet.
```

### Code Review
```
Review the diff on this branch against main. Check for security issues,
missing error handling, and breaking API changes. Be specific.
```

---

## Resources

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Official MCP Servers repo](https://github.com/modelcontextprotocol/servers) — Reference implementations by Anthropic
- [MCP Server Directory](https://modelcontextprotocol.io/servers) — Browsable list of available servers
- [Awesome MCP Servers (community)](https://github.com/punkpeye/awesome-mcp-servers) — Large curated list of 1000+ MCP servers
- [MCP.so](https://mcp.so/) — Searchable directory of MCP servers
- [Smithery](https://smithery.ai/) — Another searchable MCP directory
- [ActivityWatch](https://activitywatch.net/) — Open source tool usage tracker (discover which tools to connect)

---

## Tips from Experience

1. **Ask Claude to set things up for you.** "Install the Figma MCP" is faster than editing JSON.

2. **Start with 2-3 MCPs, not 10.** GitHub + your project tracker + your communication tool. Add more as you need them.

3. **Your CLAUDE.md is a living document.** Update it as your project evolves. If Claude keeps getting something wrong, add a rule.

4. **Memory saves you from repeating yourself.** If you tell Claude "do not mock the database in tests" once, it remembers. You can also explicitly say "remember this."

5. **Skills compound over time.** Every repetitive workflow you turn into a skill saves you time on every future run.

6. **Be specific in prompts.** Bad: "help me with this bug." Good: "Search Jira for login issues, check the auth service in GitHub, and create a ticket if none exists."

7. **Tell Claude which tools to use.** "Search Teams for..." is better than "find out if anyone mentioned..." — it tells Claude exactly where to look.

8. **Bootstrap your CLAUDE.md from your own data.** Connect your meeting/chat tools first, then ask Claude to build your profile from your actual communication.

9. **Use ActivityWatch to discover your tool stack.** Install it, let it run for a week, then check which sites and apps you use most. Find MCP servers for those.

10. **You do not need to understand every config option.** Start using it. You will learn what to customize as you hit real needs.
