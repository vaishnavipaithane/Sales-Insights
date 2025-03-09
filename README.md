# Data Analysis Using MySQL  

## Overview  
This project analyzes **sales transactions** using MySQL and Power BI. It includes retrieving customer records, data transformations, calculated measures, and insights on revenue, profit margins, and sales performance.  

**1. Show All Customer Records**  
Retrieve all customer details from the `customers` table.  
```sql
SELECT * FROM customers;
```

**2. Show Total Number of Customers**  
Count the total number of customers.  
```sql
SELECT COUNT(*) FROM customers;
```

**3. Show Transactions for Chennai Market**  
Filter transactions where the market code for Chennai is `'Mark001'`.  
```sql
SELECT * FROM transactions WHERE market_code = 'Mark001';
```

**4. Show Distinct Product Codes Sold in Chennai**  
Get unique product codes from transactions in Chennai.  
```sql
SELECT DISTINCT product_code FROM transactions WHERE market_code = 'Mark001';
```

**5. Show Transactions Where Currency is in USD**  
Retrieve all transactions that were made in US dollars.  
```sql
SELECT * FROM transactions WHERE currency = "USD";
```

**6. Show Transactions in 2020 (Joined with Date Table)**  
Retrieve transactions from the year **2020** by joining with the `date` table.  
```sql
SELECT transactions.*, date.* 
FROM transactions  
INNER JOIN date ON transactions.order_date = date.date  
WHERE date.year = 2020;
```

**7. Show Total Revenue in Year 2020**  
Calculate total revenue in **2020** for all transactions in **INR or USD**.  
```sql
SELECT SUM(transactions.sales_amount)  
FROM transactions  
INNER JOIN date ON transactions.order_date = date.date  
WHERE date.year = 2020  
AND (transactions.currency = "INR" OR transactions.currency = "USD");
```

**8. Show Total Revenue in January 2020**  
Filter revenue only for **January 2020**.  
```sql
SELECT SUM(transactions.sales_amount)  
FROM transactions  
INNER JOIN date ON transactions.order_date = date.date  
WHERE date.year = 2020  
AND date.month_name = "January"  
AND (transactions.currency = "INR" OR transactions.currency = "USD");
```

**9. Show Total Revenue in Chennai in 2020**  
Find total revenue from Chennai (`Mark001`) for **2020**.  
```sql
SELECT SUM(transactions.sales_amount)  
FROM transactions  
INNER JOIN date ON transactions.order_date = date.date  
WHERE date.year = 2020  
AND transactions.market_code = "Mark001";
```

# Data Analysis Using Power BI  

**Normalize Sales Amount**  
We convert all **USD sales** to INR using an exchange rate of **85**.  

```powerquery
= Table.AddColumn(#"Filtered Rows", "norm_sales_amount", each if [currency] = "USD" then [sales_amount]*85 else [sales_amount])
```

**Remove Blank Zones**  
To clean the dataset, we **remove rows where the "zone" column is blank**.  

```powerquery
= Table.SelectRows(sales_markets, each ([zone] <> ""))
```

# **Power BI Measures for Analysis**  

**1. Revenue Calculation**  
```DAX
Revenue = SUM('sales transactions'[norm_sales_amount])
```
**Total normalized sales revenue across all transactions.**  

**2. Revenue Contribution %**  
```DAX
Revenue Contribution % = DIVIDE([Revenue], CALCULATE([Revenue],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))
```
**Shows the percentage of revenue contributed by a product, customer, or market.**  

**3. Total Sales Quantity**  
```DAX
Sales Qty = SUM('sales transactions'[sales_qty])
```
**Total number of units sold.**  

**4. Total Profit Margin**  
```DAX
Total Profit Margin = SUM('sales transactions'[profit_margin])
```
**Sum of profit margins across all transactions.**  

**5. Profit Margin %**  
```DAX
Profit Margin % = DIVIDE([Total Profit Margin], [Revenue], 0)
```
**Percentage of profit relative to revenue.**  

**6. Profit Margin Contribution %**  
```DAX
Profit Margin Contribution % = DIVIDE([Total Profit Margin], CALCULATE([Total Profit Margin],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))
```
**Shows how much a specific product, customer, or market contributes to the total profit margin.** 

## Reference  
This mini-project is based on the tutorial by **Codebasics** on YouTube.  
Check out their channel for more awesome content: [Codebasics YouTube](https://www.youtube.com/@codebasics)
