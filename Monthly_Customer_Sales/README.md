# Monthly Customer Sales with Clothing Company

### Business Task 
Analyze customer orders to garner general information on customer behavior and demographics. 


### Data Set
- Orders & Sales data from [Break Into Tech SQL Course](https://www.dropbox.com/s/cvsasmtr8syq2c2/BIT_DB?dl=0)


### Entity Relationship Diagram
<img width="1109" alt="Screen Shot 2023-05-28 at 5 26 57 PM" src="https://github.com/naakordaiaddy/SQL-Portfolio-Projects/assets/126539576/80156e36-3e6d-459f-b247-f179352ab842">

### Questions & Solutions
### 1. How many orders were placed in January?
```ruby
SELECT COUNT(orderID)
FROM BIT_DB.JanSales
WHERE length(orderid) = 6 
AND orderid <> 'Order ID';
```

