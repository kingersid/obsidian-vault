## ⚡ Services & Projects

Complete inventory of all services, their types, ports, and purposes.

---

### 🎛️ Systemd Services

| Service | Type | Port | Description |
|---------|------|------|-------------|
| `meta-ads-conversion-tracking` | Node.js | 3004 | Meta CAPI server + join community page |
| `whatsapp-community-listener` | Node.js | — | WhatsApp JOIN message listener |
| `mastra-agent` | Node.js | ? | Mastra AI agent orchestration server |
| `hermes-gateway` | Python | ? | Hermes agent messaging gateway |
| `obsidian-mcp` | Python | ? | Obsidian MCP server for AI tools |
| `daily-vitals` | Node.js | ? | Daily health tracking server |
| `discord-bot` | Node.js | — | Discord bot (Mastra bridge) |
| `telegram-bot` | Node.js | — | Telegram bot (Mastra bridge) |
| `tor@default` | Tor | 9050 | SOCKS5 proxy for WhatsApp bots |

---

### 📋 PM2 Processes

| Name | Type | Port | Description |
|------|------|------|-------------|
| `discord-bot` | Node.js | — | PM2-managed Discord bot |
| `lead-dashboard` | Node.js | 8080 | WhatsApp leads dashboard |
| `sid-agent` | Node.js | ? | SID agent |

---

### 🐳 Docker Containers

| Container | Image | Port | Status |
|-----------|-------|------|--------|
| `wordpress` | WordPress | ? | Up 3 days |
| `db` | MariaDB | 3003 | Up 3 days |
| `n8n` | n8n | 5678 | Up 4 days |
| `lead-manager-mcp` | ? | ? | Up 4 days (unhealthy) |

---

### 🗓️ Cron Jobs

| Schedule | Command | Description |
|----------|---------|-------------|
| `07:30 UTC daily` | `send_daily_link.sh` | Daily store link reminder |
| `Every 6 hours` | Docker restart | Docker health check/restart |
| `Every 6 hours` | LLM pipeline | LLM token pipeline update |
| `Every 5 minutes` | Obsidian sync | Obsidian vault sync to git |
| `06:30 UTC daily` | `run-leads-cron.sh` | WhatsApp leads automation |

---

### 📁 Project Directory Map

```
/root/
├── workspace/
│   ├── meta-ads-conversion-tracking/    ← CAPI server (port 3004)
│   ├── meta-ads/                        ← Meta Ads CLI (Python)
│   ├── meta-ads-mastery-deck/           ← Training modules
│   ├── daily-vitals/                    ← Health tracking
│   ├── stock-dashboard/                 ← Stock data
│   └── conversion-tracker/              ← (older version)
├── whatsapp-leads-automation/           ← WhatsApp lead bot
├── mastra-agent/                        ← Mastra AI orchestration
├── hermes-webui/                        ← Hermes gateway
├── custom-rag/                          ← Custom RAG system
├── voice-agent/                         ← Python voice agent
├── livekit-voice-agent/                 ← LiveKit voice agent
├── obsidian-mcp-server/                 ← Obsidian MCP
├── obsidian-vault/                      ← Synced vault
├── content-calendar/                    ← Content planning
├── lead-saas-site/                      ← LeadFlow SaaS frontend
├── awesome-stock-skills/               ← Stock analysis skills
└── files-vault/ filesmd/               ← File management
```

---

### 📌 Related
- [[VPS Architecture Overview]]
- [[WhatsApp System]]
- [[Meta Ads System]]
- [[Docker & Infrastructure]]
