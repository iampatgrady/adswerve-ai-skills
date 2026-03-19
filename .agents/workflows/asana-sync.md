---
description: Syncs local codebase changes with Asana and updates the Project Manager.
---

# Asana Sync Workflow

**Trigger:** `/asana-sync`

**Context:** Use the IDs provided in the `.agents/rules/asana-context.md` file. 

**Step 1: Git Operations**
- Check `git status`. If uncommitted changes exist, ask the user for a commit message, commit them, and push the current branch to origin.
- Construct the hypothetical Pull Request URL (e.g., `https://bitbucket.org/workspace/repo/pull-requests/new?source=<current-branch>`).

**Step 2: PM Handoff via Asana MCP**
- Ask the user: *"Which Asana Task ID did you just complete?"* (or use search tools to find the in-progress task assigned to the user).
- **CRITICAL ASANA LIMITATIONS:** 
  - The Asana MCP *cannot* update or add tags. Do not attempt to use tags.
  - To mark a task complete, you MUST use the `asana_update_task` tool and pass `completed: true`.
  - To update the description, you MUST use `html_notes` (not `notes`), formatting it with basic HTML (e.g., `<body><strong>Update:</strong>...</body>`).
- **The Update:** Use the Asana MCP to leave a comment on the task (or update the `html_notes`). The update MUST be written for a non-technical Project Manager. It MUST include:
  1. A clear summary of the business value achieved.
  2. Any GCP infrastructure changes applied.
  3. The Pull Request URL constructed in Step 1.
- Finally, ensure the task is marked as complete.
