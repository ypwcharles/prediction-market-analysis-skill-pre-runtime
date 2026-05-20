---
name: prediction-market-analysis
description: Use when analyzing Polymarket, Kalshi, or related prediction-market contracts for tradeability, fair odds, bucket selection, contract expression, cross-market comparisons, or Kelly-based sizing. Trigger when the user asks to analyze a specific market, scan a theme or event for opportunities, compare adjacent time buckets or equivalent expressions, decide which contract best fits a thesis, review a past prediction-market trade for direction-vs-timing mistakes, estimate a probability range, reject an over-specific timing market, or size a prediction-market position conservatively.
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

## Trade Archetypes

Classify the setup before doing directional work. Every trade must start in exactly one primary bucket:

### 1. Resolution Arb

The real-world outcome is already effectively decided, but the market has not fully resolved or repriced yet.

Prioritize:

- rule text
- oracle / resolution source behavior
- settlement ambiguity
- capital lock-up
- operational tail risk

### 2. Directional Event

The main question is whether an event happens at all, and timing is secondary.

Prioritize:

- event state
- causal drivers
- asymmetric evidence
- best broad expression of the thesis

### 3. Time-Bucket Trade

The main risk is not only whether the event happens, but whether it happens inside a specific window.

Prioritize:

- procedural gates
- known calendars and lags
- operational constraints
- catalysts that narrow timing, not just direction

### 4. Cross-Bucket Structure

The edge comes from comparing nearby contracts that express the same thesis with different clocks, strikes, thresholds, or rule scopes.

Prioritize:

- monotonicity
- adjacent-bucket pricing
- calendar ladders
- rule-scope differences
- whether the best trade is a different bucket or expression, not the asked contract

If contracts differ in named actors, settlement verbs, or event scope, default to this archetype unless the rule text is otherwise identical apart from the deadline.

## Workflow

### 1. Normalize the input

Extract or infer:

- event or question
- platform and market identifiers when available
- settlement rule
- settlement time horizon
- bankroll or position constraints
- existing portfolio context when provided
- whether the user is asking for analysis, discovery, or post-trade review

### 2. Classify the trade archetype

Decide whether the setup is:

- `resolution arb`
- `directional event`
- `time-bucket trade`
- `cross-bucket structure`

Do not analyze a resolution arb like a normal prediction trade.
Do not analyze a time-bucket trade as if direction alone were sufficient.
If nearby contracts differ in named actors, settlement verbs, or event scope, treat that as a rule-scope problem before treating it as a pure timing problem.
If both deadline and rule scope differ, classify as `cross-bucket structure`, not `time-bucket trade`.

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
- **Grok / `x_search` social overlay** when Hermes has xAI OAuth or `XAI_API_KEY` configured

#### Grok / X-search overlay for Charles

For Charles's Hermes setup, when the task involves current Polymarket opportunities, market moves, breaking-news catalysts, public figures, crypto launches, politics/geopolitics, AI model releases, or Telegram-forwarded trading signals, run `x_search` in parallel with normal web/source checks whenever the tool is available.

Use Grok as a **discovery and corroboration layer**, not as the final judge:
- Ask for concrete X posts / threads with URLs and timestamps, not generic narrative.
- Prefer official handles, eyewitnesses, journalists/domain experts, project/team accounts, and high-signal market participants.
- Treat influencer calls, anonymous alpha, and engagement-bait as low-quality evidence unless independently corroborated.
- Map each X claim to: `claim`, `source URL`, `directional implication`, `timing implication`, `rule relevance`, and `evidence quality`.
- If Grok returns no citations/URLs, do not use the claim in the trade case except as background noise.
- If X chatter explains a price move but does not alter settlement probability under the rules, classify it as narrative/order-flow, not edge.

Recommended order for live Polymarket work:
1. Read settlement rule / identify exact child market.
2. Verify executable CLOB bid/ask/depth.
3. Use `x_search` for fresh X catalysts and cited posts.
4. Cross-check rule-relevant claims with official/web sources when possible.
5. Decide whether the market has underreacted, overreacted, or simply repriced correctly.

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

If portfolio context is unavailable, apply the portfolio-blind haircut from `references/probability-and-kelly.md` and say so explicitly.

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

### 12. Return a binary verdict

Final verdict must be one of:

- `TRADE`
- `NO TRADE`

No "maybe trade" verdict.

If the asked contract is inferior but a nearby expression is materially better, the verdict may still be `NO TRADE` for the asked market while recommending the cleaner expression.

