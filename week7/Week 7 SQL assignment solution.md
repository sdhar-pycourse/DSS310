# Week 7 SQL Assignment

The following is the ER diagram for reference.
▶️ is the direction of 1 in 1:M relationships
![pict](https://documentation.red-gate.com/dms6/files/49646072/49646073/3/1559655630714/ERDiagramNorthwind.png)

### Q1. For each Customer, get the CompanyName, CustomerId, and "total expenditures". Output the bottom 5 Customers, as measured by total expenditures.
🗒️ Last assignment we had defined what TotalExpenditure is defined as ```SUM(UnitPrice*Quantity*(1-Discount))```
```sql
SELECT CompanyName, CustomerId, SUM(od.UnitPrice*od.Quantity*(1-od.Discount)) as TotalExpenditures
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
SELECT CategoryName, count(p.Id), round(AVG(UnitPrice),2) AS Average, MIN(UnitPrice) AS MinimumUnitPrice, MAX(UnitPrice) AS MaximumUnitPrice, count(UnitsOnOrder) AS NumberOfProducts, SUM(UnitsOnOrder) AS UnitsOnOrder
FROM Category c
LEFT JOIN Product p ON p.CategoryId = c.Id
GROUP BY CategoryId
HAVING count(p.Id) > 10
```

### Q3. What is the total $ orders handled by employees of the Cambridge, Boston and Braintree territory in 2010? 

Look at the relationship between Employee and EmployeeTerritories. Employee to Order is 1:M and that with EmployeeTerritories is 1:M. Employee is top of the hierarchy. Therefore you need a seperate query to fetch the Employees in the 3 territories mentioned in the question and then search for them in the main query. Hence you need a subquery.
```sql
select count(distinct o.id) as Orders, SUM(UnitPrice*Quantity*(1-Discount)) AS 'Total$'
from 'ORDER' o
INNER JOIN OrderDetail od ON od.OrderId = o.Id
where (OrderDate BETWEEN '2012-01-01' AND '2012-12-31') and o.employeeid in (
  select DISTINCT et.EmployeeId
  from EmployeeTerritory et
  inner join Territory t on et.TerritoryId = t.Id
  where t.TerritoryDescription= 'Braintree' or t.territorydescription= 'Boston' 
  or t.TerritoryDescription= 'Cambridge')
```

### Q4. Which customers haven't ever ordered anything?

This is customers i the customer table ```EXCEPT``` customers in the order table. Omce you can find in the customwr table and not in the order table

```sql
SELECT Id
FROM Customer
EXCEPT
SELECT CustomerId
FROM 'order'
```

### Q5. For each Shipper, find the $ value of orders which are late?

```sql
SELECT s.CompanyName, s.Id, SUM(od.UnitPrice*od.Quantity*(1-od.Discount)) as Value
FROM Shipper s
INNER JOIN 'order' o ON o.ShipVia = s.Id
INNER JOIN OrderDetail od ON od.OrderId = o.Id
WHERE o.ShippedDate > o.RequiredDate
GROUP BY s.CompanyName
```

### Q6. Total annual $ and numbers of  order's ShipCountry is in North America. For our purposes, this is 'USA', 'Mexico', 'Canada' for each year

- Total annual $ is the Total annual order $, number of orders is the count of orders
- 'For each year' requires you to extract the year from the date field ```strftime('%Y',OrderDate)```
- 'is in North America': requires that the countries are in North America and put in a ```where``` clause
- You can actually skip adding the ```ShipCountry``` in the ```select``` clause if you want to

```sql
SELECT ShipCountry, strftime('%Y',OrderDate) AS Year, round(SUM(od.UnitPrice*od.Quantity*(1-od.Discount)),1) as TotalAnnualOrders, count(o.Id) as NumberOfOrders
FROM 'order' o
INNER JOIN OrderDetail od ON od.OrderId = o.Id
WHERE ShipCountry IN ('USA','Mexico', 'Canada')
GROUP BY strftime('%Y',OrderDate), ShipCountry
```
### Q7. Get all unique ShipNames from the Order table that contain a hyphen '-'

Containing a '-' is a wild card search 
- ```LIKE``` 
```sql
SELECT ShipName
FROM 'order'
WHERE ShipName LIKE '%-%'
```

### Q8. Provide a descending list of top selling category and Shipping Country.

### Q9. Find the Top 5 Employee and Customer combination with the highest discount. 

### Q10. How much order $ value comes from the same city as the Employee?



