Retail Sales Analysis Using SQL

📌 Project Overview

This project is a complete SQL walkthrough where I explored, cleaned, and analyzed retail sales data using PostgreSQL. It covers the essential SQL skills expected from an entry-level data analyst — including data cleaning, exploration, and answering business questions using SQL.

The project helped me strengthen my fundamentals and practice writing real-world queries commonly used in data analysis.

🗂️ Project Goals
✔️ 1. Set up a database

Create a database (sql_project_p2)

Create the main table: retail_sales

✔️ 2. Clean the dataset

Identify rows with missing values

Remove incomplete records

✔️ 3. Explore the dataset

Count total sales

Count unique customers

Explore product categories

✔️ 4. Perform business analysis

Answer key business questions using SQL

Analyze customer behavior, sales trends, and product performance

🛠️ Database & Table Setup
CREATE DATABASE sql_project_p2;

DROP TABLE IF EXISTS retail_sales;

CREATE TABLE retail_sales
(
    transaction_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);

🧹 Data Cleaning

I checked for null values across key fields and removed incomplete rows to ensure accuracy.

SELECT * FROM retail_sales
WHERE 
    transaction_id IS NULL OR sale_date IS NULL OR sale_time IS NULL OR
    gender IS NULL OR category IS NULL OR quantity IS NULL OR
    cogs IS NULL OR total_sale IS NULL;

DELETE FROM retail_sales
WHERE 
    transaction_id IS NULL OR sale_date IS NULL OR sale_time IS NULL OR
    gender IS NULL OR category IS NULL OR quantity IS NULL OR
    cogs IS NULL OR total_sale IS NULL;

🔍 Exploratory Data Analysis (EDA)
Total sales in the dataset
SELECT COUNT(*) as total_sale FROM retail_sales;

Unique customers
SELECT COUNT(DISTINCT customer_id) as unique_customers FROM retail_sales;

Product categories
SELECT DISTINCT category FROM retail_sales;

📊 Business Questions & SQL Answers

Below are some of the analysis questions I worked on:

1️⃣ Sales on a specific date (2022-11-05)
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';

2️⃣ Clothing sales with quantity > 4 in Nov-2022
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;

3️⃣ Total sales & order count by category
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

4️⃣ Average age of Beauty category customers
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

5️⃣ High-value transactions (total_sale > 1000)
SELECT *
FROM retail_sales
WHERE total_sale > 1000;

6️⃣ Number of transactions by gender and category
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

7️⃣ Best-selling month in each year
SELECT year, month, avg_sale
FROM (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date)
                    ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) t1
WHERE rank = 1;

8️⃣ Top 5 customers by total sales
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

9️⃣ Unique customers per category
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;

🔟 Order count by time-of-day shift
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;

📈 Key Insights

Certain product categories generate significantly higher revenue.

Premium transactions (₹1000+) occur frequently, showing strong purchasing power.

Monthly and yearly trends help identify peak sales seasons.

Clothing and Beauty are among the most active categories in terms of customer engagement.

Morning vs afternoon vs evening sales patterns help understand shopper behavior.

🧑‍💻 How to Run This Project

Clone the repository

Import the SQL file

Run the table creation and data cleaning queries

Execute the analysis queries to explore insights

📎 Folder Structure
/Retail-Sales-SQL-Analysis
│
├── retail_sales_sql.sql       # Main SQL analysis script
└── README.md                  # Project documentation

🙌 About This Project

This project is part of my SQL learning journey.
I followed along with online resources and then recreated and structured the analysis in my own way to practice writing clean SQL and developing analytical thinking.
