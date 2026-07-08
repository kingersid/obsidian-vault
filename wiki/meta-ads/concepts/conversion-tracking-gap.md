---
title: Conversion Tracking Gap
created: 2026-07-08
updated: 2026-07-08
type: concept
tags: [tracking-gap, conversions, ethnic-vogue]
sources: [raw/meta-ads-snapshot-2026-07-08.md]
confidence: high
---

# Conversion Tracking Gap

The single most important problem in the Ethnic Vogue Meta account: **clicks are flowing but zero conversions are recorded**, despite meaningful spend ($665.25 in the last 7 days, ~$2,124 over 30 days across all campaigns).

## What We Observe
- `conversions` field = 0 across the entire account in the 2026-07-08 snapshot.
- `cost_per_conversion` = N/A.
- High CTR (4.57%) and low CPC ($0.62) mean the *top of funnel* works — people click.
- But nothing downstream is attributed.

## Likely Causes (in priority order)
1. **Meta Pixel not firing on labelethnicvogue.shop** — purchase / add-to-cart events not implemented or broken.
2. **Conversions API (CAPI) not set up** — no server-side event redundancy, so even if the browser pixel misfires, nothing backstops it.
3. **Optimization goal misaligned** — the active campaign optimizes for CONVERSATIONS, not purchases, so the algorithm is rewarded for chats, not sales (see [[optimization-goal-conversations]]).
4. **Attribution window / delays** — Meta reports can have 24–48h lag, but 30 days of zero is not lag; it is a structural gap.

## Why It Matters
- You cannot know which of the 4 active ads ([[promoting-copy-campaign]]) actually drives revenue.
- The algorithm is learning the wrong signal (conversations), which misallocates budget.
- Every dollar spent now is blind spend. Fix tracking BEFORE reallocating or scaling.

## Required Fix
- Install Meta Pixel on labelethnicvogue.shop with Purchase + AddToCart events.
- Add Conversions API (server-side) for resilience.
- Consider switching the active campaign's optimization goal from CONVERSATIONS to PURCHASES once pixel data flows.
- This is cross-referenced with the WooCommerce tracking work in [[woocommerce-scaling]].

## Related
- [[ethnic-vogue-meta-account]] — account context
- [[optimization-goal-conversations]] — compounding misalignment
- [[meta-ads-efforts-summary]] — action item #1
