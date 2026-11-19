# Research result exports — Steps 25 and 26

This folder contains **12 user-provided CSV exports** from historical NSE Nifty 500 opening-gap research. They are summary statistics and daily aggregate signal results, not the raw daily OHLC source dataset.

## Step 25 — pattern discovery

- `STEP25_DISCOVERY_ALL.csv` — 530 tested rule summaries.
- `STEP25_TRAIN_70PLUS.csv` — 29 training-selected rules with win rates ≥70%.
- `STEP25_VALIDATION.csv` — later-period validation of those 29 rules, with sample sufficiency and pass flags.
- `STEP25_VALIDATED_70PLUS.csv` — header-only export: **zero qualifying validated patterns**. This is expected, not a missing file.
- `STEP25_BASELINE.csv` — training/test metrics for long and short gap bins.

## Step 26 — 5–<7% gap-down robustness audit

- `STEP26_MARCH_SENSITIVITY.csv` — full sample vs excluding 2 March 2026.
- `STEP26_BEST_FIVE.csv` — five best remaining signal days.
- `STEP26_BEST_FIVE_SENSITIVITY.csv` — removing the five strongest days as a stress test.
- `STEP26_TRAIN_VALIDATION.csv` — training/test with and without 2 March.
- `STEP26_CROWDED_DAYS.csv` — 15 highest signal-count days.
- `STEP26_QUIET_VS_CROWDED.csv` — signal-day concentration comparison.
- `STEP26_DAILY_RETURNS.csv` — 66 signal days, each with daily mean net return, signal count, and training/test label.

## Field definitions

- `Trades`, `Signal_Days`/`Days`: qualifying stock observations and unique signal dates.
- `Win_Rate_Pct`, `Trade_Win_Pct`: percentage of net-positive stock trades.
- `Avg_Net_Pct`, `Avg_Trade_Net_Pct`: average net stock trade return in percentage points.
- `Avg_Daily_Net_Pct`, `Daily_Net_Pct`: average net return within each signal day, with signal days equally weighted when aggregated.
- `Positive_Days_Pct`: fraction of signal days whose average net return is positive.
- `Sum_Net_Pct`: arithmetic sum of stock-level percentage returns on a day; **not a portfolio return**.
- `Enough_Test_Data`, `Passed_70_Pct`: sample-sufficiency and validation screening flags from the Step 25 export.

**Limitations:** 0.20 percentage-point round-trip cost assumed; buy at open, sell at close; no stops or targets; unverified fills, corporate actions, liquidity, survivorship bias, and short history. The CSVs do not independently prove execution feasibility.

**Disclaimer:** Independent educational research. Not affiliated with or endorsed by NSE, NSE Indices or SEBI. No investment advice, recommendations or predictions. Publication is subject to applicable source-data terms.
