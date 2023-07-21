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
## Questions and Solutions
### 1. What is the total amount each customer spent at the restaurant?
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


### 2. How many days has each customer visited the restaurant?
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

  
### 3. What was the first item from the menu purchased by each customer?


### 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
### 5. Which item was the most popular for each customer?
### 6. Which item was purchased first by the customer after they became a member?
### 7. Which item was purchased just before the customer became a member?
### 8. What is the total items and amount spent for each member before they became a member?
### 9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier — how many points would each customer have?
### 10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi — how many points do customer A and B have at the end of January?

