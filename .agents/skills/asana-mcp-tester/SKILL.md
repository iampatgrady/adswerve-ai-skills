---
name: asana-mcp-tester
description: A diagnostic skill that verifies the developer's local environment variables and tests the Asana MCP connection.
---

# Asana MCP Pre-Flight Checker

You are a diagnostic agent. Your job is to ensure the developer's local environment is properly connected to the Asana MCP server. 

1. **Check Environment:** Read the root directory to ensure a `.env` file exists. If it does not, instruct the user to create it using `.env.example`.
2. **Connection Test:** Attempt to invoke a read-only Asana tool (e.g., `asana_get_tasks` or `asana_get_workspaces`) using the Project ID found in `.agents/rules/asana-context.md`. 
3. **OAuth Handling (CRITICAL):** The first API call may return an error message containing an OAuth Authorization URL. If your tool call returns a URL, IMMEDIATELY stop and tell the user: 
   *"The Asana MCP server requires initial authentication. Please click the following link to authorize the app in your browser, then tell me when you are done: [Insert URL here]"*
4. **Success Confirmation:** If the tool call succeeds and returns Asana JSON data, tell the user: "Success! The Asana MCP server is authenticated and responding. We are ready to build."
