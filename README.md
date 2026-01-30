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
> 
<img width="681" height="373" alt="Screenshot 2026-01-31 021816" src="https://github.com/user-attachments/assets/3aba607b-3b07-48ae-8fbf-c738a1ad914a" />
<img width="647" height="426" alt="8" src="https://github.com/user-attachments/assets/0497e756-8b35-4a30-a0fb-385eaf5efeae" />
<img width="297" height="383" alt="14" src="https://github.com/user-attachments/assets/9aa89a95-15d4-4a59-9b4e-15a3ed2b4568" />
<img width="754" height="414" alt="3" src="https://github.com/user-attachments/assets/33b7dd9e-8c2b-4699-b91c-e08734c17b29" />
<img width="744" height="423" alt="11" src="https://github.com/user-attachments/assets/1943f444-3090-464c-9b4f-57155aa15632" />
<img width="755" height="420" alt="5" src="https://github.com/user-attachments/assets/e4569b0f-8352-432f-a666-f96138ea77d0" />
<img width="760" height="431" alt="12" src="https://github.com/user-attachments/assets/a823d355-9dd2-49f4-be73-62d65b1b43ec" />
<img width="724" height="415" alt="7" src="https://github.com/user-attachments/assets/fc1b07b0-4c99-48bd-ab42-4ab7e4acdc7b" />
<img width="752" height="426" alt="13" src="https://github.com/user-attachments/assets/edad0e46-88eb-41f0-a9d7-e520877944d6" />




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
