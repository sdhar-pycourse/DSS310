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

Mark the changed semantics, *how many*- so this must be a count. Count of what? Customers. What is the identifier/ primary key of customer table? **Id**
Does CompanyName or any other field work? Well for the sake of this assignment I may give you the 1 point but in reality the primary key is the correct choice.

```sql
SELECT COUNT(Id) from Customer
```

## Question 3:
*How many customers are from the 'British Isles'?*

The only difference betwen Q2 is additional clause on *'British Isles'*. Where can we find the information on British Isles? The ideal way to find out the column name where there is a entry *'British Isles'*. If you look through the columns and write a query such the following,  for each of the columns. Pick the column where the count() = 1 

```sql
SELECT count(distinct Region) from Customer
WHERE Region= 'British Isles'
-- 8
```
❔ If you had 2 columns that had count()= 1, thenw hat nornal form do you think you would violate?

The query tells us that 'British Isles' is an entry in the Region column

Now for the final solution to get the count of customers:

```sql
SELECT count(id) from Customer
WHERE Region= 'British Isles'
```

## Question 4: 
*How many Employees are based in 'North America'?*

Now that is a change in pace!! Where can we find Employee information and their geography? 
```sql
SELECT count(DISTINCT id) as NAEmployees
FROM Employee 
WHERE Region= 'North America'
```

## Question 5:
*Show all rows in the Region table*

No surprises here. SELECT * gets you al the columns, no need to specify column names
```sql
SELECT *
FROM Region
```

## Question 6:
*How many suppliers are based in the 'New Orleans'?*
Count the Ids and not any other attributes in the table
```sql
SELECT count(DISTINCT Id) as NOLASuppliers
FROM Supplier 
WHERE City= 'New Orleans'
```

## Question 7:
*Find all products that are Discontinued and has less than 10 UnitPrice*
A SELECT * is fine or you could just show the product name
```sql
SELECT *
FROM Product
GROUP BY UnitPrice
HAVING sum(UnitPrice) < 10 and Discontinued = 1
```

## Question 8:
*From the OrderDetail table find the 'Total Order Amount'*
What is Total Order Amount? 
It has to have a quantity of order, actual price. Actual price is a fucntion of discount. So QuantityxActualPrice(1-Discount)
```sql
SELECT sum(UnitPrice*Quantity*(1-Discount)) as TotalOrderAmount
FROM OrderDetail
```
🗒️: Some of you may have calcualted this at a order Id level or had addituonal attributes. Irrespective, I have tried to give full credit if I you got the basic formula correct

## Question 9:
*How many customers actually placed orders?*
Where do we have customers related to orders? It is in the Order table
```sql
SELECT count(DISTINCT CustomerId) as Customers
FROM 'Order'
```
⚠️: Order is a key word in SQLIte and requires a quote around the noun to make the SQL work

## Question 10:
*Find the Top 5 CusotmerIDs with the highest number of orders*

Questions to ask yourself:
- Highest number of what? Order Id. Therefore some kind of ```COUNT```
- Top 5 who? Customers
- 






