# Chandni Silk Mills — CTWA Campaign Log

## Active Campaign
- **Name:** `[6/16/2026] Promoting - Copy`
- **ID:** 52569581724878
- **Objective:** Click-to-WhatsApp (CTWA)
- **Targeting:** Wholesale dealers, Tier 2/3 cities
- **Audience:** Advantage+ (broad, Meta-optimised)
- **Placements:** To verify — should be Advantage+ Placements

---

## Day-over-Day Performance (INR)

| Metric | 28 Jun | 29 Jun | Δ |
|---|---|---|---|
| Spend | ₹82.37 | ₹93.04 | +13.0% |
| Impressions | 2,994 | 3,744 | +25.0% |
| Reach | 2,756 | 3,619 | +31.3% |
| Frequency | 1.086 | 1.035 | -4.7% |
| Clicks | 181 | 187 | +3.3% |
| CTR | 6.05% | 4.99% | -1.06 pp |
| CPC | ₹0.455 | ₹0.498 | +9.4% |
| CPM | ₹27.51 | ₹24.85 | -9.7% |
| Conversions | 0 | 0 | — |

---

## Key Insights (as of 29 Jun 2026)

### What's working
- **CTR 5–6%** — exceptional for B2B wholesale targeting
- **CPC ₹0.45–0.50** — less than 50 paise per click, very efficient
- **CPM ₹24–27** — low and healthy for Indian B2B
- **~185 clicks/day at ₹93 budget** — strong volume for the spend
- **Low frequency (1.03)** — zero fatigue risk, room to scale
- **CPM dropping while reach grows** — Meta finding cheaper inventory

### What needs fixing
- **0 conversions tracked** — CAPI/pixel not firing after WhatsApp initiation
- `ctwa_clid` being captured in Node.js lead manager on VPS but NOT sent back to Meta via CAPI
- Meta flying blind — no optimisation signal, can't identify best-converting audience segments

### Diagnosis
- CTR dip 6% → 5% is normal Advantage+ audience expansion dilution, not a crisis
- High CTR + high CPM (earlier observation) was likely tight audience — Advantage+ has resolved inventory constraint
- Real unknown: what % of 185 daily WhatsApp clicks are turning into dealer conversations

---

## Action Items

- [ ] Fix CAPI webhook — fire conversion event when WhatsApp chat initiates (`ctwa_clid` → Meta CAPI)
- [ ] Audit WhatsApp inbox — count actual dealer conversations from last 2 days vs dead opens
- [ ] Hold budget steady at ~₹93/day until CAPI fires 10–15 events
- [ ] Do NOT tighten audience or cap budget until tracking is live
- [ ] After CAPI live → drill ad-set level to find which audience segment is dragging CTR

---

## Budget Notes
- ₹93/day is workable for CTWA at these CPCs (not enough for conversion-optimised campaigns)
- For learning phase exit on conversion objective: need 50 events in 7 days — not achievable at this budget
- Strategy: keep CTWA objective (not conversion), fix CAPI for visibility only, optimise manually

---

## Open Questions
- What % of ~185 daily WhatsApp clicks are genuine dealer inquiries?
- Is Meta AI bot qualifying leads inside WhatsApp?
- How many converting to sample requests / orders per day?
