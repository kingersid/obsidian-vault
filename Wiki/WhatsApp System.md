## 💬 WhatsApp System

The VPS runs multiple WhatsApp automation systems using [[whatsapp-web.js|whatsapp-web.js]] (via Puppeteer/Chrome) for business communication.

---

### 📱 Active WhatsApp Systems

#### 1. WhatsApp Leads Automation
- **Location:** `/root/whatsapp-leads-automation/`
- **Script:** `index.js` — batch lead extraction tool
- **Auth:** `LocalAuth` with session ID `session-whatsapp-leads-agent`
- **How it works:** Runs daily via cron (`06:30 UTC`), iterates through all chats, extracts unsaved contacts who messaged today, scores them using NVIDIA NIM (Llama 3.3 70B), saves leads to JSON/CSV
- **Dashboard:** Node server at port 8080 (PM2: `lead-dashboard`)
- **Cron:** `/root/whatsapp-leads-automation/run-leads-cron.sh`

#### 2. WhatsApp Community Join Listener (New)
- **Location:** `/root/workspace/meta-ads-conversion-tracking/server/whatsapp-listener.js`
- **Service:** `whatsapp-community-listener.service`
- **Auth:** `LocalAuth` with session ID `whatsapp-community-join`
- **How it works:** Runs 24/7 as a systemd service. Listens for incoming "JOIN" messages → fires [[Meta Ads System#Mid-Funnel Event - joinCommunity|joinCommunity CAPI event]] → replies with WhatsApp community invite link
- **Phone:** `+919537097267`
- **Puppeteer:** Uses `/usr/bin/google-chrome-stable` with Tor SOCKS5 proxy (`socks5://127.0.0.1:9050`)

#### 3. Legacy /whatsap-leads
- **Location:** `/root/whatsap-leads/`
- Older version of the leads automation, similar approach

---

### ⚙️ Technical Setup

```javascript
// Puppeteer config (both bots use this)
{
  headless: true,
  executablePath: "/usr/bin/google-chrome-stable",
  args: [
    "--no-sandbox",
    "--disable-setuid-sandbox",
    "--disable-dev-shm-usage",
    "--disable-accelerated-2d-canvas",
    "--disable-gpu",
    "--disable-background-timer-throttling",
    "--disable-renderer-backgrounding",
    "--proxy-server=socks5://127.0.0.1:9050"  // Tor proxy
  ]
}
```

### 🔄 Flow: Community Join

```
User visits /join-community (port 3004)
  → Clicks "Tap to confirm and join →"
    → Opens wa.me/919537097267?text=JOIN
      → User sends "JOIN"
        → WhatsApp listener receives message
          → POST /api/event/joinCommunity (CAPI fire)
            → Meta receives joinCommunity event
          → Replies with community invite link
```

### 📌 Related
- [[Meta Ads System#Mid-Funnel Event - joinCommunity]]
- [[Services & Projects#Lead Dashboard]]
- [[Docker & Infrastructure#Cron Jobs]]
