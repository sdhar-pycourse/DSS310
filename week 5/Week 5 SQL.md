# Week 5 SQL Solution
In this assignment all the queries should be from a single table and apply DQL statement.

## Question 1:
*Select the first 5 rows in the category table?*

This is a very simple query, you are asked to find all the data in the first 5 rows of data. A SELECT * from the table shall give all the rows of data. A LIMIT 5 would reduce the result set to only the first  rows
```sql
SELECT * from Category
limit 5
```

## Question 2:
*How many customers are there in the customer table?*

Mark the changed semantics, *how many* so this must be a count. Count of what? Customers. What is the identifier/ primary key of customer table? **Id**
Does CompanyName or any other field work? Well for the sake of this assignment I may give you the 1 point but in reality the primary key is the correct choice.

```sql
SELECT COUNT(Id) from Customer
```

## Question 3:
*How many customers are from the 'British Isles'?*

The only difference betwen Q2 is additional clause on *'British Isles'*. Where can we find the information on British Isles? The ideal way to find out the column name 


