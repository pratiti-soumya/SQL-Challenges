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

## Output
![q3](/path_to_image_in_your_repository/q3.png)

### 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
```sql
SELECT M.product_name, COUNT(S.product_id) AS most_ordered
FROM Sales S
JOIN menu M ON S.product_id = M.product_id
GROUP BY M.product_name
ORDER BY most_ordered DESC
LIMIT 1;
```
- The SQL query selects the product_name from the menu table and counts the number of times each product was ordered (most_ordered).
- It retrieves data from the Sales table and joins it with the menu table based on matching product_id.
- The results are grouped by product_name.
- The COUNT(S.product_id) function calculates the number of occurrences of each product_id in the Sales table.
- The query then presents the product_name and its corresponding count as most_ordered for each product.
- Next, the results are sorted in descending order based on the most_ordered column, so the most ordered product appears first.
- The LIMIT 1 clause is used to restrict the result to only one row, effectively showing the most ordered product.

## Output
![q4](/path_to_image_in_your_repository/q4.png)

### 5. Which item was the most popular for each customer?
```sql
WITH CTE AS
(SELECT S.customer_id, M.product_name, COUNT(M.product_id) AS order_count, DENSE_RANK() OVER(PARTITION BY S.customer_id ORDER BY COUNT(S.product_id) DESC) AS rnk
 FROM sales S
 JOIN menu M ON S.product_id = M.product_id
 GROUP BY S.customer_id, M.product_name)
 
SELECT customer_id, product_name, order_count
FROM CTE
WHERE rnk = 1;
```
- The SQL query uses a Common Table Expression (CTE) named CTE to generate a temporary result set.
- Within the CTE, it selects the customer_id, product_name, and counts the number of times each product was ordered (order_count) for each customer.
- It retrieves data from the sales table and joins it with the menu table based on matching product_id.
- The results are grouped by customer_id and product_name to get the count of orders for each product of each customer.
- The COUNT(M.product_id) function calculates the number of occurrences of each product_id in the menu table.
- The DENSE_RANK() function assigns a rank to each row within the partition of each customer_id based on the order count of products in descending order.
- Each customer_id has its own partition and separate ranks based on the number of product orders.
- Next, the main query selects the customer_id, product_name, and order_count from the CTE.
- It filters the results and only includes rows where the rank rnk is equal to 1, which means the most ordered product for each customer_id.
- As a result, the query returns the customer's ID, the most ordered product, and the number of times it was ordered by that customer.

## Output
![q5](/path_to_image_in_your_repository/q5.png)

### 6. Which item was purchased first by the customer after they became a member?
```sql
SELECT DISTINCT ON (s.customer_id)
    s.customer_id,
    m.product_name
FROM sales s
JOIN members mbr ON s.customer_id = mbr.customer_id
JOIN menu m ON s.product_id = m.product_id
WHERE s.order_date > mbr.join_date
ORDER BY s.customer_id;
```
- The SQL query retrieves distinct rows for each unique customer_id with their corresponding product_name from the sales and menu tables.
- It filters the data based on the condition that the order_date in the sales table is greater than the join_date of the customer in the members table.
- The sales table is aliased as s, the members table is aliased as mbr, and the menu table is aliased as m.
- The query performs inner joins between sales and members tables on matching customer_id and between sales and menu tables on matching product_id.
- Only rows that meet the join condition and the order_date > join_date condition are considered in the result set.
- The query selects the customer_id and corresponding product_name for each customer who has placed an order after their join_date.
- The results are sorted in ascending order based on the customer_id.
- The DISTINCT ON (s.customer_id) clause ensures that only the first occurrence of each customer_id is included in the result set.
- As a result, the query returns a unique list of customer_id along with the first product_name they ordered after joining as a member.

## Output
![q6](/path_to_image_in_your_repository/q6.png)

### 7. Which item was purchased just before the customer became a member?
```sql
SELECT DISTINCT ON (s.customer_id)
    s.customer_id,
    m.product_name
FROM sales s
JOIN members mbr ON s.customer_id = mbr.customer_id
JOIN menu m ON s.product_id = m.product_id
WHERE s.order_date < mbr.join_date
ORDER BY s.customer_id;
```
- The SQL query retrieves distinct rows for each unique customer_id with their corresponding product_name from the sales and menu tables.
- It filters the data based on the condition that the order_date in the sales table is less than the join_date of the customer in the members table.
- The sales table is aliased as s, the members table is aliased as mbr, and the menu table is aliased as m.
- The query performs inner joins between sales and members tables on matching customer_id and between sales and menu tables on matching product_id.
- Only rows that meet the join condition and the order_date < join_date condition are considered in the result set.
- The query selects the customer_id and corresponding product_name for each customer who has placed an order before their join_date.
- The results are sorted in ascending order based on the customer_id.
- The DISTINCT ON (s.customer_id) clause ensures that only the first occurrence of each customer_id is included in the result set.
- As a result, the query returns a unique list of customer_id along with the first product_name they ordered before joining as a member.

## Output
![q7](/path_to_image_in_your_repository/q7.png)

