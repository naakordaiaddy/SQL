
# 🧾 Sales Agent Invoices 

## Table of Contents
- [Business Task](#business-task)
- [Data Set](#data-set)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- [Questions & Solutions](#questions--solutions)

---
### Business Task
Analyze sales data to garner general information on all sales agents output and effeciency. 


### Data Set 
- Invoice data from [yugabyteDB] (https://docs.yugabyte.com/preview/sample-data/chinook/)


### Entity Relationship Diagram 
<img width="807" alt="Screen Shot 2023-06-13 at 9 21 51 PM" src="https://github.com/naakordaiaddy/SQL-Portfolio-Projects/assets/126539576/b0bdaffb-bfc0-4bbe-b8f8-d84626d23caf">

## Questions & Solutions
### 1. Show Customers (their full names, customer ID, and country) who are not in the US. 
```ruby
SELECT FirstName, LastName, CustomerId, Country
FROM chinook.customers
WHERE Country <> 'USA';
```

### Steps: 
- Use WHERE to isolate `customers` who are not located in the United States

### Answer: 
| FirstName | LastName | CustomerId | Country |
| --- | --- | --- | --- |
| Luís | Gonçalves | 1 | Brazil |
| Leöne  | Koheler | 2 | Germany |
| Françoise | Tremblay | 3 | Canada |

- There were 46 customers who were not located in the US listed in the query.




### 2. Show only the Customers from Brazil.
```ruby
SELECT FirstName, LastName, CustomerId, Country
FROM chinook.customers
WHERE Country LIKE '%Brazil%';
```

### Steps: 
- Use WHERE to isolate `customers` who are in located in the Brazil

### Answer: 
| FirstName | LastName | CustomerId | Country |
| --- | --- | --- | --- |
| Luís | Gonçalves | 1 | Brazil |
| Eduardo | Martins | 10 | Brazil |
| Alexandre | Roche | 11 | Brazil |




### 3. Find the Invoices of customers who are from Brazil. The resulting table should show the customer's full name, Invoice ID, Date of the invoice, and billing country.
```ruby
SELECT c.FirstName, c.LastName, i.InvoiceID, i.InvoiceDate, i.BillingCountry
FROM chinook.customers c
INNER JOIN chinook.invoices i
ON i.invoiceId=c.customerId
WHERE i.BillingCountry LIKE '%Brazil%';
```

### Steps: 
- Use INNER JOIN to overlap like fields within the `customers` & `invoices` table
- Use WHERE to only show the invoices of `customers` form Brazil

### Answer:
| FirstName | LastName | InvoiceId | InvoiceDate | BillingCountry |
| --- | --- | --- | --- | --- |
| Victor | Stevens | 25 | 2009-04-09 00:00:00 | Brazil |
| João | Fernandes | 34 | 2009-05-23 00:00:00 | Brazil |
| Madalena | Sampaio | 35 | 2009-06-05 00:00:00 | Brazil |




### 4. Show the Employees who are Sales Agents.
```ruby
SELECT LastName, FirstName
FROM chinook.employees
WHERE Title = 'Sales Support Agent';
```

### Steps: 
- Use WHERE to isolate only `employee` Sales Agents

### Answers:
| LastName | FirstName |
| --- | --- |
| Peacock | Jane |
| Park | Margaret |
| Johnson | Steve |

- There are 3 sales agents that are on the team.





### 5. Find a unique/distinct list of billing countries from the Invoice table.
```ruby
SELECT * 
FROM chinook.invoices
WHERE BillingCountry LIKE '%c';
```

### Steps:
- Use WHERE to list all `BillingCountry` that start with the letter 'C'

### Answer:
| InvoiceId | CustomerID | InvoiceDate | BillingAddress | BillingCity | BillingState | BillingCountry | BillingPostingCode | Total |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 46 | 6 | 2009-07-11 00:00:00	 | Rilská 3174/6 | Prague | NULL | Czech Republic | 14300 | 8.91 |
| 77 | 5 | 2009-12-08 00:00:00 | Klanova 9/506 | Prague | NULL | Czech Republic | 14700 | 1.98 |
| 100 | 5 | 2010-03-12 00:00:00 | Klanova 9/506 | Prague | NULL | Czech Republic | 14700 | 3.96 |





### 6. Provide a query that shows the invoices associated with each sales agent. The resulting table should include the Sales Agent's full name.
```ruby
SELECT emp.LastName, emp.Firstname, inv.InvoiceId
FROM chinook.Employees emp 
JOIN chinook.Customers cust 
ON cust.SupportRepId = emp.EmployeeId
JOIN chinook.Invoices Inv 
ON Inv.CustomerId = cust.CustomerId;
```

### Steps: 
- Use multiple INNER JOINS to connect `customers`, `employees`, `invoices` tables

### Answer:
| LastName | FirstName | InvoiceId |
| --- | --- | --- |
| Peacock | Jane | 98 |
| Peacock | Jane | 121 |
| Peacock | Jane | 143 |

### 7. Show the Invoice Total, Customer name, Country, and Sales Agent name for all invoices and customers.
```ruby
SELECT inv.Total, cust.FirstName, cust.LastName, cust.Country, emp.FirstName, emp.LastName
FROM chinook.customers cust
JOIN chinook.invoices inv
ON inv.InvoiceId=cust.CustomerId
JOIN chinook.employees emp
ON emp.EmployeeId=cust.SupportRepId;
```


### 8. How many Invoices were there in 2009?
```ruby
SELECT COUNT(InvoiceId) AS 'tot_invoices'
FROM chinook.invoices
WHERE InvoiceDate LIKE '%2009%';
```


### 9. What are the total sales for 2009?
```ruby
SELECT ROUND(SUM(Total),2) AS 'tot_sales'
FROM chinook.invoices
WHERE InvoiceDate LIKE '%2009%';
```


### 10. Write a query that includes the purchased track name with each invoice line ID.
```ruby
SELECT t.Name, i.invoiceId
FROM chinook.tracks t
INNER JOIN chinook.invoice_items i
ON i.TrackId=t.TrackId;
```


### 11. Write a query that includes the purchased track name AND artist name with each invoice line ID.
```ruby
SELECT tr.Name, art.Name, inv.InvoiceId
FROM chinook.invoice_items inv
JOIN chinook.tracks
ON inv.TrackId = tr.TrackId
JOIN chinook.albums alb
ON alb.AlbumId=tra.AlbumId
JOIN chinook.artists art
ON art.ArtistId=alb.ArtistId;
```


### 12. Provide a query that shows all the Tracks, and include the Album name, Media type, and Genre.
```ruby
SELECT alb.Title, med.Name, gen.GenreId
FROM chinook.albums alb
JOIN chinook.tracks tr
ON alb.AlbumId=tr.AlbumId
JOIN chinook.media_types med
ON med.MediaTypeId=tr.MediaTypeId
JOIN chinook.genres gen
ON gen.GenreId=tr.GenreId;
```


### 13. Show the total sales made by each sales agent.
 ```ruby
SELECT emp.FirstName, emp.LastName, ROUND(SUM(inv.Total),2) as 'Total Sales'
FROM chinook.Employees emp
JOIN chinook.Customers cust 
ON cust.SupportRepId=emp.EmployeeId
JOIN chinook.Invoices inv
ON inv.CustomerId=cust.CustomerId
WHERE emp.Title LIKE '%agent%'
GROUP by emp.FirstName;
```

### 14. Which sales agent made the most dollars in sales in 2009?
```ruby
SELECT emp.FirstName, emp.LastName, ROUND(SUM(inv.Total),2) as 'Total Sales'
FROM chinook.Employees emp
JOIN chinook.Customers cust 
ON cust.SupportRepId=emp.EmployeeId
JOIN chinook.Invoices inv
ON inv.CustomerId=cust.CustomerId
WHERE inv.InvoiceDate LIKE '%2009%'
GROUP BY emp.FirstName
ORDER BY 'Total Sales' DESC
LIMIT 1;
```