## Output Format

ALWAYS use this exact structure:

Use all eight numbered sections in order, even when the answer is short.
Do not replace the template with custom headings.
Do not start with a prose summary before section 1.
Do not use bold summary headers as substitutes for the numbered template.
If a field is unavailable, say `not provided`, `unknown`, or `not assessable from prompt` rather than omitting it.
For every specific market or preferred expression you mention, include a direct market URL when it is available.

Before writing any substantive content, first emit the exact eight section headers in order and then fill them.

### 1. Verdict
- `TRADE` or `NO TRADE`

### 2. Market Summary
- platform
- market title
- market link
- trade archetype
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

### 7. Sizing
- raw Kelly
- conservative Kelly
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

### Cross-Bucket Structure

Approve only if:

- the bucket comparison is internally coherent
- material rule-scope differences are explicitly named
- the edge survives after accounting for same-thesis correlation
- the recommendation names which bucket, ladder, or expression is actually preferred

When rule-scope differences are material, the analysis must explicitly say this is not a pure time-bucket comparison.

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

## Crypto token-sale commitment threshold markets

Use this workflow for markets like `Over $X committed to the <project> public sale?`:

1. Read the rule source first. These markets usually settle from the official sale page, not project fundraising headlines, VC rounds, token FDV, or secondary ICO aggregators.
2. Separate three quantities that are often confused:
   - target raise / hardcap (e.g. $2M target)
   - current total commitments shown on the sale page
   - Polymarket threshold bucket (e.g. >$4M, >$40M)
3. Build the full threshold ladder from the parent event and check every child market. Low thresholds may be closed/resolved Yes while higher thresholds remain open and near-zero. Do not infer >$40M from >$2M being true.
4. Verify executable prices with CLOB `/price` for the exact child market. Search-result snippets and Polymarket page summaries can be stale; Gamma `outcomePrices` are discovery only, and batch endpoints can fail suspiciously.
5. If the official sale page is Cloudflare-protected or JS-heavy, use layered evidence: official page snippets, browser extraction, project/X announcements, third-party ICO pages, and Polymarket rules. Mark source reliability explicitly.
6. Treat refund/cancellation/governance events as rule-risk catalysts. A full refund announcement or CEO resignation can mean the final commitment amount remains below thresholds, or that settlement hinges on whether “commitments before refund” vs “final verifiable commitments” count.
7. For high-probability NO trades, do not let raw Kelly overstate size. The main risks are rule interpretation, oracle discretion, source unavailability, extension clauses, and capital lock-up. Require a much cheaper NO entry than a naive probability model suggests.

## AI model-release / flagship timing markets

Use this workflow for markets like `Will a new Gemini/OpenAI/Claude flagship be released by <date>?`:

1. Read the rule text before reading tech news. These markets often hinge on exact words such as `released`, `made available`, `general public`, `GA`, `preview`, `reasoning-focused`, `flagship`, `Pro`, `Deep Think`, `Ultra`, and exclusions for Flash/Lite/Nano/TTS/embedding/tooling variants.
2. Build the full sibling time ladder from the parent event. Compare adjacent buckets such as `by May 15`, `by May 22`, `by May 31`, `by Jun 30`. A later bucket near 95% while a near bucket is 50–60% usually means the market believes the event is directionally likely but timing is concentrated around one catalyst window.
3. For price-move explanations, pull CLOB price history for the Yes token and Data API recent trades. A fast drop may be a mix of official news failing to satisfy rules plus one or two large No/Yes-sell trades breaking thin liquidity.
4. Treat official changelogs, model docs, launch blogs, model cards, and conference schedules as primary sources. Tech blogs and search snippets are useful for discovery but not enough for settlement-critical claims.
5. Classify each news item as:
   - qualifies or likely qualifies under rules (e.g. new Pro/Ultra/Deep Think public release, or existing flagship GA if rules name GA)
   - directionally relevant but not enough (e.g. I/O promises latest model updates)
   - explicitly non-qualifying (e.g. Flash/Lite/Nano, TTS, embedding, File Search, webhooks, API schema changes, agent/tooling updates)
6. Separate `announcement risk` from `availability risk`: a keynote demo or roadmap may move prices but may not resolve Yes if the model is not made available to the required audience.
7. For `GA`-sensitive markets, verify current launch stage from official docs (`Preview`, `Public preview`, `GA`, `Limited availability`) rather than relying on media headlines saying “released.”
8. In the final output, say whether the move was caused by hard negative news, lack of expected positive catalyst, rule reinterpretation, adjacent-bucket repricing, or order flow. Include confidence and what future official wording would flip the assessment.

