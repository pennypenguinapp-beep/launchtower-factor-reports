# LaunchTower — Whop Product Listing Status (2026-09-16)

## Product
- **Name:** LaunchTower Factor Research Pack (2026-09)
- **Price:** $29 USD
- **Headline:** Dated momentum + quality factor report, 81 US stocks, full data
- **Store:** https://whop.com/biz_PafLwqOjrf2HRB/

## What's in the pack
1. **LaunchTower Factor Research Report (2026-09-14)** — 81 US large/mid-cap equities, full methodology, top-15 / bottom-10 ranked tables, in-sample backtest description, sector read-through, exact data sources
2. **factor_scores.csv** — complete 81-row factor table (momentum_12_1, ret_3m, ann. vol, ROA, beta, P/E, log_mcap, composite score)

## Status: ⚠️ BLOCKED — no purchase_url generated

The Whop API created the product object 4 times (prod_yXjADHixilgix, prod_UFaYo4WhQ2eGH, prod_tBfygaN448ZPa, prod_PXvFeNaIcWuN9) but **checkout/plan creation failed every time** with:

> `Failed to create dynamic plan: You must specify a currency for this plan.`

### Root cause
The `create_whop_product` tool does not send a `currency` field in the plan-creation API call. Whop requires one. This is a tool/API integration gap, not a content or pricing issue.

### Fix options (owner action required)
1. **Preferred:** Update the `create_whop_product` tool to include `currency: "USD"` in the dynamic plan creation payload.
2. **Alternative:** Set a default currency on the Whop store at https://whop.com/biz_PafLwqOjrf2HRB/ (Settings → Store → Default currency → USD).

### What to do once fixed
- Re-run `create_whop_product` with the same name/price/description
- Copy the returned `purchase_url`
- Add it to:
  - The GitHub README (via `publish_github`, content_file)
  - The landing page HTML (via `write_artifact` + `publish_github`)
  - Any Dev.to articles (via `publish_devto`)
- Save the live purchase_url to long-term memory

## Deliverable files already ready
- `job1891/launchtower_factor_report_2026-09-14.md` — full report (5,020 bytes)
- `job1891/launchtower_index.html` — landing page
- `job1884/launchtower-factor-reports-README.md` — GitHub README

---
*LaunchTower — independent market-data research. Not investment advice.*
