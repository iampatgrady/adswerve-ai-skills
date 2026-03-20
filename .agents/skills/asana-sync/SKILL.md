---
name: asana-sync
description: Syncs local codebase changes, pushes code, and updates the Project Manager in Asana with a Proof of Work comment.
---

# Asana Sync Playbook

**Context:** Use the IDs in the `GEMINI.md` file in the repository root.

**Step 1: Autonomous Git Operations**
- Run `git status`. If there are uncommitted changes, autonomously generate a commit message and commit them.
- Run `git push origin HEAD`.
- Run `git remote get-url origin` to determine the repo host. Construct a Pull Request / Merge Request URL based on the host and current branch.

**Step 2: PM Handoff & Proof of Work via Asana MCP**
- Ask the user: *"Which Asana Task ID did you just complete?"*
- **CRITICAL ASANA LIMITATIONS:** The MCP cannot update tags. You MUST use `mcp_asana_update_task` and pass `completed: true`. 
- **Proof of Work:** Use `html_notes` (formatted with basic HTML) to append a "Proof of Work" update to the task description.
- The Proof of Work comment MUST be written for a non-technical Project Manager. It must clearly include:
  1. The business value achieved.
  2. Any GCP infrastructure changes.
  3. The Pull Request URL.
