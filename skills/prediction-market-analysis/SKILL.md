---
name: prediction-market-analysis
description: Use when analyzing Polymarket, Kalshi, or related prediction-market contracts for tradeability, edge, expression selection, market-move diagnosis, structural scans, smart-money signals, account-style reviews, held-position management, timing buckets, or Kelly sizing. Trigger on market URLs, theme scans, adjacent-bucket comparisons, copy-trade questions, historical win-rate/payoff reviews, bankroll-aware sizing, or questions about why a market moved.
---

# Prediction Market Analysis

## Overview

This skill acts like a conservative prediction-market risk committee plus a contract-selection coach.

Its job is not only to decide whether a thesis is tradeable. Its job is also to decide whether the proposed contract is the right way to express that thesis at all.

Default posture:

- reject weak setups
- separate direction edge from timing edge
- prefer the cleaner expression over the more exciting expression
- size only from conservative edge
- preserve a proven user style instead of forcing higher-variance trades

## When to Use

Use this skill when the user wants to:

- analyze a specific Polymarket or Kalshi market
- search a theme or event for tradeable markets
- compare related contracts across platforms
- compare adjacent time buckets, strike levels, or mutually exclusive outcomes
- decide which market best expresses a thesis
- estimate fair odds or a probability range
- decide whether a setup is strong enough to trade
- size a position using Kelly or fractional Kelly
- review a past or proposed trade for bucket-selection, expression, or sizing mistakes
- re-evaluate a proposed trade in the context of existing exposure
- analyze a wallet, account, or personal trading history for style, win rate, payoff ratio, or strategy fit
- judge whether a smart-money, insider, whale, alert-bot, or copy-trade signal is worth following
- diagnose whether a market move came from hard news, rule reinterpretation, catalyst repricing, or order flow

Do not use this skill for:

- automatic order execution
- pure historical backtesting detached from a current market
- open-ended financial commentary with no market or event to analyze

## Core Principles

1. Reject-first. Try to disprove the trade before approving it.
2. Expression matters. A correct thesis in the wrong contract is still a bad trade.
3. Direction and timing are different risks. Price them separately.
4. Intervals beat false precision. Always produce a main probability plus a confidence interval.
5. Conservative Kelly only. Size from the conservative boundary, never the central estimate.
6. Portfolio-aware by default. A good isolated trade can still be a bad portfolio trade.
7. Strategy-fit matters. A trade outside the user's proven edge lanes needs a stronger evidence bar and a smaller size.

## Supported Entry Modes

### Single-Market Deep Analysis

Use when the input is a specific market URL, contract, or market ID.

### Market-Move / News Diagnosis

Use when the user asks why a market moved, what new information changed, or whether a recent price move was caused by news versus order flow. This is an explanatory mode, not necessarily a trade recommendation.

In this mode:

- still read the settlement rules first
- still inspect adjacent buckets / sibling markets when available
- compare official-source news against market-native price history and recent trades
- separate rule-relevant news from generic narrative news
- explain whether the move appears driven by hard news, rule reinterpretation, catalyst repricing, liquidity/order flow, or a mix
- do **not** force a binary `TRADE` / `NO TRADE` verdict unless the user asks whether to trade
- do **not** force the full eight-section trade template; use a concise diagnostic structure with conclusion, facts, interpretation, risks, and confidence

### Theme-Driven Discovery

Use when the input is a thesis, event, or topic. Discover related markets first, then analyze only the candidates that survive screening.

### Broad Structural Edge Scan

Use when the user asks to scan Polymarket/Kalshi broadly for `edge >= X`, arbitrage, stale markets, or cross-market mispricings. Default to structural/rules-based edge, not subjective opinion edge, unless the user explicitly asks for model-driven fair odds.

For this mode, follow `references/structural-scan-arb.md` and the scan hygiene notes in `references/polymarket-broad-scan-2026-05-16.md`:

