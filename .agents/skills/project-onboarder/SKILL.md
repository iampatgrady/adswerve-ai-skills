---
name: project-onboarder
description: Configures the Asana MCP, searches for client projects, and initializes the workspace. Run this first!
---

# Project Onboarder Playbook

You are the initialization agent. You must execute this workflow intelligently based on the current state of the MCP server.

**Step 1: MCP State Check**
Attempt to call an Asana MCP tool (e.g., `asana_get_workspaces` or `asana_get_users`).
- **If the tool is missing or returns an error (Phase 1 - Plumbing):**
  1. Ask the user for their Asana App `Client ID` and `Client Secret`.
  2. Read `~/.gemini/antigravity/mcp_config.json`.
  3. Inject/Update the `asana` object with the `mcp-remote` config, embedding the provided credentials. Include the `disabledTools` array: `["get_items_for_portfolio", "get_portfolios", "get_portfolio", "get_workspace_users"]`.
  4. Save the file.
  5. **STOP.** Tell the user: *"I have configured your MCP plumbing. Please restart Antigravity, click the Asana Authorization link when it pops up, and then run `@project-onboarder` again so we can find your project!"*

- **If the tool succeeds (Phase 2 - Context Gathering):**
  1. Ask the user: *"What is the name of the Client or Project we are working on?"*
  2. **Fuzzy Search Logic:** Use Asana MCP tools to find the IDs based on their answer:
     - Search for a Team that matches the client name.
     - **Fallback:** If no dedicated Team exists, search for Projects matching the client name within the "Adswerve" Team (Team ID: `2275827584484`).
  3. Present the findings to the user and ask them to confirm the correct Team ID and Project ID.
  4. Ask the user for the target `GCP Project ID`.
  5. Read `.agents/rules/asana-context.template.md`, replace the `{{VARIABLES}}` with the confirmed IDs, and save the new file as `.agents/rules/asana-context.md`.
  6. Delete `.agents/rules/asana-context.template.md` to cleanly finalize the initialization.
  7. Tell the user: *"Workspace initialized! You can now use `/asana-start` to begin vibe coding."*
