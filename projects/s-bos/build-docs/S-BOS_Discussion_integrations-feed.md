# Discussion: Integrations + Feed (connect your tools → Kompass reads them)

**Status:** Foundation built & Gmail proven live (2026-07-07). Remaining work is per-provider ingestion + activation, to be done step by step. This is the S-BOS analog of the marketingsecrets "Chief of Staff" pattern — connect external tools, ingest their signals into a Feed, and let Kompass reason over that Feed alongside operating data.

## 1. Architecture — 4 layers
```
CONNECT   user authorizes a tool (Gmail, Calendar, Drive, QuickBooks, Intacct…) via OAuth
INGEST    pull data on a schedule/webhook (proxy calls per provider)
FEED      normalize into feed_items — the universal signals store
KOMPASS   reads the Feed + operating data → summarize, extract, act
```
**Broker: [Nango](https://nango.dev)** — Nango handles OAuth, stores the tokens, refreshes them. S-BOS stores only the *connection reference* (tokens never touch S-BOS).

## 2. What's built (migration 071 + code)
- **`integration_connections`** — per-owner Nango connection refs (provider, integration_id, nango_connection_id, display_name, status).
- **`feed_items`** — universal signals store: `source`/`kind`/`title`/`summary`/`payload` jsonb/`entity_id`/`parent_type`+`parent_id`/`occurred_at`/`processed`; dedup on `(source, external_id)`.
- **`lib/nango.ts`** — gated server client + the `INTEGRATIONS` catalog (id/provider/label/blurb/category).
- **API** — `/api/integrations/session` (connect-session token), `/api/integrations/webhook` (persist connection id), `GET/DELETE /api/integrations` (list/disconnect). All gated on `NANGO_SECRET_KEY`.
- **`/portal/integrations`** — Nango Connect UI; providers grouped by category with an X/Y-connected count; read-only; disconnect.
- **Scripts** — `sync-nango-connections.mjs` (pull connections → table; webhook backstop), `ingest-gmail.mjs` (Gmail via Nango proxy → feed_items).
- **Kompass** — `app/api/capture/ask/route.ts` context now includes recent `feed_items`.

## 3. Current status (2026-07-07)
- **16 integrations configured** in Nango: `google-mail`, `google-calendar`, `google-drive`, `google-meet`, `google-docs`, `google-sheet`, `google-slides`, `google-forms`, `google-tasks`, `google-workspace-admin`, `quickbooks`, `sage-intacct`, `docusign`, `oura`, `strava-web` (+ a stray `google-sheet-77xr` to delete).
- **Only 1 connection: Gmail → clint@stitserbuilt.com — healthy.** Gmail → Feed → Kompass proven end-to-end (Kompass answered from the real inbox).
- **Everything else is configured-but-NOT-connected.**

### ⚠️ Key learning: *configured ≠ connected*
Registering an integration in Nango (the app/OAuth) is step one. A **Connection** only exists after an account is authorized through it. The 15 non-Gmail integrations look "not working" simply because no account has been authorized yet. Connect each via `/portal/integrations`.

## 4. Nango pricing & the free→team→licensing path
| Tier | Price | Connections |
|---|---|---|
| Free | $0 | 10 |
| Starter | $50/mo | 20, then $1/conn |
| Growth | $500/mo | 100, then $1/conn |
| Enterprise | custom | self-host + SLA |

Plan: **Free now** (Gmail + a few) → **Starter** as the team grows → **self-host Nango (OSS) before onboarding licensees** — avoids per-connection cost at scale *and* keeps licensee tokens in our infra (data residency). On Nango Cloud, tokens live on Nango's infra; acceptable for the internal team, not for licensee data.

## 5. Roadmap (step by step, later)
- [ ] Add `NANGO_SECRET_KEY` to **Railway env** so the live Connect flow works (only in `.env.local` today).
- [ ] **Connect** the remaining accounts via `/portal/integrations`.
- [ ] **Per-provider ingestion** → `feed_items` (Gmail done). Priorities: Calendar (deadlines/meetings), Drive (reports + Gemini meeting notes), then QuickBooks/Intacct (financials), Oura/Strava (Body pillar → `stat_logs`).
- [ ] **Scheduled ingest routine** (reuse `routines`/`scheduled_jobs`) so the Feed self-refreshes.
- [ ] **Entity-link** feed items (e.g. a Riverside email → the Riverside project) so Kompass answers are entity-aware.
- [ ] **Operating-statement watch** → parse the PM's monthly statement → `stat_logs` → asset KPIs self-update.
- [ ] **Sage Intacct** connector = the financial-actuals feed (GL per entity → `stat_logs`); **retires the manual Intacct→SmartSuite mirror**.
- [ ] Delete the stray `google-sheet-77xr` in Nango.
- [ ] **Rotate** the Nango key (pasted in chat) + the earlier GitHub/Anthropic keys.
- [ ] **Tighten RLS** on `feed_items`/`integration_connections` to per-owner before licensing (currently anon_read per the app-wide convention).

## 6. Relationship to the rest
The Feed is the "layer on top" that makes Kompass a chief-of-staff: it joins external signals with the operating data (projects, assets, the [Strategic Framework](S-BOS_Strategic_Framework.md) scorecards). Financial connectors (Intacct/QuickBooks) feed the **asset scorecard** KPIs; Oura/Strava feed the **Body** pillar of the personal game. Same Feed, every game.
