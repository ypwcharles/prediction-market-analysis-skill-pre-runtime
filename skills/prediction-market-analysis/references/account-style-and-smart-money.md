# Account Style and Smart-Money Signals

Use this reference for wallet reviews, personal trade-history analysis, bankroll updates, PolyBeats-style alerts, whale trades, and copy-trade questions.

## Account review workflow

Separate:

- closed market-level realized PnL
- open mark-to-market PnL
- current exposure and concentration
- win rate
- payoff ratio
- profit factor
- average win and average loss
- strategy lanes where profits actually came from

For Polymarket accounts, aggregate by `conditionId` or market slug before quoting win rate, payoff ratio, or realized PnL. Outcome-token rows can double-count or misclassify market results.

## Strategy-fit labels

- `core lane`: repeatedly profitable categories or structures.
- `adjacent lane`: related to proven strengths but less tested.
- `hazard lane`: historically loss-prone, noisy, or easy to express in the wrong bucket.
- `lottery lane`: small optionality exposure, not evidence-backed edge.

Use these labels to adjust the evidence bar, sizing cap, and kill criteria.

## High-win-rate / low-payoff profiles

Do not reflexively recommend buying longer-shot contracts to improve payoff ratio. Usually improve the profile by:

- entering cleaner expressions
- trimming hazardous positions earlier
- reducing average loss
- avoiding narrative-heavy buckets with weak timing evidence
- sizing smart-money-only trades as observation positions

## Smart-money validation

Approve a smart-money signal only if:

- the wallet has relevant profitable history in this market type
- the position is meaningful relative to the wallet
- the wallet is not hedged, laddered across incompatible expressions, or already trimming
- multiple high-quality wallets independently agree, or one wallet has a verifiable information edge
- the signal maps directly to the settlement rule and deadline
- current executable price still leaves conservative edge after the alert move

Reject or cap at observation size when:

- the wallet is new or visible only in this market
- several alerts are duplicates of one wallet or follower cascade
- strong opposing flow exists from similarly credible wallets
- independent real-world evidence does not support the same side
- the thesis is narrative tension rather than rule-level evidence

## Sizing guidance

- Core lanes can receive normal conservative-Kelly sizing if independently verified.
- Adjacent lanes require a strategy-fit haircut.
- Hazard lanes should usually be observation size.
- Smart-money-only trades are observation size until independently confirmed.
- Lottery lanes should be capped at a fixed loss the user is comfortable writing to zero.
