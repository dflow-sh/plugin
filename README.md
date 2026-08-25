# dFlow plugin

Official plugin for [dFlow](https://dflow.sh) — an open-source PaaS for hosting apps, databases, and storage on infrastructure you already pay for.

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

Once listed on the [xAI plugin marketplace](https://github.com/xai-org/plugin-marketplace), install from Grok Build’s marketplace UI.

Until then, from a clone of this repo (or via GitHub):

```bash
grok plugin install dflow-sh/plugin --trust
```

Complete the OAuth prompt, then run a read-only tool such as listing applications before mutating anything.

## Layout

```text
.
├── mcp.json                 # Canonical MCP config (Cursor / Agent Plugins)
├── .grok-plugin/plugin.json # Grok / xAI manifest; points at ./mcp.json
├── README.md
└── LICENSE
```

One MCP payload, two discovery paths. xAI indexes `.grok-plugin/plugin.json` and follows `mcpServers` to `mcp.json`. Cursor can later auto-discover the same `mcp.json` without a second server file.

## Network and security

- **Endpoint:** `https://app.dflow.sh/api/mcp` only
- **Credentials:** OAuth; tokens stay in the MCP client
- **No** lifecycle hooks, stdio servers, `postinstall` scripts, or telemetry
- Service reads redact secrets; do not paste git/registry tool output into public places

## License

[MIT](./LICENSE)
