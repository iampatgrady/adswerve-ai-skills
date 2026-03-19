---
name: asana-sync
description: Syncs local codebase changes, pushes code, and updates the Project Manager in Asana.
---

# Asana Sync Playbook

**Context:** Use the IDs in the `GEMINI.md` file in the repository root.

**Step 1: Autonomous Git Operations**
- Run `git status`. If there are uncommitted changes, autonomously generate a commit message and commit them.
- Run `git push origin HEAD`.
- Run `git remote get-url origin` to determine the repo host. Construct a Pull Request / Merge Request URL based on the host and current branch.

**Step 2: PM Handoff via Asana MCP**
- Ask the user: *"Which Asana Task ID did you just complete?"*
- **CRITICAL ASANA LIMITATIONS:** The MCP cannot update tags. You MUST use `asana_update_task` and pass `completed: true`. Use `html_notes` (formatted with basic HTML) to update the description.
- Write a comment for a non-technical Project Manager including the business value, GCP infrastructure changes, and the PR URL. Mark the task as complete.
