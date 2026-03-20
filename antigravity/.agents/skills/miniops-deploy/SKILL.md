---
name: miniops-deploy
description: Executes the MiniOps install.sh script, monitors Terraform deployment, and updates Asana with the infrastructure status.
license: Apache-2.0
compatibility: Requires Gemini CLI.
metadata:
  author: Adswerve-MLOps
  version: "1.0.0"
  tags: [miniops, gcp, terraform, deploy, asana]
---
# MiniOps Deployment & PMO Sync Playbook

You are the Adswerve MLOps Deployment Agent. Your objective is to run the infrastructure deployment and report the status to the Project Manager.

## Phase 0: Identify PMO State
1. Run `git branch --show-current` in the terminal to extract the Active Asana Task ID (e.g., `feature/asana-task-<ID>`). If not found, ask the user.

## Phase 1: Execution
1. Call `task_boundary` to indicate deployment is starting.
2. Ensure you are in the project root. Autonomously execute the following preparation steps in the terminal:
   - `python3 -m venv .venv` (to create the environment if it does not exist)
   - `source .venv/bin/activate` (to activate it)
   - `pip install -r requirements.txt` (to ensure `PyYAML` and other dependencies are installed)
   - `chmod +x install.sh`
   - `./install.sh`
3. Monitor the terminal output closely. This script runs Terraform and BigQuery deployments.

## Phase 2: Result Analysis & PMO Sync
### Path A: Success
If the script completes successfully (look for "VAI-MiniOps Hybrid Setup Complete"):
1. Use the Asana MCP to append this note to the Active Asana Task ID. **To avoid overwriting, you MUST first fetch the task using `mcp_asana_get_task`**, read the existing `html_notes`, append a timestamped separator along with:
   *"<b>Adswerve AI Update:</b> Infrastructure successfully deployed via Terraform. The MiniOps pipelines are now active in GCP."*, and send the combined text via `mcp_asana_update_task`.
2. Use `notify_user` to tell the analyst: *"Deployment successful! I have updated Asana. When you are ready to wrap up this ticket and push your configuration code, run `@asana-sync`."*

### Path B: Failure
If the script fails (e.g., IAM permission denied, API not enabled, Terraform error):
1. Read the stderr/stdout to determine the root cause.
2. Use the Asana MCP to append this note to the Active Asana Task ID. **To avoid overwriting, you MUST first fetch the task using `mcp_asana_get_task`**, read the existing `html_notes`, append a timestamped separator along with:
   *"<b>Adswerve AI Update - FAILED:</b> Infrastructure deployment failed. Root cause identified: [Insert brief 1-sentence summary of the error]. The analyst is currently troubleshooting."*, and send the combined text via `mcp_asana_update_task`.
3. Use `notify_user` to present the error summary to the analyst and offer your assistance in fixing the bug.
