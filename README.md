# Claude Code Starter Kit

Everything you need to go from zero to a fully connected Claude Code setup.

### TL;DR — 5 minutes to get started

1. `npm install -g @anthropic-ai/claude-code`
2. `brew install gh && gh auth login`
3. Tell Claude: *"Read the repo at github.com/BijlesAanHuis/claude-code-starter-kit and help me set up my environment"*

That is it. Claude reads this guide and walks you through the rest. Everything below explains what each part does and why.

---

## What is Claude Code?

Claude Code is Anthropic's CLI tool that lets you work with Claude directly in your terminal or IDE. What makes it powerful is not just the AI — it is the setup around it. Connect your tools, teach it your workflow, and it becomes a multiplier for everything you do.

## The Key Insight

> You do not need to configure everything manually.
> Just tell Claude: **"Set up the Jira MCP server for me"** and it will edit your config, install packages, and test the connection.
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

Before doing anything else, install the GitHub CLI and log in:

```bash
brew install gh
gh auth login
```

Follow the prompts — it authenticates via browser, no tokens needed. Once logged in, Claude can read repos, create PRs, and search code through `gh` commands.

Now give Claude this repo:

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

### Step 4: Add MCP Servers

MCP (Model Context Protocol) is an open standard that lets Claude talk to external tools. You plug in a server, Claude gets new capabilities.

**The easy way:**
```
Tell Claude: "Set up the Jira MCP server for me"
```
Claude will edit your config, install the right packages, and configure auth.

**The manual way:**
Edit `~/.claude/settings.json`. See [examples/mcp-configs.md](./examples/mcp-configs.md) for configs for 20+ tools.

For details on file structure, server patterns, and common pitfalls, see the [setup guide](./docs/setup-guide.md).

Browse the official MCP servers repo for reference implementations: **[github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)**

### Step 5: Add a Skill

A skill is a reusable prompt saved as a markdown file. You invoke it with `/skill-name`.

Drop a markdown file into `~/.claude/skills/` and it becomes a slash command. See [skills/code-review.md](./skills/code-review.md) for a working example.

The best skills are the ones you build yourself — think about the workflows you repeat every week and turn them into skills. Start with one, add more as you go.

### Step 6: Try It

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

**MCP Servers** — Connect Claude to your tools (Jira, Figma, databases, etc.). For GitHub, use the `gh` CLI instead.

**CLAUDE.md** — Teach Claude your project context, team, and conventions.

**Memory** — Claude remembers decisions, preferences, and feedback across sessions. File-based, stored in `~/.claude/projects/{project}/memory/`.

**Skills** — Reusable prompts you invoke with `/skill-name`. Build them for your repetitive workflows.

**Auto-Invoke** — Define rules in CLAUDE.md that automatically trigger skills based on what you say. For example:

```
"bug" or "broken"        -->  /bug-triage
"ship" or "deploy"       -->  /ship
"how many" or "show me"  -->  /data-query
```

You say "bug" — Claude does not just listen, it runs your full triage workflow automatically.

---

## Prompt Cheat Sheet

See [cheat-sheet.md](./cheat-sheet.md) for a full list. Here are the highlights:

**Multi-tool workflow:**
```
Pull all Jira tickets completed this sprint, cross-reference with
GitHub PRs, and create a summary in Google Sheets.
```

**Bug triage:**
```
There is a bug where users cannot log in after resetting their password.
Search Jira for existing tickets, check GitHub for recent auth changes,
and create a ticket if one does not exist.
```

**Data analysis:**
```
Query the database for signups by month for the last 6 months,
compare with the same period last year, and put the results in a spreadsheet.
```

---

## Docs

- [Setup Guide](./docs/setup-guide.md) — File structure, MCP server patterns, common pitfalls
- [MCP Configs](./examples/mcp-configs.md) — Example configs for 20+ tools
- [Cheat Sheet](./cheat-sheet.md) — Prompt examples for common workflows
- [CLAUDE.md Template](./CLAUDE.md.template) — Starting point for your own CLAUDE.md

---

## Resources

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Official MCP Servers repo](https://github.com/modelcontextprotocol/servers) — Reference implementations by Anthropic
- [MCP Server Directory](https://modelcontextprotocol.io/servers) — Browsable list of available servers
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers) — Community curated, 1000+ servers
- [MCP.so](https://mcp.so/) — Searchable directory of MCP servers
- [Smithery](https://smithery.ai/) — Another searchable MCP directory
- [ActivityWatch](https://activitywatch.net/) — Open source tool usage tracker

---

## Tips from Experience

1. **Ask Claude to set things up for you.** "Install the Figma MCP" is faster than editing JSON.

2. **Start with 2-3 MCPs, not 10.** Your project tracker + your communication tool + one more. Add as you need them.

3. **Your CLAUDE.md is a living document.** Update it as your project evolves. If Claude keeps getting something wrong, add a rule.

4. **Memory saves you from repeating yourself.** Tell Claude "do not mock the database in tests" once, it remembers across sessions.

5. **Build your own skills.** The best skills come from your real workflows, not a template.

6. **Be specific in prompts.** "Search Jira for login issues, check the auth service in GitHub, create a ticket if none exists" beats "help me with this bug."

7. **Bootstrap your CLAUDE.md from your own data.** Connect your meeting/chat tools first, then ask Claude to build your profile from your actual communication.

8. **Use ActivityWatch to discover your tool stack.** Let it run for a week, then find MCP servers for your most-used tools.

9. **You do not need to understand every config option.** Start using it. You will learn what to customize as you hit real needs.
