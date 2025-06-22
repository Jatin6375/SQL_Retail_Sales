# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `sql_project_1`

This is a simple SQL project I did to practice my SQL skills as a beginner. I worked with a retail sales dataset and tried to answer some business-related questions using SQL queries. This project helped me understand how to create databases and tables, clean data, and write useful queries to analyze and get insights from the data. It’s a small but complete project to showcase what I’ve learned so far.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `sql_project_1`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sql_project_1;

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

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL
    OR
    sale_time IS NULL
    OR
    customer_id IS NULL
    OR 
    gender IS NULL
    OR
    age IS NULL
    OR
    category IS NULL
    OR 
    quantity IS NULL
    OR
    price_per_unit IS NULL
    OR
    cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL
    OR
    sale_time IS NULL
    OR
    customer_id IS NULL
    OR 
    gender IS NULL
    OR
    age IS NULL
    OR
    category IS NULL
    OR 
    quantity IS NULL
    OR
    price_per_unit IS NULL
    OR
    cogs IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
 SELECT * FROM retail_sales
  WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
 SELECT * FROM retail_sales
  WHERE 
	category = 'Clothing'
    AND
    DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
    AND 
    quantiy >= 4;
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
		category,
		SUM(total_sale) AS net_sales_per_category,
        COUNT(transactions_id) AS total_orders
        FROM retail_sales
GROUP BY CATEGORY;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT 
    ROUND(AVG(age)) AS avg_age
	FROM retail_sales
WHERE category= 'Beauty';
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > '1000';
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT category, gender, count(transactions_id) AS total_trans FROM retail_sales
GROUP BY category, gender
ORDER BY 1, 2;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT * FROM (
SELECT 
	YEAR(sale_date) AS year,
    MONTH(sale_date) AS month,
    AVG(total_sale) AS avg_sale_monthly,
    RANK() OVER (PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC ) AS rank_sale
FROM retail_sales
 GROUP BY 1,2
 ORDER BY 1
 ) AS t1
 WHERE rank_sale = '1';
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT customer_id, SUM(total_sale) AS net_sale FROM retail_sales
 GROUP BY 1
 ORDER BY net_sale DESC LIMIT 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT category, COUNT(DISTINCT customer_id) as Unique_Customer FROM retail_sales
 GROUP BY 1;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale 
AS
(
SELECT *,
	CASE 
		WHEN HOUR(sale_time) < 12 THEN 'Morning'
        WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
	END AS Shift
 FROM retail_sales
 )
 SELECT shift, count(*) FROM hourly_sale
 GROUP BY shift
```

## Key Learnings
1. Got hands-on experience with creating databases and tables in SQL.
2. Practiced basic data cleaning using NULL checks and DELETE.
3. Learned how to use GROUP BY, CASE, CTE, and RANK() in SQL.
4. Understood how SQL can help answer real business questions using data.

## Reports & Insights

**Top Product Categories:** "Clothing" and "Beauty" are among the most frequently purchased categories.
**High-Value Transactions:** Several transactions had a total sale amount above 1000, indicating premium purchases.
**Monthly Sales Trends:** Some months showed higher average sales, suggesting seasonal buying patterns.
**Customer Loyalty:** The top 5 customers contributed significantly to total sales.
**Unique Customers per Category:** Each product category attracted a distinct set of customers.
**Shift-wise Sales:** Evening shift (after 5 PM) had the most number of transactions, followed by afternoon and morning.

## Conclusion

This project helped me practice and apply SQL skills in a real-world scenario. I learned how to create databases, clean data, and write queries to analyze retail sales. By answering different business questions, I understood how data can reveal important insights like customer behavior, sales trends, and product performance.
Overall, this was a great learning experience and gave me more confidence in using SQL for data analysis. I look forward to exploring more datasets and improving my skills further!

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## About Me — Jatin Goyal
I’m learning data analysis and building projects to improve my SQL and analytics skills. This project is part of my learning journey. If you're also learning or have any suggestions for improvement, feel free to connect!
