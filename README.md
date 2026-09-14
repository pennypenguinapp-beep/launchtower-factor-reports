# LaunchTower — Factor Research (2026-09)

**Latest signal: 2026-09-14** (data through 2026-09-11 close)

## What this is

A reproducible, dated factor-score dataset for 44 liquid US mega-caps.
Each row is a ticker with:

| Column | Definition |
|---|---|
| `ret_12_1` | 12-month return skipping the most recent month (classic momentum) |
| `ret_3m` | 3-month return (short-term momentum) |
| `vol_6m` | 6-month annualized volatility (quality/low-vol proxy) |
| `rs_vs_universe` | 12-1 return minus universe mean (relative strength) |
| `score` | Composite: 50% rank(12-1) + 25% rank(3m) + 25% rank(inverted vol) |

## How to reproduce

```python
import yfinance as yf, pandas as pd, numpy as np

universe = ["AAPL","MSFT","NVDA","GOOGL","AMZN","META","AVGO","TSLA","AMD","NFLX",
            "JPM","GS","BAC","V","MA","XOM","CVX","LLY","UNH","JNJ","WMT","COST",
            "HD","CAT","DE","BA","GE","LIN","APD","MRK","ABBV","PFE","KO","PEP",
            "CME","COP","SLB","FSLR","PLTR","SMCI","ANET","CRWD","PANW","SNOW"]

data = yf.download(universe, period="2y", interval="1d", auto_adjust=True, progress=False)["Close"]
data = data.dropna(axis=1, how="any")
px = data.pct_change()

ret_12_1 = (data.iloc[-21]/data.iloc[-252] - 1)
ret_3m   = (data.iloc[-63]/data.iloc[-126] - 1)
vol_6m   = px.iloc[-126:].std() * np.sqrt(252)
rs       = ret_12_1 - ret_12_1.mean()
rank = lambda s: s.rank(pct=True)
score = 0.5*rank(ret_12_1) + 0.25*rank(ret_3m) + 0.25*(1-rank(vol_6m))

out = pd.DataFrame({"ret_12_1": ret_12_1, "ret_3m": ret_3m,
                    "vol_6m": vol_6m, "rs_vs_universe": rs, "score": score})
out = out.sort_values("score", ascending=False).round(4)
```

## Files

- `factors_2026-09-14.csv` — latest dated signal (44 tickers)
- `report_2026-09-14.md` — full research report (methodology, top/bottom picks)

## Buy the full pack

**LaunchTower Factor Research Pack (2026-09) — $29**

Includes: dated factor dataset (CSV), full research report (Markdown),
reproduction script (Python), and this repo.

> ⚠️ **Checkout status (2026-09-16):** The Whop product object exists
> (`prod_62MnGDMxlvZUg`) but the checkout/plan has not yet been activated
> due to a currency-field integration gap on the listing tool. A live
> purchase link will appear here as soon as it is resolved.
> Store: https://whop.com/biz_PafLwqOjrf2HRB/

## Disclaimer

This is research data, not investment advice. Past factor performance does not
guarantee future results. Do your own due diligence.

---
*LaunchTower — independent market-data research.*
