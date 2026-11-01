# OpenHuman

- Canonical repository: https://github.com/tinyhumansai/openhuman
- Website: https://tinyhumans.ai/openhuman
- Plans: https://tinyhumans.ai account plans page, signed in on 2026-11-01
- Reviewed: 2026-10-31. Hands-on on macOS through 2026-11-01.
- License: GPL-3.0-only
- Status: **Tried**. Drop the TinyHumans Free plan.

## Cost
The TinyHumans **Free** card says **No active subscription**. It is not a usable plan.

On 2026-11-01 the signed-in plans page listed:

| Plan | Price | Memory storage | Credits |
| --- | --- | --- | --- |
| Free | No active subscription | None. `memory/scopes` returns `MEMORY_PLAN_REQUIRED` (HTTP 403) | None on the plans card |
| Basic | $20/month | 1 GB | $21 each month |
| Pro | $200/month | 20 GB | $210 each month |
| Enterprise | Custom | Custom | Custom |

"Free memory inference" on the Basic and Pro cards is inference inside that paid plan. The page says subscription payments and credit purchases are non-refundable, including unused credits. Cancel stops later charges. Access runs to the end of the period already paid.

## Required integrations
| Integration | Evidence | Validation |
| --- | --- | --- |
| Slack | Advertised | Not connected in this run |
| Gmail | Advertised | Not connected in this run |
| GitHub | Advertised | Not connected in this run |
| Sentry | MCP path | Not connected in this run |

## Hands-on
Cloned with `try` on macOS on 2026-10-31 into `2026-10-31-tinyhumansai-openhuman`. `main` at `218d35f4b9` did not compile against its TinyChannels pin. The release tag `v0.64.20` (`3e0c980be8`) did. The Tauri app stayed off.

- The headless core listens on `127.0.0.1:7788`. The Vite UI listens on `1420`.
- A local `justfile` provides `just up`. The public name is https://human.itman.fyi/ through the Cloudflare tunnel `human`. `bot.itman.fyi` stays on OpenMausBot.
- Sign-in from the public hostname ends on the TinyHumans page that asks for the desktop app. Sign-in from `http://127.0.0.1:1420/` returns to the browser.
- Memory settings call `api.tinyhumans.ai` `memory/scopes` and show `MEMORY_PLAN_REQUIRED` on the Free account.
- TinyComputer `v0.15.2` reported the browser available and the desktop unavailable until macOS Accessibility is granted to the core binary.

Slack, Gmail, GitHub, and Sentry were not completed in this run.

Primary source: https://github.com/tinyhumansai/openhuman
