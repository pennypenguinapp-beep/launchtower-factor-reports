# LaunchTower — Factor Research (2026-09)

**Latest signal: 2026-09-15** (data through 2026-09-11 close)

## What this is

A reproducible, dated factor-score dataset for 151 liquid US large-caps.
Each row is a ticker with:

| Column | Definition |
|---|---|
| `ret_1m` | 1-month return |
| `ret_3m` | 3-month return |
| `ret_6m` | 6-month return |
| `ret_12m` | 12-month return |
| `vol_ann` | Annualized volatility (252-day) |
| `max_drawdown` | Max drawdown over the period |
| `from_52w_high` | Distance from 52-week high |
| `momentum_score` | Momentum percentile (0-1) |
| `quality_score` | Quality percentile (0-1, inverse volatility) |
| `composite_score` | Composite: 50% momentum + 50% quality |

## How to reproduce

```python
import yfinance as yf
import pandas as pd
import numpy as np
from datetime import datetime

universe = ["AAPL","MSFT","NVDA","GOOGL","AMZN","META","TSLA","AVGO","AMD","NFLX","ORCL","CRM","ADBE","CSCO","QCOM","TXN","MU","INTC","IBM","NOW","INTU","PLTR","SNOW","DDOG","NET","CRWD","PANW","ZS","FTNT","ANET","SMCI","ARM","MRVL","LRCX","AMAT","KLAC","ASML","ON","MPWR","MCHP","TER","ADSK","CDNS","SNPS","GFS","MRNA","LLY","NVO","UNH","JNJ","PFE","MRK","ABBV","BMY","TMO","DHR","ISRG","VRTX","REGN","AMGN","GILD","BSX","CVS","CI","HUM","ABT","SYK","ALGN","MDT","BABA","JD","PDD","SE","BIDU","UBER","ABNB","DASH","COIN","HOOD","PYPL","V","MA","AXP","BLK","SCHW","C","BAC","WFC","JPM","GS","MS","SPGI","ICE","CME","MCO","AIG","MET","PRU","TRV","ALL","CB","PGR","SPOT","T","VZ","TMUS","CMCSA","DIS","WMT","COST","HD","MCD","NKE","SBUX","TGT","UPS","CAT","DE","GE","BA","HON","UNP","CSX","NSC","LIN","APD","ECL","SHW","EMR","ETN","PH","ROK","WM","RSG","COP","XOM","CVX","SLB","OXY","EOG","DVN","PSX","VLO","MPC","PBR","BP","SHEL","TTE","RIO","FCX","NEM"]

end = datetime(2026, 9, 15)
start = datetime(2024, 9, 15)

data = yf.download(universe, start=start.strftime('%Y-%m-%d'), end=end.strftime('%Y-%m-%d'), 
                   auto_adjust=True, progress=False, threads=True)

close = data['Close']
rets = close.pct_change(fill_method=None)

mom_12m = close.shift(21) / close.shift(252) - 1
mom_3m = close.shift(21) / close.shift(63) - 1
vol = rets.rolling(252).std() * np.sqrt(252)
quality = -vol

mom_z = (mom_12m - mom_12m.mean()) / mom_12m.std()
qual_z = (quality - quality.mean()) / quality.std()
composite = 0.5 * mom_z + 0.5 * qual_z

results = pd.DataFrame({
    'ticker': universe,
    'momentum_12m': mom_12m.iloc[-1].values,
    'momentum_3m': mom_3m.iloc[-1].values,
    'volatility': vol.iloc[-1].values,
    'quality_score': qual_z.iloc[-1].values,
    'composite_score': composite.iloc[-1].values
}).sort_values('composite_score', ascending=False).reset_index(drop=True)

results.to_csv('factor_scores_2026-09-15.csv', index=False)
```

## Files

- `factors_2026-09-14.csv` — latest dated signal (151 tickers)
- `report_2026-09-15.md` — full research report (methodology, top/bottom picks)

## Buy the full pack

**LaunchTower Momentum Signal — $29**

Includes: dated factor dataset (CSV), full research report (Markdown),
reproduction script (Python), and this repo.

> **🛒 BUY NOW:** https://whop.com/checkout/ch_lRnmk2lkgdB92fl/
>
> **Store:** https://whop.com/biz_PafLwqOjrf2HRB/

## Disclaimer

This is research data, not investment advice. Past factor performance does not
guarantee future results. Do your own due diligence.

---
*LaunchTower — independent market-data research.*
