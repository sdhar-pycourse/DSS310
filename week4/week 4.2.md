# Data Query Language (DQL)

Please have the chinook.db downloaded and the SQLite DB viewer installed :smile:
## SELECT STATEMENT 
For a business analyst SELECT and it's clauses are most important syntax as this allows access to data and ability to analyze data

A representative SQL statement is given below. Understanding the various parts and order of execution is important
```sql
SELECT BillingCity, sum(Total) as TotalInvoiceValue, count(distinct InvoiceId) as Invoices
FROM invoices 
WHERE BillingCountry= 'USA'
GROUP BY BillingCity
HAVING sum(Total) > 30
ORDER BY count(distinct InvoiceId) DESC
LIMIT 5
```
## Order of Execution
![order](https://learnsql.com/blog/sql-order-of-operations/1.png)
### FROM and JOINs

The tables specified in the FROM clause (including JOINs), will be evaluated first, to determine the **entire working set** which is relevant for the query. The database will merge the data from all tables, according to the JOINs ON clauses (coming later)

```sql
SELECT BillingCity, BillingCountry
FROM invoices
```

### WHERE clause

The WHERE clause will be the second to be evaluated, after the FROM clause. We have the working data set in place, and now we can filter the data according to the conditions in the WHERE clause. Some database's optimizer will choose to evaluate the WHERE part first, to see which part of the working set can be left out (preferably using indexes), so it won't inflate the data set too much if it doesn't really have to.

```sql
SELECT BillingCity
FROM invoices 
WHERE BillingCountry= 'USA'
```

### GROUP BY clause

Now that we filtered the data set using the WHERE clause, we can aggregate the data according to one or more columns appearing in the GROUP BY clause. Grouping the data is actually splitting it to different chunks or buckets.
- Columns selected in a `GROUP BY` are lower cardinality
- Usually used to roll up a hiearchy from granular information to higher dimensional aggregates e.g. store -> City-> State-> Region
- Fundamental to aggregate analysis
- Rolls up milion of rows of data to few human readable rows for insights

```sql
SELECT BillingCity, sum(Total) as TotalInvoiceValue, count(distinct InvoiceId) as Invoices
FROM invoices 
WHERE BillingCountry= 'USA'
GROUP BY BillingCity
```

### HAVING clause

Now that we have grouped the data using the GROUP BY clause, we can use the HAVING clause to filter out some buckets. The conditions in the HAVING clause can refer to the aggregation functions `count(distinct InvoiceId)`
As we've already grouped the data, we can no longer access the original rows at this point, so we can only apply conditions to filter entire buckets, and not single rows in a bucket.
```sql
SELECT BillingCity, sum(Total) as TotalInvoiceValue, count(distinct InvoiceId) as Invoices
FROM invoices 
WHERE BillingCountry= 'USA'
GROUP BY BillingCity
HAVING sum(Total) > 30
```

### SELECT clause

Now that we are done with discarding rows from the data set and grouping the data, we can select the data we want to be fetched from the query. You can use column names, aggregations etc inside the SELECT clause. Keep in mind that if you're using a reference to an aggregation function, such as COUNT(*) in the SELECT clause, it's merely a reference to an aggregation which already occurred when the grouping took place, so the aggregation itself doesn't happen in the SELECT clause, but this is only a reference to its result set.

### DISTINCT keyword
The DISTINCT operation takes place after the SELECT. When using the DISTINCT keyword, the database will discard rows with duplicate values from the remaining rows left after the filtering and aggregations took place.

### ORDER BY clause
Sorting takes place once the database has the entire result set ready (after filtering, grouping, duplication removal). Once we have that, the database can now sort the result set using columns, selected aliases, or aggregation functions, even if they aren't part of the selected data. 

You can choose to sort the data using a descending (DESC) order or an ascending (ASC) order. The order can be unique for each of the order parts, so the following is valid: ORDER BY firstname ASC, age DESC

```sql
SELECT BillingCity, sum(Total) as TotalInvoiceValue, count(distinct InvoiceId) as Invoices
FROM invoices 
WHERE BillingCountry= 'USA'
GROUP BY BillingCity
HAVING sum(Total) > 30
ORDER BY count(distinct InvoiceId) DESC
```

### LIMIT and OFFSET
In most use cases (excluding a few like reporting), we would want to discard all rows but the first X rows of the query's result. The LIMIT clause, which is executed after sorting, allows us to do just that. In addition, you can choose which row to start fetching the data from and how many to exclude, using a combination of the LIMIT and OFFSET keywords. 

```sql
SELECT BillingCity, sum(Total) as TotalInvoiceValue, count(distinct InvoiceId) as Invoices
FROM invoices 
WHERE BillingCountry= 'USA'
GROUP BY BillingCity
HAVING sum(Total) > 30
ORDER BY count(distinct InvoiceId) DESC
LIMIT 5
```

## Advanced Topics
We get to [Window Functions](https://www.sqlitetutorial.net/sqlite-window-functions/) afer we have mastered basic SQL syntax. It is important to mention that Window function is applied once the data columns are retrieved via the SELECT statement


