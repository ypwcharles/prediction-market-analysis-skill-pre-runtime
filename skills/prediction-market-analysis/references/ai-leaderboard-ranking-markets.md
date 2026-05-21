# AI Leaderboard / Model-Ranking Markets

Use this workflow for markets like `Best Chinese AI company end of May?`, `Which company has the best AI model by <date>?`, `Will a Chinese company have the best AI model?`, or any market that resolves from LMArena / Chatbot Arena / benchmark leaderboards.

## Rule-first checklist

Read the market rule before discussing model quality or company fundamentals. These markets usually resolve from a narrow table, not from broad AI strength.

Extract exactly:

- resolution source, e.g. LMArena / Chatbot Arena / Hugging Face leaderboard dataset;
- arena or benchmark, e.g. `Text Arena`, `Search Arena`, `Code Arena`, `Vision Arena`;
- category tab, e.g. `Overall`, `Math`, `Coding`, `Expert`, `Hard Prompts`;
- style-control setting, e.g. `style control off`, `no-style-control`, or default style-controlled leaderboard;
- check time and timezone;
- ranking key, e.g. `Rank` first, then score / unrounded score as tiebreaker;
- eligible entities, e.g. `primarily Chinese companies`, named outcome set, `Other`, or global company list;
- fallback behavior if the source is unavailable.

If any one of arena/category/style-control/check-time differs, treat it as a different contract.

## Source hierarchy

1. The market's own written rule and child outcome mapping.
2. The authoritative leaderboard page or official dataset named by the rule.
3. Official leaderboard changelog / model-addition announcement.
4. Model-provider launch notes only for model identity and ownership, not for settlement rank.
5. Search snippets, social posts, and model-news articles for discovery only.

For Polymarket:

- Use Gamma/Event API for discovery and rule text, but do not rely on Gamma prices as executable prices.
- Use CLOB bid/ask/depth for entry and exit assumptions.
- If the web extractor blocks a JS-heavy/Cloudflare page, use a browser fetch or `requests` with a normal `User-Agent`; LMArena pages often server-render enough table rows to parse directly.
- If the rule cites a static dataset, prefer that dataset over a search snippet.

## Leaderboard parsing discipline

When analyzing a live leaderboard market:

1. Fetch the exact leaderboard variant named by the rule.
2. Capture retrieval time and page/update date if available.
3. Parse the top eligible models, not only the market favorite.
4. Map each model to the company named in the market outcome.
5. For each contender, record:
   - global rank;
   - rank spread if shown;
   - model name;
   - company / organization;
   - score and uncertainty, e.g. `1475 ±10`;
   - vote count;
   - preliminary / provisional status;
   - whether a second model from the same company also ranks near the top.

Do not use rounded score alone when the rule says rank resolves first. If rank and rounded score conflict, state the rule hierarchy and whether unrounded data may matter.

## Company-level vs model-level edge

These markets often settle at the **company** level, while the data is model-level.

Separate:

- current top model by company;
- the company's backup models already on the board;
- the closest rival company's model;
- whether a new model could be added before check time;
- whether an existing preliminary model could drift after more votes.

A company with the current #1 model plus another model near the top has a company-level buffer, even if the #1 model's score lead is thin.

## Measuring lead quality

Do not describe a leaderboard lead as large just because it is first place. Quantify it.

Report:

- rank gap: e.g. `rank 8 vs rank 10`;
- score gap: e.g. `+3 Arena points`;
- uncertainty overlap: e.g. `1475 ±10` vs `1472 ±7`;
- vote-count stability: e.g. `3.7k votes and Preliminary` vs `9.0k votes and Preliminary`;
- rough signal strength: compare score gap to combined displayed uncertainty: `gap / sqrt(err_a^2 + err_b^2)`.

Interpretation guide:

- `gap < combined uncertainty`: thin lead; sensitive to new votes / updates.
- `gap ~ combined uncertainty`: moderate lead; still not settlement-safe.
- `gap > 2x combined uncertainty`: strong leaderboard lead, assuming no new model/catalyst before check time.

Do not overstate precision: displayed `±` is not necessarily a clean standard error, but it is still a useful warning about overlap.

## Price and fair-odds framing

For mutually exclusive company-outcome markets:

- Do not treat sum of Yes asks >100c as arbitrage; it is taker spread.
- Use executable Yes asks for entry and Yes bids for exit.
- Fair odds should combine:
  - current rank position;
  - lead quality;
  - preliminary/provisional drift risk;
  - days until check time;
  - model-addition / changelog risk;
  - company backup-model buffer;
  - source availability / fallback risk.

A current leader with a tiny, preliminary score lead can be the correct favorite while still being a bad buy if the market price already assumes settlement certainty.

## Common traps

- Confusing default leaderboard with `no-style-control` or `style control off` when the rule specifies one variant.
- Using model-provider press releases as if they were settlement evidence.
- Treating `best Chinese AI company` as a business/fundamentals question instead of a narrow leaderboard-rank question.
- Ignoring preliminary status and vote counts.
- Treating a one-model lead as a company moat without checking the same company's backup models.
- Ignoring `Other` / inactive hidden outcomes in named company markets.
- Missing recent leaderboard changelog entries that add new models shortly before the check time.
- Assuming rank is score: the rule may resolve on `Rank` first and score only as tiebreaker.

## Preferred output for this market type

Use a concise structure when the user asks for analysis rather than a formal trade decision:

1. `Conclusion` - current likely resolver and whether the market price is cheap/fair/expensive.
2. `Rule` - exact source, table, style-control setting, and check time.
3. `Current leaderboard` - top eligible companies with rank, score, votes, preliminary status.
4. `Lead quality` - rank gap, score gap, uncertainty overlap, vote-count stability.
5. `Market pricing` - executable bid/ask and whether the edge survives spread.
6. `Risks / catalysts` - new model, score drift, source fallback, rule ambiguity.
7. `Trade view` - max entry, hedge candidates, lottery candidates, and confidence.

If the user explicitly asks whether to trade, switch to the main eight-section Trade Decision template.
