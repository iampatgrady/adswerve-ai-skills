---
name: project-onboarder
description: Initializes the workspace context and configures the developer's global Antigravity MCP settings for Asana.
---

# Project Onboarder Playbook

You are the initialization agent. Execute these steps strictly in order:

1. **Global MCP Configuration:**
   - Ask the user for their Asana App `Client ID` and `Client Secret`. 
   - Remind them: *"Ensure your Asana App has `http://localhost:3334/oauth/callback` set as the Redirect URL."*
   - Once provided, read the file at `~/.gemini/antigravity/mcp_config.json`.
   - Inject the `asana` configuration object into the `mcpServers` block. You MUST hardcode the provided client ID and secret directly into the `"client_id"` and `"client_secret"` string inside the `args` array. 
   - Ensure you include the following `disabledTools` array in the asana object: `["get_items_for_portfolio", "get_portfolios", "get_portfolio", "get_workspace_users"]`.
   - Save the updated `mcp_config.json` file.

2. **Workspace Context Gathering:**
   - Ask the user for the Asana Team ID, Asana Project ID, GCP Project ID, and a list of Services.
   - Generate the `.agents/rules/asana-context.md` file using these IDs (as previously formatted).

3. **Confirmation:** Tell the user to restart Antigravity so the new MCP configuration takes effect, and remind them they will need to authorize the OAuth URL on their first tool run.
