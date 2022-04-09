# Week 7 SQL Assignment

The following is the ER diagram for reference.
▶️ is the direction of 1 in 1:M relationships
![pict](https://documentation.red-gate.com/dms6/files/49646072/49646073/3/1559655630714/ERDiagramNorthwind.png)

### Q1. For each Customer, get the CompanyName, CustomerId, and "total expenditures". Output the bottom 5 Customers, as measured by total expenditures.
🗒️ Last assignment we had defined what TotalExpenditure is defined as ```SUM(UnitPrice*Quantity*(1-Discount))```
```sql
SELECT CompanyName, CustomerId, SUM(UnitPrice*Quantity*(1-Discount)) as TotalExpenditures
FROM Customer c
INNER JOIN 'order' o ON o.CustomerId = c.Id
INNER JOIN OrderDetail od ON od.OrderId = o.Id
GROUP by OrderId
ORDER BY
SUM(UnitPrice*Quantity*(1-Discount)) ASC
LIMIT 5
```

### Q2. Get the number of products, average unit price (rounded to 2 decimal places), minimum unit price, maximum unit price, and total units on order for categories containing greater than 10 products

⚠️ The term greater than 10 products refers to the category that has more than 10 products and not the ```UnitsOnOrder``` greater than 10

```sql
SELECT CategoryName, round(AVG(UnitPrice),2) AS Average, MIN(UnitPrice) AS MinimumUnitPrice, MAX(UnitPrice) AS MaximumUnitPrice, count(UnitsOnOrder) AS NumberOfProducts, SUM(UnitsOnOrder) AS UnitsOnOrder
FROM Category c
LEFT JOIN Product p ON p.CategoryId = c.Id
GROUP BY CategoryId
HAVING count(c.Id) > 10
```

## Q3. What is the total $ orders handled by employees of the Cambridge, Boston and Braintree territory in 2010? 

## Q4. Which customers haven't ever ordered anything?

## Q5. For each Shipper, find the $ value of orders which are late?

## Q6. Total annual $ and numbers of  order's ShipCountry is in North America. For our purposes, this is 'USA', 'Mexico', 'Canada' for each year

## Q7. Get all unique ShipNames from the Order table that contain a hyphen '-'

## Q8. Provide a descending list of top selling category and Shipping Country.

## Q9. Find the Top 5 Employee and Customer combination with the highest discount. 

# Q10. How much order $ value comes from the same city as the Employee?



