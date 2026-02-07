# Exploratory Data Analysis (EDA) Guide for Customer Churn Using Power BI

## Introduction
Exploratory Data Analysis (EDA) is the process of examining and visualizing data to uncover patterns, trends, and insights. For this customer churn project, we'll use Power BI to perform descriptive and diagnostic analysis, focusing on understanding churn drivers without predictions or complex formulas. This guide is designed for beginners and business stakeholders, emphasizing business relevance over technical details.

### Why EDA Matters
EDA helps identify where and why customers churn, enabling data-driven decisions to improve retention. Using Power BI's built-in features, we'll explore overall trends, segment breakdowns, and risk factors to support actionable insights.

### Prerequisites
- Power BI Desktop (free download from Microsoft).
- Dataset files: `fact_customer_churn.csv`, `dim_customer.csv`, `dim_contract.csv`, `dim_payment.csv`.
- Basic familiarity with Power BI interface (importing data, creating visuals).

### End-to-End EDA Process in Power BI
Follow these steps sequentially to build a comprehensive EDA report.

#### Step 1: Import and Prepare Data
1. Open Power BI Desktop and select "Get Data" > "CSV" to import each file (`fact_customer_churn.csv`, etc.).
2. In the Power Query Editor (appears after import), review columns for data types (e.g., ensure dates are Date type).
3. Merge tables if needed: Go to "Home" > "Merge Queries" (e.g., merge fact table with dim_customer on Customer_ID for demographics).
4. Clean basics: Remove duplicates via "Remove Rows" > "Remove Duplicates" if any appear.
5. Handle missing values: Use "Replace Values" for blanks (e.g., fill missing Satisfaction_Score with 0).
6. Close & Apply to load data into Power BI.

#### Step 2: Build Basic Measures
Use the "Modeling" tab for simple measures (no advanced DAX).
- Total Customers: Drag Customer_ID to a card visual, set aggregation to "Count (Distinct)".
- Churn Rate: Create a measure: Churn Rate = DIVIDE(SUM('fact_customer_churn'[Churn_Flag]), COUNTROWS('fact_customer_churn')) * 100 (format as percentage).
- Average Tenure: Average of Tenure_Months.
- Total Revenue Impact: Sum of Total_Revenue where Churn_Flag = 1.

#### Step 3: Create Visuals and Explore
Switch to "Report" view. Add visuals from the "Visualizations" pane. Use filters (e.g., by Report_Month) for drill-down.
- Start with high-level overviews, then segment by dimensions.
- Apply themes: Go to "View" > "Themes" for business-friendly colors.

#### Step 4: Organize the Report
- Add pages: Right-click in "Pages" pane to add (e.g., "Overview", "Trends", "Segments").
- Use slicers: Add slicer visuals for filters like Region or Plan_Type.
- Add text boxes: "Insert" > "Text Box" for titles and notes.

#### Step 5: Interpret and Document Insights
- Hover over visuals for tooltips.
- Export insights: "File" > "Export" > "Power BI Templates" or save as .pbix.
- Note patterns: E.g., "Churn spikes in Q4" – link to business actions.

#### Step 6: Validate and Share
- Check data: Use "Data" view to spot anomalies.
- Publish: "File" > "Publish to Power BI Service" for sharing.
- Iterate: Add more visuals based on findings.

## Business Questions for EDA
Below are 12 key business questions for churn analysis. Each includes the business intent, required metrics, recommended visual, and high-level Power BI build steps.

1. **Business Question:** What is the overall churn rate?
   - **Business Intent:** Understand baseline churn to set retention targets and measure improvement.
   - **Metrics Required:** Churn Rate (percentage of churned customers).
   - **Recommended Visual:** KPI Card.
   - **Power BI Build Steps:** Drag Churn Rate measure to a Card visual. Format as percentage.

