---
name: asana-sync
description: Syncs local codebase changes, pushes code, and updates the Project Manager in Asana with a Proof of Work comment.
license: Apache-2.0
compatibility: Requires Gemini CLI.
metadata:
  author: Adswerve-MLOps
  version: "1.1.0"
  tags: [asana, git, sync, proof-of-work]
---

# Asana Sync Playbook

When this skill is active, you MUST:

**Context:** Use the IDs in the `GEMINI.md` file in the repository root.

**Step 1: Autonomous Git Operations**
- Run `git status`. If there are uncommitted changes, autonomously generate a commit message based on the diff and commit them.
- Run `git push origin HEAD`.
- Run `git remote get-url origin` to determine the repo host. Construct a Pull Request or Merge Request URL based on the host and current branch.

**Step 2: PM Handoff & Proof of Work via Asana MCP**
- Ask the user: *"Which Asana Task ID did you just complete?"*
- **CRITICAL ASANA LIMITATIONS:** The MCP cannot currently update tags or create Task Comments (Stories). You MUST use `mcp_asana_update_task` and pass `completed: true`. 
- **Proof of Work Logging:** Because we cannot create comments natively yet, you must append your "Proof of Work" update to the bottom of the existing `html_notes` (description). **To avoid overwriting the original description, you MUST first fetch the task using `mcp_asana_get_task` to read the existing `html_notes`, append a clear `<hr>` and timestamp (e.g. `<b>Update: YYYY-MM-DD</b><br>`) along with your log, and then send the COMBINED text via `mcp_asana_update_task`.**
  The Proof of Work log entry MUST be written for a non-technical Project Manager. It must clearly include:
  1. The business value achieved.
  2. Any GCP infrastructure changes.
  3. The Pull Request URL.

**Step 3: Walkthrough generation**
- Create a `walkthrough.md` artifact that documents what was accomplished locally.
- Present the PR link and the Walkthrough to the user, confirming the task is complete.

## Error Handling & Safety Rules
- **Rate Limits (429):** If an Asana or GCP API call fails with a 429 error, wait 10 seconds and retry automatically. Do not bother the user.
- **MCP Disconnection:** If the Asana or Repomix MCP tool calls fail entirely, immediately halt execution. Instruct the user: *"My MCP servers appear to be disconnected. Please verify your `~/.gemini/settings.json` file is properly configured and restart your IDE/CLI."*
