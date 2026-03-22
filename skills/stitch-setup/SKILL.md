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

## Setup Process

### Google Stitch MCP

1. Check if Stitch MCP is already configured in the user's settings
2. Add the Stitch MCP server configuration:

```json
{
  "stitch": {
    "command": "npx",
    "args": ["-y", "@anthropic/mcp-stitch@latest"]
  }
}
```

3. The user will need to authenticate with their Google account on first use
4. Verify the connection by running `mcp__stitch__list_projects`

### Figma MCP

1. Ask the user for their Figma personal access token
   - Guide: Go to Figma → Settings → Personal access tokens → Generate
2. Add the Figma MCP server configuration:

```json
{
  "figma": {
    "command": "npx",
    "args": ["-y", "@anthropic/mcp-figma@latest"],
    "env": {
      "FIGMA_ACCESS_TOKEN": "<user-token>"
    }
  }
}
```

3. Verify the connection

### Configuration location

- **Project-level** (recommended): `.mcp.json` in project root
- **User-level**: `~/.claude/settings.json` under `mcpServers`

## Post-setup

After setup, inform the user about available stitchkit skills:
- Design generation via Stitch (`stitch-design` skill)
- Prompt templates for 22+ page types
- Design-to-code export workflows
