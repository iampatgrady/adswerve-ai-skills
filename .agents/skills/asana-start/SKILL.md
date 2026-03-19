---
name: asana-start
description: Queries Asana for open tasks, asks the user what to build, creates a feature branch, and plans the execution strategy.
---

# Asana Start Playbook

**Context:** Use the IDs provided in the `GEMINI.md` file in the repository root.

**Step 1: Fetching the Backlog**
- Use `asana_get_tasks` to fetch `incomplete` tasks for our Project ID.
- Present a numbered list of open tasks to the user and ask: *"Which task would you like to build?"*

**Step 2: Autonomous Branching**
- Fetch the selected task's full description.
- Autonomously run `git checkout main`, `git pull`, and `git checkout -b feature/asana-task-<ID>`.

**Step 3: Strategic Planning**
- Read the codebase to understand the current architecture.
- Present a brief "Implementation Plan" to the user detailing what you will build. Ask for their approval to start!
