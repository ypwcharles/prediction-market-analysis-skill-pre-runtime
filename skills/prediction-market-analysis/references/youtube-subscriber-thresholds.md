# YouTube / Social Subscriber Threshold Markets

Session-derived workflow for Polymarket ladder markets such as `Will MrBeast hit ___ million subscribers by June 30?`.

## Core model

These are cumulative barrier markets:

- If 500M resolves Yes, then 497M/494M/etc. should also resolve Yes.
- Do not treat sibling markets as mutually exclusive buckets.
- For each threshold, compute required pace:

```text
needed = threshold - current_count
days_left = deadline - now
required_daily = needed / days_left
```

Compare `required_daily` to recent observed subscriber growth over multiple windows.

## Source hierarchy

1. Polymarket rule text for deadline and resolution source.
2. Official channel/platform-visible count if extractable.
3. Settlement-adjacent stat pages (Socialcounts, SocialBlade, Viewstats) for current count and daily history.
4. Credible reporting for milestone confirmation.
5. Viral projections / X posts: narrative only, not settlement-grade evidence.

## Practical extraction notes

- YouTube channel HTML may expose metadata but not the main channel subscriber count; it can instead surface related channels' `subscriberCountText`.
- Socialcounts pages may expose exact current count in rendered HTML (e.g. `484,367,692`) and an embedded app data array like:
  - `statistics:[views, subscribers, videos]`
  - `date:"$D2026-05-10T18:30:00.000Z"`
- SocialBlade/Socialcounts tables often round subscribers to whole millions. Use rounded rows for trend bands, not exact daily increments.

## Pricing workflow

1. Fetch event from Gamma by slug and parse child markets.
2. Use CLOB `/price?token_id=<YES>&side=sell` for headline Yes ask.
3. Inspect `/book` and sort asks ascending before sizing.
4. Report sweep averages for realistic sizes (100/500/1000 shares) when the book is wide/thin.
5. Reject headline edge if it only exists at dust top-of-book.

## Probability framing

For each threshold:

- Low threshold: if required daily growth is far below recent baseline, probability can be near-certain, but Yes price may already be too full.
- Middle threshold: often best expression if required daily growth is comfortably below recent baseline and ask still offers several points of edge.
- Upper threshold: requires continued acceleration; use wide confidence interval and demand much cheaper entry.

## Kill criteria / checkpoints

Convert required pace into path checkpoints. Example format:

- By `T1`, count should be at least `current + required_daily * days_to_T1`.
- If actual count is materially below checkpoint for 7–14 days, reduce or exit.
- If the market reprices above the conservative fair boundary, stop adding even if thesis remains directionally right.
