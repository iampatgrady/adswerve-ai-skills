---
description: Queries Asana for open tasks and preps the workspace for a new feature.
---

# Asana Start Workflow

**Trigger:** `/asana-start`

**Step 1: Fetching the Backlog**
- Use `asana_get_tasks` to fetch `incomplete` tasks for our Project ID.
- Present a numbered list of these open tasks to the user. Ask: *"Which task would you like to build?"*

**Step 2: Autonomous Branching**
- Once selected, fetch the task's full description.
- Autonomously run `git checkout main`, `git pull`, and `git checkout -b feature/asana-task-<ID>`. Do not explain the git commands to the user, just do them.

**Step 3: Strategic Planning**
- Read the codebase to understand the current architecture.
- Present a brief, easy-to-understand "Implementation Plan" to the user detailing what you are going to build. Ask for their approval to start vibe coding!
