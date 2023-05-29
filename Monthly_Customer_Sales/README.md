# Monthly Customer Sales with Clothing Company

## Table of Contents
- [Business Task](#business-task)
- [Data Set](#data-set)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- [Questions & Solutions][#questions-and-solutions]


--
### Business Task 
Analyze customer orders to garner general information on customer behavior and demographics. 


### Data Set
- Orders & Sales data from [Break Into Tech SQL Course](https://www.dropbox.com/s/cvsasmtr8syq2c2/BIT_DB?dl=0)


### Entity Relationship Diagram
<img width="1109" alt="Screen Shot 2023-05-28 at 5 26 57 PM" src="https://github.com/naakordaiaddy/SQL-Portfolio-Projects/assets/126539576/80156e36-3e6d-459f-b247-f179352ab842">

## Questions and Solutions
### 1. How many orders were placed in January?
```ruby
SELECT COUNT(orderID) AS total_jan_sales
FROM BIT_DB.JanSales
WHERE length(orderid) = 6 
AND orderid <> 'Order ID';
```

### Steps: 
- Use COUNT to find out `total_jan_sales` in January 
- Use WHERE and AND to to avoid incomplete or NULL values in `orderid`

### Answer: 
| total_jan_sales |
| --- |
| 9681 |

- There were 9,681 orders placed in January.




## 2. How many of those orders were for an iPhone?
```ruby
SELECT COUNT(orderID) AS total_jan_sales
FROM BIT_DB.JanSales
WHERE Product = 'iPhone'
AND length(orderid) = 6 
AND orderid <> 'Order ID';
```

### Steps: 
- Use COUNT to find out `total_jan_sales` in January 
- Use WHERE to find orders where only and iPhone was purchase. 
- Use AND to to avoid incomplete or NULL values in `orderid`

### Answer: 
| total_jan_sales |
| --- |
| 379 |

- There were 379 sales in January where only an iPhone was bought.




## 4. Which product was the cheapest one sold in January, and what was the price?
```ruby
SELECT distinct Product, price
FROM BIT_DB.JanSales
WHERE price <= (SELECT MIN (price) FROM BIT_DB.JanSales)
LIMIT 1;
```

### Steps: 
- Use WHERE within a subquery to find lowest min `price` because we are looking `MIN(price)` of all products. 
- Use LIMIT to list the cheapest `product`

### Answer: 
| Product | price |
| --- | --- |
| AAA Batteries (4-pack) | 2.99 |

- The cheapest item sold in January was a 4-pack of Triple A batteries for $2.99


## 5. What is the total revenue for each product sold in January?
```ruby
SELECT product, ROUND(SUM(quantity)*price, 2) AS revenue
FROM BIT_DB.JanSales
WHERE Product IS NOT NULL AND Product <> ' '
GROUP BY product
ORDER BY revenue DESC;
```

### Steps: 
- Use SUM and multiply by price to find the `quanitty` of each product sold
- ROUND to display `revenue` as a depicted price
- WHERE and NOT NULL removes incomplete information 
- Use GROUP BY to group information by `product`
- ORDER BY to view expesive to least expensive `product`

### Answer:
| Product | revenue |
| --- | --- |
| MacBook Pro Laptop | 399500 |
| iPhone | 265300 |
| ThinkPad Laptop | 216997.83 |
| Google Phone | 190800 |
| Apple AirPod Headphones | 122100 |




## 6. Which products were sold in February at 548 Lincoln St, Seattle, WA 98101, how many of each were sold, and what was the total revenue?
```ruby
SELECT SUM(quantity) AS amt_sold, product, SUM(Quantity)*price AS revenue
FROM BIT_DB.FEBSales 
WHERE location LIKE '%548 Lincoln St%';
```

### Steps: 
- Use COUNT and SUM to find all purchases made at that address
- Use WHERE to isolate customer location 

### Answer: 
| amt_sold | product | revenue |
| --- | --- | --- |
| 2 | AA Batteries (4-pack) | 7.68 |

- 2 Double-A batteries were sold for $7.68 at 548 Lincoln St, Seattle, WA 98101.



## 7. How many customers ordered more than 2 products at a time in February, and what was the average amount spent for those customers?
``` ruby
SELECT COUNT(distinct cust.acctnum), ROUND(AVG(quantity*price),2)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON FEB.orderid=cust.order_id
WHERE Feb.Quantity>2
AND length(orderid) = 6 
AND orderid <> 'Order ID';
```

### Steps: 
- Use COUNT distinct to find real `cust.acctnum`orders
- Use LEFT JOIN to connect `FebSales` with `customers`
- WHERE to isolate customers who ordered three or more items
- AND used to avoid incoplete or NULL values in `orderid`

### Answers: 
| total_cust | avg_amt |
| --- | --- |
| 278 | 13.83 |

- There were 278 customers who ordered more than 2 products and spent an average of $13.83 in February.




## 8. Which locations in New York received at least 3 orders in January, and how many orders did they each receive? 
```ruby
SELECT location, COUNT(location) AS order_amt
FROM BIT_DB.JanSales
WHERE location LIKE '%NY%'
AND length(orderid) = 6 
AND orderid <> 'Order ID'
GROUP BY location
HAVING COUNT(location)>=3;
```

### Steps: 
- Use COUNT and GROUP BY to isolate `location` 
- Use WHERE to find customers who live in New York 
- Use HAVING to aggregate the `location` that received at least 3 orders

### Answers: 
| location | order_amt |
| --- | --- |
| 148 Elm St, New York City, NY 10001 | 3 |
| 515 Lincoln St, New York City, NY 10001 | 3 |
| 916 Pine St, New York City, NY 10001 | 3 |
| 961 Jefferson St, New York City, NY 10001 | 4 |

- There were 4 customers in New York that made 3 or more ord4rs in the month of January. 




## 9. How many of each type of headphone were sold in February?
```ruby
SELECT product, SUM(quantity) AS quantity
FROM BIT_DB.FebSales
WHERE product LIKE '%headphone%'
GROUP BY product;
```

### Steps: 
- Use SUM and GROUP BY to find total amount of varying `quantity` sold
- WHERE and LIKE to isolate `product` type 

### Answer: 
| product | quantity |
| --- | --- |
| Apple AirPods Headphones | 1013 |
| Bose SoundSport Headphones | 844 |
| Wired Headphones | 1282 |




## 10. What was the average quantity of products purchased per account in February? 
```ruby
SELECT SUM(quantity)/count(cust.acctnum) AS avg_qty
FROM BIT_DB.FebSales feb
LEFT JOIN BIT_DB.customers cust
ON cust.order_id=feb.orderID
WHERE length(orderid) = 6 
AND orderid <> 'Order ID';
```

###Steps: 
- SUM and COUNT to find overall average not average of each account 
- LEFT JOIN `FebSales` with `customers`

### Answer: 
| avg_qty |
| --- |
| 1 |

- The average quanitty of products purchased per account in February is 1.




## 11. Which product brought in the most revenue in January and how much revenue did it bring in total?
```ruby
SELECT product, SUM(quantity*price) AS revenue
FROM BIT_DB.JanSales
GROUP BY product
ORDER BY product DESC
LIMIT 1;
```

### Steps: 
- Use SUM to find `revenue`
- Use GROUP BY and ORDER BY to structure output by `product`

### Answer: 
| product | revenue |
| --- | --- |
| iPhone | 265300 |

- The iPhone brought in the most revenue in January at $265,300.
