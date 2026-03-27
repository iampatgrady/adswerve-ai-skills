---
name: asana-sync
description: Syncs local codebase changes, pushes code, and updates the Epic/Subtask in Asana with a native Proof of Work comment. Triggered via /asana-sync.
license: Apache-2.0
metadata:
  author: Adswerve-MLOps
  version: "2.0.0"
  tags: [asana, git, sync, proof-of-work, workflow]
---

# Asana Sync Workflow

When this workflow is triggered via `/asana-sync`, you MUST:

**Context:** Use the IDs in `.agents/rules/asana-context.md` and `.agents/rules/active-task.md`.

**Step 1: Autonomous Git Operations**
- Run `git status`. If there are uncommitted changes, autonomously generate a commit message and commit them.
- Run `git push origin HEAD`.
- Determine the PR/MR URL based on the git remote.

**Step 2: The Epic/Subtask Loop & Native Comments**
- Read the active Asana Task ID from `.agents/rules/active-task.md`.
- **Proof of Work Logging:** Use the `mcp_asana_add_comment` tool to log the Proof of Work and PR links. DO NOT use the old `html_notes` fetching/appending hack. Write the comment for a non-technical PM (business value, GCP changes, PR URL).
- Ask the user: *"Is this feature completely finished?"*
  - **If YES:** Use `mcp_asana_update_tasks` with `completed: true` to close the Epic.
  - **If NO (multi-phase work):** Use `mcp_asana_create_tasks` using the `parent` parameter set to the active Task ID to create the next logical subtask, leaving the parent Epic open.

**Step 3: Walkthrough generation**
- Create a `walkthrough.md` artifact documenting what was accomplished.
- Present the PR link and Walkthrough to the user.
