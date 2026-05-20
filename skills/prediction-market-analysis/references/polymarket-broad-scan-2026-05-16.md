# Polymarket broad structural scan notes — 2026-05-16

Session context: Charles asked for a cross-category Polymarket scan for `edge >= 10`. We loaded `prediction-market-analysis` + `polymarket` and scanned high-volume active Gamma events, then verified candidates with CLOB executable asks.

## What worked

- Pull Gamma events by volume in pages of 100 and build category coverage from event-level tags.
- Use Gamma only for candidate discovery; verify all survivors with individual CLOB `/price?token_id=...&side=sell`.
- Batch CLOB `/prices` worked for first-pass screening in this session, but individual `/price` remained necessary for survivor confirmation.
- For monotonic pairs, executable structure is:
  - if A is subset of B: buy `B Yes` + buy `A No`;
  - edge = `1 - (B_yes_ask + A_no_ask)`.

## Pitfalls discovered / reinforced

### 1. Threshold parser must not treat non-binary O/U markets as binary ladders
Some sports/esports markets use outcomes like `Over` / `Under` rather than `Yes` / `No`. Do not compare them with binary Yes/No ladder logic. Require outcomes contain explicit `Yes` and `No` before monotonic threshold-pair scanning.

### 2. Bare `hit $X` is ambiguous unless HIGH/LOW is explicit
Commodity / FX path markets may say `hit $X` for both HIGH and LOW buckets. Treat direction as:
- `(HIGH)` / `High` => above ladder;
- `(LOW)` / `Low` => below ladder;
- bare `hit $X` without HIGH/LOW => ambiguous, skip automated structural scan.

### 3. Do not let date/asset numbers become thresholds
Naive number extraction can mistake:
- `10-year Treasury` as threshold 10;
- `May 31, 2026` day/year as threshold;
- event title dates as price/count thresholds.
Prefer the number immediately governed by threshold words (`over`, `above`, `below`, `dip below`, `hit (HIGH/LOW)`, etc.). Skip obvious years and month-day numbers.

### 4. Do not infer missing-year deadlines from stale Gamma `endDate`
For titles like `by June 30?`, infer year from current date / rule text, not blindly from Gamma `endDate`. Gamma end dates can be stale or parent-level and can invert time ladders.

### 5. Candidate/election strips require filing-deadline and Other checks
A buy-all-active-Yes strip with ask sum < 100c is only a trade when active outcomes are exhaustive. For candidate-primary markets:
- verify filing deadline has passed;
- verify official candidate list equals active Polymarket outcomes;
- check whether inactive `Other` / `another person` / placeholders exist;
- read rule fallback: e.g. `If no nominee is announced by Nov 3, resolve to Other`.

Examples from this scan:
- FL-23 Democratic Primary had only two active outcomes summing ~70–73c, but Florida filing deadline (2026-06-12) had not passed and inactive `another person` existed. WATCH, not trade.
- LA-05 Republican Primary had 7 active outcomes summing ~81c, but Louisiana congressional primaries were suspended after redistricting litigation and rules could resolve to `Other` if no nominee by Nov 3. NO TRADE / watch only.

### 6. Incomplete open-world strips are false positives
Large apparent strips such as Nobel Peace Prize, MLB awards, Lebanon parliamentary winner, and next PM markets often have many unlisted outcomes. `sum Yes asks < 100c` is the market pricing unlisted outcomes, not a taker edge.

## Reporting standard from this session

For broad scan output, avoid full eight-section deep dive for every false positive. Prefer:
- top-line `TRADE / WATCH / NO TRADE` counts;
- scan coverage counts;
- ranked shortlist with direct market links;
- clear reason each headline edge is rejected;
- separate `CLOB executable edge` from `paper/Gamma edge`.
