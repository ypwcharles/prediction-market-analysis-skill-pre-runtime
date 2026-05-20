# Crypto Launch FDV Threshold Markets

Use this workflow for markets like `Token FDV above $X one day after launch`.

## Rule clock

Many contracts settle at a fixed timestamp such as `4:00 PM ET on the calendar day following launch`, not at intraday high. Separate `can spike above threshold` from `can hold above threshold at the rule timestamp`.

Convert the FDV threshold into a token price:

```text
threshold_price = FDV_threshold / total_supply
```

## Live anchors

Use current premarket, perp, OTC, or post-launch spot pricing as the anchor. Old media-implied FDVs or launch-hype peaks can be stale by months.

Source hierarchy:

- current liquid perp/premarket quotes and post-launch spot/CEX data
- DropsTab/CoinGecko/CoinMarketCap if they expose current price, FDV, and volume
- thin OTC/whale quotes only as weak evidence
- old media articles and influencer narratives as background only

Practical API fallbacks:

- CoinGecko search and coin market data
- CoinMarketCap public data API detail and market-pairs endpoints
- Binance spot book ticker when listed

## Execution and pressure

Compare nearby strike buckets with executable bid/ask, not midpoint. FDV ladders often have wide CLOB spreads.

Explicitly price unlock and public-sale sell pressure. If the threshold is far above public-sale cost basis, first-day sellers may cap the move even when the project is high quality.

Preferred output: fair interval for the threshold, max Yes entry / minimum No entry, and whether the opposite side is still no-trade because edge is inside the spread.
