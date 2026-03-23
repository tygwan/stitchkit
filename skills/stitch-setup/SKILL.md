---
name: stitch-setup
description: Set up Google Stitch, Figma, and NanoBanana MCP servers for Claude Code
argument-hint: [stitch|figma|nanobanana|all]
allowed-tools: [Read, Write, Edit, Bash, Glob, Grep]
---

# Stitch, Figma & NanoBanana MCP Setup

Help the user install and configure Google Stitch, Figma, and NanoBanana MCP servers for Claude Code.

## Usage

- `/stitch-setup` or `/stitch-setup all` — Set up all three MCP servers (Stitch, Figma, NanoBanana)
- `/stitch-setup stitch` — Set up Stitch MCP only
- `/stitch-setup figma` — Set up Figma MCP only
- `/stitch-setup nanobanana` — Set up NanoBanana MCP only

## Important: API Key Security

> **WARNING**: API keys and access tokens are sensitive credentials. Follow these rules:
> - NEVER commit API keys to git. Add `.env` to `.gitignore`.
> - NEVER hardcode tokens directly in `.mcp.json` if the project is public.
> - Use environment variables or `.env` files for token storage.
> - Rotate tokens immediately if accidentally exposed.
> - Use the minimum required permissions when generating tokens.

## Setup Process

### Google Stitch MCP

Stitch uses Google Cloud API key authentication via the official Google Stitch MCP server (HTTP type).

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
    "type": "http",
    "url": "https://stitch.googleapis.com/mcp",
    "headers": {
      "X-Goog-Api-Key": "your-api-key-here"
    }
  }
}
```

Or using environment variable:

```json
{
  "stitch": {
    "type": "http",
    "url": "https://stitch.googleapis.com/mcp",
    "headers": {
      "X-Goog-Api-Key": "${GOOGLE_API_KEY}"
    }
  }
}
```

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

### NanoBanana MCP (Gemini Image Generation)

NanoBanana provides AI image generation, editing, and smart model selection powered by Google's Gemini models.

#### Step 1: Get a Google AI API Key

1. Go to [Google AI Studio](https://aistudio.google.com/apikey)
2. Click **Create API key**
3. Select or create a Google Cloud project
4. Copy the generated API key

> This is a free-tier key. It works with Gemini 2.5 Flash, 3.0 Pro, and 3.1 Flash models.

#### Step 2: Configure NanoBanana MCP

**Option A: Using environment variable (recommended)**

Add to `.env` (make sure it's in `.gitignore`):
```
GOOGLE_AI_API_KEY=your-google-ai-api-key
```

Add to `.mcp.json`:
```json
{
  "nanobanana-mcp": {
    "command": "npx",
    "args": ["-y", "@ycse/nanobanana-mcp"],
    "env": {
      "GOOGLE_AI_API_KEY": "${GOOGLE_AI_API_KEY}"
    }
  }
}
```

**Option B: Using GEMINI_API_KEY (alternative env var name)**

NanoBanana also accepts `GEMINI_API_KEY` as an alternative:
```
GEMINI_API_KEY=your-google-ai-api-key
```

#### Step 3: Verify

Run `mcp__nanobanana-mcp__gemini_generate_image` with a simple prompt to confirm the connection.

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
- NanoBanana image generation — create icons, banners, illustrations with Gemini
- Combined workflow — generate UI layouts with Stitch, then matching assets with NanoBanana

### Quick connectivity check

After configuring all servers, verify each one works:

1. **Stitch**: `mcp__stitch__list_projects` — should return (possibly empty) project list
2. **Figma**: Ask to list Figma files — should authenticate and respond
3. **NanoBanana**: `mcp__nanobanana-mcp__gemini_generate_image` with a simple prompt — should return an image

If any server fails, check that the corresponding environment variable is set and the shell has been restarted.
