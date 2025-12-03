# Retail Sales Analysis SQL Project

## 📌 Project Overview

This project demonstrates SQL skills used by data analysts to explore, clean, and analyze retail sales data. It includes database setup, exploratory data analysis (EDA), and answering business questions through SQL queries.

This project is suitable for beginners looking to strengthen their SQL fundamentals.

## Objectives

1. Set up a retail sales database: Create and populate a database for retail sales data.

2. Data Cleaning: Identify and remove records containing missing or null values.

3. Exploratory Data Analysis (EDA): Understand the dataset with basic SQL queries.

4. Business Analysis: Use SQL to answer realistic business problems.

## Project Structure
### 1. Database Setup

_**Database Creation**: Create a database named p1_retail_db.

_**Table Creation**: Create a retail_sales table with columns for transaction ID, date, time, customer ID, gender, age, category, quantity, price per unit, COGS, and total sale amount.

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
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

### 2. Data Exploration & Cleaning

_**Record Count** 
```sql
SELECT COUNT(*) FROM retail_sales;
```

_**Customer Count**
```sql
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
```

_**Category Count**
```sql
SELECT DISTINCT category FROM retail_sales;
```

_**Null Value Check**
```sql
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
    gender IS NULL OR age IS NULL OR category IS NULL OR
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

_**Deleting Null Records**
```sql
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
    gender IS NULL OR age IS NULL OR category IS NULL OR
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### 3. Data Analysis & Findings

Below are SQL queries created to answer key business questions:

1. **Retrieve all columns for sales made on 2022-11-05**
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Retrieve all clothing transactions with quantity > 4 in November 2022**
```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```

3. **Calculate total sales for each category**
```sql
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY 1;
```

4. **Average age of customers who purchased items from the Beauty category**
```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

5. **Find all transactions where the total sale is greater than 1000**
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

6. **Find the total number of transactions by gender in each category**
```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY 1;
```

7. **Find the best-selling month of each year based on average sales**
```sql
SELECT year, month, avg_sale
FROM
(
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

8. **Top 5 customers based on total sales**
```sql
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

9. **Number of unique customers per category**
```sql
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

10. **Create sales shifts: Morning (<12), Afternoon (12–17), Evening (>17)**
```sql
WITH hourly_sale AS
(
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
```

### Findings

**Customer Demographics**: The dataset includes customers across different age groups and categories.

**High-Value Transactions**: Transactions above 1000 indicate premium product purchases.

**Sales Trends**: Monthly breakdown reveals peak-performing months.

**Customer Insights**: Identifies top customers and category-level unique customer counts.

### Reports

**Sales Summary**: Overview of total sales, categories, and demographics.

**Trend Analysis**: Monthly, category-wise, and shift-based sales patterns.

**Customer Insights**: Top customers and repeat purchase behavior.

## Conclusion

This project provides a complete introduction to SQL for data analysis — covering setup, cleaning, EDA, and business-focused SQL queries.
The findings help understand customer behavior, product performance, and sales trends.

## How to Use

1. **Clone the repository**

2. **Run the SQL scripts in your SQL editor**

3. **Execute the analysis queries**

4. **Modify and experiment with your own analysis**


