# Phase 0: System Prerequisites (CLI Edition)

### 1. Install Core CLI Tools
- **Gemini CLI:** Install via your terminal package manager (e.g., `npm install -g @google/gemini-cli`).
- **Google Cloud CLI (gcloud):** Install and run `gcloud auth application-default login`.

### 2. Git & SSH Authentication
You must configure SSH keys so the CLI agent can push code.
1. Run `ssh-keygen -t ed25519 -C "your.email@adswerve.com"`
2. Start the agent: `eval "$(ssh-agent -s)"`
3. Add the key: `ssh-add ~/.ssh/id_ed25519` (Mac users: add `UseKeychain yes` to your `~/.ssh/config` and run `ssh-add --apple-use-keychain ~/.ssh/id_ed25519`).
4. Copy your public key (`cat ~/.ssh/id_ed25519.pub`) and add it to Bitbucket/GitHub.

### 3. Create an Asana Developer App
1. Go to Asana Developer Console > Create New App.
2. Name it "Gemini CLI Local MCP".
3. **CRITICAL:** Add `http://localhost:3334/oauth/callback` as a Redirect URL.
4. Note your `Client ID` and `Client Secret`.
