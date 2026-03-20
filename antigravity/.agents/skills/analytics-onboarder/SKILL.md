---
name: analytics-onboarder
description: Configures the official Google Analytics MCP server using your local GCP credentials. Use when setting up GA4 data access.
license: Apache-2.0
compatibility: Requires Gemini CLI or Google Antigravity IDE.
metadata:
  author: Adswerve-MLOps
  version: "1.0.0"
  tags: [mcp, analytics, ga4, setup]
---

# Google Analytics MCP Onboarding Playbook

When this skill is active, execute the following steps precisely to wire up the GA4 MCP server.

**Step 1: Dependency Check (`pipx`)**
1. Run `pipx --version` in the terminal. 
2. If it fails, halt and instruct the user: *"You need `pipx` installed to run the Google Analytics MCP. Please install it (e.g., `brew install pipx` on Mac, or `python3 -m pip install --user pipx` on Windows/Linux) and then run `@analytics-onboarder` again."*

**Step 2: Credential Scope Refresh & API Enablement**
The Analytics API requires an explicit OAuth scope that standard GCP logins do not provide, and the project must have the API explicitly enabled. Assume `gcloud` is already installed on the user's system.
1. Read the `config.yaml` file in the repository root to extract the target GCP Project ID (where the Asana onboarding was targeted).
2. Instruct the user: *"I am setting your active GCP project, enabling the GA4 Data APIs, and refreshing your local Google credentials to include Analytics read permissions."*
3. Autonomously execute this terminal command to switch projects: `gcloud config set project <EXTRACTED_PROJECT_ID>`
4. Autonomously execute this terminal command to enable the necessary APIs: `gcloud services enable analyticsdata.googleapis.com analyticsadmin.googleapis.com --project <EXTRACTED_PROJECT_ID>`
5. Autonomously execute this terminal command to start the login flow: `gcloud auth application-default login --scopes https://www.googleapis.com/auth/analytics.readonly,https://www.googleapis.com/auth/cloud-platform`
6. The terminal will pause while the user completes browser authentication. Ask the user to reply "Done" once they have successfully logged in before you proceed to Step 3.

**Step 3: Determine Credential Path**
Once the user replies "Done", run a terminal command to find the exact path to their `application_default_credentials.json` file. 
- On Mac/Linux: `echo ~/.config/gcloud/application_default_credentials.json`
- On Windows: `echo %APPDATA%\gcloud\application_default_credentials.json`
Save this path.

**Step 4: MCP Injection**
1. Read `~/.gemini/settings.json`.
2. Inject the `analytics-mcp` object into the `mcpServers` configuration using the following schema:
   - `command`: `"pipx"`
   - `args`: `["run", "--spec", "git+https://github.com/googleanalytics/google-analytics-mcp.git", "google-analytics-mcp"]`
   - `env`: Create an object containing `"GOOGLE_APPLICATION_CREDENTIALS": "<THE_PATH_FROM_STEP_3>"`
3. Save the file.
4. Inform the user: *"The Google Analytics MCP is configured! Please restart your IDE/CLI. Once restarted, run `@miniops-label-explorer` to analyze your GA4 data!"*
