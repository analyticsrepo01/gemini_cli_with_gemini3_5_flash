# 🚀 Gemini CLI (AGY) Setup & Configuration Guide
*A comprehensive guide to installing, authenticating, and running the Gemini CLI, including configurations for Gemini 3.5 Flash.*

---

## 📋 Prerequisites
Before setting up the Gemini CLI, make sure your local system or VM satisfies the minimum dependency requirements:
*   **Node.js:** `>= v20` (Recommended)
*   **Package Manager:** `npm` (typically bundled with Node.js)
*   **Google Cloud SDK (`gcloud`):** Needed if authenticating via Vertex AI / Application Default Credentials (ADC).

---

## 🛠️ Step 1: Global CLI Installation
You can install the latest stable version of the Gemini CLI globally via the NPM registry:

```bash
# Install the absolute latest stable version
sudo npm install -g @google/gemini-cli@latest

# Or pin to the validated 0.45.2 release
sudo npm install -g @google/gemini-cli@0.45.2
```

To verify the installation path and active version:
```bash
$ gemini --version
0.46.0
```

---

## 🚀 Antigravity CLI Installation

Work with Antigravity directly in your codebase. Build, debug, and ship from your terminal. Describe what you need, and Antigravity handles the rest.

### 🌐 Quick Install Commands

#### 🍎 macOS / 🐧 Linux
```bash
curl -fsSL https://antigravity.google/cli/install.sh | bash
```

#### 🪟 Windows PowerShell
```powershell
irm https://antigravity.google/cli/install.ps1 | iex
```

#### 🪟 Windows CMD
```cmd
curl -fsSL https://antigravity.google/cli/install.cmd -o install.cmd && install.cmd && del install.cmd
```

### 📦 Offline Packages (v1.0.9)
If your development environment is in a secure, air-gapped subnet without direct access to external installer endpoints, you can download the offline installer archives directly from this repository:
*   **Linux (x64):** [agy-linux-x64-v1.0.9.zip](agy-linux-x64-v1.0.9.zip) or [agy-linux-x64-v1.0.9.tar.gz](agy-linux-x64-v1.0.9.tar.gz)
*   **Windows (x64):** [agy-windows-x64-v1.0.9.zip](agy-windows-x64-v1.0.9.zip)

---

## 🔑 Step 2: Authentication Configurations
The Gemini CLI supports two primary authentication models. Choosing the correct one determines which API endpoints and models are available to you.

### 🔹 Option A: Vertex AI (Recommended for Enterprise / Previews)
This method authenticates requests via Google Cloud's Vertex AI platform using the active VM's Service Account or Application Default Credentials (ADC).

1. Ensure you are authenticated with your Google Cloud account:
   ```bash
   gcloud auth login
   gcloud auth application-default login
   ```
2. Set the `selectedAuthType` to `vertex-ai` inside your User Settings (`~/.gemini/settings.json`):
   ```json
   {
     "selectedAuthType": "vertex-ai",
     "theme": "Default"
   }
   ```

### 🔹 Option B: Personal OAuth Sign-In (Standard)
This method routes requests via standard developer API accounts (Gemini Code Assist plan).

1. Set `selectedAuthType` to `oauth-personal` in your `~/.gemini/settings.json` file:
   ```json
   {
     "selectedAuthType": "oauth-personal",
     "theme": "Default"
   }
   ```
2. When launching the CLI for the first time, it will prompt an interactive login link to authenticate via your web browser.

---

## 🌍 Step 3: Setting Up Gemini 3.5 Flash
Because preview models (like **Gemini 3.5 Flash**) may not be deployed in all standard regional endpoints, you must route your API calls through the `global` Vertex location to avoid `ModelNotFoundError` exceptions.

### 1. Configure the Environment Variables
Export these variables in your active terminal tab to route requests to the global Vertex endpoint:

```bash
export GOOGLE_GENAI_USE_VERTEXAI=true
export GOOGLE_CLOUD_LOCATION=global
export GEMINI_MODEL=gemini-3.5-flash
```

### 2. Persist Configurations Permanently
To make this setup permanent for every new terminal session, append these variables to your terminal profile:

```bash
# Save to bash profile
echo 'export GOOGLE_GENAI_USE_VERTEXAI=true' >> ~/.bashrc
echo 'export GOOGLE_CLOUD_LOCATION=global' >> ~/.bashrc
echo 'export GEMINI_MODEL=gemini-3.5-flash' >> ~/.bashrc

# Reload profile to apply changes
source ~/.bashrc
```

---

## 🚀 Step 4: Running the CLI

Now you are ready to use the CLI in several ways:

### 1. Interactive Chat
Start an interactive pair programming session:
```bash
gemini
```

### 2. Single-Turn Command (Headless)
Send a command and get output instantly in stdout:
```bash
gemini -p "List files in the current repository and explain what they are"
```

### 3. Target Specific Models (Override)
Force a specific model routing:
```bash
gemini -m gemini-3.5-flash -p "Review this workspace"
```

---

## 🔍 Troubleshooting FAQ

> [!WARNING]
> **Problem:** `ModelNotFoundError: Publisher Model ... was not found or your project does not have access to it.`
> *   **Cause:** Your active location is pointing to a region (like `us-central1`) where the preview model is not yet registered.
> *   **Fix:** Ensure `GOOGLE_CLOUD_LOCATION` is set to `global`.

> [!CAUTION]
> **Problem:** `Manual authorization is required but the current session is non-interactive.`
> *   **Cause:** The CLI's auth provider is set to `oauth-personal`, which is trying to trigger a browser-based login popup during a non-interactive shell command.
> *   **Fix:** Ensure your `~/.gemini/settings.json` file specifies `"selectedAuthType": "vertex-ai"` to use Application Default Credentials.