- use Gamma only for discovery and rule text;
- verify candidate edges with executable CLOB Yes/No asks;
- reject midpoint/Gamma-only monotonicity anomalies unless the executable hedge pair costs less than 100c;
- verify mutually exclusive strips are truly exhaustive, especially election candidate lists and filing deadlines;
- for candidate / nominee markets, explicitly check inactive `Other` / `another person` placeholders and any rule fallback if no nominee is announced;
- for threshold scans, require true binary Yes/No outcomes before applying Yes/No ladder logic;
- for `hit` markets, parse `(HIGH)`/`(LOW)` direction explicitly; bare `hit $X` is ambiguous and should be skipped from automated monotonicity scans;
- report depth-adjusted size limits, because top-of-book edge can disappear after a few units.

### Trade Review / Expression Audit

Use when the input is a past or proposed trade and the goal is to identify whether the real issue is:

- direction
- timing
- contract expression
- sizing
- execution

### Account Style / Wallet Review

Use when the input is a wallet, account link, trade history, bankroll update, or request to improve a trading style.

Separate closed market-level realized PnL, open mark-to-market PnL, current exposure, win rate, payoff ratio, profit factor, average win/loss, and the strategy lanes where profits actually came from. For Polymarket account review, aggregate by `conditionId` or market slug before quoting win rate, payoff ratio, or realized PnL. See `references/account-style-and-smart-money.md`.

If the user has a high-win-rate / low-payoff profile, do not reflexively recommend buying longer-shot contracts to "improve odds." Prefer preserving the hit-rate edge while reducing average loss through smaller hazardous positions, earlier thesis stop-losses, and better entry discipline.

## Trade Archetypes

Classify every setup into exactly one primary bucket before directional work:

- `resolution arb`: real-world outcome is effectively known; prioritize rule text, oracle behavior, settlement ambiguity, lock-up, and operational tail risk.
- `directional event`: main question is whether the event happens; prioritize causal drivers and the cleanest broad expression.
- `time-bucket trade`: main risk is whether the event happens inside this window; require timing-specific evidence.
- `cross-bucket structure`: edge comes from nearby clocks, strikes, thresholds, or rule scopes; named actors, settlement verbs, or event scope changes make this a rule-scope problem, not a pure timing ladder.
- `smart-money signal`: thesis comes from a wallet, whale, alert, insider cue, or copy-trade signal; validate wallet history, position size, hedges/ladders, opposing flow, rule fit, and current executable price. See `references/account-style-and-smart-money.md`.

## Workflow

### 1. Normalize the input

Extract or infer:

- event or question
- platform and market identifiers when available
- settlement rule
- settlement time horizon
- bankroll or position constraints
- existing portfolio context when provided
- account-style context when the user provides trade history, wallet data, or a stated bankroll
- smart-money or copy-trade signal source when it is part of the thesis
- whether the user is asking for analysis, discovery, or post-trade review

### 2. Classify the trade archetype

Decide whether the setup is:

- `resolution arb`
- `directional event`
- `time-bucket trade`
- `cross-bucket structure`
- `smart-money signal`

Do not analyze a resolution arb like a normal prediction trade.
Do not analyze a time-bucket trade as if direction alone were sufficient.
If nearby contracts differ in named actors, settlement verbs, or event scope, treat that as a rule-scope problem before treating it as a pure timing problem.
If both deadline and rule scope differ, classify as `cross-bucket structure`, not `time-bucket trade`.
If the user's reason for entry is primarily "a wallet bought this," classify the setup as `smart-money signal` first, then test whether it also qualifies as another archetype.

### 3. Discover the full expression set

For a single market, enrich the analysis with:

- inverse or opposing expressions
- adjacent time buckets
- mutually exclusive outcomes
- strike or threshold neighbors
- rule-scope variants
- named-actor variants
- cross-platform equivalents

For a theme prompt, first build a shortlist of candidate markets before full analysis.

For a trade-review prompt, explicitly ask: "Was the thesis wrong, or was the contract wrong?"

