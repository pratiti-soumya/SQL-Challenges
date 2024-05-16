# Case Study #1 - Danny's Diner

## Introduction
In early 2021, Danny followed his passion for Japanese food and opened "Danny's Diner," a charming restaurant offering sushi, curry, and ramen. However, lacking data analysis expertise, the restaurant struggled to leverage the basic data collected during its initial months to make informed business decisions. Danny's Diner seeks assistance in using this data effectively to keep the restaurant thriving.

## Problem Statement
Danny aims to utilize customer data to gain valuable insights into their visiting patterns, spending habits, and favorite menu items. By establishing a deeper connection with his customers, he can provide a more personalized experience for his loyal patrons. He plans to use these insights to make informed decisions about expanding the existing customer loyalty program. Additionally, Danny seeks assistance in generating basic datasets for his team to inspect the data conveniently, without requiring SQL expertise. Due to privacy concerns, he has shared a sample of his overall customer data, hoping it will be sufficient for you to create fully functional SQL queries to address his questions.

## The case study revolves around three key datasets:
- Sales
- Menu
- Members

## Entity Relationship Diagram
![Entity Relationship Diagram](/path_to_image_in_your_repository/ERD.png)

## Case Study Questions & Solutions

### 1. What is the total amount each customer spent at the restaurant?
```sql
SELECT S.customer_id, SUM(M.price) AS total_amnt
FROM sales S
JOIN menu M ON S.product_id = M.product_id
GROUP BY S.customer_id
ORDER BY customer_id;
```

- The SQL query retrieves the customer_id and calculates the total amount spent (total_amnt) by each customer at the restaurant.
- It combines data from the sales and menu tables based on matching product_id.
- The results are grouped by customer_id.
- The query then calculates the total sum of price for each group of sales records with the same customer_id.
- Finally, the results are sorted in ascending order based on the customer_id.

## Output
![q1](/path_to_image_in_your_repository/q1.png)

### 2. How many days has each customer visited the restaurant?
```sql
SELECT customer_id, COUNT(DISTINCT order_date) AS No_Days
FROM sales
GROUP BY customer_id;
```
- The SQL query selects the customer_id and counts the number of distinct order dates (No_Days) for each customer.
- It retrieves data from the sales table.
- The results are grouped by customer_id.
- The COUNT(DISTINCT order_date) function calculates the number of unique order dates for each customer.
- Finally, the query presents the total number of unique order dates as No_Days for each customer.

## Output
![q2](/path_to_image_in_your_repository/q2.png)

### 3. What was the first item from the menu purchased by each customer?
```sql
WITH CTE AS
(SELECT S.customer_id, DENSE_RANK() OVER(PARTITION BY S.customer_id ORDER BY S.order_date) AS rn, M.product_name
 FROM sales S
 JOIN menu M ON S.product_id = M.product_id)
 
SELECT customer_id, product_name
FROM CTE
WHERE rn = 1;
```
- The SQL query uses a Common Table Expression (CTE) named CTE to generate a temporary result set.
- Within the CTE, it selects the customer_id, assigns a dense rank to each row based on the order_date for each customer, and retrieves the corresponding product_name from the menu table.
- The sales table is joined with the menu table on matching product_id.
- The DENSE_RANK() function assigns a rank to each row within the partition of each customer_id based on the order_date in ascending order.
- Each customer_id has its own partition and separate ranks based on the order dates of their purchases.
- Next, the main query selects the customer_id and corresponding product_name from the CTE.
- It filters the results and only includes rows where the rank rn is equal to 1, which means the earliest purchase for each customer_id.
- As a result, the query returns the first purchased product for each customer.



