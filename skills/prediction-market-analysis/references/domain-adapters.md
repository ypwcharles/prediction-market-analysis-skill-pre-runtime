# Domain Adapters

## Purpose

Use the same core framework across domains, but adjust evidence priorities, common traps, and calibration instincts by market type.

## Politics / Macro

### Preferred evidence sources

- official government releases
- central-bank statements
- court rulings
- election authorities
- polling aggregators with transparent methodology
- major macro data releases

### Common traps

- social-media narrative outrunning settlement reality
- headline interpretation without reading the primary release
- double-counting polling and commentary about polling
- overreacting to low-signal punditry

### When to trust market price more

- high liquidity
- multiple platforms converging
- event close to resolution
- heavy institutional attention

### When to widen intervals

- legal ambiguity
- contested counting / delayed reporting
- policy wording can be interpreted multiple ways
- event still depends on multiple future decisions

## Crypto

### Preferred evidence sources

- protocol docs and governance announcements
- exchange notices
- issuer statements
- chain data
- ETF / regulatory filings
- liquid related-market price action

### Common traps

- treating narrative momentum as causal information
- over-weighting influencer commentary
- assuming all on-chain activity is informative
- ignoring basis between related crypto markets

### When to trust market price more

- deep liquidity
- strong cross-venue convergence
- direct ties to liquid underlying spot / futures markets

### When to widen intervals

- thin liquidity
- unclear catalyst timing
- governance ambiguity
- heavy reliance on social signals

## Tech / AI

### Preferred evidence sources

- the exact benchmark, leaderboard, or product page named in the market rules
- official model release notes and lab announcements
- reproducible leaderboard snapshots taken close to the resolution time
- direct browser verification when search results or aggregators are stale

### Common traps

- treating today's leaderboard rank as if it were locked in at the resolution timestamp
- using third-party summaries instead of the rule-named source
- forgetting rule details like style control, tab selection, or exact check time
- confusing product hype or social narrative with benchmark-leading status

### When to trust market price more

- the resolution check is very near
- the named leaderboard is stable and not changing daily
- multiple top models are not clustered within noise / error bars

### When to widen intervals

- the market resolves off a single future snapshot rather than a cumulative result
- the top of the leaderboard is tightly clustered
- major labs can still ship or update models before the deadline
- browser verification shows the current leader, but the contract settles at a later fixed check time

### Special note for model-release / product-launch markets

For markets about a new AI model being released by a deadline, do not price the contract from generic hype like “I/O will be AI-heavy.” Build an explicit release-cadence and rule-scope model.

Checklist:
- read whether the contract requires a **specific name** (e.g. `Gemini 3.5`) or a broader functional class (e.g. `new Gemini reasoning flagship`). Naming-specific markets can be much worse expressions than broader flagship-release markets even when the directional thesis is correct.
- compare the exact rule verbs: `announced`, `released`, `made available`, `GA`, `preview`, `open beta`, `open rolling waitlist`, `closed beta`, and `trusted testers` are materially different.
- use official model/API changelogs and prior conference cadence as first-class evidence. For Google/Gemini-style markets, check the previous I/O cycle: what was announced before I/O, at I/O, and in the 2–6 weeks after I/O.
- measure time since the last major model generation with an actual date calculation; a 6–8 month gap before a major developer conference supports a higher release probability than a fresh post-launch window.
- incorporate competitive pressure explicitly: frontier-model labs may ship or at least preview flagship models around major developer events when rivals are updating quickly, but this affects **direction** more than rule satisfaction.
- separate `P(any model/product update)` from `P(qualifying reasoning flagship)` from `P(public availability by deadline)` from `P(oracle/UMA interprets it as qualifying)`.
- for model-release markets around developer conferences, widen intervals for the common failure path: strong keynote demo or trusted-tester preview now, but public/open availability after the deadline.
- for Kelly sizing, cap single-event product-release exposure below mechanical Kelly when the edge depends on product naming, public-availability interpretation, or UMA/admin discretion. A high subjective probability like 90–95% can imply very large fractional Kelly at prices around 0.70; apply hard portfolio caps and model-error haircuts.

### Special note for leaderboard markets

For markets that resolve on a future leaderboard check (for example LM Arena / Chatbot Arena style contracts), classify the trade as a time-sensitive expression even if the prompt sounds directional.

