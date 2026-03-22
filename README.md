<p align="center">
  <img src="assets/icon.png" alt="stitchkit" width="120" height="120">
</p>

<h1 align="center">stitchkit</h1>

<p align="center">
  <strong>The ultimate Stitch toolkit for Claude Code</strong><br>
  Generate, sync, and export UI designs seamlessly with Google Stitch and Figma.
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/stitchkit"><img src="https://img.shields.io/npm/v/stitchkit?style=flat-square" alt="npm version"></a>
  <a href="https://github.com/tygwan/stitchkit/blob/main/LICENSE"><img src="https://img.shields.io/github/license/tygwan/stitchkit?style=flat-square" alt="license"></a>
  <a href="https://github.com/tygwan/stitchkit"><img src="https://img.shields.io/github/stars/tygwan/stitchkit?style=social" alt="stars"></a>
</p>

<p align="center">
  <img src="assets/banner.png" alt="stitchkit banner" width="800">
</p>

---

## What is stitchkit?

**stitchkit** is an MCP (Model Context Protocol) server that brings [Google Stitch](https://stitch.withgoogle.com) and [Figma](https://figma.com) directly into your Claude Code workflow. Design, iterate, and export — all from your terminal.

### Features

- **AI Design Generation** — Generate high-fidelity UI screens from text prompts via Google Stitch, powered by Gemini
- **Figma Sync** — Push Stitch designs to Figma or pull components from your Figma projects
- **Design-to-Code** — Export production-ready HTML/CSS from any Stitch screen
- **Variant Explorer** — Generate design variants with different layouts, colors, and styles
- **Project Management** — Create, organize, and manage Stitch projects programmatically

## Quick Start

### Prerequisites

- [Claude Code](https://claude.com/claude-code) CLI installed
- Google account with [Stitch](https://stitch.withgoogle.com) access
- (Optional) Figma account for sync features

### Installation

```bash
npm install -g stitchkit
```

### MCP Configuration

Add stitchkit to your Claude Code MCP settings:

```json
{
  "mcpServers": {
    "stitchkit": {
      "command": "npx",
      "args": ["-y", "stitchkit"]
    }
  }
}
```

### Usage

Once configured, stitchkit tools are available directly in Claude Code:

```
> Generate a mobile login screen with social auth buttons and biometric login

> Push the latest Stitch designs to my Figma project

> Export the dashboard screen as HTML/CSS

> Create 3 color variants of the landing page
```

## Available Tools

| Tool | Description |
|---|---|
| `create_project` | Create a new Stitch design project |
| `generate_screen` | Generate a UI screen from a text prompt |
| `edit_screen` | Edit existing screens with natural language |
| `generate_variants` | Create design variants (layout, color, font, etc.) |
| `list_projects` | List all your Stitch projects |
| `list_screens` | List screens within a project |
| `get_screen` | Get screen details including HTML and screenshots |
| `export_html` | Export screen as production-ready HTML/CSS |
| `figma_push` | Push Stitch designs to a Figma file |
| `figma_pull` | Pull components from Figma into Stitch |

## Examples

### Generate a Dashboard

```
> Create a SaaS analytics dashboard with KPI cards,
  revenue chart, and user segments donut chart
```

### Design-to-Code Pipeline

```
> Generate a pricing page with 3 tiers, then export as HTML
```

### Figma Integration

```
> Pull the design system from my Figma file and apply it
  to the Stitch project
```

## Architecture

```
Claude Code ──► stitchkit MCP Server
                    ├── Google Stitch API
                    │   ├── Screen Generation
                    │   ├── Screen Editing
                    │   ├── Variant Generation
                    │   └── HTML/Screenshot Export
                    └── Figma API
                        ├── File Sync
                        ├── Component Import
                        └── Design Token Export
```

## Development

```bash
git clone https://github.com/tygwan/stitchkit.git
cd stitchkit
npm install
npm run build
npm run dev
```

## Related Projects

- [awesome-stitch-design](https://github.com/tygwan/awesome-stitch-design) — Curated list of Stitch design prompts and resources
- [stitch-sdk](https://github.com/google-labs-code/stitch-sdk) — Official Google Stitch SDK
- [stitch-mcp](https://github.com/davideast/stitch-mcp) — Community Stitch MCP server

## Contributing

Contributions are welcome! Please read the [contributing guidelines](CONTRIBUTING.md) before submitting a PR.

## License

[MIT](LICENSE) — Made with Stitch by [@tygwan](https://github.com/tygwan)
