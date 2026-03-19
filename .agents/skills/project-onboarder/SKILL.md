---
name: project-onboarder
description: Configures the Asana MCP, asks for client details, and initializes the Gemini CLI workspace. Run this first!
---

# Project Onboarder Playbook

You are the initialization agent. Execute this workflow based on the MCP state.

**Step 1: MCP State Check**
Attempt to call an Asana MCP tool (e.g., `asana_get_workspaces`).
- **If the tool is missing or errors (Phase 1):**
  1. Ask the user for their Asana App `Client ID` and `Client Secret`.
  2. Read `~/.gemini/mcp.json` (create it with an empty `{"mcpServers": {}}` object if it doesn't exist).
  3. Inject the `asana` object with the `mcp-remote` config, embedding the credentials and the `disabledTools` array: `["get_items_for_portfolio", "get_portfolios", "get_portfolio", "get_workspace_users"]`.
  4. Save the file.
  5. **STOP.** Tell the user: *"I have configured your MCP plumbing. Please exit this chat, restart your Gemini CLI, authorize the Asana popup, and run `@project-onboarder` again!"*

- **If the tool succeeds (Phase 2):**
  1. Ask the user: *"What is the Asana Team ID for this client?"*
  2. Ask the user: *"What is the Asana Project ID for this repository?"*
  3. Ask the user for the target `GCP Project ID`.
  4. Read `GEMINI.template.md`, replace the `{{VARIABLES}}` with the provided IDs, and save the file as `GEMINI.md`.
  5. Tell the user: *"Workspace initialized! You can now ask me to run `@asana-start` to begin vibe coding."*
