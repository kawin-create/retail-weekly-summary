# HTML Dashboard Template

Use this template for the weekly summary HTML output. Replace all `{{placeholder}}` values with actual data.

## Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Retail Performance Summary - Week {{W21}}-{{W22}}</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { font-family: 'Segoe UI', Arial, sans-serif; background: #f5f7fa; color: #333; padding: 24px; }
  .container { max-width: 1200px; margin: 0 auto; }
  h1 { font-size: 24px; color: #1a1a2e; margin-bottom: 4px; }
  .subtitle { color: #666; font-size: 14px; margin-bottom: 24px; }
  .section { background: #fff; border-radius: 12px; padding: 20px 24px; margin-bottom: 20px; box-shadow: 0 1px 4px rgba(0,0,0,0.06); }
  .section-title { font-size: 16px; font-weight: 600; color: #1a1a2e; margin-bottom: 14px; border-bottom: 2px solid #e8ecf1; padding-bottom: 8px; }
  .kpi-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 14px; }
  .kpi-card { border: 1px solid #e8ecf1; border-radius: 10px; padding: 14px 16px; position: relative; }
  .kpi-card .category { font-size: 11px; text-transform: uppercase; letter-spacing: 0.5px; color: #888; margin-bottom: 4px; }
  .kpi-card .name { font-size: 13px; font-weight: 600; color: #444; margin-bottom: 8px; }
  .kpi-card .values { display: flex; gap: 16px; align-items: baseline; }
  .kpi-card .current { font-size: 22px; font-weight: 700; }
  .kpi-card .prev { font-size: 13px; color: #888; }
  .kpi-card .target { font-size: 11px; color: #999; margin-top: 4px; }
  .kpi-card .change { font-size: 12px; font-weight: 600; margin-top: 4px; }
  .pass { color: #16a34a; } .fail { color: #dc2626; } .warn { color: #d97706; }
  .up { color: #16a34a; } .down { color: #dc2626; } .neutral { color: #6b7280; }
  .kpi-card .status-dot { position: absolute; top: 14px; right: 14px; width: 10px; height: 10px; border-radius: 50%; }
  .dot-green { background: #16a34a; } .dot-red { background: #dc2626; } .dot-yellow { background: #d97706; }
  table { width: 100%; border-collapse: collapse; font-size: 13px; }
  th { background: #f8fafc; text-align: left; padding: 8px 10px; font-weight: 600; color: #555; border-bottom: 2px solid #e8ecf1; white-space: nowrap; }
  td { padding: 7px 10px; border-bottom: 1px solid #f0f2f5; }
  tr:hover { background: #f8fafc; }
  .num { text-align: right; font-variant-numeric: tabular-nums; }
  .cluster-fmcg { background: #fef3c7; color: #92400e; padding: 2px 6px; border-radius: 4px; font-size: 11px; font-weight: 600; }
  .cluster-el { background: #dbeafe; color: #1e40af; padding: 2px 6px; border-radius: 4px; font-size: 11px; font-weight: 600; }
  .summary-row { display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 12px; margin-bottom: 16px; }
  .summary-box { background: #f8fafc; border-radius: 8px; padding: 12px; text-align: center; }
  .summary-box .label { font-size: 11px; color: #888; text-transform: uppercase; letter-spacing: 0.3px; }
  .summary-box .value { font-size: 20px; font-weight: 700; color: #1a1a2e; margin-top: 2px; }
  .summary-box .detail { font-size: 11px; color: #999; margin-top: 2px; }
  .flag-row { background: #fef2f2 !important; }
  .highlight-row { background: #f0fdf4 !important; }
  .legend { display: flex; gap: 16px; margin-top: 10px; font-size: 12px; color: #666; flex-wrap: wrap; }
  .legend-item { display: flex; align-items: center; gap: 4px; }
  .legend-dot { width: 8px; height: 8px; border-radius: 50%; }
  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
  @media (max-width: 768px) { .two-col { grid-template-columns: 1fr; } }
</style>
</head>
<body>
<div class="container">
  <h1>Retail Performance Report Summary</h1>
  <p class="subtitle">Week {{W21}} vs Week {{W22}} Comparison &bull; Lazada Thailand Retail &bull; Generated {{DATE}}</p>

  <!-- Section 1: KPI Dashboard -->
  <!-- 6 kpi-card divs, one per KPI. Use status-dot color per KPI Status Rules in SKILL.md -->
  <!-- Each card: category, name, current value (formatted), prev value, target, change (pp difference) -->

  <div class="section">
    <div class="section-title">Overall KPI Dashboard</div>
    <div class="kpi-grid">
      <!-- Repeat this block for each KPI:
      <div class="kpi-card">
        <div class="status-dot dot-{green|yellow|red}"></div>
        <div class="category">{Store Operation|IM (Instant Messaging)}</div>
        <div class="name">{KPI Name}</div>
        <div class="values">
          <span class="current {pass|fail|warn}">{formatted value}</span>
          <span class="prev">W{{W21}}: {formatted prev value}</span>
        </div>
        <div class="target">Target: {target}</div>
        <div class="change {up|down|neutral}">{arrow} {pp change}pp vs last week</div>
      </div>
      -->
    </div>
    <div class="legend">
      <div class="legend-item"><div class="legend-dot dot-green"></div> On/Above target</div>
      <div class="legend-item"><div class="legend-dot dot-yellow"></div> Close to target</div>
      <div class="legend-item"><div class="legend-dot dot-red"></div> Below target</div>
    </div>
  </div>

  <!-- Section 2: Key Takeaways -->
  <!-- summary-row with 4 boxes: Total Sellers, KPIs Meeting Target, KPIs Improving WoW, Sellers Below CTR Target -->
  <!-- Then a table with numbered observations (6 rows typical) -->

  <div class="section">
    <div class="section-title">Key Takeaways</div>
    <div class="summary-row">
      <!-- 4 summary-box divs -->
    </div>
    <table>
      <tr><th style="width:24px">#</th><th>Observation</th></tr>
      <!-- highlight-row for positive, flag-row for negative, normal for neutral -->
    </table>
  </div>

  <!-- Section 3: Cluster Performance -->
  <!-- two-col grid with FMCG and EL tables side by side -->

  <div class="section">
    <div class="section-title">Cluster Performance Summary</div>
    <div class="two-col">
      <div>
        <h3 style="font-size:14px;margin-bottom:10px;"><span class="cluster-fmcg">FMCG</span> &nbsp; Fast-Moving Consumer Goods</h3>
        <table>
          <tr><th>Metric</th><th class="num">Week {{W22}}</th><th class="num">Week {{W21}}</th><th class="num">Change</th></tr>
          <!-- Rows: Sellers, Avg CTR, Avg Content Score, CEM Eligible & Active, CTR >= 50% -->
        </table>
      </div>
      <div>
        <h3 style="font-size:14px;margin-bottom:10px;"><span class="cluster-el">EL</span> &nbsp; Electronics</h3>
        <table>
          <tr><th>Metric</th><th class="num">Week {{W22}}</th><th class="num">Week {{W21}}</th><th class="num">Change</th></tr>
          <!-- Same rows as FMCG -->
        </table>
      </div>
    </div>
  </div>

  <!-- Section 4: Sellers Needing Attention (CTR < 30%) -->
  <!-- flag-row for each seller, sorted by CTR ascending -->

  <div class="section">
    <div class="section-title">Sellers Needing Attention (CTR &lt; 30% in Week {{W22}})</div>
    <table>
      <tr>
        <th style="width:24px">#</th><th>Seller Name</th><th>Cluster</th>
        <th class="num">W{{W22}} CTR</th><th class="num">W{{W21}} CTR</th><th class="num">Trend</th>
        <th class="num">W{{W22}} Content</th><th class="num">W{{W22}} CEM</th>
      </tr>
      <!-- Rows with class="flag-row" -->
    </table>
    <p style="font-size:12px;color:#888;margin-top:8px;">Note: Sellers with CTR shown as "-" are excluded as they have no active content.</p>
  </div>

  <!-- Section 5: Top Performers (Top 5 by CTR) -->
  <!-- highlight-row for each seller -->

  <div class="section">
    <div class="section-title">Top Performers (Highest CTR in Week {{W22}})</div>
    <table>
      <tr>
        <th style="width:24px">#</th><th>Seller Name</th><th>Cluster</th>
        <th class="num">W{{W22}} CTR</th><th class="num">W{{W21}} CTR</th><th class="num">Trend</th>
        <th class="num">W{{W22}} Content</th>
      </tr>
      <!-- Rows with class="highlight-row" -->
    </table>
  </div>

  <!-- Section 6: Full Seller Table -->
  <!-- All sellers sorted by row number, with WoW comparison -->

  <div class="section">
    <div class="section-title">Full Seller Performance &mdash; Week {{W22}} vs Week {{W21}}</div>
    <div style="overflow-x:auto;">
    <table>
      <tr>
        <th style="width:24px">#</th><th>Seller Name</th><th>Cluster</th>
        <th class="num">W{{W22}} CTR</th><th class="num">W{{W21}} CTR</th><th class="num">WoW</th>
        <th class="num">W{{W22}} Content</th><th class="num">W{{W21}} Content</th>
        <th class="num">W{{W22}} CEM</th><th class="num">W{{W21}} CEM</th>
      </tr>
      <!-- All seller rows -->
    </table>
    </div>
  </div>

  <p style="text-align:center;color:#aaa;font-size:11px;margin-top:16px;">Source: Retail Performance Report week{{W21}} &amp; week{{W22}} from DingTalk &bull; Auto-generated summary</p>
</div>
</body>
</html>
```

## Formatting Notes

- CTR values: multiply by 100 and show 1 decimal (e.g. 0.5467 → "54.67%")
- Content Score: multiply by 100 and show 1 decimal (e.g. 0.99 → "99.0%")
- IM Same Day / 10-Min: multiply by 100 and show 1-2 decimals
- Avg Response Time: show as-is with "min" suffix
- CEM: show numeric value or "N/A" for "Not eligible"
- WoW change: calculate percentage point difference, show arrow (▲ for up, ▼ for down, — for stable)
- Cluster badges: use `<span class="cluster-fmcg">FMCG</span>` or `<span class="cluster-el">EL</span>`
