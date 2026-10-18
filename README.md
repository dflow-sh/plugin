# dFlow plugin

Official plugin for [dFlow](https://dflow.sh). Deploy your apps and databases in one place. The agent uses the same tools as the dFlow dashboard.

This repository is the marketplace plugin for the hosted **dFlow MCP server**. Installing it connects the agent and installs the `dflow-deploy` and `dflow-mcp` skills. The same skills also live in [dflow-sh/skills](https://github.com/dflow-sh/skills) for agents that install skills without this plugin.

This is **dflow.sh**, not the unrelated Solana project of a similar name.

## What it does

After install, the agent connects to dFlow Cloud over MCP and can:

- Create applications, environments, and services
- Attach databases, manage env vars, and configure domains
- Trigger deploys, read logs, and inspect status
- Work with templates, git providers, backups, and worker nodes

Actions use the **same organisation and role permissions** as the dFlow dashboard.

## MCP server

| | |
| --- | --- |
| URL | `https://app.dflow.sh/api/mcp` |
| Transport | HTTP (streamable) |
| Auth | OAuth 2.0 only — no API keys |
| Scope | One organisation per sign-in |

On first connection the client opens a browser for you to sign in and approve access. dFlow does not accept pasted secrets or a hand-edited `Authorization` header.

Self-hosted dashboards expose the same path (`/api/mcp`) on your own origin. This plugin points at Cloud; point a local MCP config at your dashboard if you self-host.

Product overview: [dFlow AI workflow](https://dflow.sh/ai-workflow) · docs: [dflow.sh/docs](https://dflow.sh/docs)

## Install

### Cursor

Once listed on the [Cursor Marketplace](https://cursor.com/marketplace), install from **Cursor Settings → Plugins**. To publish, submit `https://github.com/dflow-sh/plugin` at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).

Until then you can load this repo as a local Cursor plugin (Settings → Plugins, or a local plugin path pointing at this clone). Complete the OAuth prompt on first MCP connect.

### Claude Code

Claude Code reads `.mcp.json` and `.claude-plugin/plugin.json` from this repo. On first connect, approve OAuth in the browser.

### Gemini

Gemini finds this repo when it has the GitHub topic `gemini-cli-extension`. Install it with:

```bash
gemini extensions install dflow-sh/plugin
```

The server URL is in `gemini-extension.json`. Approve OAuth in the browser on first connect.

### Grok / xAI

Once listed on the [xAI plugin marketplace](https://github.com/xai-org/plugin-marketplace), install from Grok Build’s marketplace UI.

Until then:

```bash
grok plugin install dflow-sh/plugin --trust
```

Complete the OAuth prompt, then run a read-only tool such as listing applications before mutating anything.

## Layout

```text
.
├── mcp.json                      # Cursor MCP config
├── .mcp.json                     # Claude Code MCP config
├── .cursor-plugin/plugin.json    # Cursor manifest
├── .grok-plugin/plugin.json      # Grok manifest; points at ./mcp.json
├── .claude-plugin/plugin.json    # Claude Code manifest
├── gemini-extension.json         # Gemini extension manifest
├── skills/dflow-deploy/SKILL.md
├── skills/dflow-mcp/SKILL.md
├── assets/logo.svg
├── README.md
└── LICENSE
```

One server URL. Each product only gets the manifest folder it looks for. This repo does not ship agents, commands, or hooks.

## Network and security

- **Endpoint:** `https://app.dflow.sh/api/mcp` only
- **Credentials:** OAuth; tokens stay in the MCP client
- **No** lifecycle hooks, stdio servers, `postinstall` scripts, or telemetry
- Service reads redact secrets; do not paste git/registry tool output into public places

## License

[MIT](./LICENSE)
