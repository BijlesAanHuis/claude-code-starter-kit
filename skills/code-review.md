# Code Review

Perform a structured code review on the current branch or a specific PR.

## Instructions

When the user asks for a code review:

1. **Get the diff**
   - If a PR number is given, fetch it from GitHub/GitLab
   - Otherwise, diff the current branch against the base branch (main/master)

2. **Review for correctness**
   - Does the code do what the PR title/description says?
   - Are there logic errors or off-by-one bugs?
   - Are edge cases handled?

3. **Review for security**
   - SQL injection, XSS, command injection
   - Hardcoded secrets or credentials
   - Unsafe deserialization or file operations
   - Missing authentication/authorization checks

4. **Review for quality**
   - Is there dead code or debug artifacts?
   - Are there unnecessary complexity or premature abstractions?
   - Could any part be simplified?

5. **Review for risk**
   - Does this change database schema? (migration safety)
   - Does this change API contracts? (breaking changes)
   - Does this affect performance-critical paths?
   - Is there adequate test coverage for the changes?

6. **Report findings** organized by severity:
   - **Must fix** — Bugs, security issues, breaking changes
   - **Should fix** — Quality issues, missing edge cases
   - **Consider** — Suggestions for improvement, optional refactors

Be specific. Point to exact lines. Suggest fixes, not just problems.
