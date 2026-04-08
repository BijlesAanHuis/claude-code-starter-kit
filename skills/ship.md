# Ship

Run the full ship workflow: validate, review, and create a PR.

## Instructions

When the user says "ship", "deploy", or "create a PR":

1. **Check the current state**
   - Run `git status` to see uncommitted changes
   - Run `git diff` against the base branch to see the full changeset
   - Check if the branch is pushed to remote

2. **Run tests**
   - Detect the test command from package.json, Makefile, or CI config
   - Run the test suite
   - If tests fail, report which ones and stop

3. **Review the diff**
   - Check for security issues (hardcoded secrets, SQL injection, XSS)
   - Check for missing error handling on new code paths
   - Check for breaking API changes
   - Flag anything that looks unintentional (debug logs, commented code, TODOs)

4. **Create the PR**
   - Write a clear title (under 70 characters)
   - Write a description with:
     - Summary of changes (2-3 bullet points)
     - Test plan
   - Push the branch if not already pushed
   - Create the PR using `gh pr create`

5. **Report back** with the PR URL.
