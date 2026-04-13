# E-commerce Performance Dashboard (Google Stack)

## 🔗 Live Dashboard
Interactive dashboard for tracking sales performance, revenue trends, and return metrics.

[Open interactive dashboard](https://lookerstudio.google.com/reporting/d9e9c5d7-8869-4a04-a12f-a5dccf4a1674)

---

## 📌 Project Overview

This project simulates a real-world implementation of a data analytics solution for an e-commerce business.

The goal was to:
- clean and structure raw transactional data
- define key business metrics (KPI)
- build a reporting pipeline
- deliver a decision-ready dashboard in Looker Studio

---

## 🎯 Business Problem

The dataset represents a typical SME e-commerce export:
- inconsistent data types
- missing values
- unclear structure (order vs product level)
- presence of returns and cancellations

The business lacked:
- a single source of truth
- clear KPI definitions
- a reliable reporting system

---

## 🧱 Data Understanding

- ~540,000 rows
- 1 row = 1 product in an order (order line)
- InvoiceNo = order ID
- InvoiceNo starting with "C" = cancellations/returns

### Key issues identified:
- InvoiceDate stored partially as text
- missing CustomerID (~135k rows)
- negative Quantity values (returns)
- UnitPrice anomalies (0 values, mixed types)
- inconsistent product descriptions

---

## ⚙️ Data Processing (BigQuery)

A structured pipeline was implemented:

RAW → WORKING → FINAL

### Data separation logic:
- Sales table → Quantity > 0
- Returns table → Quantity < 0

---

### 🟢 Clean Sales Table

```sql
CREATE OR REPLACE TABLE `analityka-dla-firm.01_SANDBOX_2026.WORKING_sales_clean` AS

SELECT
  InvoiceNo,
  DATE(InvoiceDate) AS InvoiceDate,
  Quantity,
  UnitPrice,
  UnitPrice * Quantity AS Revenue
FROM
  `analityka-dla-firm.01_SANDBOX_2026.RAW`
WHERE Quantity > 0
```

---

### 🔴 Returns Table

```sql
CREATE OR REPLACE TABLE `analityka-dla-firm.01_SANDBOX_2026.WORKING_returns_clean` AS

SELECT
  InvoiceNo,
  DATE(InvoiceDate) AS InvoiceDate,
  ABS(Quantity) AS Quantity,
  UnitPrice,
  ABS(UnitPrice * Quantity) AS ReturnValue
FROM
  `analityka-dla-firm.01_SANDBOX_2026.RAW`
WHERE Quantity < 0
```

This approach ensures:
- accurate KPI calculation
- clear separation between revenue and returns
- improved data reliability for reporting
---

## 📊 Key Metrics (KPI)

- Revenue: 10.64m  
- Orders: 20,728  
- Average Order Value (AOV): 513.54  
- Number of Returns: 5,172  
- Return Value: 896,812.49  

Return rate (based on revenue): ~8.4%

---

## 📈 Dashboard (Looker Studio)

The dashboard includes:
- Core KPIs (Orders, Revenue, AOV)
- Revenue trend with 7-day moving average
- Returns monitoring
- Interactive Month/Year filter

---

## 🧠 Key Insights

- Revenue growth in Q4
- High daily volatility smoothed by moving average
- Return rate within realistic e-commerce range (~8%)
- Clear separation of sales vs returns improves accuracy

---

## 🛠 Tech Stack

- BigQuery (data processing)
- SQL (transformations)
- Looker Studio (dashboard)
- Google Sheets (initial audit)

---

## 🚀 Outcome

Delivered a complete end-to-end reporting solution:
- clean data model
- validated KPIs
- interactive dashboard
- reusable structure for future clients

---

## 📷 Dashboard Preview

![Dashboard](screenshots/dashboard.png)

---

## 📌 Notes

This project represents a START-level analytics solution,
focused on data clarity, core KPIs, and decision-ready reporting.
