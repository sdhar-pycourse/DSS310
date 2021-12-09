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
1. `FROM/JOIN` and all the ON conditions
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT` (including window functions)
6. `ORDER BY`
7. `LIMIT`
