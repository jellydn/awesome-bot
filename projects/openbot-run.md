# OpenBot.run — local-first AI teammates

- Website: https://openbot.run/
- Source: https://github.com/wunitb/openbot.run
- Reviewed: 2026-10-27
- **Distinct from** CopilotKit OpenBot (https://www.copilotkit.ai/openbot) and meetopenbot/openbot.

## Overview
Desktop workspace for persistent AI teammates with independent workspaces, queues, agent-to-agent collaboration, shared files, and embedded browser control. Supports Codex, Claude Code, Gemini, Grok, OpenCode, Cursor, Cline, ACP agents, OpenAI-compatible endpoints, Ollama, and LM Studio.

## Cost and license
- Desktop app advertised at $0, using existing provider plans or API keys.
- Source-available under **PolyForm Noncommercial 1.0.0**; commercial use requires a separate license. Versions through 0.1.11 were Apache-2.0.
- Managed team hosting is a separate offering; pricing must be confirmed.

## Integration readiness
| Requirement | Status |
| --- | --- |
| Slack | Not verified as native connector |
| Gmail | Not verified as native connector |
| GitHub | Agent CLI/browser access possible; native scoped connector not verified |
| Sentry | Not verified; evaluate API/MCP approach |
| Scheduled autonomous tasks | Persistent queue exists; unattended scheduling needs testing |
| Mobile | iOS beta / Android companion advertised |
| Self-host | Desktop local-first; server deployment needs validation |

## Security warning
The upstream README describes development-preview agents running with `danger-full-access` and `approvalPolicy: never`. They can modify files, run commands, access the network, and control the browser without per-action confirmation. Test with disposable credentials and sandboxed directories only. Do not connect production secrets before auditing the permission model.

## Evaluation tasks
1. Validate connector capabilities and OAuth scopes for all four required services.
2. Test unattended 24/7 operation on Linux or Mac and mobile access.
3. Test model fallback and cost using an existing Codex subscription.
4. Compare against MausBot and OpenHuman on the Sentry → GitHub → Slack → Gmail scenario.
5. Document exact license restrictions for personal vs company use.

Sources:
- https://openbot.run/
- https://github.com/wunitb/openbot.run
