## 📊 Meta Ads System

Two interconnected systems for Meta advertising: the CLI tool for campaign management and the Conversion API (CAPI) server for conversion tracking.

---

### 🛠️ Meta Ads CLI

- **Location:** `/root/workspace/meta-ads/`
- **Script:** `meta_ads_cli.py` (Python)
- **Config:** `/root/.meta-ads/config.json`
- **Purpose:** Command-line interface for Meta Marketing API — pull campaign performance, analyze metrics, generate optimization recommendations
- **Ad Account:** `act_370818469`

#### Key Capabilities
- Campaign performance reporting
- Audience insights
- Optimization recommendations

---

### 📡 Meta CAPI Conversion Tracking Server

- **Location:** `/root/workspace/meta-ads-conversion-tracking/`
- **Service:** `meta-ads-conversion-tracking.service`
- **Port:** `3004`
- **Stack:** Node.js + Express + SQLite3
- **Config:** `config.json` (contains `pixel_id`, `access_token`, `ad_account_id`)
- **Pixel ID:** `1373300634665128`
- **Business:** Chandni Silk Mills LLP

#### API Endpoints

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/health` | Health check |
| GET | `/api/config-public` | Public config (whatsapp number, community link) |
| GET | `/join-community` | WhatsApp community landing page |
| POST | `/api/conversion` | Log a conversion |
| POST | `/api/conversion/batch` | Batch upload conversions |
| GET | `/api/conversions/pending` | Pending (unsynced) conversions |
| GET | `/api/conversions/synced` | Synced conversions (last 100) |
| GET | `/api/conversions/today` | Today's conversions |
| GET | `/api/conversions/stats` | Stats (today total/synced, pending) |
| POST | `/api/sync/manual` | Manually sync pending to Meta CAPI |
| POST | `/api/sync/mark-sync` | Mark IDs as synced |
| GET | `/api/sync/status` | Last sync timestamp |
| DELETE | `/api/conversion/:id` | Delete a conversion |
| **POST** | **`/api/event/joinCommunity`** | **Mid-funnel CAPI event** |

#### Mid-Funnel Event: `joinCommunity`

This endpoint was added for the [[WhatsApp System]] flow:
- Accepts `{ phone: "919999999999" }`
- SHA256 hashes the phone number
- Logs to SQLite DB with `event_name = "joinCommunity"`
- Fires Meta CAPI event to pixel `1373300634665128`
- Returns the WhatsApp community invite link

```javascript
// CAPI payload
{
  event_name: "joinCommunity",
  action_source: "website",
  user_data: { ph: ["<sha256_hash>"] },
  custom_data: { currency: "INR", value: 0, content_name: "WhatsApp Community Join" }
}
```

#### Sync Flow
```
Pending conversions (synced=0 in SQLite)
  → POST /api/sync/manual
    → SHA256 hashes phone, email, name, location
    → POST to graph.facebook.com/v21.0/{pixel_id}/events
    → Updates synced=1 on success
```

---

### 📚 Meta Ads Mastery Deck

- **Location:** `/root/workspace/meta-ads-mastery-deck/`
- Training modules for Meta Ads (PDF/PPTX generation)
- Build script: `build-module-01.js`

---

### 📌 Related
- [[VPS Architecture Overview]]
- [[WhatsApp System]]
- [[Custom AI Skills]]
