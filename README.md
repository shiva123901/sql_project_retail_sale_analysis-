# sql_project_retail_sale_analysis-
🛒 Retail Sales Analysis SQL Project
👨‍💻 Author: Shiva Seth
Beginner-level SQL project to demonstrate data exploration, cleaning, and business insight extraction using SQL.

📌 Project Overview
Project Title: Retail Sales Analysis

Level: Beginner

Database: p1_retail_db

This project showcases practical SQL skills applied to a real-world retail sales dataset. It involves creating a structured sales database, cleaning raw data, performing exploratory data analysis (EDA), and answering business questions using SQL queries.

It is ideal for anyone starting their data analytics journey and looking to build a strong foundation in SQL with PostgreSQL-style syntax.

🎯 Objectives
🧱 Set up a sales database

🧹 Clean raw data (remove NULLs, validate types)

🔍 Explore sales patterns and customer behavior

📊 Answer 10+ key business questions with SQL

📁 Project Structure
1. 🧱 Database Setup
sql
Copy
Edit
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
2. 🧹 Data Exploration & Cleaning
📌 Key Queries:
sql
Copy
Edit
-- Total records
SELECT COUNT(*) FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Unique categories
SELECT DISTINCT category FROM retail_sales;

-- Identify NULLs
SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
      gender IS NULL OR age IS NULL OR category IS NULL OR 
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Delete NULL records
DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
      gender IS NULL OR age IS NULL OR category IS NULL OR 
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
3. 📊 Business Questions & Queries
🔎 Q1. Sales made on '2022-11-05'
sql
Copy
Edit
SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';
🔎 Q2. Clothing sales (quantity > 4) in November 2022
sql
Copy
Edit
SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
🔎 Q3. Total sales and orders per category
sql
Copy
Edit
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales GROUP BY category;
🔎 Q4. Average age of customers who bought Beauty products
sql
Copy
Edit
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
🔎 Q5. High-value transactions (sales > 1000)
sql
Copy
Edit
SELECT * FROM retail_sales WHERE total_sale > 1000;
🔎 Q6. Transaction count by gender and category
sql
Copy
Edit
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender;
🔎 Q7. Best selling month (highest avg sale) each year
sql
Copy
Edit
SELECT year, month, avg_sale
FROM (
  SELECT EXTRACT(YEAR FROM sale_date) AS year,
         EXTRACT(MONTH FROM sale_date) AS month,
         AVG(total_sale) AS avg_sale,
         RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
  FROM retail_sales
  GROUP BY 1, 2
) AS t1
WHERE rank = 1;
🔎 Q8. Top 5 customers by total sales
sql
Copy
Edit
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
🔎 Q9. Unique customers per category
sql
Copy
Edit
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
🔎 Q10. Orders by time shift (Morning, Afternoon, Evening)
sql
Copy
Edit
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
Insights & Takeaways
1.High-value orders (₹1000+) show premium purchase behavior

2.Clothing and Beauty are among the most active categories

3.Sales are seasonal, with clear best-performing months

4.Morning & Evening shifts dominate in order volume

5.Top 5 customers account for a significant portion of revenue.

6.Sales shifts: Morning and Evening tend to show more activity than Afternoon.

🧾 Reports Generated
1.📌 Sales Summary Report

2.📈 Trend Analysis by Month & Shift

3.🧍 Customer Insights: Top buyers and engagement by category

✅ How to Use
1.Clone this repository

2.Import the SQL script into your PostgreSQL or MySQL environment

3.Run queries from the README or provided SQL files

4.Experiment and extend with your own custom analysis

👤 Author: Shiva Seth
This project is part of my data analytics portfolio, demonstrating essential SQL techniques for real-world business analysis. I'm passionate about clean data, smart queries, and building solid foundations in analytics.











