# 🍜 Case Study #1: Danny's Diner
<img width="494" alt="image" src="https://github.com/naakordaiaddy/SQL-Portfolio-Projects/assets/126539576/f5808a62-57de-4393-9e0c-b823a34185b8">


## 📚 Table of Contents
- [Business Task](#business-task)
- [Data Set](#data-set)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- [Questions & Solutions](#questions--solutions)

Please note that all the information regarding the case study has been sourced from the following link: [here.](https://8weeksqlchallenge.com/case-study-1/)

---  
### Question and Solution
Please note that all the information regarding the case study has been sourced from the following link: here.


### Business Task
Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite.


## Entity Relationship Diagram
<img width="710" alt="image" src="https://github.com/naakordaiaddy/SQL-Portfolio-Projects/assets/126539576/b616a502-3e9e-4069-9c4b-644acc7ff15e">


---
# Questions and Solutions
## 1. What is the total amount each customer spent at the restaurant?
```ruby
SELECT
  s.customer_id,
  SUM(m.price) AS amt_spent
FROM dannys_diner.sales s
INNER JOIN dannys_diner.menu m
ON m.product_id=s.product_id
GROUP BY s.customer_id;
```
### Steps:
Use JOIN to merge dannys_diner.sales and dannys_diner.menu tables as sales.customer_id and menu.price are from both tables.
Use SUM to calculate the total sales contributed by each customer.
Group the aggregated results by sales.customer_id.

### Answer:
|customer_id |	total_sales |
|---|---|
|A	| 76|
|B	| 74|
|C	| 36|

- Customer A spent $76.
- Customer B spent $74.
- Customer C spent $36.


## 2. How many days has each customer visited the restaurant?
```ruby
SELECT
  customer_id AS customer,
  COUNT(s.order_date) AS visits
FROM dannys_diner.sales s
GROUP BY customer_id;
```
### Steps:
To determine the unique number of visits for each customer, utilize COUNT(DISTINCT order_date).
It's important to apply the DISTINCT keyword while calculating the visit count to avoid duplicate counting of days. For instance, if Customer A visited the restaurant twice on '2021–01–07', counting without DISTINCT would result in 2 days instead of the accurate count of 1 day.

### Answer:
|customer|	visits|
|---|---|
|A|	4|
|B| 6|
|C|	2|

- Customer A visited 4 times.
- Customer B visited 6 times.
- Customer C visited 2 times.

  
## 3. What was the first item from the menu purchased by each customer?
```ruby
WITH ordered_sales AS (
  SELECT 
    sales.customer_id, 
    sales.order_date, 
    menu.product_name,
    DENSE_RANK() OVER (
      PARTITION BY sales.customer_id 
      ORDER BY sales.order_date) AS rank
  FROM dannys_diner.sales
  INNER JOIN dannys_diner.menu
    ON sales.product_id = menu.product_id
)

SELECT 
  customer_id, 
  product_name
FROM ordered_sales
WHERE rank = 1
GROUP BY customer_id, product_name;
```

### Steps:
- Create a Common Table Expression (CTE) named `ordered_sales_cte` Within the CTE, create a new column rank and calculate the row number using `ROW_NUMBER()` window function. The `PARTITION BY` clause divides the data by `customer_id,` and the `ORDER BY` clause orders the rows within each partition by order_date.
- In the outer query, select the appropriate columns and apply a filter in the WHERE clause to retrieve only the rows where the rank column equals 1, which represents the first row within each `customer_id` partition.
- Use the `GROUP BY` clause to group the result by `customer_id` and `product_name.`

### Answer:
|customer_id	| product_name|
|---|---|
|A	|curry|
|B	|curry|
|C	|ramen|

- Customer A placed an order for both curry and sushi simultaneously, making them the first items in the order.
- Customer B's first order is curry.
- Customer C's first order is ramen.

## 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
```RUBY
SELECT 
  menu.product_name,
  COUNT(sales.product_id) AS most_purchased_item
FROM dannys_diner.sales
INNER JOIN dannys_diner.menu
  ON sales.product_id = menu.product_id
GROUP BY menu.product_name
ORDER BY most_purchased_item DESC
LIMIT 1;
```

### Steps:
- Perform a `COUNT` aggregation on the `product_id` column and `ORDER BY` the result in descending order using most_purchased field.
- Apply the `LIMIT` 1 clause to filter and retrieve the highest number of purchased items.
  
### Answer:

|most_purchased	|product_name|
|---|---|
|8	|ramen|

Most purchased item on the menu is ramen which is 8 times. 

## 5. Which item was the most popular for each customer?

WITH most_popular AS (
  SELECT 
    sales.customer_id, 
    menu.product_name, 
    COUNT(menu.product_id) AS order_count,
    DENSE_RANK() OVER (
      PARTITION BY sales.customer_id 
      ORDER BY COUNT(sales.customer_id) DESC) AS rank
  FROM dannys_diner.menu
  INNER JOIN dannys_diner.sales
    ON menu.product_id = sales.product_id
  GROUP BY sales.customer_id, menu.product_name
)

SELECT 
  customer_id, 
  product_name, 
  order_count
FROM most_popular 
WHERE rank = 1;
Each user may have more than 1 favourite item.

Steps:
- Create a CTE named `most_popular`` and within the CTE, join the menu table and sales table using the product_id column.
- Group results by `sales.customer_id` and `menu.product_name` and calculate the count of menu.product_id occurrences for each group.
- Utilize the `DENSE_RANK()` window function to calculate the ranking of each `sales.customer_id` partition based on the count of orders `COUNT(sales.customer_id)` in descending order.
- In the outer query, select the appropriate columns and apply a filter in the `WHERE` clause to retrieve only the rows where the rank column equals 1, representing the rows with the highest order count for each customer.

### Answer:

|customer_id|	product_name	|order_count|
|---|---|---|
|A	|ramen|	3|
|B	|sushi|	2|
|B	|curry|	2|
|B	|ramen|	2|
|C	|ramen|	3|

- Customer A and C's favourite item is ramen.
- Customer B enjoys all items on the menu. He/she is a true foodie, sounds like me.


## 6. Which item was purchased first by the customer after they became a member?

```ruby
WITH joined_as_member AS (
  SELECT
    members.customer_id, 
    sales.product_id,
    ROW_NUMBER() OVER (
      PARTITION BY members.customer_id
      ORDER BY sales.order_date) AS row_num
  FROM dannys_diner.members
  INNER JOIN dannys_diner.sales
    ON members.customer_id = sales.customer_id
    AND sales.order_date > members.join_date
)

SELECT 
  customer_id, 
  product_name 
FROM joined_as_member
INNER JOIN dannys_diner.menu
  ON joined_as_member.product_id = menu.product_id
WHERE row_num = 1
ORDER BY customer_id ASC;
```

### Steps:

- Create a CTE named `joined_as_member` and within the CTE, select the appropriate columns and calculate the row number using the `ROW_NUMBER()` window function. The `PARTITION BY` clause divides the data by `members.customer_id` and the `ORDER BY` clause orders the rows within each members.customer_id partition by `sales.order_date.`
- Join tables `dannys_diner.members` and `dannys_diner.sales` on `customer_id` column. Additionally, apply a condition to only include sales that occurred after the member's join_date `(sales.order_date > members.join_date).`
- In the outer query, join the `joined_as_member` CTE with the dannys_diner.menu on the `product_id` column.
- In the `WHERE` clause, filter to retrieve only the rows where the row_num column equals 1, representing the first row within each `customer_id` partition.
- Order result by `customer_id` in ascending order.

### Answer:

|customer_id|	product_name|
|---|---|
|A	|ramen|
|B	|sushi|

- Customer A's first order as a member is ramen.
- Customer B's first order as a member is sushi.
  
## 7. Which item was purchased just before the customer became a member?

```ruby
WITH joined_as_member AS (
  SELECT
    members.customer_id, 
    sales.product_id,
    ROW_NUMBER() OVER (
      PARTITION BY members.customer_id
      ORDER BY sales.order_date) AS row_num
  FROM dannys_diner.members
  INNER JOIN dannys_diner.sales
    ON members.customer_id = sales.customer_id
    AND sales.order_date > members.join_date
)

SELECT 
  customer_id, 
  product_name 
FROM joined_as_member
INNER JOIN dannys_diner.menu
  ON joined_as_member.product_id = menu.product_id
WHERE row_num = 1
ORDER BY customer_id ASC;
```

### Steps:
- Create a CTE named `joined_as_member` and within the CTE, select the appropriate columns and calculate the row number using the `ROW_NUMBER() ` window function. The `PARTITION BY` clause divides the data by `members.customer_id` and the `ORDER BY` clause orders the rows within each `members.customer_id` partition by `sales.order_date.`
- Join tables `dannys_diner.members` and `dannys_diner.sales` on customer_id column. Additionally, apply a condition to only include sales that occurred after the member's join_date `(sales.order_date > members.join_date).`
- In the outer query, join the `joined_as_member` CTE with the dannys_diner.menu on the `product_id` column.
- In the `WHERE` clause, filter to retrieve only the rows where the `row_num` column equals 1, representing the first row within each `customer_id` partition.
- Order result by `customer_id` in ascending order.
  
### Answer:
|customer_id	|product_name|
|---|---|
|A|	ramen|
|B|	sushi|

- Customer A's first order as a member is ramen.
- Customer B's first order as a member is sushi.


## 8. What is the total items and amount spent for each member before they became a member?
```ruby
SELECT 
  sales.customer_id, 
  COUNT(sales.product_id) AS total_items, 
  SUM(menu.price) AS total_sales
FROM dannys_diner.sales
INNER JOIN dannys_diner.members
  ON sales.customer_id = members.customer_id
  AND sales.order_date < members.join_date
INNER JOIN dannys_diner.menu
  ON sales.product_id = menu.product_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id;
```

### Steps:

- Select the columns sales.customer_id and calculate the count of `sales.product_id` as `total_items` for each customer and the sum of `menu.price` as `total_sales.`
- From dannys_diner.sales table, join dannys_diner.members table on customer_id column, ensuring that sales.order_date is earlier than `members.join_date (sales.order_date < members.join_date).`
- Then, join `dannys_diner.menu` table to `dannys_diner.sales` table on `product_id` column.
- Group the results by `sales.customer_id.`
- Order the result by `sales.customer_id` in ascending order.

### Answer:

|customer_id	|total_items	|total_sales|
|---|---|---|
|A	|2|	25|
|B|	3	|40|

Before becoming members,
- Customer A spent $25 on 2 items.
- Customer B spent $40 on 3 items.

## 9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier — how many points would each customer have?
```ruby
WITH points_cte AS (
  SELECT 
    menu.product_id, 
    CASE
      WHEN product_id = 1 THEN price * 20
      ELSE price * 10 END AS points
  FROM dannys_diner.menu
)

SELECT 
  sales.customer_id, 
  SUM(points_cte.points) AS total_points
FROM dannys_diner.sales
INNER JOIN points_cte
  ON sales.product_id = points_cte.product_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id;
```

### Steps:
Let's break down the question to understand the point calculation for each customer's purchases.

- Each $1 spent = 10 points. However, product_id 1 sushi gets 2x points, so each $1 spent = 20 points.
- Here's how the calculation is performed using a conditional `CASE` statement:
- If `product_id` = 1, multiply every $1 by 20 points.
- Otherwise, multiply $1 by 10 points.
- Then, calculate the total points for each customer.

### Answer:
|customer_id	|total_points|
|---|---|
|A	|860|
|B	|940|
|C	|360|

- Total points for Customer A is $860.
- Total points for Customer B is $940.
- Total points for Customer C is $360.


