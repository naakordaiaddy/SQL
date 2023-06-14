--Show Customers (their full names, customer ID, and country) who are not in the US. 

SELECT FirstName, LastName, CustomerId, Country
FROM chinook.customers
WHERE Country <> 'USA';



--Show only the Customers from Brazil.

SELECT FirstName, LastName, CustomerId, Country
FROM chinook.customers
WHERE Country LIKE '%Brazil%';



--Find the Invoices of customers who are from Brazil. The resulting table should show the customer's full name, Invoice ID, Date of the invoice, and billing country.

SELECT c.FirstName, c.LastName, i.InvoiceID, i.InvoiceDate, i.BillingCountry
FROM chinook.customers c
INNER JOIN chinook.invoices i
ON i.invoiceId=c.customerId
WHERE i.BillingCountry LIKE '%Brazil%';



--Show the Employees who are Sales Agents.

SELECT LastName, FirstName
FROM chinook.employees
WHERE Title = 'Sales Support Agent';



--Find a unique/distinct list of billing countries from the Invoice table.

SELECT * 
FROM chinook.invoices
WHERE BillingCountry LIKE '%c';



--Provide a query that shows the invoices associated with each sales agent. The resulting table should include the Sales Agent's full name.
    
SELECT emp.LastName, emp.Firstname, inv.InvoiceId
FROM chinook.Employees emp 
JOIN chinook.Customers cust 
ON cust.SupportRepId = emp.EmployeeId
JOIN chinook.Invoices Inv 
ON Inv.CustomerId = cust.CustomerId;



--Show the Invoice Total, Customer name, Country, and Sales Agent name for all invoices and customers.

SELECT inv.Total, cust.FirstName, cust.LastName, cust.Country, emp.FirstName, emp.LastName
FROM chinook.customers cust
JOIN chinook.invoices inv
ON inv.InvoiceId=cust.CustomerId
JOIN chinook.employees emp
ON emp.EmployeeId=cust.SupportRepId;



--How many Invoices were there in 2009?

SELECT COUNT(InvoiceId) AS 'tot_invoices'
FROM chinook.invoices
WHERE InvoiceDate LIKE '%2009%';



--What are the total sales for 2009?

SELECT ROUND(SUM(Total),2) AS 'tot_sales'
FROM chinook.invoices
WHERE InvoiceDate LIKE '%2009%';



--Write a query that includes the purchased track name with each invoice line ID.
SELECT t.Name, i.invoiceId
FROM chinook.tracks t
INNER JOIN chinook.invoice_items i
ON i.TrackId=t.TrackId;



--Write a query that includes the purchased track name AND artist name with each invoice line ID.

SELECT tr.Name, art.Name, inv.InvoiceId
FROM chinook.invoice_items inv
JOIN chinook.tracks
ON inv.TrackId = tr.TrackId
JOIN chinook.albums alb
ON alb.AlbumId=tra.AlbumId
JOIN chinook.artists art
ON art.ArtistId=alb.ArtistId;



--Provide a query that shows all the Tracks, and include the Album name, Media type, and Genre.

SELECT alb.Title, med.Name, gen.GenreId
FROM chinook.albums alb
JOIN chinook.tracks tr
ON alb.AlbumId=tr.AlbumId
JOIN chinook.media_types med
ON med.MediaTypeId=tr.MediaTypeId
JOIN chinook.genres gen
ON gen.GenreId=tr.GenreId;



--Show the total sales made by each sales agent.
 
SELECT emp.FirstName, emp.LastName, ROUND(SUM(inv.Total),2) as 'Total Sales'
FROM chinook.Employees emp
JOIN chinook.Customers cust 
ON cust.SupportRepId=emp.EmployeeId
JOIN chinook.Invoices inv
ON inv.CustomerId=cust.CustomerId
WHERE emp.Title LIKE '%agent%'
GROUP by emp.FirstName;


--Which sales agent made the most dollars in sales in 2009?
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
