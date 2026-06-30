## 🐳 Docker & Infrastructure

Containerized services, process management, networking, and automation.

---

### 🐋 Docker Containers

| Container | Image | Port(s) | Status | Purpose |
|-----------|-------|---------|--------|---------|
| `wordpress` | WordPress | ? | Up 3 days | WooCommerce site (Chandni Silk Mills) |
| `db` | MariaDB | 3003 | Up 3 days | Database for WordPress |
| `n8n` | n8n | 5678 | Up 4 days | Workflow automation (n8n.io) |
| `lead-manager-mcp` | ? | ? | Up 4 days (unhealthy) | Lead management MCP |

**Docker Compose:** `/root/woocommerce/docker-compose.yml`
**Nginx Config:** `/root/woocommerce/nginx.conf`

---

### ⚡ PM2 Process Manager

PM2 runs three processes:

| Name | ID | Uptime |
|------|----|--------|
| `discord-bot` | 1 | 4 days |
| `lead-dashboard` | 2 | 4 days |
| `sid-agent` | 0 | 4 days |

Managed by `pm2-root.service` (systemd wrapper).

---

### 🎛️ systemd Services

All custom application services are managed by systemd:

```bash
# Service files
/etc/systemd/system/meta-ads-conversion-tracking.service
/etc/systemd/system/whatsapp-community-listener.service
/etc/systemd/system/mastra-agent.service
/etc/systemd/system/hermes-gateway.service
/etc/systemd/system/obsidian-mcp.service
/etc/systemd/system/daily-vitals.service
/etc/systemd/system/discord-bot.service
/etc/systemd/system/telegram-bot.service
/etc/systemd/system/tor@default.service
```

---

### 🗓️ Cron Schedule

| UTC Time | Frequency | Command | Description |
|----------|-----------|---------|-------------|
| 07:30 | Daily | `send_daily_link.sh` | Store link reminder |
| — | Every 6h | Docker restart | Health check/restart |
| — | Every 6h | LLM pipeline | Token pipeline update |
| — | Every 5m | Obsidian git sync | Vault backup |
| 06:30 | Daily | `run-leads-cron.sh` | WhatsApp leads |

---

### 🌐 Network Map

```
Internet
  │
  ├─ Port 3004 → Meta CAPI Server (Node.js)
  ├─ Port 8080 → Lead Dashboard (Node.js/PM2)
  ├─ Port 5678 → n8n (Docker)
  ├─ Port 3003 → MariaDB (Docker)
  ├─ Port 9050 → Tor SOCKS5 Proxy
  └─ Port ?    → WordPress
```

### 🔌 Tor Proxy

- **Service:** `tor@default.service`
- **Port:** `9050` (SOCKS5)
- **Purpose:** Used by WhatsApp bots for connection routing via `--proxy-server=socks5://127.0.0.1:9050`
- **Location:** `/etc/tor/`

---

### 🗄️ Databases

| Database | Type | Location | Purpose |
|----------|------|----------|---------|
| `conversions.db` | SQLite | `/root/workspace/meta-ads-conversion-tracking/data/` | CAPI conversions |
| `vitals.db` | SQLite | `/root/custom-rag/` | Health vitals |
| MariaDB | MySQL | Docker (port 3003) | WordPress/WooCommerce |

---

### 📌 Related
- [[VPS Architecture Overview]]
- [[Services & Projects]]
- [[WhatsApp System]]
