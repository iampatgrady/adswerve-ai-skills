# Agentic Workspace: Gemini CLI Edition

This repository contains the AI workflows required to turn any codebase into an autonomous, Asana-connected Agentic Workspace using the **Gemini CLI**.

## Bootstrapping a New Repository
1. Clone your target repository and open your terminal.
2. Type `gemini chat` to start your AI session.
3. Paste: *"Download the contents of the `geminiCLI` folder from `https://github.com/iampatgrady/adswerve-ai-skills` into this workspace."*
4. Run `@project-onboarder` to configure your Asana connection and set up the project context.

## Daily Workflow
Inside `gemini chat`, use these skills to iterate on code:
- **Start Work:** Type *"Run @asana-start"*. The AI will read your Asana backlog, make a branch, and plan the code. You will need to type `Y` to consent to skill execution.
- **Finish Work:** Type *"Run @asana-sync"*. The AI will save your code, push it to Git, create a PR link, update your PM in Asana, and close the ticket.

### Data Science / MiniOps Suite
For MLOps analysts deploying the MiniOps framework, use these skills to automate configuration and IT handoffs:
- `@miniops-onboarding`: Configures `config.yaml`, checks GCP IAM, generates an IT access request ZIP, and updates Asana.
- `@miniops-deploy`: Runs the `./install.sh` Terraform deployment and logs the infrastructure status to Asana.
