## 📅 VPS Change Chronology

> *"Knowledge work should be compounding."* — Andrej Karpathy

This chronology tracks every change made to the VPS systems. New entries go to the **top**. The log is append-only — nothing is ever deleted, only superseded.

[[wiki-activity-log|→ View the full activity log]]

---

### 🗓️ 2026-06-25

**New: WhatsApp Community Join Listener**
- **Service:** `whatsapp-community-listener.service`
- **Phone:** +919537097267
- Added continuous WhatsApp bot that listens for "JOIN" messages
- Integrates with Meta CAPI to fire `joinCommunity` mid-funnel events
- Replies with WhatsApp community invite link (`https://chat.whatsapp.com/G7pZBW5435aKDV8vd8FDlH`)
- [[VPS Architecture Overview|VPS Architecture]] wiki created with 6 interconnected pages
- [[Karpathy LLM Wiki Schema|Karpathy-style wiki schema]] applied

**Meta CAPI Server Updated**
- Added `GET /join-community` landing page
- Added `POST /api/event/joinCommunity` CAPI endpoint
- Added `GET /api/config-public` endpoint
- Updated `config.json` with `whatsapp_number` and `community_link`

---

### 🗓️ 2026-06-22

**Meta Ads Conversion Tracking Server Deployed**
- **Service:** `meta-ads-conversion-tracking.service`
- **Port:** 3004
- Node.js + Express + SQLite3
- Pixel ID: `1373300634665128`
- Business: Chandni Silk Mills LLP
- Ad Account: `act_370818469`
- Endpoints: conversion logging, CAPI sync, stats, batch upload

---

### 🗓️ 2026-06-10

**New Docker Containers Deployed**
- `lead-manager-mcp` (currently unhealthy)
- `wizardly_aryabhata` (ephemeral)

---

### 🗓️ 2026-05-31

**Daily Vitals Service Created**
- **Service:** `daily-vitals.service`
- **Location:** `/root/workspace/daily-vitals/`
- Tracks health data, generates daily AI reports
- Also deployed: `cloudflare-daily-vitals.service`

---

### 🗓️ 2026-05-24

**Obsidian MCP Server Deployed**
- **Service:** `obsidian-mcp.service`
- **Location:** `/root/obsidian-mcp-server/`
- Python-based Model Context Protocol server for AI → Obsidian integration

---

### 🗓️ 2026-05-17

**LLM Tokens API Created**
- **Service:** `llm_tokens_api.service`

---

### 🗓️ 2026-05-16

**WordPress + WooCommerce Deployed**
- Docker containers: `wordpress`, `db` (MariaDB, port 3003), `nginx`
- **Location:** `/root/woocommerce/`
- Nginx config at `/root/woocommerce/nginx.conf`

**Hermes Gateway Created**
- **Service:** `hermes-gateway.service`
- **Location:** `/root/hermes-webui/`
- Multi-platform messaging integration gateway

---

### 🗓️ 2026-05-13

**n8n Deployed**
- Docker container: `n8n` (port 5678)
- Workflow automation

---

### 🗓️ 2026-05-10

**Mastra Agent Framework Deployed**
- **Service:** `mastra-agent.service`
- **Location:** `/root/mastra-agent/`
- Discord bot + Telegram bot as bridges
- PM2 processes: `discord-bot`, `sid-agent`

**Discord & Telegram Bots Created**
- **Services:** `discord-bot.service`, `telegram-bot.service`
- Bridge between Discord/Telegram and Mastra agent

---

### 🗓️ 2026-05-09

**Voice Agent Projects Started**
- Python voice agent at `/root/voice-agent/`
- LiveKit voice agent at `/root/livekit-voice-agent/`

---

### 🗓️ 2026-05-01 — 2026-05-15

**WhatsApp Lead Automation Developed**
- **Location:** `/root/whatsapp-leads-automation/`
- Batch lead extraction using whatsapp-web.js
- AI scoring via NVIDIA NIM (Llama 3.3 70B)
- PM2 dashboard: `lead-dashboard` (port 8080)
- Cron: daily at 06:30 UTC

---

### 🗓️ 2026-04 — 2026-05

**Project Bootstrapping Phase**
- Custom RAG system (`/root/custom-rag/`)
- Stock dashboard (`/root/workspace/stock-dashboard/`)
- Content calendar (`/root/content-calendar/`)
- Meta Ads CLI (`/root/workspace/meta-ads/`)
- File management systems (`/root/files-vault/`, `/root/filesmd/`)
- Obsidian vault initialized and synced
- Tor proxy configured (port 9050)

---

### 📌 Connected Pages

- [[VPS Architecture Overview]] — Systems map, directory tree
- [[WhatsApp System]] — WhatsApp bots & community join
- [[Meta Ads System]] — CAPI server & CLI
- [[Services & Projects]] — Full service inventory
- [[Custom AI Skills]] — RAG, voice, stock skills
- [[Docker & Infrastructure]] — Containers, PM2, cron
- [[wiki-activity-log]] — Ongoing append-only log

### 🔧 Wiki Schema (Karpathy-style)

This wiki follows [[Karpathy LLM Wiki Schema]]:
- **index.md** → VPS Architecture Overview (the hub)
- **log.md** → wiki-activity-log (append-only chronology)
- **raw/** → source configs for reference
- Cross-linking via `[[wiki links]]`
- LLM-maintained with periodic health checks
