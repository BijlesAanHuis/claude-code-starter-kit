# MCP Server Configurations

Example configs for `~/.claude/settings.json`. Copy the ones you need into your `mcpServers` block.

> **Reminder:** You can also just tell Claude: "Set up the [tool] MCP server for me" and it will do this for you.

> **Note:** MCP servers and their configs change frequently. The examples below are a starting point — always check the server's official repo or the [MCP directory](https://modelcontextprotocol.io/servers) for the latest install instructions. If a config does not work, ask Claude to fix it or install it fresh for you.

## Table of Contents

- [Code & Version Control](#code--version-control)
- [Project Management](#project-management)
- [Communication](#communication)
- [Design](#design)
- [Data & Databases](#data--databases)
- [Documents & Knowledge](#documents--knowledge)
- [Meetings & Recordings](#meetings--recordings)
- [CRM & Sales](#crm--sales)
- [Browser & Testing](#browser--testing)
- [Automation](#automation)
- [Monitoring & Analytics](#monitoring--analytics)
- [Search & Web](#search--web)
- [Finding More Servers](#finding-more-servers)

---

## Code & Version Control

### GitHub
```json
"github": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-github"],
  "env": {
    "GITHUB_TOKEN": "ghp_your-token-here"
  }
}
```
Create a token at: github.com/settings/tokens (scopes: repo, read:org)

### GitLab
```json
"gitlab": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-gitlab"],
  "env": {
    "GITLAB_TOKEN": "your-token-here",
    "GITLAB_URL": "https://gitlab.com"
  }
}
```

---

## Project Management

### Atlassian (Jira + Confluence)
```json
"atlassian": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.atlassian.com/v1/sse"],
  "env": {}
}
```
Authenticates via browser on first use.

### Linear
```json
"linear": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.linear.app/sse"],
  "env": {}
}
```

### Asana
```json
"asana": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.asana.com/sse"],
  "env": {}
}
```

### Notion
```json
"notion": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.notion.com/sse"],
  "env": {}
}
```

---

## Communication

### Microsoft 365 (Outlook, Calendar, OneDrive)
```json
"ms365": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.microsoft365.com/sse"],
  "env": {}
}
```

### Microsoft Teams
```json
"teams": {
  "command": "npx",
  "args": ["-y", "teams-mcp"],
  "env": {
    "TEAMS_CLIENT_ID": "your-app-client-id",
    "TEAMS_TENANT_ID": "your-tenant-id"
  }
}
```
Requires Azure AD app registration.

### Slack
```json
"slack": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.slack.com/sse"],
  "env": {}
}
```

### Gmail
```json
"gmail": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.google.com/gmail/sse"],
  "env": {}
}
```

---

## Design

### Figma
```json
"figma": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.figma.com/sse"],
  "env": {}
}
```

---

## Data & Databases

### Google Sheets
```json
"google-sheets": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.google.com/sheets/sse"],
  "env": {}
}
```

### Google Docs
```json
"google-docs": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.google.com/docs/sse"],
  "env": {}
}
```

### PostgreSQL
```json
"postgres": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://user:password@localhost:5432/dbname"]
}
```

### SQLite
```json
"sqlite": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-sqlite", "/path/to/database.db"]
}
```

### MySQL
```json
"mysql": {
  "command": "npx",
  "args": ["-y", "@benborla29/mcp-server-mysql"],
  "env": {
    "MYSQL_HOST": "localhost",
    "MYSQL_PORT": "3306",
    "MYSQL_USER": "root",
    "MYSQL_PASSWORD": "your-password",
    "MYSQL_DATABASE": "your-db"
  }
}
```

---

## Documents & Knowledge

### Confluence
Included in the Atlassian MCP above.

### Google Drive
```json
"google-drive": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.google.com/drive/sse"],
  "env": {}
}
```

---

## Meetings & Recordings

### Fireflies
```json
"fireflies": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.fireflies.ai/sse"],
  "env": {}
}
```

### Loom
```json
"loom": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.loom.com/sse"],
  "env": {}
}
```

---

## CRM & Sales

### HubSpot
```json
"hubspot": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.hubspot.com/sse"],
  "env": {}
}
```

### Salesforce
```json
"salesforce": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.salesforce.com/sse"],
  "env": {}
}
```

---

## Browser & Testing

### Chrome DevTools (Headless Browser)
```json
"chrome-devtools": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.anthropic.com/chrome-devtools/sse"],
  "env": {}
}
```
Lets Claude navigate web pages, click elements, fill forms, take screenshots, and run Lighthouse audits.

---

## Automation

### n8n
```json
"n8n": {
  "command": "npx",
  "args": ["-y", "n8n-mcp-server"],
  "env": {
    "N8N_BASE_URL": "https://your-instance.n8n.cloud",
    "N8N_API_KEY": "your-api-key"
  }
}
```

---

## Monitoring & Analytics

### ActivityWatch
```json
"activitywatch": {
  "command": "npx",
  "args": ["-y", "activitywatch-mcp"],
  "env": {}
}
```
Great for tracking your own tool usage. Install [ActivityWatch](https://activitywatch.net/) first, then connect the MCP to query your activity data from Claude.

### Sentry
```json
"sentry": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-sentry"],
  "env": {
    "SENTRY_AUTH_TOKEN": "your-token-here"
  }
}
```

---

## Search & Web

### Brave Search
```json
"brave-search": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-brave-search"],
  "env": {
    "BRAVE_API_KEY": "your-api-key"
  }
}
```

### Brightdata (Web Scraping)
```json
"brightdata": {
  "command": "npx",
  "args": ["-y", "@anthropic/mcp-remote", "https://mcp.brightdata.com/sse"],
  "env": {}
}
```
Scrapes any webpage as markdown, even behind bot detection.

---

## Finding More Servers

The MCP ecosystem is growing fast. These directories list hundreds of available servers:

- **[Official MCP Servers repo](https://github.com/modelcontextprotocol/servers)** — Reference implementations by Anthropic (great for understanding how servers are built)
- **[MCP Server Directory](https://modelcontextprotocol.io/servers)** — Browsable list maintained by Anthropic
- **[Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)** — Community curated, 1000+ servers
- **[MCP.so](https://mcp.so/)** — Searchable directory with categories
- **[Smithery](https://smithery.ai/)** — Another searchable MCP directory

If a tool has an API, there is probably an MCP server for it. Search these directories or just ask Claude: "Is there an MCP server for [tool]?"

---

## Config Structure Reference

Your full `~/.claude/settings.json` looks like this:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "your-token"
      }
    },
    "atlassian": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-remote", "https://mcp.atlassian.com/v1/sse"],
      "env": {}
    }
  }
}
```

Each server needs:
- **command** — How to run it (usually `npx`)
- **args** — The package name and any arguments
- **env** — Environment variables (API keys, tokens, etc.)

Some servers use `@anthropic/mcp-remote` with a URL — these authenticate via browser popup on first use. Others use local packages with API keys in env vars.
