# Adswerve AI Skills

This repository contains the AI workflows and skills required to turn any codebase into an autonomous, Asana-connected Agentic Workspace.

To get started, please navigate to the active setup guide corresponding to your tooling:

- [**Google Antigravity IDE**](./antigravity): The native skills and commands optimized for the Google Antigravity Editor environment.
- [**Gemini CLI**](./geminiCLI): The native skills and commands optimized for the Gemini CLI terminal workflow.

## Included Core Skills

- **`@project-onboarder`**: Interactively connects your local developer workspace to the active Asana project.
- **`@asana-mcp-tester`**: Diagnostic script to securely verify the connection to the Asana MCP server.
- **`@asana-start`**: Automatically selects tasks from the project backlog, creates a new Git feature branch, and outlines an implementation strategy.
- **`@asana-sync`**: Commits and pushes your code to Git, appends a Proof of Work summary on the Asana task, and handles ticket closure.

## Data Science "MiniOps" Suite

These specialized skills automate infrastructure deployments and IT handoffs specifically for Data Analysts working on GCP:
- **`@miniops-onboarding`**: Guides the analyst through configuration, verifies GCP IAM permissions, autonomously generates IT access requests if needed, and updates the active Asana task with blocked/ready statuses.
- **`@miniops-deploy`**: Remotely executes Terraform infrastructure buildouts and pipes the terminal status stream back to the Project Manager inside Asana.
- **`@analytics-onboarder`**: Automatically installs the official Google Analytics MCP server and configures your local GCP credentials for secure read-only API access.
- **`@miniops-label-explorer`**: Queries your live GA4 data via the MCP to analyze event distributions, filtering out noise and autonomously configuring the optimal predictive label in your `config.yaml`.
