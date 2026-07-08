---
title: Meta Ads Efforts Summary
created: 2026-07-08
updated: 2026-07-08
type: summary
tags: [summary, ethnic-vogue, analysis, temple-fabric]
sources: [raw/meta-ads-snapshot-2026-07-08.md]
confidence: high
---

# Meta Ads Efforts Summary

A consolidated recap of the Ethnic Vogue Meta Ads program as of 2026-07-08, synthesized from the live account snapshot. Built as a Karpathy-style wiki page (see [[ethnic-vogue-meta-account]] and SCHEMA.md).

## What We've Done
1. **Connected the account** to the Meta Marketing API (v21.0) via a system-user token; CLI `meta-ads` is functional against act_370818469.
2. **Pulled live performance** — campaigns, ad sets, ads, account insights, full `analyze` report.
3. **Consolidated spend** — the wasteful "New Awareness Campaign" (0.03% CTR, $6.98 CPC) is now PAUSED; budget concentrated in [[promoting-copy-campaign]].
4. **Benchmarked creative** — CTR 4.57% and CPC $0.62 are top-quartile for the niche (see [[performance-vs-industry-benchmarks]]).
5. **Identified the one real problem** — a complete conversion-tracking gap ([[conversion-tracking-gap]]).
6. **Flagged goal misalignment** — active campaign optimizes for CONVERSATIONS, not purchases ([[optimization-goal-conversations]]).

## What's Working 🟢
- Creative quality is genuinely strong (4.48–4.92% CTR across all 4 active ads).
- Cheap clicks ($0.54–0.70 CPC).
- Zero ad fatigue (frequency 1.47).

## What's Broken 🔴
- **0 conversions across ~$2,124 of 30-day spend.** Root cause: Meta Pixel / Conversions API not firing on labelethnicvogue.shop.
- Optimization goal (CONVERSATIONS) likely misaligned with a sales outcome.

## Open Action Items (priority order)
1. **Fix conversion tracking** — install Meta Pixel (Purchase + AddToCart) + Conversions API on labelethnicvogue.shop. *Blocking everything else.*
2. **Resolve goal alignment** — decide sales-on-site vs WhatsApp-orders; set optimization goal accordingly ([[optimization-goal-conversations]]).
3. **Reallocate within ad set** — shift "bapa white" (highest CPC $0.70) toward "heavy work star" ($0.54) once we can see which drives conversions.
4. **Build the missing benchmark file** — `industry-benchmarks-2026.md` referenced by the skill but absent; re-run real benchmarking with it.
5. **Reactivate or archive paused campaigns** — Promoting (original) had $914/30d historically; decide keep/pause.

## Bottom Line
Spend is efficient at the click level but blind at the outcome level. The next dollar should go into tracking, not more ads.

## Related Pages
- [[ethnic-vogue-meta-account]] · [[promoting-copy-campaign]] · [[conversion-tracking-gap]] · [[optimization-goal-conversations]] · [[performance-vs-industry-benchmarks]]
- Cross-vault: [[woocommerce-index]] (the store being advertised) · [[woocommerce-scaling]]
