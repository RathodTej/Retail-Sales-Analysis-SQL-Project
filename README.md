# Retail-Sales-Analysis-SQL-Project

## Project Overview

**Project Title**: Retail Sales Analysis<br>
**Level**: Beginner<br>
**Database**: `retail_sales_database`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives
 1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
 2. **Data Cleaning**: Identify and remove any records with missing or null values.
 3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
 4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named  `retail_sales_database`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
-- create database
create database retail_sales_database;

use retail_sales_database;

-- create table
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
    cogs FLOAT,
    total_sale FLOAT
);

DESCRIBE retail_sales;
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
WHERE transactions_id IS NULL OR sale_date IS NULL
	OR customer_id IS NULL OR sale_time IS NULL OR gender IS NULL
	OR age IS NULL OR category IS NULL
	OR quantity IS NULL OR price_per_unit IS NULL
	OR cogs IS NULL OR total_sale IS NULL;


DELETE FROM retail_sales
WHERE transactions_id IS NULL OR sale_date IS NULL
	OR customer_id IS NULL OR sale_time IS NULL OR gender IS NULL
	OR age IS NULL OR category IS NULL
	OR quantity IS NULL OR price_per_unit IS NULL
	OR cogs IS NULL OR total_sale IS NULL;
```

### 3. Data Analysis & Findings
The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
select * 
from retail_sales 
where sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 3 in the month of Nov-2022**:
```sql
select *
from retail_sales
where category = 'clothing'
and quantity >3 
and month(sale_date) = 11 and year(sale_date)=2022;
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category**:
```sql
select category, 
	sum(total_sale) net_sale,
    	count(*) total_orders
from retail_sales
group by category;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category**:
```sql
select round(avg(age),2) avg_age
from retail_sales
where category = "Beauty";
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000**:
```sql
select *
from retail_sales
where total_sale > 1000;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category**:
```sql
select category,
	gender,
	count(*) as total_trans
from retail_sales 
group by category, gender
order by category,gender desc;
```

7. **Write a SQL query to calculate the average sale for each month, Find out best selling month in each year**:
```sql
select _year,_month, _avg
from
(
	select year(sale_date) _year,
	month(sale_date) _month, 
	avg(total_sale) _avg,
	rank() over(
			partition by year(sale_date) 
            		order by avg(total_sale) desc
		) as _rank
	from retail_sales
	group by year(sale_date), month(sale_date)
) t1
where _rank =1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales**:
```sql
select 
	customer_id,
    	sum(total_sale) total_sales
from retail_sales
group by customer_id
order by total_sales desc
limit 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category**:
```sql
select 
	category, 
	count(distinct customer_id) unique_customer_id_count
from retail_sales
group by category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
select 
	case 
		when hour(sale_time) < 12 then "Morning"
		when hour(sale_time) between 12 and 17 then "Afternoon"
		else "Evening"
        end shift,
	count(transactions_id) total_orders
from retail_sales
group by shift;
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

- **Total Sales**: The total sales recorded in the dataset amounted to
  	| **category**	| **net_sale**	| **total_orders**|
  	|---------------|---------------|---------------|
	|Beauty		|286790		|611		|
	|Clothing	|309995		|698		|
	|Electronics	|311445		|678		|

- **Customer Distribution**: A total of `155` unique customers made purchases, with varying preferences across categories.
- **Top Categories**: The categories with the highest sales were:
	| **category**	| **net_sale**	|
  	|---------------|---------------|
	|Beauty		|286790		|
	|Clothing	|309995		|
	|Electronics	|311445		|

- **Best-Selling Month**: The best-selling month was `July 2022`, with an average sale of `541.34`.
- **Customer Age**: The average age of customers purchasing from the Beauty category was `40.42` years.
- **Sales by Gender**: Transaction counts were fairly balanced between genders, indicating a diverse customer base.
- **Peak Sales Periods**: Most sales occurred in the Evening shift, followed by Morning and Afternoon.
- **Top Customers**: The top five customers based on total sales contributed significantly to overall revenue:
	|customer_id	|total_sales|
	|---------------|-----------|
	|3	|38440|
	|1	|30750|
	|5	|30405|
	|2	|25295|
	|4	|23580|

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights on sales trends over time, including best-selling months and peak sales periods.
- **Customer Insights**: Analysis of customer demographics, purchasing behavior, and identification of top customers

## Conclusion

This project provides a solid introduction to SQL for data analysts, encompassing database setup, data cleaning, exploratory data analysis, and practical SQL queries. The insights gained from this analysis can support better business decisions by revealing trends in sales, understanding customer behavior, and assessing product performance. Overall, it highlights the significance of using data to drive effective strategies in retail.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the [`Database Setup`](https://github.com/RathodTej/Retail-Sales-Analysis-SQL-Project?tab=readme-ov-file#1-database-setup)  to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the [`Data Analysis & Findings`](https://github.com/RathodTej/Retail-Sales-Analysis-SQL-Project?tab=readme-ov-file#3-data-analysis--findings)  to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - RJ_RATHOD
This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

### Refered by : [Zero Analyst](https://github.com/najirh)

### Stay Updated and Join the Community
For more content on SQL, data analysis, and other data-related topics, make sure to follow me on:

- **LinkedIn**: [Connect with me here](https://www.linkedin.com/in/bhanu-teja-rathod/)

Thank you for your support, and I look forward to connecting with you!