2. **Business Question:** How has churn trended over time?
   - **Business Intent:** Identify seasonal patterns or growth in churn to time interventions.
   - **Metrics Required:** Monthly Churn Rate (derived from `Churn_Date`).
   - **Recommended Visual:** Line Chart (Month on X, Churn Rate on Y).

   - **Step-by-step (simple & repeatable):**

     1. **Make sure date columns are Date type**
        - Power Query: select `Join_Date` and `Churn_Date` -> Transform -> Data Type -> **Date**.

     2. **Create `Churn_Month` (Power Query) — quick UI**
        - Add Column -> Custom Column -> Name: `Churn_Month`.
        - Formula: `if [Churn_Date] <> null then Date.ToText([Churn_Date], "yyyy-MM") else null`.
        - Optional (better sorting): use `Date.StartOfMonth([Churn_Date])` to create a Date value for month start.

     3. **(Alternative) DAX calculated column**
        - If you prefer creating it in the model instead: `Churn_Month = IF(NOT(ISBLANK('fact_customer_churn'[Churn_Date])), FORMAT('fact_customer_churn'[Churn_Date], "yyyy-MM"), BLANK())`

     4. **Create a Calendar (Date) table**
        - Modeling -> New Table -> paste:
          `Calendar = CALENDAR(MIN('fact_customer_churn'[Join_Date]), MAX('fact_customer_churn'[Churn_Date]))`
        - Add `YearMonth` column: `YearMonth = FORMAT('Calendar'[Date], "yyyy-MM")`
        - Mark as Date table: Table tools -> Mark as Date Table -> select `Date`.

     5. **Set relationships**
        - Prefer: link `Calendar[Date]` -> `fact_customer_churn[Churn_Date]` (single, inactive months without churns will still show with Calendar).
        - If you used text `Churn_Month`, link `Calendar[YearMonth]` -> `fact_customer_churn[Churn_Month]`.

     6. **Create measures (copy/paste DAX)**
        - Monthly Churns =
          `CALCULATE(COUNTROWS('fact_customer_churn'), NOT(ISBLANK('fact_customer_churn'[Churn_Date])))`

        - Customers at Month End =
          `CALCULATE(DISTINCTCOUNT('fact_customer_churn'[Customer_ID]), FILTER('fact_customer_churn', 'fact_customer_churn'[Join_Date] <= MAX('Calendar'[Date]) && (ISBLANK('fact_customer_churn'[Churn_Date]) || 'fact_customer_churn'[Churn_Date] > MAX('Calendar'[Date]))))`

        - Monthly Churn Rate = `DIVIDE([Monthly Churns], [Customers at Month End])`

     7. **Build the Line Chart**
        - Visual: Line chart
        - Axis: `Calendar[YearMonth]` (sort by `Calendar[Date]`)
        - Values: `Monthly Churn Rate`
        - Analytics: add a Trend line to see direction.

     8. **Quick checks & tips**
        - Fast check: use `Churn_Month` on X and `Monthly Churns` as value to spot seasonality quickly (counts vs rates).
        - Confirm date coverage: filter Calendar by year to inspect recent months.
        - If months have no churns, using the `Calendar` ensures they still appear on the axis.

   - **Copyable Power Query M (minimal)**

```m
let
  Source = Csv.Document(File.Contents("fact_customer_churn.csv"), [Delimiter=",", Columns=12, Encoding=1252, QuoteStyle=QuoteStyle.Csv]),
  #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
  #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Churn_Date", type date}}),
  #"Added Churn_Month" = Table.AddColumn(#"Changed Type", "Churn_Month", each if [Churn_Date] <> null then Date.ToText([Churn_Date], "yyyy-MM") else null, type text)
in
  #"Added Churn_Month"
```

   - **Why this works:** deriving month from `Churn_Date` and using a Calendar table gives an accurate, time-ordered churn rate (accounts at risk are considered), and makes trends and seasonality easy to spot and communicate.

3. **Business Question:** Which regions have the highest churn?
   - **Business Intent:** Target regional marketing or support to reduce geographic churn.
   - **Metrics Required:** Churn Rate by Region.
   - **Recommended Visual:** Bar Chart.
   - **Power BI Build Steps:** X-axis: Region; Y-axis: Churn Rate. Sort descending.

