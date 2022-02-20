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

# JOINS
There are various kinds of SQLite joins to query data from two or more tables. An artist can have zero or many albums while an album belongs to one artist.

To query data from both artists and albums tables, you use can use an INNER JOIN, LEFT JOIN in SQLite

| :memo:        | As indicated earlier SQLite doesn’t directly support the RIGHT JOIN and FULL OUTER JOIN|
|---------------|:------------------------|

## Inner Join
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

| :warning:        | The last SQL statement is syntax wise correct. But is it semantically consistent? |
|---------------|:------------------------|


