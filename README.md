# NSE Gap Lab — When the Market Gaps Down, What Happens Next?

**An interactive Nifty 500 opening-gap backtesting research project** that follows an apparently strong trading signal through robustness checks and chronological validation.

> **Main finding:** A 5–<7% opening gap-down recovery rule achieved a **73.77%** historical net win rate, but **94 of 183 signals occurred on 2 March 2026**. Without that day, the win rate was **53.93%**. In a later validation period it was **50.00%**. **None of 29 training patterns above 70% passed the predefined validation criteria.**

## Explore the dashboard

Open [`index.html`](index.html) in a browser, or publish this repository using **GitHub Pages**. The dashboard is responsive and works without a backend, API key, or JavaScript framework.

**Suggested public URL** (after you enable Pages on a repository named `nse-gap-recovery-backtesting`):
`https://manivamshit.github.io/nse-gap-recovery-backtesting/`

## At a glance

| Metric | Research sample |
|---|---:|
| Daily stock observations | 120,482 |
| Stocks | 497 |
| Trading sessions | 246 |
| Approximate sample | 18 Nov 2025 – 17 Nov 2026 |
| Hypothetical round-trip cost | 0.20 percentage points |
| Entry / exit | Open / same-day close |
| Historical trading patterns validated at 70%+ | **0** |

## Research question

**Do stocks that open sharply below their previous close recover during the same session, and is that behavior robust enough to support a repeatable strategy?**

We tested long gap-down recovery (buy open, sell close), contrasted it with short continuation, and examined sensitivity to exceptional days and time periods.

## Key findings

### 1. Headline results are sensitive to one session

| 5–<7% gap-down long strategy | Trades | Signal days | Net trade win rate | Mean net trade return | Mean daily net return |
|---|---:|---:|---:|---:|---:|
| All dates | 183 | 66 | 73.77% | +1.82% | +0.31% |
| Excluding 2 March 2026 | 89 | 65 | 53.93% | +0.68% | +0.27% |
| Excluding 2 March and the five best remaining days | 84 | 60 | 51.19% | +0.32% | −0.27% |

On **2 March 2026**, 94 qualifying trades had an average net return of **+2.90%**. That single day accounted for **51.4%** of all qualifying trades. The sum of percentage returns across stocks is **not** a portfolio return.

Removing the five best days is a **sensitivity test**, not a tradable exclusion rule.

### 2. Chronological validation weakened the apparent edge

The validation period starts **3 August 2026**.

| 5–<7% long | Training | Validation |
|---|---:|---:|
| Trades | 157 | 26 |
| Signal days | 48 | 18 |
| Win rate | 77.71% | 50.00% |
| Mean net return per trade | +2.04% | +0.45% |
| Mean daily net return | +0.62% | −0.52% |
| Worst trade | −5.69% | −14.16% |

Without 2 March, training win rate was **55.56%** across 63 trades on 47 days; validation remained **50.00%** across 26 trades on 18 days.

### 3. Pattern discovery did not produce a validated 70% system

**What is a trading pattern?** It is a rule made by combining conditions such as how far a stock gaps down at the open, whether its previous day was bearish, and how actively it traded. This is a Python rule-based backtest, **not AI model training**.

The exploratory scan tested gap ranges, prior-day trend, prior turnover, prior relative volume, and market opening breadth. It found **29 overlapping historical trading patterns** with ≥70% training win rates and positive average trade returns. **Zero** met the validation screen (≥70% wins, positive average return, at least 15 trades on at least 8 days).

The highest training win rate was **84.38%** (64 trades on 20 days). Its validation result was **75%**, but only **8 trades on 7 days**—below the preselected sample thresholds.

## Methodology

### Data

Historical daily NSE stock-level OHLC and volume, with previous close, grouped by stock and date. The research uses a roughly one-year sample of 497 stocks and 246 trading sessions.

### Formulas

```text
Opening gap (%)     = (Open / Previous close - 1) × 100
Long gross (%)      = (Close / Open - 1) × 100
Long net (%)        = Long gross (%) - 0.20
Short net (%)       = -Long gross (%) - 0.20
Trade win rate (%)  = 100 × count(net trade returns > 0) / trade count
Daily net return    = average net trade return across signals on a given day
Average daily net   = average of daily net returns across signal days
```

