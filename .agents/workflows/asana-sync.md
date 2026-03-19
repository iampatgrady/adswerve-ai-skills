---
description: Syncs local codebase changes, pushes code, and updates the Project Manager in Asana.
---

# Asana Sync Workflow

**Trigger:** `/asana-sync`
**Context:** Use the IDs in `.agents/rules/asana-context.md`.

**Step 1: Autonomous Git Operations**
- Run `git status`. If there are uncommitted changes, autonomously generate a logical commit message based on the diff and commit them. Do not ask the user to write the commit message.
- Run `git push origin HEAD`.
- Run `git remote get-url origin` to determine where this code is hosted (e.g., GitHub, Bitbucket). Use that URL and the current branch name to dynamically construct a Pull Request / Merge Request URL.

**Step 2: PM Handoff via Asana MCP**
- Ask the user: *"Which Asana Task ID did you just complete?"*
- **CRITICAL ASANA LIMITATIONS:** The MCP cannot update tags. You MUST use `asana_update_task` and pass `completed: true`. Use `html_notes` (formatted with basic HTML) to update the description.
- **The Update:** Write a comment/note for a non-technical Project Manager. Include:
  1. A clear summary of the business value achieved.
  2. Any GCP infrastructure changes applied.
  3. The Pull Request URL.
- Mark the task as complete.
