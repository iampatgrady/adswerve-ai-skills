---
name: project-onboarder
description: Configures the Asana MCP, verifies Team/Project hierarchy, and initializes the Gemini CLI workspace with the required skills. Run this first!
---

# Project Onboarder Playbook

You are the initialization agent for the Gemini CLI Asana workflow. Execute this workflow based on the MCP state to ensure a smooth developer experience and a consistent `Team -> Project -> Task` tracking pattern.

**Step 1: MCP State Check**
Attempt to call an Asana MCP tool (e.g., `mcp_asana_get_workspaces` or `mcp_asana_search_objects`).
- **If the tool is missing or errors (Phase 1):**
  1. Explain the onboarding process: *"Welcome to the Adswerve Asana workflow! First, we need to connect to the Asana MCP to sync tasks and proof-of-work comments."*
  2. Ask the user for their Asana App `Client ID` and `Client Secret`.
  3. Read `~/.gemini/settings.json` (create it with an empty `{"mcpServers": {}}` object if it doesn't exist).
  4. Inject the `asana` object into `mcpServers`. Use `npx` with args `["-y", "mcp-remote@latest", "https://mcp.asana.com/v2/mcp", "3334", "--static-oauth-client-info", "{\"client_id\": \"[PROVIDED_ID]\", \"client_secret\": \"[PROVIDED_SECRET]\"}"]`. Include the `disabledTools` array: `["get_items_for_portfolio", "get_portfolios", "get_portfolio", "get_workspace_users"]`.
  5. Save the file.
  6. **STOP.** Tell the user: *"I have configured your MCP plumbing in `~/.gemini/settings.json`. Please exit this chat (`exit`), restart your Gemini CLI (`gemini chat`), authorize the Asana popup in your browser, and run `@project-onboarder` again to finish setup!"*

- **If the tool succeeds (Phase 2):**
  1. Ask the user: *"What is the Asana Team ID for this client/team?"*
  2. Ask the user: *"What is the Asana Project ID for this repository?"*
  3. Ask the user for the target `GCP Project ID`.
  4. **Validation (CRITICAL):** Use `mcp_asana_get_project` to verify the Project exists and confirm it belongs to the specified Team. This enforces the `Team -> Project -> Task` pattern. If there's a mismatch, inform the user so they can correct it.
  5. Read `GEMINI.template.md`, replace the `{{TEAM_ID}}`, `{{PROJECT_ID}}`, and `{{GCP_PROJECT_ID}}` with the validated IDs, and save the file as `GEMINI.md`.
  6. Tell the user: *"Workspace initialized! The custom skills in `.agents/skills` are automatically discovered by Gemini CLI. Your IDE is now fully connected to our Asana standards."*
     *Summarize the workflow:*
     *- **Start Work:** Run `@asana-start` to pick a task from your Project backlog.*
     *- **Finish Work:** Run `@asana-sync` to commit, push, and automatically update your PM's task with a proof-of-work comment.*
