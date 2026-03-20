---
name: project-onboarder
description: Configures the Asana MCP, asks for client details, and initializes the workspace. Run this first!
---

# Project Onboarder Playbook

You are the initialization agent. You must execute this workflow intelligently based on the current state of the MCP server.

**Step 1: MCP State Check**
Attempt to call an Asana MCP tool (e.g., `asana_get_workspaces`).
- **If the tool is missing or returns an error (Phase 1 - Plumbing):**
  1. Ask the user for their Asana App `Client ID` and `Client Secret`.
  2. Read `~/.gemini/antigravity/mcp_config.json`.
  3. Inject/Update the `asana` object with the `mcp-remote` config, embedding the credentials. Add the `disabledTools` array: `["get_items_for_portfolio", "get_portfolios", "get_portfolio", "get_workspace_users"]`.
  4. Save the file.
  5. **STOP.** Tell the user: *"I have configured your MCP plumbing. Please restart Antigravity, click the Asana Authorization link when it pops up, and then run `Run @project-onboarder` again!"*

- **If the tool succeeds (Phase 2 - Context Gathering):**
  1. Use `notify_user` to ask the user: *"What is the Asana Team ID for this client?"* (Suggest they look at the first number in their Asana URL).
  2. Use `notify_user` to ask the user: *"What is the Asana Project ID for this repository?"* (The second number in the URL).
  3. Use `notify_user` to ask the user for the target `GCP Project ID`.
  4. **Validation (CRITICAL):** Use the `mcp_asana_get_project` tool to verify the Project exists and confirm it belongs to the specified Team. This enforces the `Team -> Project -> Task` pattern. If there is a mismatch, use `notify_user` to inform the user so they can correct it.
  5. Read `GEMINI.template.md`, replace `{{TEAM_ID}}`, `{{PROJECT_ID}}`, and `{{GCP_PROJECT_ID}}` with the provided/validated IDs, and save the new file as `GEMINI.md`.
  6. Use `notify_user` to tell the user: *"Workspace initialized! You can now mention the `@asana-start` skill to begin vibe coding."*
