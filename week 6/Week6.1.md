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
