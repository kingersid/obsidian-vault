## 🖥️ VPS Architecture Overview

```yaml
Host: vm771282340
IP: 103.194.228.56
OS: Ubuntu (Linux)
User: root
```

This wiki maps the entire labyrinth of services, bots, AI agents, and infrastructure running on the VPS.

---

### 🧭 Systems Map

```
                    VPS (vm771282340)
  ┌────────────────────────────────────────────────────┐
  │  systemd (service manager)                         │
  │  ├── meta-ads-conversion-tracking (port 3004)      │
  │  ├── whatsapp-community-listener                   │
  │  ├── mastra-agent                                  │
  │  ├── hermes-gateway                                │
  │  ├── obsidian-mcp                                  │
  │  ├── daily-vitals                                  │
  │  ├── discord-bot                                   │
  │  ├── telegram-bot                                  │
  │  └── tor (SOCKS5 proxy :9050)                      │
  │                                                     │
  │  PM2 (process manager)                              │
  │  ├── discord-bot                                    │
  │  ├── lead-dashboard (port 8080)                     │
  │  └── sid-agent                                      │
  │                                                     │
  │  Docker (container engine)                          │
  │  ├── WordPress + MariaDB (port 3003)                │
  │  ├── n8n (port 5678)                                │
  │  └── lead-manager-mcp (unhealthy)                   │
  └────────────────────────────────────────────────────┘
```

---

### 📂 Key Directories on VPS

| Path | Purpose |
|------|---------|
| `/root/workspace/meta-ads-conversion-tracking/` | Meta CAPI server (port 3004) |
| `/root/workspace/meta-ads/` | Meta Ads CLI (Python) |
| `/root/workspace/meta-ads-mastery-deck/` | Meta Ads training modules |
| `/root/workspace/daily-vitals/` | Daily health/vitals tracking |
| `/root/workspace/stock-dashboard/` | Stock data dashboard |
| `/root/mastra-agent/` | Mastra AI agent orchestration |
| `/root/hermes-webui/` | Hermes agent gateway |
| `/root/whatsapp-leads-automation/` | WhatsApp lead extraction |
| `/root/custom-rag/` | Custom RAG + health agents |
| `/root/voice-agent/` | Python voice agent |
| `/root/livekit-voice-agent/` | LiveKit voice agent |
| `/root/obsidian-mcp-server/` | Obsidian MCP server |
| `/root/obsidian-vault/` | Obsidian vault (synced to desktop) |
| `/root/content-calendar/` | Content planning |
| `/root/lead-saas-site/` | LeadFlow SaaS HTML |
| `/root/awesome-stock-skills/` | Custom stock analysis skills |

---

### 🔗 Wiki Pages

- [[WhatsApp System]] — WhatsApp bots, lead extraction, community join flow
- [[Meta Ads System]] — Meta Ads CLI, CAPI conversion tracking, ad account
- [[Services & Projects]] — All running services with port mappings
- [[Custom AI Skills]] — RAG, voice agents, stock skills, agent orchestration
- [[Docker & Infrastructure]] — Docker containers, PM2, cron jobs, Tor proxy

---

### 🔌 Quick Reference

| Service | Port | Type |
|---------|------|------|
| Meta CAPI server | 3004 | Node.js (systemd) |
| Lead dashboard | 8080 | Node.js (PM2) |
| n8n | 5678 | Docker |
| WordPress | ? | Docker |
| MariaDB | 3003 | Docker |
| Tor SOCKS5 | 9050 | systemd |
| WhatsApp listener | — | systemd (background) |

---

### 📜 Wiki Management

- [[VPS Change Chronology]] — Complete timeline of all system changes since April 2026
- [[wiki-activity-log]] — Append-only log of every wiki edit (Karpathy-style)
- [[Karpathy LLM Wiki Schema]] — Rules for AI-maintained wiki ingestion
- `raw/` directory — For immutable reference configs
