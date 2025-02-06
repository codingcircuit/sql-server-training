# Functions (Scalar & Table-Valued)

## Index
- [Introduction to T-SQL Functions](#introduction-to-t-sql-functions)
- [Scalar Functions](#scalar-functions)
  - [Creating a Scalar Function](#creating-a-scalar-function)
  - [Using a Scalar Function](#using-a-scalar-function)
- [Inline Table-Valued Functions](#inline-table-valued-functions)
  - [Creating an Inline Table-Valued Function](#creating-an-inline-table-valued-function)
  - [Using an Inline Table-Valued Function](#using-an-inline-table-valued-function)
- [Multi-Statement Table-Valued Functions](#multi-statement-table-valued-functions)
  - [Creating a Multi-Statement Table-Valued Function](#creating-a-multi-statement-table-valued-function)
  - [Using a Multi-Statement Table-Valued Function](#using-a-multi-statement-table-valued-function)
- [Real-Time Use Cases](#real-time-use-cases)
  - [Calculating Discounts on Products](#calculating-discounts-on-products)
  - [Retrieving Customer Orders with Total Amount](#retrieving-customer-orders-with-total-amount)
  - [Filtering Employees by Department](#filtering-employees-by-department)
- [Best Practices](#best-practices)

## Introduction to T-SQL Functions
Functions in T-SQL allow for encapsulating reusable logic that can be executed within queries. Functions return a value and can be classified into:
- **Scalar Functions**: Return a single value.
- **Table-Valued Functions**: Return a table.

## Scalar Functions
A scalar function returns a single value of a specific data type.

### Creating a Scalar Function
The following function calculates the total price of an order including tax.

```sql
CREATE FUNCTION dbo.CalculateTotalPrice (@UnitPrice MONEY, @Quantity INT, @TaxRate FLOAT)
RETURNS MONEY
AS
BEGIN
    RETURN (@UnitPrice * @Quantity) * (1 + @TaxRate);
END;
```

### Using a Scalar Function
```sql
SELECT dbo.CalculateTotalPrice(100, 5, 0.08) AS TotalPrice;
```

## Inline Table-Valued Functions
An inline table-valued function returns a table and behaves like a parameterized view.

### Creating an Inline Table-Valued Function
This function retrieves all orders for a specific customer.

```sql
CREATE FUNCTION dbo.GetCustomerOrders (@CustomerID INT)
RETURNS TABLE
AS
RETURN (
    SELECT SalesOrderID, OrderDate, TotalDue 
    FROM Sales.SalesOrderHeader
    WHERE CustomerID = @CustomerID
);
```

### Using an Inline Table-Valued Function
```sql
SELECT * FROM dbo.GetCustomerOrders(29825);
```

## Multi-Statement Table-Valued Functions
Multi-statement table-valued functions allow defining logic before returning a table.

### Creating a Multi-Statement Table-Valued Function
This function returns employee details based on department.

```sql
CREATE FUNCTION dbo.GetEmployeesByDepartment (@DepartmentID INT)
RETURNS @Employees TABLE (BusinessEntityID INT, FirstName NVARCHAR(50), LastName NVARCHAR(50))
AS
BEGIN
    INSERT INTO @Employees
    SELECT p.BusinessEntityID, p.FirstName, p.LastName
    FROM HumanResources.Employee e
    JOIN Person.Person p ON e.BusinessEntityID = p.BusinessEntityID
    WHERE e.DepartmentID = @DepartmentID;
    
    RETURN;
END;
```

### Using a Multi-Statement Table-Valued Function
```sql
SELECT * FROM dbo.GetEmployeesByDepartment(3);
```

## Real-Time Use Cases

### Calculating Discounts on Products
A scalar function can be used to calculate discounted prices dynamically.

```sql
CREATE FUNCTION dbo.GetDiscountedPrice (@ProductID INT, @DiscountRate FLOAT)
RETURNS MONEY
AS
BEGIN
    DECLARE @Price MONEY;
    SELECT @Price = ListPrice FROM Production.Product WHERE ProductID = @ProductID;
    RETURN @Price * (1 - @DiscountRate);
END;
```

### Retrieving Customer Orders with Total Amount
An inline table-valued function to retrieve orders with their total amount.

```sql
CREATE FUNCTION dbo.GetCustomerOrderTotals (@CustomerID INT)
RETURNS TABLE
AS
RETURN (
    SELECT soh.SalesOrderID, soh.OrderDate, SUM(sod.LineTotal) AS TotalAmount
    FROM Sales.SalesOrderHeader soh
    JOIN Sales.SalesOrderDetail sod ON soh.SalesOrderID = sod.SalesOrderID
    WHERE soh.CustomerID = @CustomerID
    GROUP BY soh.SalesOrderID, soh.OrderDate
);
```

### Filtering Employees by Department
A table-valued function to filter employees based on a department name.

```sql
CREATE FUNCTION dbo.FilterEmployeesByDepartment (@DepartmentName NVARCHAR(50))
RETURNS TABLE
AS
RETURN (
    SELECT p.BusinessEntityID, p.FirstName, p.LastName, d.Name AS DepartmentName
    FROM HumanResources.Employee e
    JOIN Person.Person p ON e.BusinessEntityID = p.BusinessEntityID
    JOIN HumanResources.Department d ON e.DepartmentID = d.DepartmentID
    WHERE d.Name = @DepartmentName
);
```

## Best Practices
- Use schema-qualified function names (`dbo.CalculateTotalPrice`).
- Inline table-valued functions are generally more efficient than multi-statement functions.
- Keep scalar functions simple to avoid performance bottlenecks.
- Ensure functions return appropriate data types to avoid implicit conversions.
