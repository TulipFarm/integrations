# GitHub MCP Setup Guide

This guide walks you through obtaining GitHub credentials to connect TulipFarm to your account.

## Option A — OAuth (Recommended)

OAuth lets TulipFarm request authorization directly from GitHub. You register an OAuth App once, then click **Connect with OAuth** in the TulipFarm UI.

### 1. Create a GitHub OAuth App

1. Go to [GitHub Settings → Developer settings → OAuth Apps](https://github.com/settings/developers).
2. Click **New OAuth App**.
3. Fill in:
   - **Application name**: `TulipFarm` (or any name)
   - **Homepage URL**: your TulipFarm instance URL (e.g. `http://localhost:8080`)
   - **Authorization callback URL**:
     ```
     http://localhost:8080/api/v1/integrations/github/oauth/callback
     ```
     Replace `localhost:8080` with your TulipFarm instance URL if deployed.
4. Click **Register application**.

### 2. Copy Client ID and Secret

1. On the app page, copy the **Client ID**.
2. Click **Generate a new client secret** and copy it.

### 3. Connect via TulipFarm

1. On the GitHub integration page, fill in:
   - **OAuth Client ID** — the Client ID from step 2
   - **OAuth Client Secret** — the Client Secret from step 2
2. Click **Connect with OAuth**.
3. GitHub will ask you to authorize the app. After approval, TulipFarm stores the access token automatically.

---

## Option B — Personal Access Token

If you prefer to paste a token directly:

### 1. Generate a classic PAT

1. Go to [GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)](https://github.com/settings/tokens/new).
2. Give it a descriptive note (e.g. `TulipFarm`).
3. Select these scopes:
   - `repo` (full repository access)
   - `read:org` (read org membership)
   - `read:user` (read user profile)
4. Click **Generate token** and copy the value immediately.

### 2. Connect via TulipFarm

On the GitHub integration page, paste the token into **Personal Access Token** and click **Connect**.

---

## Scopes used

TulipFarm requests the following OAuth scopes:

| Scope | Purpose |
|---|---|
| `repo` | Read/write repositories, issues, PRs, code |
| `read:org` | Read organization membership |
| `read:user` | Read user profile |
