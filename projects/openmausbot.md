# OpenMausBot (MausBot, formerly OpenGrokBot)

- Canonical repository: https://github.com/milind-soni/OpenMausBot
- Website: https://www.openmausbot.com/
- Reviewed: 2026-10-29. Hands-on on macOS on 2026-10-30.
- Repository: ~6.7k stars at review time
- License: Apache-2.0 for open-source core; enterprise/ has separate source-available license
- Status: **Current trial**. The local web app is acceptable so far.

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

## Hands-on
Cloned with `try` on macOS on 2026-10-30 into `2026-10-30-milind-soni-OpenMausBot`. The checkout was `openmausbot@0.1.134` at `7b9dd52c4`. Electron stays off.

- `pnpm dev:server` runs the harness on `http://127.0.0.1:8799`. `pnpm dev` runs the Vite UI on `http://127.0.0.1:5199`.
- A local `justfile` in that checkout provides `just up`. `just serve` starts the harness and the UI when those ports are down. `just tunnel` then runs `cloudflared tunnel run --url http://127.0.0.1:5199 bot`. The public name is https://bot.itman.fyi/.
- `just serve` sets `OMB_PUBLIC_URL=https://bot.itman.fyi` for a harness it starts. The harness reads that value at start, and pairing links then use the public name.
- Vite allows `bot.itman.fyi` through `__VITE_ADDITIONAL_SERVER_ALLOWED_HOSTS`. Other host names stay blocked.
- The tunnel forwards the browser `Host` and `X-Forwarded-Proto`. Do not pass `--http-host-header 127.0.0.1:5199`. That rewrite makes `Origin: https://bot.itman.fyi` fail the same-origin check with `forbidden: cross-origin request`.
- A browser on `http://127.0.0.1:5199` is the owner while `OMB_DEV_TRUST_LOCAL_BROWSER=1`. A browser on https://bot.itman.fyi/ must pair. Create the code on the local page under Settings → Remote access, then enter it on the public page. A request from another site stays forbidden.
- The server warned that agent-browser 0.41.0 is not the pinned 0.37.0. Browser tools can stall until 0.37.0 is installed.

Slack, Gmail, GitHub, and Sentry were not completed in this run.

## POC
1. Run on Mac with existing Codex CLI.
2. Connect Slack, Gmail and GitHub via scoped Composio OAuth.
3. Add read-only Sentry MCP and fetch an issue.
4. Correlate Sentry issue → GitHub PR → Slack discussion → Gmail draft.
5. Run scheduled summary unattended, record errors and monthly cost.
6. Repeat on VPS using documented deployment and mobile pairing.

Primary source: https://github.com/milind-soni/OpenMausBot
