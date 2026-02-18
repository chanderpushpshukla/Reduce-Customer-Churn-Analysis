Overview Dashboard — Implementation Guide

Purpose
- Provide a concise, at-a-glance summary of customer base and churn performance for business stakeholders.

Assumptions
- Data tables available: `dim_customer.csv`, `dim_contract.csv`, `fact_customer_churn.csv`, `dim_calendar.csv`, `dim_payment.csv` in the `dataset/` folder.
- Calendar table named `Calendar` with continuous Date values and month/year columns.
- Relationships: `fact_customer_churn[customer_id] -> dim_customer[id]`, `fact_customer_churn[date] -> Calendar[Date]`, `dim_contract[customer_id] -> dim_customer[id]`.

High-level Steps (Power BI Desktop)
1. Get data
   - Use `Get data` → `Text/CSV` and load the CSVs from `dataset/`.
   - For `dim_calendar.csv` prefer marking as Date type and set `Date` column.
2. Power Query transformations (minimal, recommended)
   - Remove duplicates on key columns (e.g., `customer_id`).
   - Standardize date formats for `fact_customer_churn[date]` → Date type.
   - Trim and clean text columns (`churn_reason`, `segment`, `plan`).
   - Promote headers and set proper data types.
   - Close & Apply.
3. Build the data model
   - Ensure `Calendar` is marked as Date table (`Modeling` → `Mark as date table`).
   - Verify relationships are active and single-directional from Calendar → fact table.
4. Add core DAX measures (use mostly standard measures)
   - Total Customers
```DAX
Total Customers = DISTINCTCOUNT(dim_customer[id])
```
   - Active Customers (period to date)
```DAX
Active Customers = CALCULATE([Total Customers], FILTER(ALL(Calendar), Calendar[Date] <= MAX(Calendar[Date])))
```
   - Churn Count (for selected period)
```DAX
Churn Count = COUNTROWS(fact_customer_churn)
```
   - Churn Rate
```DAX
Churn Rate = DIVIDE([Churn Count], [Active Customers], 0)
```
   - Churn Count Previous Period (for comparison)
```DAX
Churn Count Prev = CALCULATE([Churn Count], PREVIOUSMONTH(Calendar[Date]))
```
   - Churn Rate MoM % Change (quick comparison)
```DAX
Churn Rate MoM % = DIVIDE([Churn Rate] - CALCULATE([Churn Rate], PREVIOUSMONTH(Calendar[Date])), CALCULATE([Churn Rate], PREVIOUSMONTH(Calendar[Date])), 0)
```
5. Layout — single page, simple and scannable
   - Top row (KPI cards): `Total Customers`, `Active Customers`, `Churn Count (period)`, `Churn Rate` (show MoM % change as small delta).
   - Middle left: Line chart `Churn Count by Month` (Axis = Calendar[MonthYear], Values = [Churn Count]).
   - Middle right: Clustered column `Churn Count by Segment` (Axis = dim_customer[segment], Values = [Churn Count]).
   - Lower left: Donut chart `Churn % by Plan` (Legend = dim_contract[plan], Values = [Churn Count]).
   - Lower right: Matrix `Churn details` showing `dim_customer[name]`, `dim_customer[segment]`, `fact_customer_churn[churn_reason]`, `fact_customer_churn[date]`.
   - Left pane: Slicers — `Calendar[Year]`, `Calendar[Month]`, `dim_customer[segment]`, `dim_contract[plan]`.
6. Visual design & interactivity
   - Keep color palette neutral; use one highlight color for churn metrics.
   - Turn off unnecessary visual borders; keep titles short.
   - Enable drill-through on the matrix for Customer detail page (optional).
7. Tooltips and insight text
   - Add a report page tooltip for the line chart that shows `Churn Count`, `Churn Rate`, and `Churn Count Prev`.
   - Add a text box summary on top explaining the date range and main callout (e.g., "Churn is up X% MoM").
8. Finalize and publish
   - Save as `artifacts/overview_dashboard_v1.pbix`.
   - If publishing to Power BI Service, set row-level security as needed and update dataset credentials.

Formatting and quick tips
- Use `Card` visuals for KPIs and set `Category label` off for clean look.
- For % values use percentage format with 1 decimal.
- Use `Top N` filter on matrix if there are many churn records (e.g., show top 50 recent churn events).

Optional quick measures (if you prefer built-in Quick Measures)
- YoY/ MoM % change: create `% Change vs Previous Period` quick measure for `Churn Count`.

Notes for reuse
- Keep these measures centralized in a `Measures` table (create an empty table called `Measures` and put all DAX there) for discoverability.
- Avoid complex calculated columns; prefer measures for filter responsiveness.

End of Overview Dashboard implementation guide.

ASCII Reference Visuals (simple text-only sketches)

- Monthly churn (small bar chart, sample values)
```
Churn by Month (sample)
Jan:  ###      (3)
Feb:  #####    (5)
Mar:  ####     (4)
Apr:  ##       (2)
May:  ######   (6)
Jun:  ###      (3)
```

- Churn % by Plan (proportional bars)
```
Plan A: #################   45%
Plan B: #########            25%
Plan C: ##########           30%
```

- Small heatmap style matrix (legend: . low, o medium, O high, @ very high)
```
   Jan Feb Mar Apr May Jun
Plan A   .   o   O   O   @   O
Plan B   .   .   o   o   O   O
Plan C   o   o   O   @   @   @
```

Use these ASCII sketches as quick references when mapping visuals in Power BI: bars → `Clustered column` or `Stacked column`, heatmap → `Table/Matrix` with conditional formatting.
