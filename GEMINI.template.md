# Asana Context
This workspace is directly tied to an Asana project and a GCP environment. You must intrinsically use these IDs whenever operating the Asana MCP server or deploying infrastructure.

- **Team ID:** {{TEAM_ID}}
- **Project ID:** {{PROJECT_ID}}
- **GCP Project ID:** {{GCP_PROJECT_ID}}

# Agentic Workspace Workflow
This repository follows the strict `Team -> Project -> Task` pattern for Project Managers.
1. Run `@asana-start` to pick a task from the project backlog.
2. Complete the work locally.
3. Run `@asana-sync` to push code and provide a Proof of Work comment on the Asana task.

## Data Science Workflow (MiniOps)
If this repository contains the MiniOps framework, analysts will use a specialized skill suite. These skills automatically update Asana tasks based on the current Git branch.
1. Run `@asana-start` to pick a task and create a branch.
2. Run `@miniops-onboarding` to configure the workspace and verify IT permissions. (Updates Asana with Blocked/Ready status).
3. Run `@miniops-deploy` to build the GCP infrastructure. (Updates Asana with Success/Fail status).
4. Run `@asana-sync` to push code and close the ticket.
