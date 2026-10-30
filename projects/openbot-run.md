# OpenBot.run — local-first AI teammates

- Website: https://openbot.run/
- Source: https://github.com/nightly-labs/openbot
- Reviewed: 2026-10-27, hands-on the same day. Upstream last pushed 2026-10-30.
- **Distinct from** CopilotKit OpenBot (https://www.copilotkit.ai/openbot) and meetopenbot/openbot.
- `wunitb/openbot.run` is a fork of this repo. Its last push was 2026-09-23. Clone `nightly-labs/openbot`.

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

## Hands-on
Cloned with `try` on macOS on 2026-10-27 and started the development app.

- `bun run dev` accepts only Bun 1.4.2. Bun 1.4.3 exits before the app starts. The package script calls `bun` from `PATH`, so the 1.4.2 binary must be first on `PATH`.
- `bun run codex:doctor` reported `auth.type` `chatgpt`, `planType` `prolite`, and Codex CLI 0.164.0. The app log then marked the Codex provider available.
- The Electron window started. The renderer logged a Solid `REACTIVITY_HALTED` error during startup. The process stayed up.
- `bun run dev:stop` exited 1 because the API process group had already lost its leader. The supervisor still received SIGTERM, and ports 3100, 3101, 3102, 5173, and 9333 closed.

Slack, Gmail, GitHub, and Sentry were not connected in this run.

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
- https://github.com/nightly-labs/openbot
