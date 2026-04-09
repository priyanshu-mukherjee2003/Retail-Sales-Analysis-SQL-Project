# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis   
**Database**: `Retail Sales DB`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `Retail Sales DB`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```
-- Creating the DATABASE
CREATE DATABASE [Retail Sales DB];

-- Creating the TABLE
CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs INT,
    total_sale INT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```
-- SELECT Query with retail_sales TABLE
SELECT * FROM retail_sales

SELECT TOP 10 * FROM retail_sales

SELECT TOP 10 COUNT(*) FROM retail_sales


-- DATA CLEANING:-

-- Checking NULL VALUES
SELECT * FROM retail_sales
WHERE sale_time IS NULL

SELECT * FROM retail_sales
WHERE 
    transactions_id IS NULL
    OR
    sale_date IS NULL
    OR
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantiy IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;


-- Deleting NULL VALUES
DELETE FROM retail_sales
WHERE 
    transactions_id IS NULL
    OR
    sale_date IS NULL
    OR
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantiy IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;
	
-- How Many Sales We have?
SELECT COUNT(*) AS total_sales FROM retail_sales;

-- How Many Unique Customers We have?
SELECT COUNT(DISTINCT customer_id) AS total_unique_customers FROM retail_sales;

-- What are the Unique Categories We have?
SELECT DISTINCT category AS total_unique_categories FROM retail_sales;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

```
-- DATA ANALYIS & BUSINESS KEY Problems:-
-- My Analysis & Findings:-

-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than or equal 4 in the month of Nov-2022
SELECT * FROM retail_sales
WHERE category = 'Clothing' AND CONVERT(VARCHAR(7), sale_date, 120) = '2022-11' AND quantiy >= 4;

-- Q.3 Write a SQL query to calculate the total sales (total_sale) and total orders for each category.
SELECT category, SUM(total_sale) AS total_sales, COUNT(*) FROM retail_sales
GROUP BY  category;

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
SELECT category, AVG(age) AS avg_of_age FROM retail_sales
WHERE category = 'Beauty'
GROUP BY category;

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
SELECT * FROM retail_sales
WHERE total_sale > 1000;

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
SELECT category, gender, COUNT(transactions_id) AS total_transactions from retail_sales
GROUP BY  category, gender
ORDER BY category;

-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
SELECT year, month, avg_sale FROM 
(
    SELECT 
        YEAR(sale_date) AS year,
        MONTH(sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY YEAR(sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rank
    FROM retail_sales
    GROUP BY YEAR(sale_date), MONTH(sale_date)
) AS t1
WHERE rank = 1;

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
SELECT TOP 5 customer_id, SUM(total_sale) AS total_sales FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC;

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
SELECT category, COUNT(DISTINCT customer_id) AS total_unique_customers FROM retail_sales
GROUP BY category;

-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)
WITH hourly_sale AS (
    SELECT *,
        CASE 
            WHEN DATEPART(HOUR, sale_time) < 12 THEN 'Morning'
            WHEN DATEPART(HOUR, sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)

SELECT shift, COUNT(*) AS total_orders FROM hourly_sale
GROUP BY shift;
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Priyanshu Mukherjee

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

### My LinkedIn Profile

- **LinkedIn**: [Connect with me professionally](https://www.linkedin.com/in/priyanshu-mukherjee-570062265/)

Thank You!!!
