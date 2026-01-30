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
<img width="549" height="395" alt="2" src="https://github.com/user-attachments/assets/cb15b7c9-40b6-431c-ad39-5990f38f3a0c" />
<img width="715" height="418" alt="10" src="https://github.com/user-attachments/assets/88f182f9-e0dc-4e15-85f9-2fc29157b3c7" />
<img width="667" height="383" alt="6" src="https://github.com/user-attachments/assets/f90483b9-3879-4de6-864e-edcaa5b29a64" />
<img width="758" height="416" alt="4" src="https://github.com/user-attachments/assets/f86cf46e-b8cc-4116-a790-6df16958f247" />
<img width="754" height="414" alt="3" src="https://github.com/user-attachments/assets/886cfa32-a226-4e20-8281-822e48e4f8a2" />
<img width="744" height="423" alt="11" src="https://github.com/user-attachments/assets/97f4a9c1-f1cb-424d-8d6c-4661566371d9" />
<img width="755" height="420" alt="5" src="https://github.com/user-attachments/assets/d6cd38f2-faed-4c8c-a03b-5feb0a3982cc" />
<img width="760" height="431" alt="12" src="https://github.com/user-attachments/assets/edd7eb7d-7982-4e44-b511-7e5ea9c975d4" />
<img width="724" height="415" alt="7" src="https://github.com/user-attachments/assets/0f81cdc6-7631-4fc1-9256-5383f3dd3afd" />
<img width="752" height="426" alt="13" src="https://github.com/user-attachments/assets/effc5bd6-6d05-4261-9e61-9b7d62864853" />
<img width="297" height="383" alt="14" src="https://github.com/user-attachments/assets/cc668697-20fc-4e07-a50b-0d52722aaa4d" />
<img width="647" height="426" alt="8" src="https://github.com/user-attachments/assets/2d312705-9430-472d-b974-ffd25dd2ac21" />






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
