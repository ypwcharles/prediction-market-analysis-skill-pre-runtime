# Structural Edge Scans on Polymarket

Use this reference when the user asks to scan many Polymarket markets for `edge >= X`, `arb`, `mispricing`, or structural opportunities.

## Target edge definition

Prefer structural / rules-based edge over subjective model edge:

1. **Resolution arb / stale markets**: real-world outcome is already known or the contract window is over, but market remains tradeable.
2. **Monotonicity violations**: later deadlines, lower thresholds, or broader buckets should be at least as expensive as earlier / higher / narrower buckets when rules are otherwise identical.
3. **Mutually exclusive full-set strips**: buying all exhaustive outcomes costs less than 100c.

Do not call a subjective probability disagreement `edge >= X` unless the user explicitly asks for model-driven fair odds.

## Data source ladder

1. Gamma: discovery only — event/market metadata, rule text, active flags, tags, rough prices. For broad scans prefer `/events/keyset` cursor paging over offset-style paging.
2. CLOB V2 batch `POST /prices`: first-pass executable bids/asks for many Yes/No legs; chunk conservatively at 100 token-side requests and read `SELL` as the ask / cost to buy.
3. CLOB `/price?token_id=<id>&side=sell`: survivor-level executable taker ask for each Yes/No leg.
4. CLOB `/book?token_id=<id>`: survivor depth / slippage; sort asks by ascending price before sweeping.
5. Official / primary sources: verify candidate lists, filing deadlines, results, launch status, and settlement sources.

Gamma `outcomePrices`, `bestAsk`, and search snippets are not enough for final edge. Batch CLOB output is a speed layer, not final proof; verify every survivor with individual `/price` and `/book`.

## Monotonicity verification

For a claimed subset/superset violation where A is a subset of B:

- Example: `FDV > $1B` is subset of `FDV > $800M`.
- Example: `launch by Jun 30` is subset of `launch by Dec 31`.

A paper violation is `A_yes > B_yes`. The executable arb check is:

- Buy `B Yes` + buy `A No`.
- Cost must be `< 1.00` after CLOB asks and fees/slippage.
- If cost is `>= 1.00`, it is only midpoint / spread noise, not a taker opportunity.

For low-threshold / below-threshold markets, first map the logical subset correctly before pairing legs; do not assume the title order matches the Gamma group order.

## Mutually exclusive strip verification

A Yes strip is tradeable only if the active outcomes are exhaustive under the written rules.

Reject or downgrade when:

- inactive markets include `Other`, `Another`, `Person A`, `Candidate X`, `Contestant X`, `Party X`, or placeholders;
- filing / qualification deadlines have not passed;
- official candidate list differs from active Polymarket outcomes;
- the market is actually a time ladder, not mutually exclusive outcomes (e.g. `out by Jun 30` and `out by Dec 31` can both pay Yes);
- long-tail fields like awards, sports season winners, Nobel, reality TV, PM appointments, or parliamentary seat winners have many plausible missing outcomes.

For elections, verify:

- official candidate list / Ballotpedia or state election source;
- filing deadline status;
- whether the rule has an `Other` fallback if no nominee is announced;
- whether runoff / replacement clauses change the payout target.

## Depth and sizing

A strip with 10c edge at top-of-book can collapse after a few outcome units.

For each candidate, compute:

- top-of-book sum cost;
- sweep cost for practical sizes: 1, 5, 10, 25, 50, 100 units;
- maximum entry sum price where the edge remains worth the rule risk.

Report edge as executable edge, not Gamma edge. Label thin-depth opportunities as `small size only`.

## Reporting standard for broad scans

Start with a ranked shortlist:

- `TRADE`: executable edge survives CLOB + rule completeness.
- `WATCH`: paper edge exists but a known gating fact is unresolved, e.g. filing deadline not passed.
- `NO TRADE / false positive`: explain why it was rejected.

For each surviving candidate include:

- market link;
- exact leg structure;
- CLOB ask sum and edge;
- rule / official-source checks;
- depth limits;
- kill criteria;
- confidence.
