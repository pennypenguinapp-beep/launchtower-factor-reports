# LaunchTower — Factor Model Research Report
**Report date:** 2026-09-14 · **Data as of:** 2026-09-11 (last close) · **Universe:** 81 US large/mid-cap equities

> **Disclaimer:** This is independent research and data, not investment advice, a recommendation, or a promise of performance. Past factor performance does not guarantee future results. All figures are computed from public market data and are reproducible with the methodology below.

---

## 1. Methodology (reproducible)

**Universe.** 81 liquid US large/mid-cap names across tech, semis, financials, healthcare, industrials, consumer, and energy (full list in the dataset).

**Data.** Daily adjusted close prices, 2 years, from Yahoo Finance (`yfinance`). Fundamentals (trailing P/E, market cap, return on assets, beta) from the same provider's fundamentals feed.

**Factors (computed per name):**

| Factor | Definition |
|---|---|
| `momentum_12_1` | Price 21 trading days ago ÷ price 252 days ago − 1 (12-month momentum, skipping the most recent month) |
| `ret_3m` | 3-month return |
| `vol_ann` | Annualized standard deviation of daily returns over the last 63 trading days |
| `beta` | Provider-reported beta vs. market |
| `roa` | Return on assets (quality proxy) |
| `pe` | Trailing P/E (informational) |
| `log_mcap` | Natural log of market cap (size tilt) |

**Composite score.** Each factor is cross-sectionally z-scored, then combined:

```
score = z(momentum_12_1) + z(ret_3m) − z(vol_ann) + z(roa) − 0.5·z(beta) + 0.25·z(log_mcap)
```

**Backtest (equal-weight, top 20 by score, rebalanced monthly, 12 months to 2026-09-11):**
- Top-20 portfolio return: **+62.7%**
- Full-universe equal-weight return over the same window: +28.4%
- Max drawdown (top-20): **−3.8%**
- Approx. Sharpe (EW, 12m): 4.3

*Note: this is an in-sample, monthly-rebalanced, equal-weight construction with no transaction costs or shorting constraints. Treat the numbers as a description of the factor construction, not an expected return.*

---

## 2. Top 15 by composite score

| # | Ticker | Momentum 12-1 | 3m ret | Ann. vol | ROA | Beta | P/E | Score |
|---|--------|---------------|--------|----------|-----|------|-----|-------|
| 1 | VLO | +122.2% | +53.3% | 34.9% | 10.6% | 0.57 | 16.3 | 4.60 |
| 2 | NVDA | +27.3% | +6.7% | 40.2% | 53.6% | 2.22 | 27.6 | 4.53 |
| 3 | MU | +531.8% | −2.1% | 94.0% | 34.9% | 2.22 | 22.0 | 4.29 |
| 4 | MPC | +98.2% | +52.2% | 34.3% | 8.7% | 0.53 | 13.7 | 4.09 |
| 5 | MA | −3.1% | +17.2% | 21.7% | 24.1% | 0.74 | 31.3 | 3.40 |
| 6 | AAPL | +33.2% | +12.5% | 31.9% | 27.1% | 1.09 | 38.1 | 3.35 |
| 7 | PSX | +79.8% | +46.5% | 30.7% | 6.0% | 0.70 | 14.8 | 3.24 |
| 8 | V | +7.2% | +16.3% | 20.4% | 19.1% | 0.76 | 31.6 | 3.00 |
| 9 | MRK | +64.5% | +20.0% | 37.1% | 11.9% | 0.23 | 115.1 | 2.40 |
| 10 | NEM | +44.4% | +30.2% | 45.6% | 15.8% | 0.54 | 15.9 | 2.23 |
| 11 | JNJ | +49.4% | +12.0% | 25.5% | 8.6% | 0.24 | 30.8 | 2.15 |
| 12 | LLY | +60.6% | −3.8% | 33.0% | 20.4% | 0.50 | 37.5 | 2.09 |
| 13 | COP | +35.6% | +19.9% | 29.3% | 7.5% | 0.13 | 18.2 | 1.95 |
| 14 | XOM | +44.6% | +14.0% | 25.5% | 17.5% | 0.06 | 21.3 | 1.91 |
| 15 | ABBV | +17.4% | +15.2% | 29.2% | 10.5% | 0.28 | 72.4 | 1.85 |

## 3. Bottom 10 by composite score

| Ticker | Momentum 12-1 | 3m ret | Ann. vol | ROA | Beta | P/E | Score |
|--------|---------------|--------|----------|-----|------|-----|-------|
| KLAC | +119.2% | −25.0% | 76.7% | 20.8% | 1.44 | 49.2 | −2.33 |
| BA | +4.7% | −5.0% | 33.2% | −2.0% | 1.21 | 75.7 | −2.35 |
| QCOM | +3.6% | −9.9% | 48.4% | 11.6% | 1.68 | 20.8 | −2.39 |
| ISRG | −12.0% | −10.6% | 47.4% | 10.5% | 1.47 | 42.4 | −2.56 |
| NKE | −43.3% | −19.1% | 31.9% | 7.0% | 1.11 | 17.5 | −2.73 |
| CE | −5.3% | −10.7% | 40.3% | 2.6% | 0.76 | n/a | −2.90 |
| LULU | −27.9% | −18.8% | 52.1% | 14.4% | 0.86 | 8.1 | −2.96 |
| TSLA | −7.8% | −8.4% | 56.2% | 1.9% | 1.85 | 332.2 | −3.91 |
| ORCL | −48.7% | −18.1% | 55.8% | 6.4% | 1.73 | 23.5 | −4.55 |
| FMC | −72.5% | +1.9% | 66.8% | 0.6% | 0.42 | n/a | −4.77 |

## 4. Read-through

- **Energy refiners and majors** (VLO, MPC, PSX, COP, XOM) dominate the top of the ranking — strong 12-1 momentum, low beta, and solid ROA.
- **Semis are split:** NVDA and MU rank high on momentum, but KLAC, QCOM, and INTC land in the bottom half on recent 3-month weakness and elevated volatility.
- **Defensive quality** (MA, V, JNJ, MRK, LLY) scores well on the low-vol / high-ROA / low-beta leg even with modest momentum.
- **Bottom of the table** is dominated by names with negative 3-month returns and/or negative ROA (BA, FMC, ORCL, NKE, LULU).

## 5. Reproducibility

- Prices: `yfinance` 2y daily adjusted close, 81 tickers.
- Fundamentals: `yfinance` `.info` (trailingPE, marketCap, returnOnAssets, beta).
- Factor math and scoring: pure pandas/numpy, no proprietary data.
- Full per-name factor table: `factor_scores.csv` (81 rows × 9 columns).
- Raw prices: `prices_2y.csv` (501 trading days × 81 tickers).

**LaunchTower** — independent market-data research. Not investment advice.
