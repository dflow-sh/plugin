# dFlow plugin

Official plugin for [dFlow](https://dflow.sh). It connects an agent to dFlow Cloud so it can host apps, databases, and services.

This repository is a thin marketplace wrapper around the hosted **dFlow MCP server**. It does not ship skills, slash commands, hooks, or local binaries.

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
├── mcp.json                      # Canonical MCP config (shared)
├── .cursor-plugin/plugin.json    # Cursor marketplace manifest
├── .grok-plugin/plugin.json      # Grok / xAI manifest; points at ./mcp.json
├── assets/logo.svg
├── README.md
└── LICENSE
```

One MCP payload. Cursor auto-discovers `mcp.json`. xAI indexes `.grok-plugin/plugin.json` and follows `mcpServers` to the same file.

## Network and security

- **Endpoint:** `https://app.dflow.sh/api/mcp` only
- **Credentials:** OAuth; tokens stay in the MCP client
- **No** lifecycle hooks, stdio servers, `postinstall` scripts, or telemetry
- Service reads redact secrets; do not paste git/registry tool output into public places

## License

[MIT](./LICENSE)
