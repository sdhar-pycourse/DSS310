# Data Query Language (DQL)
## SELECT STATEMENT 
For a business analyst SELECT and it's clauses are most important syntax as this allows access to data and ability to analyze data

A representative SQL statement is given below. Understanding the various parts and order of execution is important
```sql
SELECT BillingCity, sum(Total) as TotalInvoiceValue
FROM invoices 
WHERE BillingCountry= 'USA'
GROUP BY BillingCity
HAVING sum(Total) > 30
LIMIT 5
```
[![order]](https://jvns.ca/images/sql-queries.jpeg)
