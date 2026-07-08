# Meta Ads Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, create, snapshot

## [2026-07-08] create | Meta Ads wiki initialized
- Domain: Ethnic Vogue Meta Ads (account act_370818469)
- Structure created: SCHEMA.md, index.md, log.md, raw/, entities/, concepts/, comparisons/, queries/
- Captured live snapshot 2026-07-01→07-08 via meta-ads CLI (v21.0)

## [2026-07-08] ingest | live account snapshot
- Source: meta-ads campaigns / insights / adsets / ads / analyze
- Files: raw/meta-ads-snapshot-2026-07-08.md
- Key facts: 1 active campaign ($665.25/7d), CTR 4.57%, CPC $0.62, 0 conversions

## [2026-07-08] create | entity + concept + comparison pages
- entities/ethnic-vogue-meta-account.md
- entities/promoting-copy-campaign.md
- concepts/conversion-tracking-gap.md
- concepts/optimization-goal-conversations.md
- comparisons/performance-vs-industry-benchmarks.md
- queries/meta-ads-efforts-summary.md

## [2026-07-08] create | CAPI integration config reference
- File: entities/capi-integration-config.md
- Captured full CAPI config: ad account act_370818469, Pixel 1373300634665128, server port 3004, n8n port 5679, WhatsApp 919537097267, endpoint v21.0
- Flagged: TWO divergent access tokens (Token A in ~/.meta-ads/config.json vs Token B in project config.json) — needs validation before go-live
- Flagged: n8n workflow NOT yet imported/activated; token must live in n8n env var META_ACCESS_TOKEN

## [2026-07-08] flag | missing benchmark source
- Skill references references/industry-benchmarks-2026.md but the file does not exist in the vault.
- Benchmark comparisons use general-knowledge estimates; file pending build.
- See [[performance-vs-industry-benchmarks]] and SCHEMA.md Update Policy.
