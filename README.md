# Awesome Bot

Curated research on affordable, self-hostable autonomous AI assistants as alternatives to Grok Bot, prioritizing **Slack, Gmail, GitHub, and Sentry**.

> Research snapshot: 2026-10-27. Features described as advertised are **not hands-on verified**. In particular, Sentry support is unverified for all candidates.

## Requirements

- Read Slack discussions, Gmail messages, GitHub repositories/PRs, and Sentry incidents.
- Run scheduled workflows and retain useful context.
- Support BYOK models and preferably self-hosted, always-on deployment.
- Enforce least-privilege OAuth, auditable tool calls, and approval before writes.

## Comparison

| Platform | Deployment | Slack | Gmail | GitHub | Sentry | Status |
| --- | --- | --- | --- | --- | --- | --- |
| [Grok Bot](https://docs.x.ai/grok-bot/overview) | Managed | App workflows | App/browser workflows | Repo workflows | Unverified | Paid reference baseline |
| [Rakazo](https://rakazo.com/) | Self-host claims require validation | Unverified | Unverified | Unverified | Unverified | Investigate |
| [MausBot](https://mausbot.com/pricing) | Desktop / optional cloud (verify) | Advertised | Advertised | Advertised | Unverified | **POC candidate** |
| [OpenBot](https://www.copilotkit.ai/openbot) ([source](https://github.com/CopilotKit/openbot)) | Self-hostable | Custom MCP/tools | Custom MCP/tools | Custom MCP/tools | Custom MCP/tools | Platform candidate |
| [OpenHuman](https://tinyhumans.ai/openhuman) | Local-first (verify daemon hosting) | Advertised | Advertised | Advertised | MCP path to validate | **POC candidate** |

**Important:** An advertised integration is not proof of production-ready OAuth scopes, bidirectional actions, or unattended execution.

## Platform notes

### Grok Bot
Persistent cloud computer, automation, and approval workflows. Check whether an existing eligible Cursor subscription includes access before purchasing a separate Grok subscription. The target [Grok Bot template](https://x.ai/bot/6p_TaK4DPsDKksSIaIcId) has **not** been inspected or reproduced.

### Rakazo
Verify upstream repository, license, deployment instructions, active maintenance, and each connector before making capability claims.

### MausBot
Promising candidate for advertised Slack, Gmail and GitHub integrations. Verify whether Slack integration means reading Slack data or operating as a Slack bot, and whether third-party connector costs apply. Confirm current checkout pricing.

### OpenBot
Customizable agent platform with MCP-oriented tools and self-hosted architecture. Expect integration engineering; do not assume native connectors for the four required services.

### OpenHuman
Promising memory-oriented personal assistant with advertised Slack, Gmail, GitHub and MCP support. Verify headless/server operation, source license, and exact permissions.

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
- [MausBot pricing](https://mausbot.com/pricing)
- [OpenBot](https://www.copilotkit.ai/openbot)
- [OpenBot GitHub](https://github.com/CopilotKit/openbot)
- [OpenHuman](https://tinyhumans.ai/openhuman)

Contributions welcome: please provide primary-source evidence, version/date checked, and distinguish advertised from tested features.
