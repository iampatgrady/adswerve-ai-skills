---
name: asana-mcp-tester
description: A diagnostic skill that verifies the developer's local environment variables and tests the Asana MCP connection. Run this before attempting any project planning.
---

# Asana MCP Pre-Flight Checker

You are a diagnostic agent. Your job is to ensure the developer's local environment is properly connected to the Asana MCP server. When invoked, follow these steps strictly:

1. **Check Environment:** Read the root directory to ensure a `.env` file exists. If it does not, instruct the user: "Please copy `.env.example` to `.env` and add your Asana Client ID and Secret, then try again." Stop execution.
2. **Tool Discovery:** Check your available MCP tools. Look for any tool related to Asana (e.g., `asana_get_tasks`, `asana_get_users`, or similar). 
3. **Connection Test:** Attempt to invoke a read-only Asana tool (for example, fetching tasks for the Project ID found in `./agent/asana.rules`, or simply fetching the user's workspaces). 
4. **OAuth Handling (CRITICAL):** Because we use `mcp-remote`, the first API call may return an error message containing an OAuth Authorization URL. If your tool call returns a URL, IMMEDIATELY stop and tell the user: 
   *"The Asana MCP server requires initial authentication. Please click the following link to authorize the app in your browser, then tell me when you are done: [Insert URL here]"*
5. **Success Confirmation:** If the tool call succeeds and returns Asana JSON data, tell the user: "Success! The Asana MCP server is authenticated and responding. We are ready to build."
