---
description: Syncs local codebase changes with Asana and updates the Project Manager.
---

# Asana Sync Workflow

**Trigger:** `/asana-sync`

**Context:** Use the IDs provided in the `@.agents/rules/asana-context.md` rule file.

**Step 1: Code Verification & Git Operations**
- Check the `git status` to ensure all tests pass and changes are committed.
- If uncommitted changes exist, pause and ask the user for a commit message, then commit them to the current feature branch.
- Push the branch to the remote origin.

**Step 2: PR URL Construction**
- Dynamically construct the hypothetical Pull Request URL based on the current branch name (e.g., `https://bitbucket.org/workspace/repo/pull-requests/new?source=current-branch-name`). 

**Step 3: PM Handoff via Asana MCP**
- Use the Asana MCP tools to locate the specific task ID you just worked on.
- Use the Asana MCP `asana_add_comment` tool to leave a rich update on the task. The comment MUST be written for a non-technical Project Manager. 
- The comment MUST include: 
  1. A summary of the business value achieved.
  2. Any GCP infrastructure changes applied.
  3. The Pull Request URL constructed in Step 2.
- Finally, use `asana_update_task` to mark the task as `completed`.
