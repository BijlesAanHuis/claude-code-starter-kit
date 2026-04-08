# Bug Triage

Systematically triage a bug report by gathering context from multiple sources before creating a ticket.

## Instructions

When the user reports a bug:

1. **Search for existing tickets** — Check the project tracker (Jira, Linear, etc.) for existing tickets about this issue. If one exists, link to it and stop.

2. **Search team communication** — Look through recent messages in Teams/Slack for similar reports from other team members or support.

3. **Check recent code changes** — Search GitHub/GitLab for recent commits or PRs that touch the affected area. Look for anything that could have introduced this.

4. **Assess severity** — Based on what you found:
   - Critical: affects all users, blocks core functionality
   - High: affects many users, has no workaround
   - Medium: affects some users, has a workaround
   - Low: cosmetic or edge case

5. **Create a ticket** with:
   - Clear title describing the bug
   - Steps to reproduce (if known)
   - Expected vs actual behavior
   - Affected area / component
   - Related code changes (with links)
   - Related reports from other channels
   - Severity assessment
   - Suggested assignee (if obvious from recent changes)

6. **Notify the relevant person** — If the bug clearly relates to someone's recent work, send them a short message with a link to the ticket.