Gap bins are half-open by magnitude: `1–<3%`, `3–<5%`, `5–<7%`, and `7–<10%`. A 5–<7% gap-down means `-7% < opening gap <= -5%`.

### Validation design

- Chronological split: training before **3 August 2026**, validation from that date onward.
- Minimum exploratory screening thresholds: **40 training trades on 15 days** and **15 validation trades on 8 days**.
- Robustness checks: excluding **2 March 2026** and separately excluding the **five strongest remaining days**.
- Equal-weight daily basket statistics are reported separately from per-trade averages.

## Limitations — read before drawing trading conclusions

1. **Opening-price fills are not verified.** Daily OHLC cannot prove executable fills at official opens. Filters based on today's opening breadth may not be available early enough to trade at those prices.
2. **Trading costs are assumed.** A flat 0.20 percentage-point round-trip deduction is illustrative and does not model actual brokerage, taxes, slippage, spreads, or short-selling constraints.
3. **Corporate-action adjustment is incomplete.** Several extreme gaps were flagged as possible split or corporate-action artifacts; the events have **not been individually verified**. Gaps below 10% may also contain adjustment issues.
4. **Constituent survivorship bias.** Using a contemporary Nifty 500 constituent list rather than historical membership can bias results.
5. **Limited market history.** Roughly one year does not represent multiple market regimes.
6. **Multiple testing and overlapping signals.** Twenty-nine rules are not twenty-nine independent confirmations. Once validation outcomes are inspected to design more rules, that period is no longer a pristine holdout.
7. **Not an executed portfolio.** Equal-weight daily averages are hypothetical; there is no verified capital allocation, order book, capacity, funding, or risk management model.

## Reproducibility and provenance

The website contains **static, manually transcribed summary results** from Steps 23–26 of the research. It does **not** regenerate results from raw NSE data in the browser. To fully reproduce the analysis, add your actual cleaned input data (only if redistribution is permitted), Colab notebooks, and exported result CSVs.

Suggested repository structure:

```text
nse-gap-recovery-backtesting/
├── index.html                    # Interactive, static dashboard
├── README.md                     # Research narrative and methods
├── data/
│   └── README.md                 # Dataset and CSV upload instructions
└── assets/
    └── (optional future figures)
```

Step 26 output files you can add under `data/`:

- `STEP26_MARCH_SENSITIVITY.csv`
- `STEP26_BEST_FIVE.csv`
- `STEP26_BEST_FIVE_SENSITIVITY.csv`
- `STEP26_TRAIN_VALIDATION.csv`
- `STEP26_CROWDED_DAYS.csv`
- `STEP26_QUIET_VS_CROWDED.csv`
- `STEP26_DAILY_RETURNS.csv`

Step 25 outputs: `STEP25_DISCOVERY_ALL.csv`, `STEP25_TRAIN_70PLUS.csv`, `STEP25_VALIDATION.csv`, `STEP25_VALIDATED_70PLUS.csv`, and `STEP25_BASELINE.csv`.

## Publish to GitHub Pages

1. Create a **public** GitHub repository, e.g. `nse-gap-recovery-backtesting`.
2. Upload the contents of this folder, keeping `index.html` at the repository root.
3. Go to **Settings → Pages**.
4. Under **Build and deployment**, select **Deploy from a branch**, then `main` and `/(root)`.
5. Save, wait for deployment, and open the Pages URL shown by GitHub.

No npm, build process, paid hosting, or API key is required.

## Project conclusion

> Large negative opening gaps can coincide with intraday recoveries, but high aggregate historical win rates can be dominated by a small number of correlated market sessions. In this one-year Nifty 500 study, no 70%+ strategy met the predefined later-period validation criteria. The result is a demonstration of the importance of robustness, time-split validation, and realistic execution assumptions.

**Educational research only — not investment advice or evidence of guaranteed future returns.**


## Independence and educational-use disclaimer

Independent educational research. Not affiliated with or endorsed by NSE, NSE Indices or SEBI. This dashboard does not offer investment advice, stock recommendations or predictions. Aggregated statistics are presented for portfolio demonstration; publication remains subject to applicable source-data terms.

Before redistributing raw exchange-derived data or stock-level records, verify the applicable licensing and redistribution permissions.
