---
title: Meta CAPI Integration — Full Config Reference
created: 2026-07-08
updated: 2026-07-08
type: entity
tags: [meta-account, capi, config, tracking-gap, ethnic-vogue, temple-fabric]
sources: [raw/meta-ads-snapshot-2026-07-08.md, references/session-2026-06-22-chandni-silk-mills.md]
confidence: high
---

# Meta CAPI Integration — Full Config Reference

Complete, self-contained configuration for the offline Conversions API (CAPI) pipeline that attributes offline conversions (phone calls, WhatsApp leads, exhibition orders) to Meta ad spend on account `act_370818469`.

> ⚠️ SECURITY NOTE (read first): The access tokens below are secret. This vault is local/private — DO NOT commit it to a public git repo or sync it anywhere shareable. In production the token belongs ONLY in the n8n environment variable `META_ACCESS_TOKEN`; the n8n workflow export should carry a placeholder, never the real value. See [[conversion-tracking-gap]].

---

## 1. Core Identifiers

| Key | Value | Notes |
|-----|-------|-------|
| Ad Account ID | `act_370818469` | Ethnic Vogue / Chandni Silk Mills shared account |
| Pixel ID | `1373300634665128` | Pixel name: "Chandni Silk Mills Pixel" |
| Business Manager | none configured | `business_id` = null in project config |
| Business name (label) | `Chandni Silk Mills LLP` | From project `config.json` |
| Brand / store | `labelethnicvogue.shop` | Ethnic Vogue — temple/devotional fabrics |
| Conversion event | `Purchase` | |
| Currency | `INR` | |
| action_source | `phone_call` | For offline (call/WhatsApp/retail) conversions |

### CAPI endpoint URL
```
https://graph.facebook.com/v21.0/1373300634665128/events
```
- Graph API version: **v21.0**
- Method: POST
- Auth: bearer `META_ACCESS_TOKEN`

---

## 2. Access Tokens (⚠ two distinct values found — see flag)

### Token A — canonical (per skill session notes, `~/.meta-ads/config.json`)
```
[REDACTED — live token removed; real value lives only in ~/.meta-ads/config.json and n8n META_ACCESS_TOKEN env var. Do NOT commit the real token.]
```
Source file: `~/.meta-ads/config.json` (field `access_token`). This is the value the integration skill designates as the source of truth.

### Token B — ~~deployed in project `config.json`~~ **REMOVED (invalid)**
```
[REMOVED 2026-07-10 — this token returned HTTP 400 (invalid/expired); it has been purged from config.json and from this doc]
```
Source file: `/root/workspace/meta-ads-conversion-tracking/config.json` (field `access_token` — now `null`).

> ✅ RESOLVED — Token B was invalid (HTTP 400) and has been removed everywhere. Use **Token A** (canonical, in `~/.meta-ads/config.json`) as the source of truth.
> `curl -s -X GET "https://graph.facebook.com/v21.0/me?access_token=<TOKEN>"`
> Use the one that returns the account. Set THAT one as `META_ACCESS_TOKEN` in n8n.

---

## 3. Server (Collector)

| Setting | Value |
|---------|-------|
| Tech | Express + SQLite (Node.js) |
| Port | `3004` (3000 = mastra-agent, 3002 = daily-vitals — both occupied) |
| Health | `GET /api/health` |
| DB path | `data/conversions.db` (absolute: `/root/workspace/meta-ads-conversion-tracking/data/conversions.db`) |
| Project root | `/root/workspace/meta-ads-conversion-tracking/` |
| Server file | `server/server.js` |
| WhatsApp listener | `server/whatsapp-listener.js` (API base `http://localhost:3004`) |
| UI | `ui/index.html` (single + batch entry, pending/synced views, 30s poll) |
| WAL mode | enable for concurrent n8n↔server access |

### API Endpoints
- `POST /api/conversion` — single entry; validates Indian mobile `^[6-9]\d{9}$` (after stripping `+91`/`91`)
- `POST /api/conversion/batch` — bulk upload
- `GET /api/conversions/pending` — unsynced rows
- `GET /api/conversions/synced` — last 100 synced
- `GET /api/conversions/today` — today's entries
- `GET /api/conversions/stats` — counts
- `POST /api/sync/mark-sync` — body `{ "ids": [...] }` → sets `synced=1`

