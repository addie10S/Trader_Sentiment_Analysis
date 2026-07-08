
Data Science / Analytics Intern — Round-0 Assignment (Primetrade.ai)

Analyzes how Bitcoin market sentiment (Fear/Greed) relates to trader behavior and
performance on Hyperliquid, using historical trade data and the Fear/Greed Index.

## Project Structure
trader-sentiment-analysis/

├── Trader_sentiment_analysis.ipynb   # main notebook (Parts A, B, C + bonus)

├── historical_data.zip                # trade history (zipped)

├── fear_greed_index.csv               # daily sentiment index

├── README.md                          # this file

└── WRITEUP.md                         # methodology, insights, strategy recommendations

## Setup

**Requirements:** Python 3.9+ and Jupyter Notebook/JupyterLab installed.

1. Download this repository:
   - On the repo page, click the green **"Code"** button → **"Download ZIP"**
   - Extract the ZIP to a folder on your computer

2. Install dependencies:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```
```

## How to Run

1. Launch Jupyter:
```bash
jupyter notebook
```

2. Open `Trader_sentiment_analysis.ipynb`

3. Run all cells top to bottom (**Kernel → Restart & Run All**). The notebook reads
   both data files using relative paths, so no file-path changes are needed as long as
   the notebook and CSVs stay in the same folder:
```python
hist_data = pd.read_csv("historical_data.csv")   
fg_data = pd.read_csv("fear_greed_index.csv")
```

## What the Notebook Covers

- **Part A — Data Preparation:** row/column counts, missing values, duplicate checks,
  timestamp parsing and date alignment between the two datasets, key metrics (daily PnL,
  win rate, average trade size, trades/day, long-short ratio).
- **Part B — Analysis:** performance comparison across Fear/Greed/Neutral sentiment
  (PnL, win rate, drawdown proxy), behavioral changes by sentiment (trade frequency,
  size, long/short bias), and 3 trader segments (position size, trade frequency,
  win-rate consistency), each backed by charts.
- **Part C — Actionable Output:** 2 strategy recommendations derived from the analysis
  — see `WRITEUP.md` for full details.
- **Bonus:** K-means clustering of traders into 3 behavioral archetypes based on trade
  size, frequency, and win rate.

## Notes / Known Data Limitations

- No `leverage` column exists in the raw trade data, and no margin/equity field is
  available to derive one — `Size USD` is used as a position sizing proxy wherever
  leverage would otherwise apply.
- The raw `Timestamp` column (numeric) is corrupted (only 7 unique values across
  211k+ rows) — `Timestamp IST` is used for all date alignment instead.
- Win rate and PnL metrics are computed on **closing trades only** (`Close Long`,
  `Close Short`, `Sell`, etc.), since opening trades have `Closed PnL = 0` by
  definition and would dilute these metrics if included.
- The trader universe is small (32 unique accounts), noted as a caveat on all
  account-level segmentation and clustering results.

## Full Write-up

See `WRITEUP.md`  for methodology, key insights, and strategy
recommendations.