## YouTube / social follower-count threshold ladders

Use this workflow for markets like `Will <creator/channel> hit <N> subscribers/followers by <date>?`:

1. Read the rule clock first. Polymarket event-level `endDate` can differ from the rule text; use the rule sentence such as `June 30, 2026, 11:59 PM ET` as the deadline for all probability math.
2. Treat sibling thresholds as a **cumulative ladder**, not mutually exclusive buckets. Higher thresholds imply all lower thresholds; do not sum Yes prices as if they partition outcomes.
3. Anchor on current stock plus required flow:
   - `needed = threshold - current_count`
   - `required_daily = needed / days_left`
   - compare that to recent 7/14/29/30-day subscriber growth.
4. Prefer primary or settlement-adjacent sources in this order:
   - official channel page / platform-visible count when extractable
   - reputable live-count/stat pages with exact current count (e.g. Socialcounts, SocialBlade) as secondary settlement-adjacent data
   - credible reporting for major milestone confirmation
   - viral projections or X posts only as low-quality narrative.
5. Beware rounded history. SocialBlade/Socialcounts daily tables often show counts rounded to whole millions; use them for trend bands, not false exactness. If the page embeds raw app data, extract exact `statistics` arrays / current counter from HTML before relying on snippets.
6. Build threshold-specific fair probabilities from growth-rate bands, not from one linear projection. Low thresholds whose required daily growth is far below the recent baseline can be near-certain; upper thresholds requiring acceleration should remain wide-interval tail bets.
7. Inspect CLOB depth for each threshold. These entertainment ladders can have absurd headline spreads and dust top-of-book. Report both headline ask and sweep prices for realistic sizes (e.g. 100/500/1000 shares). Reject an apparent edge if it only exists at dust size.
8. Choose the cleanest expression: usually the threshold whose required daily growth is comfortably below recent trend but whose Yes price has not already collapsed to ~99c. Avoid overpaying for the upper-tail bucket unless the growth data proves acceleration.
9. Set path checkpoints. Convert the required daily pace into milestone dates/counts; use those as kill criteria rather than waiting until the final deadline.

## IPO lead-underwriter / lead-left markets

Use this workflow for markets like `Will Goldman Sachs serve as the lead underwriter in SpaceX's IPO?`:

1. Read the rule before interpreting news. These markets often resolve to the **primary lead underwriter / lead-left**, not merely any bank participating as underwriter, bookrunner, or active bookrunner.
2. Separate three probabilities:
   - bank is in the underwriting syndicate;
   - bank is an active/senior bookrunner;
   - bank is the primary lead / top-listed underwriter under the market rules.
3. Build the full sibling set and compare all primary-lead candidates plus `Other`. If the rule picks one primary bank, sibling Yes markets are competing expressions even if several banks are called “lead banks” in ordinary reporting.
4. Use the public S-1/F-1/prospectus cover and underwriting section as the highest-priority source. If hierarchy is unclear, prospectus ordering is often the settlement-relevant tie-breaker.
5. Treat Reuters/Bloomberg/WSJ statements like `active bookrunners` or `lead banks managing the deal` as syndicate evidence, not automatic primary-lead evidence. Require explicit words such as `lead-left`, `primary lead`, `front-runner`, or a top-listed official filing to push a single bank materially above 50%.
6. For sharp price moves, inspect CLOB history and recent trades; a thin book plus one large buyer can look like hard news. Do not infer a new fact from price action alone.
7. Audit expression: if a named bank is overpriced as primary lead, buying that bank's `No` may be cleaner than buying a rival's `Yes`, because it wins under all non-that-bank outcomes.
8. For detailed pitfalls and the SpaceX/Musk-specific pattern, read `references/ipo-lead-underwriter-markets.md`.

## Crypto launch FDV threshold markets

Use this workflow for markets like `Token FDV above $X one day after launch`:

