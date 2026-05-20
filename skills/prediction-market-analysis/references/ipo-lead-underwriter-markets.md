# IPO lead-underwriter / lead-left markets

Use for prediction markets asking which bank will be the `lead underwriter`, `primary lead underwriter`, `lead-left`, or top-listed underwriter in an IPO.

## Core rule distinction

Do not confuse these roles:

- `underwriter` / `bookrunner` / `active bookrunner` / `lead manager`: participation in the syndicate; many banks can satisfy this description.
- `lead underwriter` in Polymarket-style rules: often means the **primary lead** among several banks.
- `lead-left`: the bank listed first on the prospectus cover / underwriting section; often the cleanest proxy for primary lead.
- `representatives of the underwriters`: may include several banks; order still matters if the market rule falls back to hierarchy/order.

A news report saying `Bank A, Bank B, Bank C are active bookrunners` is strong evidence that each is in the core syndicate, but only weak-to-moderate evidence that any one of them is the primary lead.

## Evidence hierarchy

1. Public S-1 / F-1 / final prospectus cover and underwriting section.
2. Official company disclosures naming `lead-left`, `primary lead`, or representatives, especially in ordered form.
3. Reuters / Bloomberg / WSJ reporting that explicitly says `lead-left`, `primary`, `front-runner`, or `top-listed`.
4. Reporting that lists active bookrunners / lead managers without hierarchy.
5. Generic articles, rewritten snippets, AI summaries, or market commentary.

## Analysis workflow

1. Read the market rule before pricing. Many markets resolve to one primary bank even when several are `lead underwriters` in ordinary financial-news language.
2. Build the full sibling set: Morgan Stanley, Goldman, JPMorgan, BofA, Citi, Other, etc. Treat them as competing outcomes when the rule asks for one primary lead.
3. Compare current executable Yes/No quotes with CLOB, not Gamma snapshots.
4. Check whether the apparent catalyst distinguishes `primary lead` from `active bookrunner`. If not, apply a heavy haircut.
5. Inspect price history and recent trades when a market moves sharply; thin CLOB books can turn one buyer into a 20pt move.
6. Consider expression quality: if you think a named bank is overpriced as primary lead, buying that bank's `No` can be cleaner than buying a single rival's `Yes`, because `No` wins under all non-that-bank outcomes.

## SpaceX / Musk-specific pattern observed

For SpaceX IPO lead-bank markets, Reuters reported a 21-bank syndicate and named Morgan Stanley, Goldman Sachs, JPMorgan, Bank of America, and Citigroup as active bookrunners. This should not be treated as Goldman/Morgan/JPM/etc. all being near-certain under a `primary lead underwriter` rule.

Musk-specific relationship evidence matters:

- Morgan Stanley has long-standing Musk/Tesla/Twitter/X financing ties.
- Reuters previously described Morgan Stanley as a front-runner for a key/lead-left role.
- Michael Grimes' return to Morgan Stanley was explicitly linked by Reuters to SpaceX IPO positioning.
- Goldman has strong mega-IPO capability and Tesla IPO lead-left precedent, but active-bookrunner status alone is not enough to assign >50% primary-lead probability.

## Pitfalls

- Treating `active bookrunner` as equivalent to the Polymarket settlement phrase `lead underwriter`.
- Treating the order in a secondary article's sentence as final prospectus order; media order can vary across reports.
- Buying all named banks because all are called `lead banks`; if the rule resolves to one primary lead, sibling Yes markets are effectively competing, and ask sums can exceed 100% due spread / overpricing.
- Ignoring `Other` / no-IPO fallback when the IPO has a long deadline.
- Sizing from top-of-book after a news spike; inspect sweep prices for realistic size.

## Useful output framing

When reporting this class of market, explicitly separate:

- syndicate participation probability;
- primary lead / lead-left probability;
- settlement-rule confidence;
- executable edge after spread/depth;
- whether the cleaner trade is `Bank A No` rather than `Bank B Yes`.