### 4. Filter for tradeability

Before directional analysis, check:

- settlement clarity
- resolution-source reliability
- liquidity and book quality
- fees and likely slippage
- risk of noise, manipulation, or wash trading

If the market is not clean enough to analyze, return `NO TRADE`.

### 5. Gather and grade evidence

Actively search:

- official and primary sources
- economic or regulatory releases
- quality journalism and specialist reporting
- market-native signals
- expert social sources
- Grok / `x_search` social overlay when Hermes has xAI OAuth or `XAI_API_KEY` configured

Use Grok as a discovery and corroboration layer, not as the final judge. For Hermes/X-search evidence rows, required fields, source-quality rules, and no-URL handling, read `references/hermes-x-search-evidence.md`.

Use `references/evidence-engine.md` for source tiers, timing-vs-direction evidence, archetype-specific standards, and conflict handling.

### 6. Separate direction edge from timing edge

Before assigning a probability, split the thesis into:

- probability the event happens at all
- probability it happens within this contract window
- probability the market resolves cleanly under the written rules

If the user's thesis is mostly "this probably happens eventually" but the contract requires a narrow deadline, treat that as a contract-selection warning, not as full support for the asked market.

### 7. Audit expression and bucket selection

Explicitly test:

- is this the best expression of the thesis
- is an adjacent bucket cleaner
- are these markets actually the same event under the rules
- do named actors, verbs, or scope changes make one contract strictly cleaner
- is `No` on the earlier bucket better than `Yes` on the exact bucket
- if the thesis is right but late, does this contract still pay
- is the user accidentally paying for timing precision they do not possess

Use `references/probability-and-kelly.md` before pricing or sizing. If a nearby expression dominates the asked contract, say so clearly even if the asked market still has some edge.

### 8. Estimate probability and interval

Construct:

- anchor probability
- evidence adjustments
- main probability
- confidence interval

For resolution arbs, interpret these as resolution confidence rather than broad event probability.

Do not treat market price as literal truth.

### 9. Compute executable edge

Compare the conservative fair value implied by the interval against the best realistic executable price after:

- fees
- spread
- slippage
- execution uncertainty
- timing-mismatch risk

If edge disappears after costs and timing risk, return `NO TRADE`.

### 10. Check portfolio risk

Inspect:

- overlap with existing positions
- same-thesis duplication across markets
- cross-platform duplication
- thematic concentration
- tail-risk concentration
- same-calendar clustering
- strategy-lane concentration against the user's proven strengths and weaknesses

If portfolio context is unavailable, apply the portfolio-blind haircut from `references/probability-and-kelly.md` and say so explicitly.

If the user has a calibrated style history, label the trade as one of:

- `core lane` - matches repeatedly profitable categories or structures
- `adjacent lane` - related but less proven
- `hazard lane` - historically loss-prone, noisy, or easy to express in the wrong bucket
- `lottery lane` - low-probability position whose main value is optionality, not evidence-backed edge

Use this label to adjust evidence bar, sizing, and kill criteria.

### 11. Size or structure conservatively

Apply Kelly only if the setup survives all earlier checks.

**CRITICAL — Bucket-sum trap in mutually exclusive families:**

When analyzing mutually exclusive outcome markets (e.g., "Who will win X?"), the sum of ask prices across all outcomes will typically exceed 100%. This is the **market maker's spread**, not a taker edge.

- Ask sum > 100% ≠ "sell Yes on all outcomes for guaranteed profit"
- Bid sum < 100% ≠ "buy Yes on all outcomes for guaranteed profit"
- Both represent the cost of being a taker (crossing the spread)
- The only way to capture the overpricing is to BE the maker (post asks, wait for fills), which is a market-making strategy with directional risk, not riskless arb

**Real edge in mutually exclusive families requires one of:**
1. Resolution arb (outcome already known, market not yet resolved)
2. Stale market (past end date, verifiable outcome)
3. Directional mispricing (your fair probability differs from market price)
4. Cross-platform arbitrage (same event priced differently on different platforms)