4. **Business Question:** How does churn vary by customer age group?
   - **Business Intent:** Customize retention for age demographics (e.g., younger customers may need different engagement).
   - **Metrics Required:** Churn Rate by Age_Group.
   - **Recommended Visual:** Column Chart.
   - **Power BI Build Steps:** X-axis: Age_Group; Y-axis: Churn Rate. Use color coding for clarity.

5. **Business Question:** What is the churn rate by contract type?
   - **Business Intent:** Assess if monthly contracts drive more churn than annual, to optimize offerings.
   - **Metrics Required:** Churn Rate by Contract_Type.
   - **Recommended Visual:** Pie Chart.
   - **Power BI Build Steps:** Legend: Contract_Type; Values: Churn Rate. Ensure percentages are shown.

6. **Business Question:** How does payment method affect churn?
   - **Business Intent:** Identify if certain payment methods correlate with churn for billing improvements.
   - **Metrics Required:** Churn Rate by Payment_Method.
   - **Recommended Visual:** Stacked Bar Chart.
   - **Power BI Build Steps:** X-axis: Payment_Method; Y-axis: Churn Rate. Stack by Churn_Flag.

7. **Business Question:** What is the relationship between tenure and churn?
   - **Business Intent:** Determine if new or long-term customers churn more, to focus onboarding or loyalty.
   - **Metrics Required:** Churn Rate by Tenure_Months (bucketed).
   - **Recommended Visual:** Scatter Chart.
   - **Power BI Build Steps:** X-axis: Tenure_Months; Y-axis: Churn Rate. Add trend line.

8. **Business Question:** How does monthly usage impact churn?
   - **Business Intent:** Spot declining usage as a churn signal for proactive outreach.
   - **Metrics Required:** Average Monthly_Usage by Churn_Flag.
   - **Recommended Visual:** Histogram.
   - **Power BI Build Steps:** Values: Monthly_Usage; Group by Churn_Flag. Adjust bins for distribution.

9. **Business Question:** Are customers with more support tickets more likely to churn?
   - **Business Intent:** Improve support quality to reduce churn from dissatisfaction.
   - **Metrics Required:** Average Support_Tickets by Churn_Flag.
   - **Recommended Visual:** Box and Whisker Plot.
   - **Power BI Build Steps:** Category: Churn_Flag; Values: Support_Tickets. Show quartiles.

10. **Business Question:** How does satisfaction score relate to churn?
    - **Business Intent:** Use feedback to enhance service and retention.
    - **Metrics Required:** Average Satisfaction_Score by Churn_Flag.
    - **Recommended Visual:** Gauge Chart.
    - **Power BI Build Steps:** Value: Average Satisfaction_Score; Target: Set to 4 (ideal). Filter by Churn_Flag.

11. **Business Question:** Which customer groups are at high risk of churn?
    - **Business Intent:** Prioritize retention efforts on vulnerable segments.
    - **Metrics Required:** Churn Rate by combined segments (e.g., Age_Group + Region).
    - **Recommended Visual:** Treemap.
    - **Power BI Build Steps:** Group: Age_Group; Details: Region; Values: Churn Rate. Size by rate.

12. **Business Question:** What is the revenue impact of churn?
    - **Business Intent:** Quantify financial loss to justify retention investments.
    - **Metrics Required:** Sum of Total_Revenue where Churn_Flag = 1.
    - **Recommended Visual:** Waterfall Chart.
    - **Power BI Build Steps:** Category: Report_Month; Values: Revenue Impact. Show cumulative loss.

## Conclusion
This EDA guide provides a foundation for churn insights using Power BI's intuitive tools. Start with data import, build measures, and create visuals for each question. Iterate by adding filters or new pages. For advanced users, explore drill-through or tooltips. Remember, EDA is iterative—refine based on findings to drive retention strategies.</content>
<parameter name="filePath">c:\GIT\Reduce-Customer-Churn-Analysis\eda_guide_power_bi.md