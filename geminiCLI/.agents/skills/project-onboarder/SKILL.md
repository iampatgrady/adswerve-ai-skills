---
name: project-onboarder
description: Configures the Asana and Repomix MCPs, extracts Asana Workspace and Project IDs from a URL, and initializes the workspace context. Use when the user asks to "setup", "onboard", or "run project-onboarder".
license: Apache-2.0
compatibility: Requires Gemini CLI.
metadata:
  author: Adswerve-MLOps
  version: "2.0.0"
  tags: [asana, mcp, setup, context, repomix]
---

# Project Onboarder Playbook

You are the initialization agent for the Agentic Workflow Architecture V2. When this skill is active, execute this workflow:

**Step 1: MCP State Check**
Attempt to call an Asana MCP tool (e.g., `mcp_asana_search_objects`).
- **If the tool is missing or errors (Phase 1):**
  1. Explain the onboarding process: *"Welcome to the Adswerve Asana workflow! First, we need to connect to the Asana MCP."*
  2. Ask the user for their Asana App `Client ID` and `Client Secret`.
  3. Read `~/.gemini/settings.json` (create it with an empty `{"mcpServers": {}}` object if it doesn't exist).
  4. Inject TWO objects into the `mcpServers` configuration:
     - First, inject the `asana` object. Use command `npx` with args `["-y", "mcp-remote@latest", "https://mcp.asana.com/v2/mcp", "3334", "--static-oauth-client-info", "{\"client_id\": \"[PROVIDED_ID]\", \"client_secret\": \"[PROVIDED_SECRET]\"}"]`. Include the `disabledTools` array: `["get_items_for_portfolio", "get_portfolios", "get_portfolio", "get_workspace_users"]`.
     - Second, inject a `repomix` object. Use command `npx` with args `["-y", "repomix", "--mcp"]`.
  5. Save the file.
  6. **STOP.** Tell the user: *"I have configured your MCP plumbing in `~/.gemini/settings.json`. Please exit this chat (`exit`), restart your Gemini CLI (`gemini chat`), authorize the Asana popup in your browser, and run `@project-onboarder` again to finish setup!"*

- **If the tool succeeds (Phase 2):**
  1. Ask the user: *"Please provide the full Asana Project URL for this repository."*
  2. Ask the user for the target `GCP Project ID`.
  3. **URL Parsing:** Autonomously extract the Workspace ID and Project ID from the provided Asana URL. (e.g., in `https://app.asana.com/0/<WORKSPACE_ID>/<PROJECT_ID>`, extract those values).
  4. **Validation:** Use `mcp_asana_get_project` to verify the Project exists.
  5. **Context Injection:** Read `.agents/rules/asana-context.template.md`, replace the `<WORKSPACE_ID>`, `<PROJECT_ID>`, and `<GCP_PROJECT_ID>` placeholders with the extracted/provided IDs, and save the file as `.agents/rules/asana-context.md`.
  6. Tell the user: *"Workspace initialized! The workflow is ready."*
     *Summarize the workflow:*
     *- **Start Work:** Run `/asana-start` to pick a task from your Project backlog.*
     *- **Finish Work:** Run `/asana-sync` to commit, push, and automatically update your PM's task.*
