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