Checklist:
- verify the exact leaderboard page and tab named by the rules
- verify any switches such as style control on/off
- verify the exact resolution timestamp and timezone
- compare the **child-market description** against the **parent event wrapper**; if they differ (for example `Rank` vs `Score`), trust the child-market rule text first and treat the wrapper as discovery-only metadata
- check whether the market **stops trading before** the rule check timestamp; if there is a close-before-resolution window, explicitly haircut edge for the blind period
- in live Polymarket scans, also compare the raw API `endDate` / `endDateIso` against the written rule check time. They can conflict materially. A market may show a raw endDate like `2026-04-30T00:00:00Z` while the rule text settles on `April 30, 2026, 12:00 PM ET`, creating a real blind window of many hours. Treat the written rule clock as the true settlement clock and the raw endDate as only an operational trading-close clue.
- if raw timestamps imply the market closes before the rule check, do **not** price the trade as if current leaderboard state flows directly to settlement; haircut harder for update risk during the blind window
- separate `best model right now` from `best model at check time`
- for company-winner markets, distinguish `latest released model` from `highest-ranked model from that company`; the market usually resolves on the company's top model at check time, not whichever launch has the most hype
- do not assume a newly released flagship is the relevant challenger if an older sibling model from the same lab still ranks higher on the rule-named table
- when the live leaderboard shows `Score ±CI`, `Rank Spread`, and vote counts, treat those as first-class evidence about stability. A small point lead with overlapping confidence intervals and rank-spread ranges that still include `#1` is **not** a locked ranking, even if the headline rank is currently first.
- if the user claims the leaderboard is already effectively fixed, verify with methodology, not intuition: `lmarena/arena-rank` explicitly recomputes ratings from pairwise votes and confidence intervals, so ratings are estimation outputs that continue to move as more votes arrive.
- if you need empirical stability evidence, pull historical public snapshots (for example daily Arena snapshot repos) and compare score / CI / vote drift after a model first appears. Use this to separate `directionally stable leader` from `already locked and no longer movable`.
- reject trades where the market is pricing the current rank as if no meaningful update risk remains

Operational note from live Polymarket scans:
- the rule-linked `lmarena.ai` / Hugging Face mirror may currently redirect to `arena.ai/leaderboard/text`
- the page may default to **Style Control = On** even when Polymarket rules specify **Style Control = Off**
- for settlement work, explicitly switch to the rule-matching toggle before reading ranks
- switching style-control modes on the live site can trigger bot / reCAPTCHA verification, especially in browser automation
- in Charles's current environment, `arena.ai` can also be intermittently unreachable from browser automation, markdown extractors, and even direct `curl`; do not silently replace it with weaker sources and continue as if verified
- if you cannot directly verify the rule-matching toggle state on the rule-named page, treat current visible ranks, search snippets, and third-party summaries as **weak directional evidence only**, not settlement-grade evidence
- do not rely on overview cards, cached third-party summaries, or style-control-on snapshots as substitutes for the rule-named table
- do **not** substitute `openlm.ai/chatbot-arena` or other aggregate / meta leaderboards for the rule-named `arena.ai/leaderboard/text` table; they can show a different ranking methodology and a different top model set
- practical implication: a contract that looks obviously mispriced from an aggregate leaderboard can be fairly priced once you switch to the exact rule-named Arena page with the correct style-control setting
- practical trading implication: if direct verification of `arena.ai/leaderboard/text` is unavailable and the market is not massively dislocated, default to `NO TRADE` rather than pretending the current leaderboard state is confirmed
- useful historical fallback: the public repo `oolong-tea-2026/arena-ai-leaderboards` stores **daily snapshots** of Arena leaderboards and is good for checking whether ranks and scores drift after launch. Use it to test claims like “scores are basically fixed once a model appears” or “rank order never changes.”
- but keep the scope straight: that repo is strongest for **overall text** history and other exposed boards it snapshots; it is **not proof of the exact rule-named subtable's history** unless the matching subtable is actually present. Use it as behavioral evidence about Arena drift and flip frequency, not as a substitute settlement source for a missing Math/Style-Control-Off daily history.
- practical inference from live use: new models can appear on Arena very quickly after launch, so separate **“can get listed”** from **“can climb to #1 in the remaining window.”** The latter is the harder event and should carry most of the timing haircut.

## Sports

### Preferred evidence sources

- official injury reports
- lineup announcements
- league and team releases
- weather data
- market-wide price movement across books and exchanges

### Common traps

- fan loyalty and recency bias
- rumor-driven overreaction
- not distinguishing probability from payout
- underestimating lineup uncertainty

### When to trust market price more

- major games with deep liquidity
- broad bookmaker alignment
- close-to-start markets with confirmed lineups

### When to widen intervals

- uncertain player availability
- late-breaking weather risk
- lower-tier or niche events with thin books
- markets dominated by casual sentiment
