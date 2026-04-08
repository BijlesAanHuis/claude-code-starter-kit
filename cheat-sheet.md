# Prompt Cheat Sheet

Good prompts for common workflows. Copy, adapt, use.

---

## Setup & Configuration

### Bootstrap your CLAUDE.md from your own data
```
Look through my meeting transcripts and messages from the last year.
Based on what you find — my role, my team, how I work, what tools I use,
what I care about — write me a CLAUDE.md.
```

### Add a new MCP server
```
Set up the [Jira / GitHub / Figma / etc.] MCP server for me.
```

### Discover what tools to connect
```
Look at my ActivityWatch data from the past week. Which sites and apps
do I use most? For each one, check if there is an MCP server available.
```

---

## Multi-Tool Workflows

### Quarterly summary
```
I need a Q1 summary. Pull together:
1. Key metrics from the database: [list your metrics]. Compare Q1 vs Q4.
2. Everything we shipped: get all completed tickets from Q1, grouped by theme.
3. What the team discussed: search messages for recurring topics and decisions.
4. Put it in a spreadsheet with tabs: Metrics, Shipped, Themes.
5. Send it to [person] with a short summary.
```

### Sprint review
```
Pull all tickets completed this sprint from Jira. Cross-reference with
GitHub PRs to see what actually shipped. Create a summary grouped by
feature area.
```

### Onboarding doc
```
I am onboarding a new developer. Based on our repo structure, recent PRs,
CLAUDE.md, and Confluence docs, create a getting-started guide that covers:
setup, architecture overview, key conventions, and who to ask about what.
```

---

## Bug & Issue Management

### Triage a bug
```
Users report [describe the bug]. Search Jira for existing tickets about
this. Check GitHub for recent changes to [affected area]. Check messages
for similar reports. Create a ticket if none exists.
```

### Investigate a production issue
```
We are seeing [error/symptom] in production. Check Sentry for recent errors.
Look at GitHub for deployments in the last 48 hours. Check if there is a
pattern in the error rate.
```

---

## Code Workflows

### Review a PR
```
Review PR #[number]. Check for security issues, missing error handling,
and breaking API changes. Be specific about what to fix.
```

### Ship
```
Run tests, review the diff against main, and create a PR.
Flag anything that looks risky.
```

### Refactor
```
Refactor [component/module] to [goal]. Keep the same external behavior.
Run tests after each change to make sure nothing breaks.
```

### Debug
```
[Describe the bug]. Investigate the root cause. Do not guess — trace
the issue through the code. Only suggest a fix when you understand why
it is broken.
```

---

## Communication & Docs

### Draft a message
```
Draft a message to [person/channel] about [topic].
Keep it short and actionable.
```

### Meeting summary
```
Summarize my last meeting with [person/team]. Pull the transcript from
Fireflies/Loom. Highlight decisions, action items, and open questions.
```

### Update documentation
```
We just shipped [feature]. Read the existing docs in [location].
Update them to reflect what changed. Do not rewrite sections that are
still accurate.
```

---

## Data & Analysis

### Query data
```
How many [metric] did we have in [time period]? Query the database
and present the results in a table.
```

### Compare periods
```
Compare [metric] for [period A] vs [period B]. Show the delta and
percentage change. Flag anything unusual.
```

### Create a report
```
Pull [data points] from [sources]. Create a spreadsheet with
[describe structure]. Share it with [person].
```

---

## Tips for Better Prompts

1. **Name the tools.** "Search Jira for..." is better than "check if there is a ticket..."
2. **Be specific about output.** "Create a spreadsheet with 3 tabs" is better than "put it somewhere."
3. **Chain steps.** "Do X, then Y, then Z" works. Claude follows sequences.
4. **Say what NOT to do.** "Do not mention names" or "Do not modify tests" prevents surprises.
5. **Give context.** "We just shipped a release" helps Claude prioritize what to look at.
6. **One complex prompt > five simple ones.** Claude keeps context within a prompt, so bundling is more efficient.
