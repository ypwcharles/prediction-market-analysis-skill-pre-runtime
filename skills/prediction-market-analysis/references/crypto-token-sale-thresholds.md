# Crypto Token-Sale Commitment Threshold Markets

Use this workflow for markets like `Over $X committed to the <project> public sale?`

## Core distinction

Read the rule source first. These markets usually settle from the official sale page, not project fundraising headlines, VC rounds, token FDV, or secondary ICO aggregators.

Separate:

- target raise / hardcap
- current total commitments shown on the sale page
- Polymarket threshold bucket

Build the full threshold ladder from the parent event. Low thresholds may resolve Yes while higher thresholds remain open and near-zero. Do not infer `>$40M` from `>$2M` being true.

## Data path

1. Read Gamma rule text and resolution source.
2. Check every child threshold.
3. Verify executable prices with CLOB `/price` for the exact child market.
4. Use official sale page data when accessible.
5. If Cloudflare/JS blocks the page, layer official snippets, browser extraction, project/X announcements, third-party ICO pages, and Polymarket rules.

Gamma `outcomePrices`, search snippets, and Polymarket page summaries are discovery only.

## Rule-risk catalysts

Refunds, cancellations, governance changes, CEO resignations, sale extensions, or source unavailability can change whether final verifiable commitments count. Mark these as rule-risk catalysts, not ordinary event evidence.

## Sizing

For high-probability No trades, raw Kelly can overstate size. Main risks are rule interpretation, oracle discretion, source unavailability, extension clauses, and capital lock-up.
