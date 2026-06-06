---
name: retail-weekly-summary
description: Summarize Lazada Thailand Retail weekly performance from the Bizops Weekly Report DingTalk folder. Downloads the latest 2 weeks' Excel reports, extracts KPIs (CTR, Content Score, CEM, IM response metrics), and generates an HTML dashboard with cluster breakdowns, top/bottom performers, and seller-level comparison. Use when the user asks for "weekly summary", "retail performance", "bizops report", or "weekly report summary".
---

# Retail Weekly Performance Summary

## Overview

Generates a weekly HTML dashboard summarizing the last 2 weeks of Lazada Thailand Retail performance. Data source is the DingTalk folder **1. Bizops_Weekly report** (nodeId: `vy20BglGWOxjGpq0C0n3wGenVA7depqY`).

## Workflow

### Step 1: List folder contents

```bash
dws doc list --folder "vy20BglGWOxjGpq0C0n3wGenVA7depqY" --format json
```

Identify the **last 2 files** by name (e.g. "Retail Performance Report week22", "Retail Performance Report week21"). Sort by the week number in the filename. Record their `nodeId` values.

### Step 2: Download both files

For each nodeId, get a download URL:

```bash
dws doc download --node "<nodeId>" --format json
```

Then download using curl (remember `--ssl-no-revoke` on this machine):

```bash
curl --ssl-no-revoke -L -o "<workspace>/Retail_Performance_Report_week<N>.xlsx" "<resourceUrl>"
```

### Step 3: Parse Excel data

Use ExcelJS (install if needed: `npm install exceljs` in workspace).

The newest file contains all historical weeks. Read these sheets from it:

**Overall sheet** (rows 2-7):
- Row structure: `category | KPI name | target | currentWeek | prevWeek | ...`
- Extract these 6 KPIs:
  1. Store operation - CTR (target: 0.5)
  2. Store operation - Content Score (target: 1)
  3. Store operation - CEM (target: 0.5)
  4. IM - IM Same day response rate (target: 0.95)
  5. IM - 10mins Response Rate (target: 0.8)
  6. IM - IM Average response time (target: <10Min)

**Current week sheet** (e.g. "Week22") and **previous week sheet** (e.g. "Week21"):
- Header rows: 1-2 (row 2 has actual column names)
- Data starts row 3, last row is an AVERAGE formula row (skip it)
- Columns: No. | Shortcode | Seller Name | CTR | Content score | Total post CEM | Cluster
- CTR and Content score are decimals (e.g. 0.75 = 75%)
- CEM is a number or "Not eligible"
- Cluster is "FMCG" or "EL"
- Some sellers have "-" for CTR/Content (no active content)

### Step 4: Compute summaries

**Cluster-level stats** (FMCG vs EL):
- Count of sellers
- Average CTR (exclude "-" values)
- Average Content Score (exclude "-" values)
- Count of CEM-eligible sellers (those with numeric CEM values)
- Count of sellers with CTR >= 50%

**Seller-level flags**:
- Sellers with CTR < 30% in current week → "Needs Attention"
- Top 5 sellers by CTR in current week → "Top Performers"
- Week-over-week CTR change direction (up/down/stable)

### Step 5: Generate HTML dashboard

Use the template in [html-template.md](html-template.md). Replace all placeholder values with actual computed data.

The dashboard includes:
1. **KPI Dashboard** - 6 cards with current value, previous value, target, and status indicator
2. **Key Takeaways** - Summary boxes + numbered observations table
3. **Cluster Performance** - Side-by-side FMCG vs EL comparison
4. **Sellers Needing Attention** - Table of sellers with CTR < 30%
5. **Top Performers** - Top 5 sellers by CTR
6. **Full Seller Table** - All sellers with W22 vs W21 comparison

Save the output HTML to the workspace outputs folder.

### Step 6: Present the file

Use `present_files` to share the HTML dashboard with the user. Include a brief text summary of the 2-3 most important findings.

## KPI Status Rules

| KPI | Green (pass) | Yellow (warn) | Red (fail) |
|-----|-------------|---------------|------------|
| CTR | >= 50% | 45-49% | < 45% |
| Content Score | >= 99% | 97-98% | < 97% |
| CEM | >= 50% | - | < 50% |
| IM Same Day Response | >= 95% | 90-94% | < 90% |
| 10-Min Response Rate | >= 80% | 70-79% | < 70% |
| Avg Response Time | < 10 min | 10-12 min | >= 12 min |

## Notes

- The newest Excel file contains all historical week sheets. Always read from the newest file.
- Week numbers in filenames may not be sequential (e.g. week22 follows week21, but after week52 comes week1).
- Sellers with "-" for CTR have no active content that week; exclude them from averages and CTR-based rankings.
- CEM values: numeric = number of broadcasts that week, "Not eligible" = store type not eligible, "Close" = store closed.
- Cluster mapping: FMCG = Fast-Moving Consumer Goods, EL = Electronics.