### Systemd
- Unit: `meta-ads-conversion-tracking.service` → `/etc/systemd/system/meta-ads-conversion-tracking.service`
- Auto-restart; environment vars injected here (incl. token if not using n8n-only)

---

## 4. n8n Workflow (Daily Sync)

| Setting | Value |
|---------|-------|
| Workflow file | `workflows/meta-capi-daily-sync.json` |
| Cron | `0 19 * * *` — **timezone `Asia/Kolkata`** (7 PM IST) |
| n8n port | `5679` |
| Token var | `META_ACCESS_TOKEN` (n8n env var — NOT hardcoded) |
| Status | ⚠ NOT YET imported/activated (known gap from build session) |

### Node chain
1. Cron trigger `0 19 * * *` (Asia/Kolkata)
2. HTTP GET `http://localhost:3004/api/conversions/pending`
3. Code node — map each conversion → CAPI payload (schema below)
4. HTTP POST `https://graph.facebook.com/v21.0/1373300634665128/events`
5. Code node — inspect `events_receiver` for `200` status
6. HTTP POST `http://localhost:3004/api/sync/mark-sync` (mark synced)
7. IF node + optional Telegram alert on failure

### CAPI payload schema (per event)
```json
{
  "event_name": "Purchase",
  "event_time": <Unix timestamp>,
  "event_id": "conv_<DB_ID>_<epoch>",
  "action_source": "phone_call",
  "event_source_url": "https://labelethnicvogue.shop",
  "user_data": {
    "ph": [{ "country_code": "91", "phone_number": "<10digit>" }]
  },
  "custom_data": {
    "currency": "INR",
    "value": <order_value>,
    "order_id": "<DB_ID>",
    "content_name": "<campaign label>"
  }
}
```

### Field mapping (v21.0) — see [[field-aliases]]
- `ph` = `[{country_code, phone_number}]` — preferred, dedup-safe
- `event_id` = primary dedup key (reuse on retry)
- `event_source_url` MUST be a domain associated with the Pixel (use `labelethnicvogue.shop`)
- Legacy `country`+`phone` aliases — avoid when `ph` available
- Add `client_ip_address` / `client_user_agent` to improve match rate

---

## 5. Other Config Values (from `config.json`)

| Key | Value |
|-----|-------|
| `server_port` | `3004` |
| `n8n_port` | `5679` |
| `whatsapp_number` | `919537097267` |
| `community_link` | `https://chat.whatsapp.com/G7pZBW5435aKDV8vd8FDlH` |
| `business_name` | `Chandni Silk Mills LLP` |

---

## 6. Pitfalls / Operational Notes
- Port conflict: 3000 (mastra-agent) and 3002 (daily-vitals) occupied — this service uses 3004.
- Meta Graph API timeouts common on slow providers → respect retry backoff (2s, 5s). n8n built-in retry covers the code node.
- SQLite WAL mode required for n8n→server concurrent access.
- Indian mobile validation: strip `+91`/`91`, then `^[6-9]\d{9}$`.
- CTWA gap: `ctwa_clid` is captured in the Node.js lead manager but NOT yet sent back via CAPI — see [[conversion-tracking-gap]] and `Wiki/Chandni-Silk-Mills/CTWA-Campaign-Log.md`.

## 7. Verification (run after deploy)
```bash
curl http://localhost:3004/api/health
curl -X POST http://localhost:3004/api/conversion \
  -H "Content-Type: application/json" \
  -d '{"mobile":"9876543210","campaign":"Test","value":0}'
curl http://localhost:3004/api/conversions/pending
# after n8n run:
curl http://localhost:3004/api/conversions/synced
```

## Related
- [[ethnic-vogue-meta-account]] — account overview
- [[conversion-tracking-gap]] — why CAPI is the priority fix
- [[optimization-goal-conversations]] — compounding misalignment
- `Wiki/Chandni-Silk-Mills/CTWA-Campaign-Log.md` — CTWA action items
