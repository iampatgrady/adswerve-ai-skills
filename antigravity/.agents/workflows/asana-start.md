---
name: asana-start
description: Queries Asana for open tasks, creates a semantic feature branch, analyzes the codebase using Repomix, and generates an implementation plan. Triggered via /asana-start.
license: Apache-2.0
metadata:
  author: Adswerve-MLOps
  version: "2.0.0"
  tags: [asana, git, planning, repomix, workflow]
---

# Asana Start Workflow

When this workflow is triggered via `/asana-start`, you MUST:

**Context:** Use the IDs provided in `.agents/rules/asana-context.md`.

**Step 1: Fetching the Context & Backlog**
- Ask the user for the "GOAL for the day" or to select a task.
- **Direct URL Input:** If the user provides an Asana Task URL, parse it to extract the Task ID.
- **Backlog Query:** Otherwise, use `mcp_asana_search_objects` to fetch incomplete tasks for our Project ID. Ask the user: *"Which task would you like to build?"*

**Step 2: Autonomous Semantic Branching**
- Fetch the chosen task's full description using `mcp_asana_get_task`.
- Autonomously generate a semantic branch name based on the goal (e.g., `feature/<goal-name>`).
- **Silent Git Operations:** Execute `git checkout main`, `git pull`, and `git checkout -b <semantic-branch-name>` *silently* without narrating the terminal steps to the user.
- **Pro-Tip Integration:** Autonomously write the active Asana Task ID to `.agents/rules/active-task.md` so other workflows can find it.

**Step 3: Context Compression & Planning**
- Use the Repomix MCP server to generate a compressed Abstract Syntax Tree (AST) XML representation of the repository.
- Ensure you are operating in a read-only/planning state. Generate an `implementation_plan.md` artifact detailing the code to be written.

**Step 4: User Approval**
- Present the generated implementation plan to the user for approval.
