# Slack MCP Setup Guide

This guide walks you through creating a Slack app and obtaining the credentials needed to connect TulipFarm to your workspace.

## Option A — OAuth (Recommended)

OAuth lets TulipFarm request authorization directly from Slack. You register an app once, then click **Connect with OAuth** in the TulipFarm UI.

### 1. Create a Slack app

1. Go to [api.slack.com/apps](https://api.slack.com/apps) and click **Create New App**.
2. Choose **From scratch**, give it a name (e.g. `TulipFarm`), and select your workspace.

### 2. Add OAuth redirect URI

1. In your app settings, go to **OAuth & Permissions**.
2. Under **Redirect URLs**, click **Add New Redirect URL** and enter:
   ```
   http://localhost:8080/api/v1/integrations/slack/oauth/callback
   ```
   Replace `localhost:8080` with your TulipFarm instance URL if deployed.
3. Click **Save URLs**.

### 3. Copy Client ID and Secret

1. Go to **Basic Information → App Credentials**.
2. Copy the **Client ID** and **Client Secret**.

### 4. Connect via TulipFarm

1. On the Slack integration page, fill in:
   - **OAuth Client ID** — the Client ID from step 3
   - **OAuth Client Secret** — the Client Secret from step 3
   - **Team ID** — your workspace ID (see below)
2. Click **Connect with OAuth**.
3. Slack will ask you to authorize the app. After approval, TulipFarm stores the bot token automatically.

---

## Option B — Direct Token

If you prefer to paste a token directly:

### 1. Create a Slack app and install it

1. Go to [api.slack.com/apps](https://api.slack.com/apps) → **Create New App → From scratch**.
2. Go to **OAuth & Permissions → Scopes → Bot Token Scopes** and add:
   - `channels:history`, `channels:read`, `chat:write`
   - `groups:history`, `groups:read`
   - `im:history`, `im:read`, `mpim:history`, `mpim:read`
   - `reactions:read`, `reactions:write`
   - `users:read`, `users:read.email`
3. Click **Install to workspace** and approve.
4. Copy the **Bot User OAuth Token** (starts with `xoxb-`).

### 2. Get your Team ID

Your Team ID starts with `T` (e.g. `T012AB3CD`). To find it:

- Open Slack in a browser — the URL contains your workspace ID.
- Or run: `curl -s -H "Authorization: Bearer xoxb-YOUR-TOKEN" https://slack.com/api/auth.test | jq .team_id`

### 3. Connect via TulipFarm

On the Slack integration page, fill in **Bot Token** and **Team ID**, then click **Connect**.
