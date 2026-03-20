---
name: asana-start
description: Queries Asana for open tasks, creates a feature branch, and generates an implementation plan for the user to review.
---

# Asana Start Playbook

You are the project planning agent for this Asana workspace.
**Context:** Use the IDs provided in the `GEMINI.md` file in the repository root.

**Step 1: Fetching the Backlog**
- Call `task_boundary` to visually indicate you are looking for work.
- Use `mcp_asana_get_tasks` (or `mcp_asana_search_objects`) to fetch `incomplete` tasks for our Project ID.
- Use `notify_user` to present a numbered list of open tasks to the user and ask: *"Which task would you like to build?"*

**Step 2: Autonomous Branching and Planning**
- Once the user selects a task, fetch the task's full description using `mcp_asana_get_task`.
- Autonomously run `git checkout main`, `git pull`, and `git checkout -b feature/asana-task-<ID>`.
- Read the codebase to understand the current architecture and how it relates to the task.
- Generate an `implementation_plan.md` artifact detailing what you will build.

**Step 3: User Approval**
- Use the `notify_user` tool with `BlockedOnUser: true` to present the generated implementation plan to the user. Ask for their approval to start executing the plan!
