Performance Dashboard — Implementation Guide

Purpose
- Provide focused operational insights into churn drivers, trends, and short-term performance to help product/ops prioritize retention actions.

Assumptions
- Same data tables and model as the Overview dashboard.
- `Calendar` table present and marked as Date table.

High-level Steps (Power BI Desktop)
1. Data & modeling
   - Reuse the same dataset and data model as Overview; keep measures in the `Measures` table.
2. Key DAX measures to include (mostly reuse)
   - Rolling 3-month Churn Count
```DAX
Churn 3M = CALCULATE([Churn Count], DATESINPERIOD(Calendar[Date], LASTDATE(Calendar[Date]), -3, MONTH))
```
   - Rolling 12-month Churn Rate
```DAX
Churn Rate 12M = CALCULATE([Churn Rate], DATESINPERIOD(Calendar[Date], LASTDATE(Calendar[Date]), -12, MONTH))
```
   - Retention Rate (simple)
```DAX
Retention Rate = 1 - [Churn Rate]
```
   - Avg Churn by Reason (measure example)
```DAX
Churn by Reason = CALCULATE([Churn Count], ALLEXCEPT(fact_customer_churn, fact_customer_churn[churn_reason]))
```
3. Layout — focused multi-section page
   - Top row (KPIs): `Churn Count (Period)`, `Churn Rate`, `Churn 3M`, `Churn Rate 12M` (with trend arrows showing MoM change).
   - Section: Trends
     - Line chart `Churn Count by Month` with the rolling 3M overlay (add second measure `Churn 3M`).
     - Area chart `Active Customers by Month`.
   - Section: Drivers
     - Stacked bar `Churn Count by Reason and Segment` (Axis = reason, Legend = segment).
     - Clustered bar `Churn Count by Region` (Axis = region, Values = [Churn Count]).
   - Section: Operational table/matrix
     - Matrix with rows = `dim_contract[plan]`, columns = `Calendar[MonthYear]`, values = `[Churn Count]` (use conditional formatting to highlight high churn cells).
   - Section: Top issues
     - Table `Top churn reasons` with columns: `churn_reason`, `[Churn Count]`, `Churn Rate for reason` (measure: DIVIDE([Churn Count for reason], [Active Customers],0)).
4. Interactivity and filters
   - Add slicers for rolling window: `Last N months` (use what-if parameter if needed) and `segment`.
   - Sync slicers across pages if you have multiple pages.
   - Use bookmarks for quick toggles (e.g., show `Last 3 months` vs `Last 12 months`).
5. Conditional formatting and KPI visuals
   - Use icons or KPI indicators on cards (green arrow down for decreasing churn; red up for rising churn).
   - On matrix, set background color scale for churn intensity.
6. Operational tips
   - Add an exportable table for Customer Success: include `customer_id`, `churn_date`, `churn_reason`, `segment`, `last_active_date`.
   - Create a button that filters to the most recent month for quick operational view.
7. Validation and QA
   - Cross-check totals: Sum of `Churn Count by Month` should equal overall `Churn Count` for the same time interval.
   - Validate rolling measures against simple period measures for sample months.
8. Finalize and publish
   - Save as `artifacts/performance_dashboard_v1.pbix`.
   - Provide a short one-page summary with recommended actions for business users (place as a separate report page or a PDF attachment).

Example useful quick measures (for convenience)
- Month-over-month % change for `Churn Count` using `DIVIDE([Churn Count]-[Churn Count Prev],[Churn Count Prev],0)`.
- Year-to-date churn count: `CALCULATE([Churn Count], DATESYTD(Calendar[Date]))`.

Design & delivery notes
- Keep pages focused: Overview for executives, Performance for operations.
- Prioritize read-only tables for operational exports (avoid exposing sensitive PII in Service exports).

End of Performance Dashboard implementation guide.

ASCII Reference Visuals (simple text-only sketches)

- Rolling 3-month churn sparkline (sample, last 6 months)
```
Rolling 3M Churn (sample)
Jan:  ###     Feb:  ####    Mar:  #####   Apr:  ###    May:  ####   Jun:  ######
(Each '#' ≈ same unit)
```

- Top churn reasons (proportional bars)
```
Billing:  #####   40%
Usage:    ###     25%
Support:  ##      15%
Price:    ##      20%
```

- Conditional-format example (text bands: low / med / high)
```
Plan\Month  Apr   May   Jun
Basic       low   med   high
Pro         med   med   high
Enterprise  low   low   med
```

Use these ASCII sketches as quick references: sparklines → `Line chart`, proportional bars → `Bar/Column`, conditional bands → `Matrix` with background color rules.
