## Casper Vision

Casper is the AI assistant built into GhostBSD.
It runs on your machine, in your channels, with your rules.

This document explains the current state and direction of the project.
Project overview: [`README.md`](README.md)
Contribution guide: [`CONTRIBUTING.md`](CONTRIBUTING.md)

The goal is simple: a personal assistant that ships with GhostBSD out of the box, requires no configuration to start using, respects your privacy, and actively helps you maintain your system.

Casper is a fork of [OpenClaw](https://github.com/openclaw/openclaw), adapted for the GhostBSD desktop experience and preconfigured to use Claude (Anthropic) as the default AI backend.

The current focus is:

Priority:

- Stability and secure defaults for GhostBSD users
- Automated bug reporting and issue triage
- MATE desktop integration
- FreeBSD rc.d service reliability

Next priorities:

- Automated fix proposals and pull request workflows
- Deeper integration with the GhostBSD package system (pkg)
- System health monitoring and proactive alerts
- Improved channel support (Telegram, Discord, IRC, Matrix)

Contribution rules:

- One PR = one issue/topic. Do not bundle multiple unrelated fixes/features.
- PRs over ~5,000 changed lines are reviewed only in exceptional circumstances.
- Do not open large batches of tiny PRs at once; each PR has review cost.
- For very small related fixes, grouping into one focused PR is encouraged.

## Security

Security is a deliberate tradeoff: strong defaults without killing capability.
The goal is to stay powerful for real work while making risky paths explicit and operator-controlled.

Canonical security policy and reporting:

- [`SECURITY.md`](SECURITY.md)

## Plugins and Skills

Casper inherits the OpenClaw plugin API.
Core stays lean; optional capability ships as plugins.

New skills should be published as standalone packages, not added to core by default.
Core skill additions should be rare and require a strong product or security reason.

## Why TypeScript?

Casper is primarily an orchestration system: prompts, tools, protocols, and integrations.
TypeScript keeps it hackable by default — widely known, fast to iterate in, and easy to read, modify, and extend.

## What We Will Not Merge (For Now)

- New core skills that can live as standalone packages
- Full-doc translation sets for all docs
- Commercial service integrations that do not fit the model-provider category
- Agent-hierarchy frameworks as a default architecture
- Heavy orchestration layers that duplicate existing agent and tool infrastructure

This list is a roadmap guardrail, not a law of physics.
Strong user demand and strong technical rationale can change it.
