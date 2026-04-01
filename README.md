# Casper — GhostBSD AI Assistant

<p align="center">
  <a href="https://github.com/ghostbsd/casper/actions/workflows/ci.yml?branch=main"><img src="https://img.shields.io/github/actions/workflow/status/ghostbsd/casper/ci.yml?branch=main&style=for-the-badge" alt="CI status"></a>
  <a href="https://github.com/ghostbsd/casper/releases"><img src="https://img.shields.io/github/v/release/ghostbsd/casper?include_prereleases&style=for-the-badge" alt="GitHub release"></a>
  <a href="https://discord.gg/ghostbsd"><img src="https://img.shields.io/discord/1078728983068344330?label=Discord&logo=discord&logoColor=white&color=5865F2&style=for-the-badge" alt="Discord"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="MIT License"></a>
</p>

**Casper** is GhostBSD's built-in AI assistant — forked from [OpenClaw](https://github.com/openclaw/openclaw) and tailored for the GhostBSD desktop experience.

Casper ships **preinstalled and preconfigured** as part of GhostBSD 27.1. No setup required. It starts with the system and is ready to use immediately, powered by **Claude** (Anthropic) as the default AI backend.

Casper runs locally on your GhostBSD system, monitors your environment, and assists you through the channels you already use (Telegram, Discord, IRC, Matrix, Slack, Signal, WebChat). It can file bug reports on your behalf, attempt to fix issues it finds, and open pull requests with proposed fixes.

[GhostBSD](https://www.ghostbsd.org) · [Discord](https://discord.gg/ghostbsd)

## What Casper does

- **Automated bug reporting** — Casper monitors your system and files bug reports on your behalf when it detects issues, complete with logs, hardware details, and reproduction steps
- **Automated fixes** — When Casper identifies a known or diagnosable issue, it attempts to resolve it and opens a pull request with the proposed fix for human review
- **System awareness** — Casper has full context of your GhostBSD environment, installed packages, running services, and hardware configuration
- **MATE desktop integration** — Integrated directly into the MATE desktop with a persistent, always-available panel applet
- **Messaging app support** — Interact with Casper through Telegram, Discord, IRC, Matrix, and other platforms you already use
- **Local-first** — Casper runs entirely on your machine. Your data stays with you

## Getting started

Casper is preinstalled in GhostBSD 27.1 and starts automatically with the system. To start a conversation:

```bash
casper agent --message "What is the status of my system?"
```

Or interact through any connected messaging channel.

## AI backend

Casper is preconfigured to use **Claude** (Anthropic) as its AI backend. Claude is chosen for its strong reasoning capabilities, reliable code understanding, and low prompt-injection risk.

The default configuration is managed by GhostBSD at `/etc/casper/casper.json`. User overrides can be placed at `~/.casper/casper.json`.

## How it works

```
Telegram / Discord / IRC / Matrix / Slack / Signal / WebChat
               │
               ▼
┌───────────────────────────────┐
│            Gateway            │
│       (control plane)         │
│     ws://127.0.0.1:18789      │
└──────────────┬────────────────┘
               │
               ├─ Pi agent (RPC)
               ├─ CLI (casper …)
               ├─ WebChat UI
               └─ MATE panel applet
```

The Gateway runs as an rc.d service and stays running in the background. It is the single control plane for sessions, channels, tools, and events.

## Channels

Casper connects to the messaging platforms most used by the BSD community. Channels are configured in `~/.casper/casper.json`:

- **[Telegram](https://docs.openclaw.ai/channels/telegram)** — set `channels.telegram.botToken`
- **[Discord](https://docs.openclaw.ai/channels/discord)** — set `channels.discord.token`
- **[IRC](https://docs.openclaw.ai/channels/irc)** — configure `channels.irc` with server and nick
- **[Matrix](https://docs.openclaw.ai/channels/matrix)** — configure `channels.matrix` with homeserver and credentials
- **[Slack](https://docs.openclaw.ai/channels/slack)** — set `channels.slack.botToken` + `channels.slack.appToken`
- **[Signal](https://docs.openclaw.ai/channels/signal)** — requires `signal-cli` and a `channels.signal` config section
- **WebChat** — served directly from the Gateway at `http://localhost:18789`; no separate port

## Security defaults

Casper connects to real messaging surfaces. Treat inbound DMs as **untrusted input**.

Default behavior:

- **DM pairing** (`dmPolicy="pairing"`): unknown senders receive a short pairing code and Casper does not process their message until approved
- Approve with: `casper pairing approve <channel> <code>`
- Public inbound DMs require explicit opt-in: set `dmPolicy="open"` and include `"*"` in the channel allowlist

Run `casper doctor` to surface risky or misconfigured DM policies.

## Chat commands

Send these in Telegram / Discord / IRC / Matrix / WebChat:

- `/status` — compact session status (model + tokens)
- `/new` or `/reset` — reset the session
- `/compact` — compact session context (summary)
- `/think <level>` — off|minimal|low|medium|high|xhigh
- `/verbose on|off`
- `/usage off|tokens|full` — per-response usage footer
- `/restart` — restart the gateway (owner-only in groups)

## Agent workspace and skills

- Workspace root: `~/.casper/workspace` (configurable via `agents.defaults.workspace`)
- Injected prompt files: `AGENTS.md`, `SOUL.md`, `TOOLS.md`
- Skills: `~/.casper/workspace/skills/<skill>/SKILL.md`

## Security model

- **Default:** tools run on the host for the **main** session, so Casper has full access when it is just you
- **Group/channel safety:** set `agents.defaults.sandbox.mode: "non-main"` to run non-main sessions (groups/channels) in per-session sandboxes
- **Sandbox defaults:** allowlist `bash`, `process`, `read`, `write`, `edit`, `sessions_list`, `sessions_history`, `sessions_send`; denylist `browser`, `cron`, `gateway`

## From source (development)

```bash
git clone https://github.com/ghostbsd/casper.git
cd casper

pnpm install
pnpm build

# Dev loop (auto-reload on source/config changes)
pnpm gateway:watch
```

## License

MIT

---

Forked from [OpenClaw](https://github.com/openclaw/openclaw). GhostBSD-specific patches and integration maintained by the GhostBSD team.