Use:

- conservative interval boundary
- net executable odds
- uncertainty haircut
- correlation haircut
- liquidity haircut
- drawdown haircut
- time-precision haircut

If the best expression is a ladder or split structure rather than a single contract, recommend that instead of forcing a one-line answer.

When a user has a high-win-rate / low-payoff style, size to preserve that style:

- Core lanes can receive normal conservative-Kelly sizing if the edge is independently verified.
- Adjacent lanes require a strategy-fit haircut.
- Hazard lanes should usually be observation size unless rule-level evidence is unusually strong.
- Smart-money-only trades are observation size until the wallet signal is independently confirmed.
- Lottery lanes should be capped at a small fixed loss the user is comfortable writing to zero.

If the user gives a current bankroll, use that explicit bankroll for concrete sizing. Do not infer bankroll from visible open positions or historical capital unless the user asks for that estimate.

Improving payoff ratio should usually mean cutting losers earlier and entering cleaner expressions, not buying lower-probability long shots.

### 12. Return a binary verdict

Final verdict must be one of:

- `TRADE`
- `NO TRADE`

No "maybe trade" verdict.

If the asked contract is inferior but a nearby expression is materially better, the verdict may still be `NO TRADE` for the asked market while recommending the cleaner expression.

## Output Format

Select the output mode from the user's request:

- **Trade Decision**: use the exact eight-section template below and return `TRADE` or `NO TRADE`.
- **Market-Move Diagnosis**: do not force a trade verdict unless asked; use `Conclusion`, `Facts`, `Price-action interpretation`, `Rule relevance`, `Risks`, and `Confidence`.
- **Broad Structural Scan**: start with coverage counts, then `TRADE / WATCH / NO TRADE` counts, then a ranked shortlist with direct links, executable edge, depth limits, and false-positive reasons.
- **Account Style / Wallet Review**: separate realized performance, open exposure, concentration, payoff profile, strategy lanes, and style-calibrated changes.

For Trade Decision mode, use all eight numbered sections in order, even when the answer is short. Do not start with a prose summary before section 1. If a field is unavailable, say `not provided`, `unknown`, or `not assessable from prompt` rather than omitting it. For every specific market or preferred expression you mention, include a direct market URL when it is available.

### 1. Verdict
- `TRADE` or `NO TRADE`

### 2. Market Summary
- platform
- market title
- market link
- trade archetype
- strategy-fit lane
- expression / rule-scope differences
- settlement rule
- settlement time
- executable price(s)
- liquidity / fee notes

### 3. Probability Assessment
- anchor probability
- adjusted main probability
- confidence interval
- direction vs timing decomposition
- main uncertainty drivers

### 4. Evidence Review
- decisive evidence
- rule-scope differences
- smart-money / wallet-signal quality
- timing-specific evidence
- directional evidence
- conflicting evidence
- discarded / noise evidence
- source reliability notes

### 5. Mispricing / Edge
- conservative fair value
- executable value
- best expression
- worse expression(s)
- if thesis is right but late
- net edge after costs
- why edge is or is not sufficient

### 6. Portfolio Impact
- related existing positions
- incremental thematic exposure
- concentration / correlation concerns
- strategy-fit concerns

### 7. Sizing
- raw Kelly
- conservative Kelly
- style-calibrated cap
- preferred structure / ladder
- final recommended fraction
- concrete size if bankroll is provided
- maximum entry price / minimum required price

### 8. Kill Criteria
- what invalidates the thesis
- what removes the edge
- what triggers reduction or exit
- exit type

If the input is thematic and multiple candidate markets are discovered:

- start with a short ranked shortlist
- include the preferred expression for each surviving thesis
- include a direct market link for each preferred expression in the shortlist
- then provide full detailed reports only for markets that survive screening

## Archetype-Specific Standards

### Resolution Arb

Approve only if:

