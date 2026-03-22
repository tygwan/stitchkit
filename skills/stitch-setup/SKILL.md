---
name: stitch-setup
description: Set up Google Stitch and Figma MCP servers for Claude Code
argument-hint: [stitch|figma|all]
allowed-tools: [Read, Write, Edit, Bash, Glob, Grep]
---

# Stitch & Figma MCP Setup

Help the user install and configure Google Stitch and/or Figma MCP servers for Claude Code.

## Usage

- `/stitch-setup` or `/stitch-setup all` — Set up both Stitch and Figma MCP
- `/stitch-setup stitch` — Set up Stitch MCP only
- `/stitch-setup figma` — Set up Figma MCP only

## Important: API Key Security

> **WARNING**: API keys and access tokens are sensitive credentials. Follow these rules:
> - NEVER commit API keys to git. Add `.env` to `.gitignore`.
> - NEVER hardcode tokens directly in `.mcp.json` if the project is public.
> - Use environment variables or `.env` files for token storage.
> - Rotate tokens immediately if accidentally exposed.
> - Use the minimum required permissions when generating tokens.

## Setup Process

### Google Stitch MCP

Stitch uses Google Cloud API key authentication via the Nano Banana (nanobanana) gateway.

#### Step 1: Get a Google Cloud API Key

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create or select a project
3. Navigate to **APIs & Services → Credentials**
4. Click **+ CREATE CREDENTIALS → API key**
5. (Recommended) Click **Edit API key** to restrict it:
   - Application restrictions: Set by HTTP referrer or IP
   - API restrictions: Restrict to Stitch/Generative AI APIs only
6. Copy the API key

#### Step 2: Configure Stitch MCP

**Option A: Using environment variable (recommended for public repos)**

Create a `.env` file in your project root (make sure it's in `.gitignore`):
```
GOOGLE_API_KEY=your-api-key-here
```

Add to `.mcp.json`:
```json
{
  "stitch": {
    "command": "npx",
    "args": ["-y", "@anthropic/mcp-stitch@latest"],
    "env": {
      "GOOGLE_API_KEY": "${GOOGLE_API_KEY}"
    }
  }
}
```

**Option B: Using OAuth (interactive, no API key needed)**

```json
{
  "stitch": {
    "command": "npx",
    "args": ["-y", "@anthropic/mcp-stitch@latest"]
  }
}
```

The user will be prompted to authenticate with their Google account on first use.

#### Step 3: Verify

Run `mcp__stitch__list_projects` to confirm the connection.

---

### Figma MCP

#### Step 1: Get a Figma Personal Access Token

1. Go to [Figma](https://figma.com) → click your profile avatar (top-left)
2. **Settings → Security → Personal access tokens**
3. Click **Generate new token**
4. Give it a description (e.g., "Claude Code - stitchkit")
5. Select scopes:
   - `file:read` — Read access to files (required)
   - `file:write` — Write access (needed for push/sync features)
6. Copy the token immediately (it won't be shown again)

#### Step 2: Configure Figma MCP

**Option A: Using environment variable (recommended)**

Add to `.env`:
```
FIGMA_ACCESS_TOKEN=figd_your-token-here
```

Add to `.mcp.json`:
```json
{
  "figma": {
    "command": "npx",
    "args": ["-y", "@anthropic/mcp-figma@latest"],
    "env": {
      "FIGMA_ACCESS_TOKEN": "${FIGMA_ACCESS_TOKEN}"
    }
  }
}
```

**Option B: Direct (private repos only)**

```json
{
  "figma": {
    "command": "npx",
    "args": ["-y", "@anthropic/mcp-figma@latest"],
    "env": {
      "FIGMA_ACCESS_TOKEN": "figd_your-token-here"
    }
  }
}
```

> **WARNING**: Only use Option B in private repositories. Never commit tokens to public repos.

#### Step 3: Verify

Use Figma MCP tools to list files and confirm access.

---

### Configuration Location

| Location | File | Use case |
|---|---|---|
| **Project-level** (recommended) | `.mcp.json` in project root | Project-specific MCP servers |
| **User-level** | `~/.claude/settings.json` → `mcpServers` | Global MCP servers available everywhere |

### .gitignore Check

Always ensure these are in `.gitignore`:
```
.env
.env.local
.env.*.local
```

## Post-setup

After setup, inform the user about available stitchkit features:
- `stitch-design` skill — auto-triggers on design requests
- Prompt templates for 22+ page types in `references/prompt-templates.md`
- `design-explorer` agent — generates multiple design alternatives
- Design-to-code export via Stitch HTML export
