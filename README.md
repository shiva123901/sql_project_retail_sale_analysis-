# 🛒 Retail Sales Analysis using SQL

A beginner-friendly SQL project to explore, clean, and analyze retail transaction data. The goal is to extract meaningful insights through structured queries and build a solid foundation in SQL for data analytics.

---

## 📚 Project Summary

- **Project Title:** Retail Sales Analysis  
- **Level:** Beginner  
- **Tools:** PostgreSQL (or any SQL-compatible DBMS)  
- **Database:** `p1_retail_db`  
- **Table Used:** `retail_sales`  
- **Focus:** Data Cleaning, EDA, and Business-Oriented SQL Queries

---

## 🎯 Skills Demonstrated

- SQL database design & schema creation  
- Data cleaning (`NULL` handling, record validation)  
- Exploratory Data Analysis (EDA)  
- Business queries (GROUP BY, CASE, RANK, etc.)  
- Time-based analysis (monthly trends, hourly shifts)  
- Use of window functions (`RANK()`, `AVG()`, etc.)

---

## 🗂️ Project Structure

### 1. Database Setup

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales (
  transactions_id INT PRIMARY KEY,
  sale_date DATE,
  sale_time TIME,
  customer_id INT,
  gender VARCHAR(10),
  age INT,
  category VARCHAR(35),
  quantity INT,
  price_per_unit FLOAT,
  cogs FLOAT,
  total_sale FLOAT
);
```

### 2. Data Cleaning & Validation

```sql
DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL
  OR gender IS NULL OR age IS NULL OR category IS NULL
  OR quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

## 📊 Business Questions & SQL Queries

### Q1. Retrieve all sales made on `'2022-11-05'`
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

### Q2. Clothing transactions where quantity > 4 during Nov 2022
```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```

### Q3. Total sales and order count for each category
```sql
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

### Q4. Average age of customers who bought from 'Beauty' category
```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

### Q5. Transactions where total_sale > 1000
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

### Q6. Total number of transactions by gender and category
```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

### Q7. Average monthly sale and best-selling month per year
```sql
SELECT year, month, avg_sale
FROM (
  SELECT 
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(MONTH FROM sale_date) AS month,
    AVG(total_sale) AS avg_sale,
    RANK() OVER (
      PARTITION BY EXTRACT(YEAR FROM sale_date)
      ORDER BY AVG(total_sale) DESC
    ) AS rank
  FROM retail_sales
  GROUP BY 1, 2
) AS t1
WHERE rank = 1;
```

### Q8. Top 5 customers by total sales
```sql
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

### Q9. Number of unique customers per category
```sql
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

### Q10. Shift-wise order counts (Morning, Afternoon, Evening)
```sql
WITH hourly_sale AS (
  SELECT *,
    CASE
      WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
      WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
      ELSE 'Evening'
    END AS shift
  FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

---

## 📈 Key Insights

- High-value transactions identified with `total_sale > 1000`
- Best-selling categories include Clothing and Beauty
- Seasonal spikes in sales across specific months
- Mornings and evenings show higher order volumes
- Top 5 customers contribute significantly to overall revenue

---

## 🚀 How to Run This Project

1. Clone this repo  
2. Import the SQL script into your PostgreSQL or MySQL database  
3. Run queries in a SQL editor (e.g., DBeaver, pgAdmin)  
4. Modify or extend queries for deeper insights

---

## 👤 Author: **Shiva Seth**

This project is part of my data analytics portfolio, showcasing real-world SQL skills through exploratory and business-oriented queries.