1. Read the rule clock first. Many contracts settle at a fixed timestamp such as `4:00 PM ET on the calendar day following launch`, not at intraday high. Separate `can spike above threshold` from `can hold above threshold at the rule timestamp`.
2. Convert the FDV threshold into a token price using total supply: `threshold_price = FDV_threshold / total_supply`. This often makes the question concrete (e.g. $2B FDV with 10B supply = $0.20/token).
3. Use current premarket/perp/OTC pricing as the live anchor, not old launch-hype articles. Old Hyperliquid or media-implied FDVs can be stale by months.
4. Grade data sources by liquidity and relevance:
   - most relevant: current liquid perp/premarket quotes and post-launch spot/CEX data
   - secondary: DropsTab/CoinGecko/CoinMarketCap aggregators if they expose current premarket price, FDV, and volume
   - weak/noisy: Whales/OTC quotes with thin fills, old media articles, influencer launch narratives
   - practical post-launch API anchors when web pages are blocked or JS-heavy:
     - CoinGecko search: `https://api.coingecko.com/api/v3/search?query=<token-or-project>` to recover the coin id
     - CoinGecko market data: `https://api.coingecko.com/api/v3/coins/<id>?localization=false&tickers=false&market_data=true&community_data=false&developer_data=false&sparkline=false` for price, FDV, total/max supply, circulating supply, and 24h volume
     - CoinMarketCap public data API if CMC page extraction fails: `https://api.coinmarketcap.com/data-api/v3/cryptocurrency/detail?id=<cmc_id>` for statistics, FDV, supply, 24h high/low/change; and `https://api.coinmarketcap.com/data-api/v3/cryptocurrency/market-pairs/latest?slug=<slug>&start=1&limit=10&category=spot&centerType=all&sort=cmc_rank_advanced` for exchange-level prices/volumes/depth
     - Binance spot book ticker, when listed: `https://api.binance.com/api/v3/ticker/bookTicker?symbol=<SYMBOL>USDT` as a live CEX cross-check; retry if the first SSL attempt fails
5. Compare nearby strike buckets for internal coherence and execution. FDV ladders can have very wide CLOB spreads, so use executable bid/ask and reject apparent edge that exists only at midpoint.
6. Explicitly price unlock / public-sale sell pressure. Public sale FDV and ROI matter: if the threshold is far above public sale cost basis, first-day sellers may cap the move even when the project is high quality.
7. Preferred output: fair interval for the threshold, max Yes entry / minimum No entry, and a clear note that the opposite side may still be no-trade if the edge is inside the spread.

## Common Mistakes

- Treating a large volume of low-quality evidence as strong conviction.
- Treating market price as literal truth instead of one input.
- Confusing "event probably happens" with "event happens inside this exact clock."
- Treating contracts with different named actors or settlement verbs as if they were only different time buckets.
- Paying for timing precision the evidence does not justify.
- Evaluating a resolution arb like a normal prediction trade.
- Using the central estimate for Kelly sizing.
- Ignoring existing correlated exposure across the same narrative cluster.
- Calling theoretical edge a real edge when execution destroys it.
- In crypto FDV launch markets, treating an old premarket peak or launch-day spike thesis as enough evidence for a fixed next-day settlement timestamp.
- In AI model-release markets, confusing “company will announce a major model update” with “the update satisfies the exact model name/version required by the rules.” Always audit naming clauses, successor language, exclusions for higher major versions, public-access requirements, and whether product-family variants (Flash, Lite, Deep Think, TTS, robotics, etc.) count. A bullish conference/update thesis can still be bearish for a contract that specifically requires a mid-cycle version such as `3.5` while excluding `4`.

## References

- Read `references/evidence-engine.md` when grading sources, separating timing from direction, or evaluating a resolution arb.
- Read `references/probability-and-kelly.md` before generating intervals, choosing the best expression, pricing edge, or sizing a trade.
- Read `references/domain-adapters.md` when the market falls into politics/macro, crypto, or sports.
- Read `references/youtube-subscriber-thresholds.md` for YouTube/social follower-count threshold ladders, exact-count extraction notes, growth-rate modeling, and depth checks.
- Read `references/ipo-lead-underwriter-markets.md` for IPO lead-underwriter / lead-left markets, active-bookrunner vs primary-lead distinctions, and SpaceX/Musk-specific pitfalls.
- Read `references/research-and-open-source.md` when you need the research foundation or design rationale behind this skill.
- Read `references/structural-scan-arb.md` when scanning many markets for structural edge, arbitrage-like strips, stale markets, threshold/time ladder anomalies, or `edge >= X` opportunities.
- Read `references/polymarket-broad-scan-2026-05-16.md` for Charles-specific lessons from a cross-category `edge >= 10` scan: binary-outcome filtering, HIGH/LOW `hit` parsing, date/number extraction pitfalls, candidate filing-deadline/Other checks, and examples of false-positive strips.
