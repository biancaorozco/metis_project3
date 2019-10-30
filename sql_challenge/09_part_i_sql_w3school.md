# Challenge Set 9
## Part I: W3Schools SQL Lab 

*Introductory level SQL*

--

This challenge uses the [W3Schools SQL playground](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all). Please add solutions to this markdown file and submit.

1. Which customers are from the UK? 

```sql
SELECT CustomerName FROM Customers WHERE country='UK';
```

2. What is the name of the customer who has the most orders?
   
```sql
SELECT * FROM OrderDetails ORDER BY Quantity DESC;
```
OrderID = 10398

```sql
SELECT * FROM Orders WHERE OrderID=10398;
``` 
CustomerID = 71

```sql
SELECT CustomerName FROM Customers WHERE CustomerID=71;
```
CustomerName = Save-a-lot Markets

3. Which supplier has the highest average product price?

```sql
SELECT SupplierID, AVG(Price) FROM Products GROUP BY SupplierID ORDER BY AVG(Price) DESC;
``` 
SupplierID = 18

```sql
SELECT SupplierName FROM Suppliers WHERE SupplierID=18;
```
SupplierName = Aux joyeux ecclésiastiques 

4. How many different countries are all the customers from? (*Hint:* consider [DISTINCT](http://www.w3schools.com/sql/sql_distinct.asp).)

```sql
SELECT COUNT(DISTINCT Country) FROM Customers;
```
Distinct Countries = 21

5. What category appears in the most orders?

```sql
SELECT *, COUNT(Categories.CategoryID) as CategoryCount
FROM OrderDetails
LEFT JOIN Products 
ON OrderDetails.ProductID = Products.ProductID
LEFT JOIN Categories 
ON Products.CategoryID = Categories.CategoryID
GROUP BY Categories.CategoryID
ORDER BY CategoryCount DESC;
```
CategoryName = Dairy Products, Count=100

6. What was the total cost for each order?
```sql
SELECT OrderDetails.OrderID, SUM(Price) as TotalPrice
FROM OrderDetails
LEFT JOIN Products 
ON OrderDetails.ProductID = Products.ProductID
GROUP BY OrderDetails.OrderID
ORDER BY TotalPrice DESC;
```

7. Which employee made the most sales (by total price)?
```sql
SELECT Employees.EmployeeID, Employees.FirstName, Employees.LastName, Products.Price, SUM(Quantity * Price) as TotalSales
FROM Employees
LEFT JOIN Orders 
ON Employees.EmployeeID = Orders.EmployeeID
LEFT JOIN OrderDetails 
ON Orders.OrderID = OrderDetails.OrderID
LEFT JOIN Products 
ON OrderDetails.ProductID = Products.ProductID
GROUP BY Employees.EmployeeID
ORDER BY TotalSales DESC;
```
EmployeeName = Margaret Peacock

8. Which employees have BS degrees? (*Hint:* look at the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator.)

```sql
SELECT * FROM [Employees]
WHERE Notes LIKE '%BS%';
```
EmployeeName = Janet Leverling, Steven Buchanan

9. Which supplier of three or more products has the highest average product price? (*Hint:* look at the [HAVING](http://www.w3schools.com/sql/sql_having.asp) operator.)jupyt

```sql
SELECT Suppliers.SupplierName, COUNT(Suppliers.SupplierID) as ProductsPerSupplier, AVG(Products.Price)
FROM Suppliers
LEFT JOIN Products 
ON Suppliers.SupplierID = Products.SupplierID
GROUP BY Suppliers.SupplierName
HAVING COUNT(Suppliers.SupplierID) >= 3
ORDER BY AVG(Products.Price) DESC;
```

SupplierName = Tokyo Traders