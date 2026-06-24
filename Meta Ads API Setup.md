# Meta Ads API Setup Guide

## Overview
CLI tool at `/root/workspace/meta-ads/meta_ads_cli.py` (command: `meta-ads`) connects to Meta Marketing API to pull campaign data, analyze performance, and generate optimization recommendations.

**Ad Account:** `act_370818469`

---

## One-Time Setup Steps

### Step 1: Create a Meta App
1. Go to https://developers.facebook.com/apps/
2. Click **"Create App"** → Select **Business** type
3. Name it (e.g. "Ethnic Vogue Ads Manager")
4. In App Dashboard → **Add Product** → **Marketing API**

### Step 2: Get your Ad Account ID
1. Open Meta Ads Manager
2. Look in the URL: `adsmanager/manage/campaigns?act=XXXXXXXXXX`
3. The number after `act=` is your ad account ID
   - Current: `act_370818469`

### Step 3: Generate a System User Token
1. Go to **Business Settings** → **System Users**
2. Create a system user (or use existing)
3. Click **"Generate New Token"**
4. Select your app and ad account
5. Enable permissions:
   - `ads_read` — read campaign data and insights
   - `ads_management` — create/edit/pause campaigns (optional, for write access)
6. Copy the token

### Step 4: Authorize the Token on your Ad Account
1. Go to **Business Settings** → **Ad Accounts**
2. Select ad account `act_370818469`
3. Click **"Assign Partners"** or **"Assign System Users"**
4. Add your system user with **"Manage campaigns"** or **"View campaigns"** access
5. The token MUST be granted explicit permission on the ad account — this is the most common gotcha

### Step 5: Configure the CLI
```bash
meta-ads config --token YOUR_ACCESS_TOKEN
meta-ads config --account act_XXXXXXXXXX
```

---

## CLI Commands

| Command | Description |
|---------|-------------|
| `meta-ads campaigns` | List all campaigns with 7-day performance metrics |
| `meta-ads analyze` | Full report with optimization recommendations |
| `meta-ads insights` | Account-level KPIs (spend, CTR, CPC, etc.) |
| `meta-ads insights --days 30` | Last 30 days insights |
| `meta-ads adsets CAMPAIGN_ID` | Drill into a specific campaign's ad sets |
| `meta-ads ads ADSET_ID` | Drill into a specific ad set's ads |
| `meta-ads config` | Show current config |

---

## What `meta-ads analyze` Detects

- **[FATIGUE]** Frequency > 3 — rotate creative or expand audience
- **[LOW CTR]** CTR < 0.5% — test new creative/copy or refine targeting
- **[RISING CPC]** 7d CPC > 30d CPC by 30%+ — check auction competition
- **[NO CONVERSIONS]** Spending >$10 with 0 conversions — pause or fix funnel
- **[PAUSED]** Paused campaigns that were previously active — review if worth reactivating

---

## Troubleshooting

### "Ad account owner has NOT grant ads_management or ads_read permission"
This means the token was generated but NOT authorized on the specific ad account. Fix:
1. Go to Business Settings → Ad Accounts → select your ad account
2. Assign the system user with proper access level
3. Regenerate the token if needed

### Token expired
Meta tokens expire. Generate a long-lived token:
1. Use the App Dashboard → Tools → Access Token Tool
2. Or exchange short-lived for long-lived via the API

### Rate limiting
Marketing API has rate limits. The CLI uses sequential requests so this is rarely an issue for single-account usage.

---

## Architecture

```
Meta Marketing API (v21.0)
  └── Ad Account (act_370818469)
       ├── Campaigns
       │    └── Ad Sets (budget, targeting, bidding)
       │         └── Ads (creative, copy)
       └── Insights (spend, CTR, CPC, frequency, conversions)
```

## Files
- CLI tool: `/root/workspace/meta-ads/meta_ads_cli.py`
- Config: `~/.meta-ads/config.json`
- SDK: `facebook-business` (pip)
