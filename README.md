# Customer Behavior & Sales Analytics Project 📊

## 📌 Project Overview
This project focuses on analyzing customer purchase patterns, identifying churn risks, and visualizing sales performance using **SQL Server** and **Power BI**. The goal is to transform raw sales data into actionable business insights, specifically targeting customer retention and lifetime value.

## 🛠️ Tech Stack
* **Database:** SQL Server (T-SQL)
* **Visualization:** Power BI
* **Logic:** CTEs, Window Functions, DAX

## 🚀 Key Features & Analysis

### 1. Customer Segmentation (RFM Foundation)
I developed a SQL view to identify "dormant" customers—those who haven't made a purchase in over **180 days**. 
* **Metrics:** Recency (Days since last order), Frequency (Total orders), and Monetary Value (Total sales).
* **Goal:** Helping marketing teams target high-value customers who are at risk of churning.

### 2. Cohort Analysis (Retention Tracking)
Using **CTEs** and **Date Functions**, I built a cohort model to track how long customers stay active after their first purchase.
* **Insight:** Identifying the specific month where customer drop-off is highest to improve retention strategies.

### 3. Monthly Performance Dashboard
A dynamic reporting layer that aggregates daily transactions into monthly trends.
* **KPIs:** Total Revenue, Profit Margin, and Order Volume.
* **Time Intelligence:** Year-over-Year (YoY) growth and month-on-month performance.

## 📈 Dashboard Preview
> [!TIP]
> *Insert a screenshot of your Power BI Dashboard here to make an immediate impact!*



## 💻 Featured SQL Logic: Cohort Analysis
Here is a snippet of the logic used to calculate months since the first purchase:

```sql
WITH FirstOrder AS (
    SELECT customer_id, MIN(order_date) AS CohortDate
    FROM dbo.fact_orders
    GROUP BY customer_id
)
SELECT 
    o.customer_id, 
    DATEDIFF(month, f.CohortDate, o.order_date) AS MonthsSinceFirstOrder,
    COUNT(DISTINCT o.customer_id) AS ReturningCustomers,
    DATEFROMPARTS(YEAR(o.order_date), MONTH(o.order_date), 1) AS LinkDate
FROM dbo.fact_orders AS o 
INNER JOIN FirstOrder AS f ON o.customer_id = f.customer_id
GROUP BY o.customer_id, f.CohortDate, o.order_date;
