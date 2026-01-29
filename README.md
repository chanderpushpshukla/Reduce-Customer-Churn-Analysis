# Reduce Customer Churn – Power BI Analytics Project

## 📌 Project Overview

This project focuses on analyzing and reducing **customer churn** using a clean, structured, and analytics-ready dataset designed specifically for **Power BI**. The objective is to enable end-to-end churn analysis—covering data modeling, KPI tracking, trend analysis, segmentation, and business insights—**without using machine learning or complex DAX**.

The datasets follow a **star schema** approach and are suitable for real-world business analytics scenarios.

---

## 🎯 Project Objectives

* Measure and monitor customer churn rate
* Identify churn patterns across time, region, and customer segments
* Analyze behavioral and service-related churn drivers
* Enable stakeholder-friendly dashboards in Power BI
* Support data-driven customer retention strategies

---

## 🛠 Tools & Technologies

* **Power BI** (Data Modeling, DAX – basic, Dashboards)
* **Power Query** (for optional transformations)
* **CSV datasets** (analytics-ready)

---

## 📂 Dataset Structure

The project uses multiple CSV files to support a scalable Power BI data model.

### 1️⃣ Fact Table

**fact_customer_churn.csv** (30,000+ records)

| Column Name        | Description                     |
| ------------------ | ------------------------------- |
| Customer_ID        | Unique customer identifier      |
| Join_Date          | Customer onboarding date        |
| Tenure_Months      | Customer tenure in months       |
| Churn_Flag         | Yes / No indicator              |
| Churn_Date         | Date of churn (blank if active) |
| Customer_Segment   | Retail / SME / Corporate        |
| Region             | North / South / East / West     |
| Contract_Type      | Monthly / Quarterly / Annual    |
| Payment_Method     | UPI / Credit Card / Debit Card  |
| Monthly_Charges    | Average monthly billing         |
| Usage_Score        | Engagement score (1–100)        |
| Support_Tickets    | Number of support complaints    |
| Satisfaction_Score | Customer rating (1–5)           |

---

### 2️⃣ Dimension Tables

#### dim_customer.csv

* Customer_ID
* Customer_Segment
* Region

#### dim_contract.csv

* Contract_Type

#### dim_payment.csv

* Payment_Method

---

## 🧩 Data Model (Power BI)

Recommended relationships:

* `fact_customer_churn[Customer_ID]` → `dim_customer[Customer_ID]`
* `fact_customer_churn[Contract_Type]` → `dim_contract[Contract_Type]`
* `fact_customer_churn[Payment_Method]` → `dim_payment[Payment_Method]`

Schema Type: **Star Schema**

---

## 📊 Key KPIs & Analysis Use-Cases

* Total Customers
* Churned Customers
* Churn Rate (%)
* Churn Trend (Monthly / Quarterly)
* Churn by Customer Segment
* Churn by Region
* Usage vs Churn
* Support Tickets vs Churn
* Satisfaction Score Impact

---

## 📈 Dashboard Recommendations

* KPI cards for high-level metrics
* Line chart for churn trend over time
* Bar charts for churn by segment and region
* Tables for detailed customer drill-down
* Slicers for Region, Segment, Contract Type

---

## ✅ Data Quality & Assumptions

* All categorical values are standardized
* Dates are ISO formatted (YYYY-MM-DD)
* No duplicate customer records
* Churn_Date populated only for churned customers
* Dataset is synthetic but business-realistic

---

## 🚀 How to Use

1. Download all CSV files
2. Load into Power BI
3. Create relationships in Model View
4. Build measures and dashboards
5. Derive insights and recommendations

---

## 📌 Project Status

**Ongoing** – Dataset prepared for full lifecycle analytics, dashboarding, and reporting.

---

## 🧠 Intended Audience

* Data Analysts
* Business Analysts
* Power BI Developers
* Analytics Students / Portfolio Projects

---

**This project emphasizes business clarity, clean data engineering, and practical analytics over complexity.**
