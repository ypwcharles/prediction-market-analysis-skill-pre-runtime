# Hermes / Grok X-Search Evidence

Use this reference when Hermes has xAI OAuth or `XAI_API_KEY` access and can run Grok / `x_search` for current Polymarket work.

## Role boundary

Grok is a discovery and corroboration layer, not the final judge. The skill still decides rule fit, probability, edge, expression quality, and sizing.

Run X-search in parallel with normal web/source checks for:

- breaking-news catalysts
- market-move diagnosis
- politics/geopolitics
- crypto launches
- AI model releases
- public-figure rumors
- Telegram-forwarded trading signals
- markets where X is likely to contain source-adjacent evidence

## Required row schema

Ask Hermes/Grok to return JSON evidence rows:

```json
{
  "source_id": "@handle-or-domain",
  "source_kind": "x",
  "url": "https://x.com/...",
  "published_at": "2026-05-20T12:34:00Z",
  "claim": "single concrete claim",
  "directional_implication": "yes|no|neutral",
  "timing_implication": "supports_deadline|misses_deadline|neutral",
  "rule_relevance": "direct|indirect|noise",
  "evidence_quality": "primary|strong|supplementary|weak",
  "conflict_status": "supports|contradicts|unresolved"
}
```

## Evidence rules

- Require concrete post/thread URLs and timestamps.
- Prefer official handles, project/team accounts, regulators, journalists, domain experts, and eyewitnesses.
- Treat influencer calls, anonymous alpha, and engagement-bait as weak unless independently corroborated.
- If Grok returns no URL, use the claim only as a search lead or background noise.
- If X chatter explains a price move but not settlement probability, classify it as narrative/order-flow, not edge.
- Cross-check rule-relevant claims with official or high-quality web sources whenever possible.

## Recommended live order

1. Read settlement rule and exact child market.
2. Verify executable CLOB bid/ask/depth.
3. Use X-search for fresh catalysts and cited posts.
4. Cross-check direct rule claims with official/web sources.
5. Decide whether the market underreacted, overreacted, or correctly repriced.
