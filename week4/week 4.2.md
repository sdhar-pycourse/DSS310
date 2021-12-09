# Data Query Language (DQL)

Please have the chinook.db downloaded and the SQLite DB viewer installed :smile:
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
## Order of Execution
![order](https://learnsql.com/blog/sql-order-of-operations/1.png)
###FROM and JOINs

The tables specified in the FROM clause (including JOINs), will be evaluated first, to determine the entire working set which is relevant for the query. The database will merge the data from all tables, according to the JOINs ON clauses

```sql
```
