# JOINS & Other Interesting DQL Topics

# Chinook.db schema
Before we sart the class let's take a look at the Chinook.db data model to grab a high level view of what the database looks like:

![chinook](https://schemaspy.org/sample/diagrams/summary/relationships.real.compact.png)

### Key Takeaways:

1. In general data in RDBS are aligned as entities/ tables and as attributes/ columns there in
2. Each table usually maps to a business concept (noun) and each row of data is a instance of the entity
3. Attributes/ Columns hold information that decribe each specific entity
4. Entitie are identified via **primary keys** and related via **foreign keys**
5. Each foreign key relationship is a **word sentence** of a model that SQL queries must conform to
6. Foreign Key relationships enforce business rules:
    - Cardinality: Determines the number of entity instances on one side of the relationship that can be joined to a single entity on the other side( N to 0,1,M)
    - Optionality: Specifies how entity inatance on one side must be joined to an entity on the other side (N=0 or 1)
7. SQL JOINS help us to fetch information across entity tables following the defined business rules
8. GROUP BY clauses are used to **aggregate** information from higher cardinality tables to lower cardinality tables
9. **Granularity** is a measure of the level of detail in a data structure. 
    - InvoiceLine is at the granularity of a Invoice, track and date

| :memo:        | In the following sections we shall use a question asnwer type format to ask a business question and answer it with a SQL using the chinook.db practise database|
|---------------|:------------------------|

# Set Operations
## UNON
## INTERSECT
## EXCEPT

# JOINS
There are various kinds of SQLite joins to query data from two or more tables. An artist can have zero or many albums while an album belongs to one artist.

To query data from both artists and albums tables, you use can use an INNER JOIN, LEFT JOIN in SQLite

| :memo:        | As indicated earlier SQLite doesn’t directly support the RIGHT JOIN and FULL OUTER JOIN|
|---------------|:------------------------|

## INNER Join
INNER JOIN clause matches each row from the albums table with every row from the artists table based on the join condition (artists.ArtistId = albums.ArtistId) specified after the ON keyword. If the join condition evaluates to true (or 1), the columns of rows from both albums and artists tables are included in the result set.

*Who are the top 5 artists with the highest number of tracks to their credit?*

```sql
SELECT Name, count(Title) as TitleNum
FROM 
    albums
INNER JOIN artists 
    ON artists.ArtistId = albums.ArtistId
GROUP BY Name
ORDER BY count(Title) DESC
LIMIT 5
```
| :bulb:        | Observe that Name of artist and Title are not available on the same table|
|---------------|:------------------------|

## LEFT OUTER join

Let's try a new question:

*What are the 10 artists with the least number of albums?*

```sql
SELECT a.name, count (al.AlbumId) as AlbumCount
FROM artists a
INNER JOIN albums al ON al.ArtistId = a.ArtistId
GROUP BY a.name
ORDER BY count(al.AlbumId) ASC
LIMIT 20
```

| :warning:        | The last SQL statement is syntax wise correct. But is it semantically consistent? Remember a artist may or maynot have a album |
|---------------|:------------------------|

Let's try to understand the situation better with the following diagram:

![outer](https://www.sqlitetutorial.net/wp-content/uploads/2015/12/SQLite-Left-Join-Venn-Diagram.png)

- 13: Are the artists who have a album (yellow portion between 2 circles)
- 2 : Are the artists who have no albums! (left yellow portion)
- 5:  Would be albums with no artists. (right white portion) But in our case there ae no albums without a artist, therefore this is a 0 set

Therefore in oyr case our query needs to get us the album count across 13+2.
- INNER JOIN: Only retrieve result set for 13
- LEFT OUTER JOIN: Retrieves result set for (13+2) 
- RIGHT OUTER JOIN: Retrieves result set for (5+13)
- FULL OUTER JOIN: Retrieves result set for (5+2+13)

```sql
SELECT a.name, count (al.AlbumId) as AlbumCount
FROM artists a
LEFT JOIN albums al ON al.ArtistId = a.ArtistId
GROUP BY a.name
ORDER BY
count(al.AlbumId) ASC
LIMIT 20
```
The above query should return 20 rows of artists with 0 albums to their credit. Often these are called bottom performers
# WHERE Clause 

## Subquery
## EXISTS
## IS NULL

# Aggregate Functions
# String Functions
# Window Functions


# DATE Manipulation
Often you may be asked to manipulate date ranges in SQL. Example:-
*Find me the count of invoices and the sum of the values of all invoices in Q1 2010*

```sql
select count(InvoiceId) as InvoiceCount, sum(Total) InvoiceDollar
from invoices
where InvoiceDate between '2010-01-01' and '2010-03-31'
```
| :memo:        | In this course the date and time functions use a subset of IS0-8601 date and time formats. The date() function returns the date in this format: YYYY-MM-DD. The time() function returns the time as HH:MM:SS. The datetime() function returns "YYYY-MM-DD HH:MM:SS".|
|---------------|:------------------------|

## DATE function

The date() function accepts a time string and zero or more modifiers as arguments. It returns a date string in this format: YYYY-MM-DD.

```sql
SELECT 
    DATE('now',
        'start of month',
        '+1 month',
        '-1 day')
```

The function works as follows:
- First, start of month is applied to the current date specified by the now time string so the result is the first day of the current month.
- Second, +1 month is applied to the first day of the current month that results on the first day of next month.
- Third, -1 day is applied to the first day of the next month which results in the last day of the previous month.

## strftime() function

Often you may need to extract specific parts of a date string and perform aggregation exercise. Example:-
*What are the number of in invoices and billed $ for USA every month and year?*

```sql
SELECT strftime('%Y-%m',InvoiceDate), count(InvoiceId), sum(Total)
from invoices
where BillingCountry= 'USA'
group by strftime('%Y-%m',InvoiceDate)
```

The strftime() function is used to format a datetime value based on a specified format. The format_string specifies the format for the datetime value specified by the time_string value.

The following table shows the complete list of valid markers for constructing format:

|Format|	Description|
|------|---------------|
|%d	|day of the month: 01-31|
|%f	|fractional seconds: SS.SSS|
|%H	|hour: 00-24|
|%j	|day of the year: 001-366|
|%J	|Julian day number|
|%m	|month: 01-12|
|%M	|minute: 00-59|
|%s	|seconds since 1970-01-01|
|%S	|seconds: 00-59|
|%w	|day of week 0-6 with Sunday==0|
|%W	|week of the year: 00-53
|%Y	|year: 0000-9999
