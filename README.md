Customer Behavior Analysis Using RFM, Churn Prediction & Segmentation

Overview

This project analyzes retail customer purchasing patterns using SQL and Looker Studio.
The goal is to understand customer value, identify churn-risk groups, and create actionable customer segments for targeted marketing.


---

Dataset

Source: Online Retail transactional dataset (2010–2011)
Rows: ~500K
Columns:

Invoice

StockCode

Description

Quantity

InvoiceDate

Price

CustomerID

Country

TotalPrice


TotalPrice was created as:

TotalPrice = Quantity * Price


---

Tools Used

BigQuery (SQL): Data cleaning, RFM scoring, churn logic

Looker Studio: Dashboards & visual analysis

Excel: Initial data checking



---

1. Data Cleaning (SQL)

Key steps:

Removed null CustomerID rows

Removed cancelled orders (Invoice starting with “C”)

Converted datatypes (timestamp, integer, float)

Generated TotalPrice column

Prepared final cleaned table for analysis


Example cleaning query:

SELECT
  Invoice,
  StockCode,
  Description,
  Quantity,
  InvoiceDate,
  Price,
  CustomerID,
  Country,
  (Quantity * Price) AS TotalPrice
FROM `project.dataset.raw_data`
WHERE CustomerID IS NOT NULL
  AND Invoice NOT LIKE 'C%';


---

2. RFM Analysis

Calculated for each customer:

Recency: Days since last purchase

Frequency: Count of transactions

Monetary: Total spending


Assigned scores (1–5) using quantiles.

Key insights:

A small group of high-value customers generated most revenue.

Majority of customers purchased only once.

UK contributed most high-value customers.



---

3. Customer Segmentation

Segments created using RFM score combinations:

Champions

Loyal Customers

Potential Loyalists

At-Risk Customers

Hibernating Customers

Low-Value Customers


These groups help marketing target the right audience.


---

4. Churn Prediction (SQL Logic)

Churn logic:

Customer is considered churned if they have no purchases in the last 90 days.


Findings:

Over 60% of customers did not return after the first purchase.

High RFM customers showed much lower churn rates.


Example query:

SELECT
  CustomerID,
  MAX(InvoiceDate) AS last_purchase,
  DATE_DIFF('2011-12-31', MAX(InvoiceDate), DAY) AS recency_days,
  CASE WHEN DATE_DIFF('2011-12-31', MAX(InvoiceDate), DAY) > 90
       THEN 'Churned'
       ELSE 'Active'
  END AS churn_status
FROM `project.dataset.cleaned`
GROUP BY CustomerID;


---

5. Key Dashboards (Looker Studio)

Page 1: Sales Performance Overview

Top-selling products

Top revenue-generating countries

Monthly sales trend


Page 2: Customer Value & Behavior Analysis

Customer segment distribution

Monetary value by segment

Recency vs Frequency scatter analysis



---

6. Insights & Business Recommendations

1. Retain high-value customers
Offer exclusive loyalty programs, priority support, or special discounts.

2. Re-engage at-risk and churned customers
Email reminders, limited-time offers, personalized product recommendations.

3. Improve international markets
Focus on countries with consistent small sales to expand reach beyond the UK.

4. Optimize inventory
A small percentage of products drive most revenue—focus on stock availability & promotion.


---

7. Project Value

This project demonstrates ability to:

Clean and prepare large transactional datasets

Write advanced SQL queries

Build RFM, churn, and segmentation models

Create dashboard visualizations

Translate data into actionable business insights


### Dashboard Report
You can view the full dashboard here:
[Dashboard PDF](Dashboard%20Report.pdf)
