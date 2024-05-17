# Case Study: Analyzing Customer Orders at Davie's Burgers

## Introduction
Davie's Burgers, a popular burger joint, collects customer order data to improve their services and customer experience. The data contains various details about the orders, including special instructions left by customers for the kitchen or delivery team. This case study aims to analyze the special instructions provided by customers, identify patterns, and derive insights that can help Davie's Burgers enhance their operations.

## Problem Statement
The primary objective is to analyze the special instructions provided by customers to understand their preferences and any specific requests. By examining these instructions, Davie's Burgers can tailor their services to meet customer needs better and improve overall satisfaction.

## Data Analysis

### Step 1: Preview the Data
To get an initial understanding of the data, we preview the first 10 rows of the orders table.
```sql
SELECT * 
FROM orders 
LIMIT 10;
```
### Step 2: Assess Data Recency : Determine the recency of the data by finding all unique order dates.
```sql
SELECT DISTINCT order_date 
FROM orders 
ORDER BY order_date;
```

### Step 3: Extract Special Instructions : Retrieve only the special instructions from the orders, limiting the result to 20 rows
```sql
SELECT special_instructions 
FROM orders 
LIMIT 20;
```

### Step 4: Filter Non-Empty Special Instructions : Filter the special instructions to exclude empty values.
```sql
SELECT special_instructions 
FROM orders 
WHERE special_instructions IS NOT NULL 
LIMIT 20;
```
### Step 5: Sort Special Instructions : Alphabetically Sort the non-empty special instructions in alphabetical order (A-Z).
```sql
SELECT special_instructions 
FROM orders 
WHERE special_instructions IS NOT NULL 
ORDER BY special_instructions 
LIMIT 20;
```

### Step 6: Search for Instructions : Containing 'Sauce' Identify special instructions that mention 'sauce'.
```sql
SELECT special_instructions 
FROM orders 
WHERE special_instructions LIKE '%sauce%';
```

### Step 7: Search for Instructions : Containing 'Door' Identify special instructions that mention 'door'.
```sql
SELECT special_instructions 
FROM orders 
WHERE special_instructions LIKE '%door%';
```

### Step 8: Search for Instructions : Containing 'Box' Identify special instructions that mention 'box'.
```sql
SELECT special_instructions 
FROM orders 
WHERE special_instructions LIKE '%box%';
```

### Insights
- Data Recency: The orders data spans a specific range of dates, providing a timeline of customer interactions with Davie's Burgers.
- Customer Preferences: By analyzing special instructions, Davie's Burgers can identify common requests and preferences, such as extra sauce or specific delivery instructions.
- Order Customizations: Common keywords in special instructions, like 'sauce', 'door', and 'box', indicate areas where customers frequently provide additional details or requests.
- Improving Services: Understanding these instructions can help the restaurant better accommodate customer needs, potentially improving customer satisfaction and loyalty.

### Conclusion
By analyzing the special instructions provided by customers, Davie's Burgers can gain valuable insights into customer preferences and behavior. This information can be used to improve service quality, tailor customer experiences, and ultimately enhance customer satisfaction and loyalty.
