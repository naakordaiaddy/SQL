# Monthly Customer Sales with Clothing Company

### Business Task 
Analyze customer orders to garner general information on customer behavior and demographics. 


### Data Set
- Orders & Sales data from [Break Into Tech SQL Course](https://www.dropbox.com/s/cvsasmtr8syq2c2/BIT_DB?dl=0)


### Entity Relationship Diagram
<img width="1109" alt="Screen Shot 2023-05-28 at 5 26 57 PM" src="https://github.com/naakordaiaddy/SQL-Portfolio-Projects/assets/126539576/80156e36-3e6d-459f-b247-f179352ab842">

## Questions & Solutions
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
ORDER BY revenue DESC
```

## Steps: 
- Use SUM and multiply by price to find the `quanitty` of each product sold
- ROUND to display `revenue` as a depicted price
- WHERE and NOT NULL removes incomplete information 
- Use GROUP BY to group information by `product`
- ORDER BY to view expesive to least expensive `product`

## Answer:
| Product | revenue |
| --- | --- |
| MacBook Pro Laptop | 399500 |
| iPhone | 265300 |
| ThinkPad Laptop | 216997.83 |
| Google Phone | 190800 |
| Apple AirPod Headphones | 122100 |


