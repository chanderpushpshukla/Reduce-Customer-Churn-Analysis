# Project Idea: Customer Churn Reduction (Excel & Power BI)

## Problem Statement
Customer churn leads to revenue loss and increased acquisition costs. Organizations often lack clear visibility into *where*, *when*, and *why* customers leave. This project aims to analyze historical customer data to understand churn patterns and recommend practical retention actions using **Excel and Power BI only**.

---

## Objective
To measure customer churn, identify key churn drivers, and present actionable business insights through simple, stakeholder-friendly dashboards without using machine learning or complex DAX.

---

## Tools & Technologies
- **Microsoft Excel** (data cleaning, pivot tables, summary analysis)
- **Power Query** (data transformation)
- **Power BI** (visualization and dashboards)

---

## Scope of Analysis
- Overall churn rate calculation
- Churn analysis by time (monthly/quarterly)
- Churn by customer segment, geography, and tenure
- Behavioral indicators such as usage decline and complaints

---

## Methodology
1. Define churn and analysis period
2. Collect and validate customer-related datasets
3. Clean and transform data using Excel and Power Query
4. Perform exploratory and descriptive analysis
5. Build Power BI dashboards with key KPIs and trends
6. Interpret results and derive business insights

---

## Key Deliverables
- Cleaned and structured dataset
- Power BI churn dashboard
- Insight summary highlighting churn drivers
- Business-focused retention recommendations

---

## Expected Business Impact
- Improved understanding of customer behavior
- Identification of high-risk customer segments
- Support for proactive retention strategies
- Better decision-making through data visualization

---

## Use Cases
This framework can be applied to:
- Telecom
- Banking & Financial Services
- Retail
- Subscription-based businesses
n---

*This project emphasizes clarity, business relevance, and practical analytics over complex modeling.*

---

## Dataset Description

The project uses a **relational-style analytical dataset** designed specifically for churn analysis using Excel and Power BI. The data is structured in a **flat analytical model** (single fact table with derived attributes) to avoid complex joins and advanced DAX.

### Dataset Overview
- **Granularity:** One row per customer per reporting period (monthly)
- **Records:** 25,000+ rows (scalable)
- **Primary Key:** Customer_ID + Month
- **Time Period:** Historical (12–36 months)

---

## Table: Customer_Churn_Fact

This is the core table used for all analysis and dashboarding.

### 1. Customer Identification Columns

| Column Name | Data Type | Description |
|------------|----------|-------------|
| Customer_ID | Text | Unique identifier for each customer |
| Account_ID | Text | Internal account reference number |
| Customer_Type | Text | Individual / Business |
| Signup_Date | Date | Date when the customer joined |

---

### 2. Demographic & Geographic Columns

| Column Name | Data Type | Description |
|------------|----------|-------------|
| Gender | Text | Customer gender (Male/Female/Other) |
| Age_Group | Text | Age bucket (18–25, 26–35, etc.) |
| City | Text | Customer city |
| State | Text | Customer state/region |
| Region | Text | Business region (North, South, East, West) |

---

### 3. Account & Subscription Details

| Column Name | Data Type | Description |
|------------|----------|-------------|
| Plan_Type | Text | Subscription plan name |
| Contract_Type | Text | Monthly / Quarterly / Annual |
| Monthly_Charges | Number | Monthly subscription fee |
| Payment_Method | Text | Credit Card / Debit / UPI / Cash |
| Auto_Renewal | Yes/No | Whether auto-renew is enabled |

---

### 4. Usage & Engagement Metrics

| Column Name | Data Type | Description |
|------------|----------|-------------|
| Monthly_Usage | Number | Product/service usage metric |
| Avg_Usage_Last_3_Months | Number | Rolling average usage |
| Usage_Change | Number | Increase or decrease in usage |
| Login_Frequency | Number | Number of logins per month |

---

### 5. Customer Support & Experience

| Column Name | Data Type | Description |
|------------|----------|-------------|
| Support_Tickets | Number | Number of support tickets raised |
| Complaint_Flag | Yes/No | Whether any complaint was registered |
| Resolution_Time_Days | Number | Avg resolution time |
| Satisfaction_Score | Number | CSAT score (1–5) |

---

### 6. Financial & Transactional Columns

| Column Name | Data Type | Description |
|------------|----------|-------------|
| Total_Revenue | Number | Revenue generated till date |
| Last_Payment_Date | Date | Most recent payment |
| Outstanding_Amount | Number | Pending dues |
| Discount_Applied | Yes/No | Whether discount was given |

---

### 7. Time & Derived Columns (Calculated in Excel / Power Query)

| Column Name | Data Type | Description |
|------------|----------|-------------|
| Report_Month | Date | Month of analysis |
| Tenure_Months | Number | Customer tenure in months |
| Churn_Flag | Yes/No | 1 = Churned, 0 = Active |
| Churn_Reason | Text | Voluntary / Price / Service / Unknown |

---

## Supporting Reference Tables (Optional)

### Table: Date_Dimension

| Column Name | Description |
|------------|-------------|
| Date | Calendar date |
| Month | Month name |
| Year | Year |
| Quarter | Fiscal quarter |


### Table: Plan_Master

| Column Name | Description |
|------------|-------------|
| Plan_Type | Plan identifier |
| Plan_Category | Basic / Standard / Premium |
| Plan_Price | Standard price |

---

## Data Design Rationale
- Optimized for **Excel pivot tables and Power BI visuals**
- Minimal joins to reduce complexity
- Supports time-based, segment-based, and behavior-based churn analysis
- Easily extendable for future predictive modeling

---

## Data Quality Rules
- No duplicate Customer_ID + Report_Month combinations
- Mandatory fields: Customer_ID, Report_Month, Churn_Flag
- Standardized category values
- Valid date ranges only