- the real-world state is already effectively known
- written rules still point the same way
- oracle or resolution discretion is not the main risk
- capital lock-up and tail risk are explicitly priced

Reject if the trade is merely "probably going to resolve that way soon" without enough rule-level certainty.

### Directional Event

Approve only if:

- the evidence changes the event probability, not just the narrative tone
- the asked contract is a clean expression of the thesis
- a broader or cleaner adjacent market does not dominate it

### Time-Bucket Trade

Approve only if:

- the evidence speaks to timing, not only direction
- the market deadline matches the cadence of the event
- the user is not buying a narrow `Yes` on vague "soon" evidence

Short-dated `Yes` contracts need a higher bar than short-dated `No` contracts when timing precision is weak.

For geopolitical, military, diplomatic, or crisis markets, short-dated `Yes` needs an especially high bar because narrative tension often does not map cleanly to the contract clock or settlement verb. If the evidence is mostly "something may happen soon," prefer smaller sizing, later buckets, broader expressions, or short-dated `No`.

### Cross-Bucket Structure

Approve only if:

- the bucket comparison is internally coherent
- material rule-scope differences are explicitly named
- the edge survives after accounting for same-thesis correlation
- the recommendation names which bucket, ladder, or expression is actually preferred

When rule-scope differences are material, the analysis must explicitly say this is not a pure time-bucket comparison.

### Smart-Money Signal

Approve only if:

- the signal wallet has a relevant track record or verifiable reason to be informed
- the position is not merely a tiny probe relative to the wallet
- the wallet is not obviously hedged, laddered across incompatible expressions, or already trimming
- the signal maps to the exact settlement rule and deadline
- independent real-world evidence supports the same side
- the entry price after the alert still leaves conservative edge

Reject or cap at observation size if:

- the wallet is new or only visible in this one market
- several alerts are duplicates of the same wallet or copy-traders
- the signal is large in absolute terms but not proven to be informed
- strong opposing flow exists from similarly credible wallets
- the thesis is mostly narrative tension rather than rule-level evidence

## Refusal Rules

Return `NO TRADE` if any of the following is true:

1. No informational edge survives scrutiny.
2. Evidence conflict is too high to support a disciplined interval.
3. Net edge vanishes after fees, slippage, or execution assumptions.
4. Settlement ambiguity is material.
5. Liquidity is too weak to trust the paper edge.
6. Portfolio concentration is too high.
7. The thesis may be right, but the asked contract is the wrong expression.
8. Timing evidence is too weak for the narrowness of the bucket.
9. Material rule-scope differences exist but have not been analyzed.
10. A smart-money signal cannot be validated beyond "a wallet bought this" and no independent edge remains.
11. The setup falls in a user-specific hazard lane and the requested size is larger than observation size.

## Domain-Specific References

Read the relevant reference only when the market type requires it:
- Core references: `references/evidence-engine.md`, `references/probability-and-kelly.md`, `references/domain-adapters.md`, and `references/research-and-open-source.md`.
- Account / signal references: `references/account-style-and-smart-money.md` and `references/hermes-x-search-evidence.md`.
- Scan references: `references/structural-scan-arb.md` and `references/polymarket-broad-scan-2026-05-16.md`.
- Domain references: `references/ai-model-release-markets.md`, `references/crypto-token-sale-thresholds.md`, `references/crypto-fdv-launch-markets.md`, `references/youtube-subscriber-thresholds.md`, and `references/ipo-lead-underwriter-markets.md`.

## Common Mistakes

- Treating low-quality evidence volume, market price, or midpoint paper edge as tradeable conviction.
- Confusing "event probably happens" with "event happens inside this exact clock."
- Treating rule-scope differences as pure time buckets.
- Evaluating resolution arbs like normal forecasts.
- Sizing from central probability rather than the conservative interval boundary.
- Ignoring correlated exposure, strategy-fit hazards, or execution depth.
- Letting smart-money alerts, old crypto launch hype, or AI product-news headlines substitute for rule-relevant evidence.
