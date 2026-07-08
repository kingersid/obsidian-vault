---
title: Optimization Goal — Conversations
created: 2026-07-08
updated: 2026-07-08
type: concept
tags: [optimization-misaligned, ethnic-vogue, conversions]
sources: [raw/meta-ads-snapshot-2026-07-08.md]
confidence: high
---

# Optimization Goal — Conversations

The active campaign [[promoting-copy-campaign]] and its ad set are both set to optimize for **CONVERSATIONS** (Meta messaging/click-to-message events), not for purchases or conversions.

## Why This Is a Problem
- The campaign is rewarded when users start a conversation (e.g. WhatsApp/Messenger), not when they buy.
- Combined with [[conversion-tracking-gap]], the account has *no purchase signal at all* — it optimizes for chats while conversions read as zero.
- If the true business goal is sales on labelethnicvogue.shop, CONVERSATIONS as the sole goal is misaligned and compounds the measurement problem.

## When Conversations Goal Is Legitimate
For a temple-fabric business doing custom/wholesale inquiries via WhatsApp (the user's number is 918320129806), conversations can be a valid top-of-funnel goal — IF those conversations are themselves the product (custom orders taken by chat). In that case the tracking gap is "we don't measure whether chats convert to orders," not "we don't track purchases."

## Decision Needed
- If sales happen on-site → switch optimization goal to PURCHASES and fix pixel (see [[conversion-tracking-gap]]).
- If sales happen via WhatsApp → keep CONVERSATIONS but instrument a way to measure chat→order conversion (offline conversion upload or manual tracking).

## Related
- [[promoting-copy-campaign]] — the campaign using this goal
- [[conversion-tracking-gap]] — the compounding measurement problem
- [[ethnic-vogue-meta-account]] — account overview
