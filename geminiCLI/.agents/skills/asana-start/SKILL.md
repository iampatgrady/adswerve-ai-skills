---
name: asana-start
description: Queries Asana for open tasks, creates a feature branch, analyzes the codebase using Repomix, and generates an implementation plan.
license: Apache-2.0
compatibility: Requires Gemini CLI.
metadata:
  author: Adswerve-MLOps
  version: "1.1.0"
  tags: [asana, git, planning, repomix]
---

# Asana Start Playbook

When this skill is active, you MUST:

**Context:** Use the IDs provided in the `GEMINI.md` file in the repository root.

**Step 1: Fetching the Context & Backlog**
- **Direct URL Input:** If the user provides an Asana Task URL (e.g., `https://app.asana.com/0/<workspace_id>/<project_id>/<task_id>`), parse it immediately to extract the Workspace, Project, and Task IDs. Skip to Step 2.
- **Backlog Query:** Otherwise, use `mcp_asana_get_tasks` (or `mcp_asana_search_objects`) to fetch `incomplete` tasks for our Project ID. Present a numbered list to the user and ask: *"Which task would you like to build?"*

**Step 2: Autonomous Branching and Context Compression**
- Fetch the chosen task's full description using `mcp_asana_get_task`.
- Autonomously run `git checkout main`, `git pull`, and `git checkout -b feature/asana-task-<ID>`.
- **CRITICAL - AST Analysis:** Do not blindly `grep` or read files one by one. Use the Repomix MCP server to generate a compressed Abstract Syntax Tree (AST) XML representation of the repository. Read this compressed context to understand the current architecture and how it relates to the task.
- **Plan Mode:** Ensure you are operating in a read-only/planning state. Generate an `implementation_plan.md` artifact detailing exactly what code you will write and what files you will modify. Do not write any application code yet.

**Step 3: User Approval**
- Present the generated implementation plan to the user. Ask for their approval to start executing the plan!

## Error Handling & Safety Rules
- **Rate Limits (429):** If an Asana or GCP API call fails with a 429 error, wait 10 seconds and retry automatically. Do not bother the user.
- **MCP Disconnection:** If the Asana or Repomix MCP tool calls fail entirely, immediately halt execution. Instruct the user: *"My MCP servers appear to be disconnected. Please verify your `~/.gemini/settings.json` file is properly configured and restart your IDE/CLI."*
