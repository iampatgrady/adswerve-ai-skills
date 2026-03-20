# Agentic Workspace Framework

This repository contains the AI workflows and skills required to turn any codebase into an autonomous, Asana-connected Agentic Workspace.

## How to install this into an existing repository:
1. Open your target repository (e.g., `aeo-pulse`) in **Google Antigravity**.
2. Open the AI Chat and paste this exact prompt:
   > *"Download the `.agents` folder and `GEMINI.template.md` from `https://github.com/iampatgrady/asana-ai-dev` into this workspace. You can clone it to a temp directory, copy the files over to my root, and then delete the temp directory."*
3. Wait for the AI to copy the files, then type: `Run @project-onboarder` to configure your Asana connection and set up the project context.

## Daily Vibe Coder Workflow
Once installed, use these two skills to build software:
- `@asana-start`: The AI reads your Asana backlog, makes a branch, and plans the code in a task boundary.
- `@asana-sync`: The AI saves your code, pushes it to Git, creates a PR link, updates your PM in Asana, and closes the ticket.

### Data Science / MiniOps Suite
For MLOps analysts deploying the MiniOps framework, use these skills to automate configuration and IT handoffs:
- `@miniops-onboarding`: Configures `config.yaml`, checks GCP IAM, generates an IT access request ZIP, and updates Asana.
- `@miniops-deploy`: Runs the `./install.sh` Terraform deployment and logs the infrastructure status to Asana.
