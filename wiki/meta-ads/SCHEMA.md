# Meta Ads Wiki Schema

## Domain
Paid social advertising (Meta / Facebook / Instagram) for Ethnic Vogue — temple & devotional fabrics sold via labelethnicvogue.shop. Covers campaign structure, performance metrics, optimization decisions, and the creative-vs-measurement gap.

## Conventions
- File names: lowercase, kebab-case, no spaces.
- Every wiki page starts with YAML frontmatter (see below).
- Use `[[wikilinks]]` to link between pages (minimum 2 outbound links per page).
- When updating a page, always bump the `updated` date.
- Every new page must be added to `index.md`.
- Every action must be appended to `log.md`.
- Provenance marker `^[raw/meta-ads-snapshot-YYYY-MM-DD.md]` on paragraphs sourced from a snapshot.

## Structure (Karpathy three-layer)
```
meta-ads/
├── SCHEMA.md           # this file
├── index.md            # sectioned catalog
├── log.md              # append-only action log
├── raw/                # Layer 1: immutable snapshots from meta-ads CLI
├── entities/           # Layer 2: ad account, campaigns, ad sets, ads
├── concepts/           # Layer 2: tracking gap, optimization-goal topics
├── comparisons/        # Layer 2: performance vs industry benchmarks
└── queries/            # Layer 2: filed syntheses (effort summaries, etc.)
```

## Frontmatter (wiki pages)
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy]
sources: [raw/meta-ads-snapshot-2026-07-08.md]
confidence: high | medium | low
---
```

## Tag Taxonomy
- Account/structure: meta-account, campaign, adset, ad, ethnic-vogue, temple-fabric
- Metrics: spend, ctr, cpc, cpm, frequency, conversions
- Issues: tracking-gap, ad-fatigue, optimization-misaligned, wasteful-spend
- Process: benchmark, analysis, snapshot, reallocation
- Meta: comparison, summary, entity

Rule: every tag on a page must appear here. Add new tags here first.

## Update Policy
- New snapshot (new dates) → re-run analysis, update entity/comparison/query pages, flag drift.
- Contradictions (e.g. a campaign flips active/paused) → note dates, mark `contested` if unresolved.
- The missing skill file `industry-benchmarks-2026.md` is a known gap — benchmark comparisons use general-knowledge estimates until that file is built. See [[performance-vs-industry-benchmarks]].

## Cross-Reference to Other Wikis
- Meta Ads drives traffic to the WooCommerce store documented in [[woocommerce-index]] and [[woocommerce-scaling]].
- Brand voice / campaign strategy lives in the Ethnic Vogue marketing skill (`Skills/user/ethnic-vogue-marketing-strategy.md`).