### 8. What is the total items and amount spent for each member before they became a member?
```sql
SELECT S.customer_id,
    COUNT(S.product_id) AS total_item,
    SUM(M.price) AS total_amont
FROM sales S
JOIN menu M ON S.product_id = M.product_id
JOIN members ME ON S.customer_id = ME.customer_id
WHERE S.order_date < ME.join_date
GROUP BY S.customer_id
ORDER BY S.customer_id;
```
- The SQL query retrieves the customer_id along with the total count of items ordered (total_item) and the total amount spent (total_amont) by each customer.
- It retrieves data from the sales table and joins it with the menu table based on matching product_id.
- It also joins the sales table with the members table based on matching customer_id.
- The results are filtered based on the condition that the order_date in the sales table is less than the join_date of the customer in the members table.
- The COUNT(S.product_id) function calculates the number of occurrences of each product_id in the sales table, giving the total number of items ordered by each customer.
- The SUM(M.price) function calculates the sum of the price from the menu table, providing the total amount spent by each customer.
- Results are grouped by customer_id to get the totals for each customer.
- The query then presents the customer_id, total_item, and total_amont for each customer who placed orders before joining as a member.
- Finally, the results are sorted in ascending order based on the customer_id.

## Output

### 9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
```sql
SELECT s.customer_id,
    SUM(CASE
            WHEN m.product_name = 'sushi' THEN price * 2
            ELSE price
        END) * 10 AS total_points
FROM sales s
JOIN menu m ON s.product_id = m.product_id
GROUP BY s.customer_id
ORDER BY s.customer_id;
```
- The SQL query retrieves the customer_id and calculates the total points (total_points) earned by each customer based on their purchases from the sales and menu tables.
- It retrieves data from the sales table and joins it with the menu table based on matching product_id.
- The query uses a CASE statement to differentiate between 'sushi' and other products.
- If the product name is 'sushi', the price is multiplied by 2 to give double points.
- Otherwise, the regular price is considered.
- The SUM function calculates the total points for each customer by adding up the points earned from their purchases.
- The total points are then multiplied by 10 to give a scaled value.
- Results are grouped by customer_id to get the total points for each customer.
- The query then presents the customer_id and the scaled total_points for each customer based on their purchases.
- Finally, the results are sorted in ascending order based on the customer_id.

## Output

### 10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
```sql
WITH dates_cte AS (
    SELECT 
        customer_id, 
        join_date, 
        join_date + INTERVAL '6 days' AS valid_date, 
        DATE_TRUNC('month', '2021-01-31'::DATE) + INTERVAL '1 month' - INTERVAL '1 day' AS last_date
    FROM members
)

SELECT 
    s.customer_id, 
    SUM(CASE
        WHEN m.product_name = 'sushi' OR (s.order_date BETWEEN dates.join_date AND dates.valid_date) THEN 2 * 10 * m.price
        ELSE 10 * m.price 
    END) AS points
FROM sales s
INNER JOIN dates_cte AS dates ON s.customer_id = dates.customer_id
AND dates.join_date <= s.order_date
AND s.order_date <= dates.last_date
INNER JOIN menu m ON s.product_id = m.product_id
GROUP BY s.customer_id
ORDER BY s.customer_id;
```
- The SQL query starts by creating a Common Table Expression (CTE) named dates_cte.
- Within the CTE, it selects customer_id, join_date, join_date + INTERVAL '6 days' as valid_date, and the last day of the month for the date '2021-01-31' as last_date.
- The CTE is used to generate date ranges for each customer, from their join_date to 6 days later, and the last day of the month for January 2021.
- Next, the main query selects the customer_id and calculates the total points (points) earned by each customer based on their purchases from the sales and menu tables.
- It retrieves data from the sales table and joins it with the dates_cte CTE using a combination of JOIN and ON clauses.
- The query uses a CASE statement to differentiate between 'sushi' purchases and other products.
- If the product name is 'sushi' or the order date falls within the range of join_date to valid_date, the points are calculated as 2 times 10 times the price of the product.
- Otherwise, for other products, the points are calculated as 10 times the price of the product.
- The SUM function calculates the total points for each customer by adding up the points earned from their purchases.
- Results are grouped by customer_id to get the total points for each customer.
- The query then presents the customer_id and the calculated points for each customer based on their purchases.
- Finally, the results are sorted in ascending order based on the customer_id.

## Output

### Key Insights

### Customer Spending

There's a big difference in how much each customer spends at Danny's Diner. Some customers spend a lot more than others, which shows we might have some really loyal customers.

### Customer Visits

How often customers come to the diner varies too. Some come a lot, while others only a few times.

### First Purchases

Knowing what items new customers buy first helps us figure out which dishes attract people to Danny’s Diner.

### Most Popular Item

Finding out which item is ordered the most helps Danny manage his stock better and focus on what customers like most.

### Personalized Recommendations

If we know which items are favorites for each customer, Danny can suggest dishes they might like, making their visits even better.

### Additional Insights

- Customer Loyalty: Looking at what customers buy before and after they join our loyalty program shows us how well the program is working.
- Bonus Points for New Members: Giving double points in the first week encourages new loyalty members to spend more.
- Member Points: Tracking the points each member earns helps us see who our most loyal customers are and lets us offer them special deals.
- Data Visualization: Charts and graphs based on our data help Danny spot trends and make decisions based on facts.
- Customer Segmentation: By understanding how different customers spend, Danny can create marketing that's more likely to appeal to different groups.
- Expanding Membership: Insights from the data can help improve the loyalty program and draw in new members.
- Inventory Management: Knowing which items are least and most popular can help Danny reduce waste and increase profits.
- Menu Optimization: This data can also help Danny decide which menu items to keep or change and think about adding new dishes that customers might like.
- Customer Engagement: Understanding why customers keep coming back helps Danny make the diner more appealing.
- Long-Term Growth: Using all this data, Danny can make informed choices that help the diner grow and succeed over time.




