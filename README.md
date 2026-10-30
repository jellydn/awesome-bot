# Awesome Bot

Curated research on affordable, self-hostable autonomous AI assistants as alternatives to Grok Bot, prioritizing **Slack, Gmail, GitHub, and Sentry**.

> Research snapshot: 2026-10-30. Rakazo, OpenBot.run, and OpenMausBot were tried on macOS. Rakazo is not the next install. OpenBot.run runs on a ChatGPT plan and does not enforce per-action approval. OpenMausBot is the current trial. Sentry support is unverified for all candidates.

## Requirements

- Read Slack discussions, Gmail messages, GitHub repositories/PRs, and Sentry incidents.
- Run scheduled workflows and retain useful context.
- Support BYOK models and preferably self-hosted, always-on deployment.
- Enforce least-privilege OAuth, auditable tool calls, and approval before writes.

## Comparison

| Platform | Deployment | Slack | Gmail | GitHub | Sentry | Status |
| --- | --- | --- | --- | --- | --- | --- |
| [Grok Bot](https://docs.x.ai/grok-bot/overview) | Managed | App workflows | App/browser workflows | Repo workflows | Unverified | Paid reference baseline |
| [Rakazo](https://rakazo.com/) | Hosted from $10/month. Desktop v0.3.5 local install failed | Unverified | Unverified | Unverified | Unverified | Tested, not selected |
| [OpenMausBot / MausBot](https://github.com/milind-soni/OpenMausBot) | Desktop / VPS / Docker | Composio | Composio | Composio | Custom MCP; unverified | **Current trial** |
| [OpenBot](https://www.copilotkit.ai/openbot) ([source](https://github.com/CopilotKit/openbot)) | Self-hostable | Custom MCP/tools | Custom MCP/tools | Custom MCP/tools | Custom MCP/tools | Platform candidate |
| [OpenHuman](https://tinyhumans.ai/openhuman) | Local-first (verify daemon hosting) | Advertised | Advertised | Advertised | MCP path to validate | **POC candidate** |
| [OpenBot.run](https://openbot.run/) ([source](https://github.com/nightly-labs/openbot)) | Desktop local-first | Unverified | Unverified | Unverified | Unverified | Tried locally |

**Important:** An advertised integration is not proof of production-ready OAuth scopes, bidirectional actions, or unattended execution.

## Platform notes

### Grok Bot
Persistent cloud computer, automation, and approval workflows. Check whether an existing eligible Cursor subscription includes access before purchasing a separate Grok subscription. The target [Grok Bot template](https://x.ai/bot/6p_TaK4DPsDKksSIaIcId) has **not** been inspected or reproduced.

### Rakazo
Tried on macOS on 2026-10-27. Keep it in the comparison. Do not install it next.

The source repo is Apache-2.0 and active. The hosted onboarding at `app.rakazo.com/onboarding` offers **$10/month** to use your ChatGPT or Claude plan (7-day trial) or **Rakazo credits** from **$20/month**, where that amount goes to model usage. Both cards list bots, browser automation, scheduled routines, and web, desktop, and mobile.

**This computer** on desktop v0.3.5 fails. `ghcr.io/elie222/rakazo/app:v0.3.5` and `ghcr.io/elie222/rakazo/computer:v0.3.5` return 404. The tag `edge` exists. Filed as [elie222/rakazo#2414](https://github.com/elie222/rakazo/issues/2414).

Connecting Twitter returns Composio HTTP 400 `ToolRouterV2_NoManagedAuth` (code 4308): Composio does not manage auth for that toolkit, and the app shows the raw JSON. Filed as [elie222/rakazo#2415](https://github.com/elie222/rakazo/issues/2415). Slack, Gmail, GitHub, and Sentry were not connected in this trial.

### OpenMausBot / MausBot
Canonical upstream: https://github.com/milind-soni/OpenMausBot (formerly OpenGrokBot). Apache-2.0 open-source core; `enterprise/` has separate source-available terms. Supports Codex/Claude/Grok CLIs, self-hosted VPS deployment, schedules, webhooks, phone pairing, MCP and Composio for Slack, Gmail and GitHub. Sentry remains unverified; test custom MCP. Composio and optional Boat cloud computers may add costs.

Tried on macOS on 2026-10-30 at `openmausbot@0.1.134` (`7b9dd52c4`). Keep this app. The web UI and harness are acceptable so far, and Electron stays off. `just up` in the try checkout starts both, then opens the Cloudflare tunnel `bot`. The public name is https://bot.itman.fyi/. The tunnel forwards the browser `Host`. A remote browser pairs before it can call the API. Slack, Gmail, GitHub, and Sentry were not completed in this run. See [detailed review](projects/openmausbot.md).

### OpenBot
Platform candidate. This is not the current trial. MIT-licensed template you clone and run. An administrator supplies the model key. Tool calls go through a gateway that writes an audit row before the call. Expect to add Slack, Gmail, GitHub, and Sentry as MCP tools. Composio is also in its catalog, so do not use that path for Twitter. The local routine worker starts from `scripts/start.sh`. A one-container image does not fire routines unless a worker is added. Self-hosted memory needs CopilotKit Intelligence. The free Developer plan covers a local Mac preview.

### OpenHuman
Promising memory-oriented personal assistant with advertised Slack, Gmail, GitHub and MCP support. Verify headless/server operation, source license, and exact permissions.

### OpenBot.run
Distinct from CopilotKit OpenBot. Local desktop app that uses an existing Codex or Claude login, including a ChatGPT plan. Tried on macOS on 2026-10-27: Codex CLI 0.164.0 was signed in as ChatGPT (`prolite`) and the provider became available. Development requires Bun 1.4.2. The license is PolyForm Noncommercial 1.0.0. Agents run with `danger-full-access` and `approvalPolicy: never`, so this does not meet the approval rule. Slack, Gmail, GitHub, and Sentry were not connected. See [detailed review](projects/openbot-run.md).

## Proof of concept: Sentry → GitHub → Slack → Gmail

1. Retrieve a Sentry issue, stack trace, and relevant release.
2. Identify potentially related GitHub commits and PRs, with citations.
3. Search authorized Slack channels for related incident discussions.
4. Prepare a Gmail draft containing an incident summary and source links.
5. Require explicit approval before sending email or posting to Slack.

### Acceptance checklist

- [ ] Validate upstream source repository and license for each candidate.
- [ ] Confirm Sentry support via official API or maintained MCP server.
- [ ] Test least-privilege GitHub repository/PR read access.
- [ ] Test Slack channel history scopes and rate limits.
- [ ] Test Gmail read access and draft creation without send permission.
- [ ] Test scheduled unattended runs and restart recovery.
- [ ] Confirm encrypted secrets, audit logs, and per-tool approval policies.
- [ ] Measure success rate, latency, interventions, and monthly LLM/hosting costs over 10 representative tasks.

## Proposed architecture

```text
Web / mobile UI
      |
Agent orchestrator --- Model gateway (BYOK / local LLM)
      |
Tool permission gateway
  |       |       |       |
Slack   Gmail   GitHub  Sentry
      |
Audit logs + approval workflow + scheduler + persistent state
```

Start read-only. Do not permit email sending, Slack posting, GitHub modifications, or Sentry mutations without human confirmation.

## Primary sources

- [Grok Bot documentation](https://docs.x.ai/grok-bot/overview)
- [Rakazo](https://rakazo.com/)
- [OpenMausBot](https://github.com/milind-soni/OpenMausBot)
- [OpenBot](https://www.copilotkit.ai/openbot)
- [OpenBot GitHub](https://github.com/CopilotKit/openbot)
- [OpenBot.run](https://openbot.run/)
- [OpenBot.run GitHub](https://github.com/nightly-labs/openbot)
- [OpenHuman](https://tinyhumans.ai/openhuman)

Contributions welcome: please provide primary-source evidence, version/date checked, and distinguish advertised from tested features.
