# OpenMausBot (MausBot, formerly OpenGrokBot)

- Canonical repository: https://github.com/milind-soni/OpenMausBot
- Website: https://www.openmausbot.com/
- Reviewed: 2026-10-29
- Repository: ~6.7k stars at review time
- License: Apache-2.0 for open-source core; enterprise/ has separate source-available license
- Status: **Top POC candidate** for replacing paid Grok Bot

## Architecture
Electron/React/Vite frontend, local harness server, Codex/Claude/Grok CLI drivers, SSE events, permission broker, Composio connected apps, optional Boat cloud computer. Local state in `~/.openmausbot`.

## Required integrations
| Integration | Evidence | Validation |
| --- | --- | --- |
| Slack | Advertised via Composio Sessions | Test scopes, search, read-only access |
| Gmail | Advertised via Composio Sessions | Test read, draft, avoid send |
| GitHub | Advertised via Composio Sessions | Test repository and PR scopes |
| Sentry | No native connector confirmed | Test Sentry API or custom MCP |

## Deployment and cost
- macOS Apple Silicon/Intel, Windows x64, Ubuntu 24.04 x64 beta.
- CLI and documented VPS / Docker hosting; mobile pairing via HTTPS or Tailscale.
- Reuses existing Codex or Claude CLI login where supported; verify provider terms and usage limits.
- Optional Composio, Boat cloud computers, and paid managed services may cost extra.
- Schedules, webhook triggers, and app-event triggers are documented; event limits and hosting requirements depend on configuration.

## Security
- Approval cards for risky actions; verify effective tool policy per driver.
- API keys are stored in local config; upstream documentation states they are plaintext with Unix file mode 0600. Encrypt volumes and restrict host access.
- Use a dedicated Composio project, minimum OAuth scopes, and read-only integrations initially.
- Check license of enterprise/ separately.

## POC
1. Run on Mac with existing Codex CLI.
2. Connect Slack, Gmail and GitHub via scoped Composio OAuth.
3. Add read-only Sentry MCP and fetch an issue.
4. Correlate Sentry issue → GitHub PR → Slack discussion → Gmail draft.
5. Run scheduled summary unattended, record errors and monthly cost.
6. Repeat on VPS using documented deployment and mobile pairing.

Primary source: https://github.com/milind-soni/OpenMausBot
