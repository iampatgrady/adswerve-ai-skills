# Phase 0: System Prerequisites

Before using this Agentic Workspace, you must configure your local machine. You only need to do this once per computer.

### 1. Install Core CLI Tools
- **Google Antigravity IDE:** Download and install the latest version.
- **Gemini CLI:** Install via your terminal.
- **Google Cloud CLI (gcloud):** Install and run `gcloud auth application-default login` to grant local agents access to GCP.

### 2. Git & SSH Authentication
Ensure you have an SSH key generated (`~/.ssh/id_rsa` or `id_ed25519`) and added to your Bitbucket/GitHub account settings. This allows the AI to run `git push` without being prompted for passwords.

### 3. Create an Asana Developer App
1. Go to Asana Developer Console > Create New App.
2. Name it "Antigravity Local MCP".
3. **CRITICAL:** Add `http://localhost:3334/oauth/callback` as a Redirect URL.
4. Note your `Client ID` and `Client Secret`. You will give these to the AI during Phase 1 onboarding.
