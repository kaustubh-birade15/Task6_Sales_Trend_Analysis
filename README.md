# Task 6 – Sales Trend Analysis Using Aggregations

## 📌 Objective
Analyze monthly revenue and order volume from the sales dataset using SQL aggregate functions and date-based grouping.

## 🛠 Tools Used
- **SQLite** (DB Browser for SQLite)
- **SQL** for data aggregation and analysis
- **CSV exports** for results
- **GitHub** for submission

## 📂 Dataset
The dataset (`online_sales`) contains invoice-level sales data with the following relevant columns:
- **InvoiceNo** → Order/Invoice ID  
- **InvoiceDate** → Date and time of the sale  
- **Quantity** → Number of units sold  
- **UnitPrice** → Price per unit  
- **StockCode / Description** → Product details  
- **CustomerID** → Customer reference

## 📝 Steps Performed
1. **Data Loading**
   - Imported the dataset CSV into SQLite using DB Browser.
   - Ensured `InvoiceDate` is in a date-compatible text format.

2. **Monthly Revenue & Order Volume**
   - Extracted **year** and **month** from `InvoiceDate` using `strftime()`.
   - Calculated monthly revenue:
     ```sql
     SUM(Quantity * UnitPrice) AS total_revenue
     ```
   - Counted unique orders:
     ```sql
     COUNT(DISTINCT InvoiceNo) AS total_orders
     ```
   - Grouped results by year and month.
   - Sorted results in chronological order.

3. **Top 3 Months by Sales**
   - Ordered months by `total_revenue` in descending order.
   - Limited output to top 3 months.

4. **NULL Handling**
   - Used `COALESCE()` for revenue calculation to handle any missing values:
     ```sql
     SUM(COALESCE(Quantity * UnitPrice, 0))
     ```

## 📊 SQL Queries

### Monthly Revenue & Orders
```sql
SELECT 
    strftime('%Y', InvoiceDate) AS year,
    strftime('%m', InvoiceDate) AS month,
    SUM(Quantity * UnitPrice) AS total_revenue,
    COUNT(DISTINCT InvoiceNo) AS total_orders
FROM 
    online_sales
GROUP BY 
    year, month
ORDER BY 
    year, month;
```
### Top 3 Months by Sales
```sql
SELECT 
    strftime('%Y', InvoiceDate) AS year,
    strftime('%m', InvoiceDate) AS month,
    SUM(Quantity * UnitPrice) AS total_revenue
FROM 
    online_sales
GROUP BY 
    year, month
ORDER BY 
    total_revenue DESC
LIMIT 3;
```

## 📈 Summary of Findings
-Revenue and orders vary significantly across months.
-Identified **top 3 months** with highest revenue.
-Peaks may indicate **seasonal trends** or promotional events.
-Useful for **inventory planning** and sales forecasting.

## 📁 Repository Contents
-`task6_sales_trend.sql` → SQL script with both queries.
-`monthly_revenue_orders.csv` → Exported results for monthly trend.
-`top_3_months_sales.csv` → Exported results for top 3 months.
