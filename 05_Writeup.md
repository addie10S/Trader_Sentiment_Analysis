## Methodology

Two datasets were used: Hyperliquid trader history (211,224 trade level rows) and the
Bitcoin Fear/Greed Index (2,644 daily sentiment readings). Trade timestamps were parsed
from the  `Timestamp IST` column (the raw numeric `Timestamp` column was found to be
corrupted, only 7 unique values across 211k rows, and was excluded from alignment).
Trades were merged to sentiment on calendar date, matching 211,218 of 211,224 rows
(99.997%).

A key data quality finding: roughly half of all rows are position *opening* trades
(`Open Long`, `Open Short`, `Buy`), which have Closed PnL = 0 by definition since PnL
only realizes on close. All win rate and PnL-based metrics were therefore computed on
closing trades only (104,730 rows) to avoid diluting results with non-events.

No `leverage` field exists in the raw data, and no margin/equity column is available to
derive one, leverage distribution could not be computed. `Size USD` (trade size) was
used as a position sizing proxy wherever leverage would otherwise apply. The trader
universe is also small (32 unique accounts), which is noted as a caveat on all
account-level segmentation.

## Insights

**1. Extreme sentiment states, not simple Fear-vs-Greed, separate performance best.**
Win rate and mean PnL peak on Extreme Greed days (89.1% win rate, $130.13 mean PnL) and
are lowest on Neutral and Extreme Fear days (~$71 mean PnL, with Extreme Fear's win rate
dropping to 76.2%). The moderate Fear/Greed states cluster closer together the real
signal is at the extremes.

**2. Traders overtrade into Extreme Fear rather than pulling back.**
Trade frequency nearly triples on Extreme Fear days (1,528.6 avg trades/day) versus any
other sentiment state, with the largest average trade size ($7,816 on regular Fear days)
also occurring during fear conditions. Long/short bias also flips with sentiment 63.6%
long-biased on Fear days versus 41.8% long on Greed days, suggesting a "buy the dip"
pattern rather than panic behavior.

**3. Win rate consistency does not predict total profitability; size and activity do.**
High size and high frequency accounts earned substantially more total PnL than
low size/infrequent accounts (~$417K vs $227K, and ~$497K vs $147K respectively). But
consistent winners (higher win rate) actually earned *less* total PnL ($282K) than
inconsistent accounts ($362K), a few large wins appear to matter more than a high hit
rate for this trader group.

**Bonus — Behavioral archetypes via clustering:** K-means clustering (3 clusters, on
trade size/frequency/win rate) revealed three distinct trader types: a small whale
group (4 accounts, $22,540 avg size, 85% win rate), a larger high frequency group (15
accounts, 8,085 avg trades, 75% win rate), and a sharp shooter group (13 accounts,
fewer trades, 97% win rate), reinforcing insight 3 that selective, lower frequency
trading correlates with higher win rate but not necessarily higher total profit.

## Strategy Recommendations

**1: Scale size specifically on Extreme Greed days, not all Greed days.**
Extreme Greed shows the strongest performance (89.1% win rate, $130.13 mean PnL),
clearly outperforming regular Greed (76.3% win rate, $84.80 mean PnL). 
Point:
increase position size only when sentiment crosses into Extreme Greed; treat regular
Greed days at baseline sizing.

**2: Reduce activity during Extreme Fear for high frequency traders (Cluster
0), leave high consistency traders (Cluster 2) unchanged.** Overall activity nearly
triples on Extreme Fear days while win rate drops, but this risk is concentrated in the
high frequency cluster (75% win rate at 8,085 trades), not the sharp shooter cluster
(97% win rate at fewer trades). 
Point: during Extreme Fear, cap trade frequency
and size for high-frequency/lower win rate traders; no change needed for traders whose
existing behavior is already selective.
