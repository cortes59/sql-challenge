# Instructions

Using the provided SQLite database, `small-business.db`, solve each of the challenges below. If the database GUI that you typically use isn't compatible with SQLite, try using [DB Browser for SQLite](https://sqlitebrowser.org/).

## Challenge 1

Write the SQL to retrieve the list of territories that employee Anne Dodsworth oversees.

```sql
-- your answer here
SELECT * FROM "Territories" where "TerritoryID" IN (SELECT "TerritoryID" FROM "EmployeeTerritories" WHERE "EmployeeID" = (SELECT "EmployeeID" FROM "Employees" WHERE "FirstName"='Anne' AND "LastName"= 'Dodsworth'))
```

## Challenge 2

Write the SQL to find out how many different products that Janet Leverling was responsible for selling in 2017.

```sql
-- your answer here
SELECT COUNT(DISTINCT("ProductID")) FROM "Order Details" WHERE "OrderID" IN (SELECT "OrderID" FROM "Orders" WHERE "EmployeeID" = (SELECT "EmployeeID" FROM "Employees" WHERE "FirstName"='Janet' AND "LastName"= 'Leverling'))
```

## Challenge 3

Write the SQL to list the number of customers from each country, sorted high to low.

```sql
-- your answer here
SELECT Country, COUNT(*) AS CustomerCount FROM Customers GROUP BY Country ORDER BY CustomerCount DESC;
```

## Challenge 4

Write the SQL to list all employees who have registered more than 25 orders in the past 5 years.

```sql
-- your answer here
SELECT e.EmployeeID, e.FirstName, e.LastName, COUNT(o.OrderID) AS OrderCount
FROM Employees AS e
JOIN Orders AS o ON e.EmployeeID = o.EmployeeID
WHERE CAST(strftime('%Y', o.OrderDate) AS INTEGER) >= CAST(strftime('%Y', date('now', '-5 years')) AS INTEGER)
GROUP BY e.EmployeeID, e.FirstName, e.LastName
HAVING COUNT(o.OrderID) > 25;
```

## Challenge 5

Write the SQL to find every order along with it's calculated subtotal in dollars. The query should round the subtotal to a maximum of two decimal places. Note: The unit price is given in cents and a discount of 0.2 indicates a 20% discount.

```sql
-- your answer here
SELECT
  od.OrderID,
  ROUND(SUM((od.UnitPrice * od.Quantity * (1 - od.Discount)) / 100.0), 2) AS Subtotal
FROM "Order Details" AS od
GROUP BY od.OrderID;
```

## Challenge 6

Write the SQL to find every product whose price is above the average price for all products.

```sql
-- your answer here
SELECT * FROM "Products" WHERE UnitPrice > ( SELECT AVG(UnitPrice) FROM "Products");
```
