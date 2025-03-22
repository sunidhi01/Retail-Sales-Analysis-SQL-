# Retail-Sales-Analysis-SQL
Project Overview

Project Title: Retail Sales Analysis 
Database: 

This project showcases essential SQL skills and techniques used by data analysts to explore, clean, and analyze retail sales data. It involves setting up a retail sales database, conducting exploratory data analysis (EDA), and using SQL queries to answer key business questions. Designed for beginners in data analysis, this project helps build a strong foundation in SQL.

Objectives

1.Set Up a Retail Sales Database: Create and populate a database using the provided sales data.

2.Data Cleaning: Detect and handle missing or null values to ensure data accuracy.

3.Exploratory Data Analysis (EDA): Analyze key trends and patterns within the dataset.

4.Business Analysis: Leverage SQL queries to answer critical business questions and extract actionable insights.

Structure of Project

1.Setup of database

Database Creation: Begin by creating a database named p1_retail_db to store retail sales data.

1.Table Creation: Set up a retail_sales table with essential columns, including transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
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

2.Exploration of Data and Cleaning 

Total Records: Count the total number of records in the dataset.

Unique Customers: Identify the number of distinct customers.

Product Categories: List all unique product categories.

Data Cleaning: Check for null values and remove records with missing data.

SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

    3.Analysis of Data and finding 

    a.Write a SQL query to retrieve all columns for sales made on '2022-11-05:
    
    SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';

b. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:

SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4

    c.Write a SQL query to calculate the total sales (total_sale) for each category.:
    
    SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1

d.Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:

SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'

e.Write a SQL query to find all transactions where the total_sale is greater than 1000.:

SELECT * FROM retail_sales
WHERE total_sale > 1000

f.Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:

SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1

e.Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:

SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1

f.Write a SQL query to find the top 5 customers based on the highest total sales.

SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5

g.Write a SQL query to find the number of unique customers who purchased items from each category.:

SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category

h.Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):

WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift


FINDINGS

Customer Demographics: Customers belong to diverse age groups, with sales spanning categories like Clothing and Beauty.

High-Value Transactions: Multiple transactions exceed $1,000, highlighting premium purchases.

Sales Trends: Monthly sales analysis reveals seasonal trends and peak periods.

Customer Insights: Identifies top-spending customers and the most popular product categories.

REPORTS

Sales Summary: A comprehensive report covering total sales, customer demographics, and category performance.

Trend Analysis: Insights into sales patterns across different months and time periods.

Customer Insights: Identification of top customers and unique customer counts within each category.
