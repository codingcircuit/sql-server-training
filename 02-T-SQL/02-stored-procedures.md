# Stored Procedures

## Index
- [Introduction to Stored Procedures](#introduction-to-stored-procedures)
- [Advantages of Using Stored Procedures](#advantages-of-using-stored-procedures)
- [Creating a Basic Stored Procedure](#creating-a-basic-stored-procedure)
- [Stored Procedure with Parameters](#stored-procedure-with-parameters)
- [Stored Procedure with Output Parameters](#stored-procedure-with-output-parameters)
- [Modifying and Dropping a Stored Procedure](#modifying-and-dropping-a-stored-procedure)
- [Error Handling in Stored Procedures](#error-handling-in-stored-procedures)
- [Using Transactions in Stored Procedures](#using-transactions-in-stored-procedures)
- [Real-Time Use Cases](#real-time-use-cases)
  - [Automating Order Processing](#automating-order-processing)
  - [Monthly Sales Report Generation](#monthly-sales-report-generation)
  - [Customer Loyalty Points Calculation](#customer-loyalty-points-calculation)
- [Best Practices](#best-practices)

## Introduction to Stored Procedures
Stored procedures in T-SQL are precompiled SQL code that can be executed multiple times, helping to improve performance, maintainability, and security.

## Advantages of Using Stored Procedures
- **Improved Performance**: Execution plans are cached.
- **Security**: Reduces SQL injection risks by using parameterized queries.
- **Code Reusability**: Encapsulates logic for reuse.
- **Reduced Network Traffic**: Only procedure execution is sent over the network, not entire queries.

## Creating a Basic Stored Procedure
The following example creates a stored procedure to retrieve sales orders from the AdventureWorks database.

```sql
CREATE PROCEDURE GetSalesOrders
AS
BEGIN
    SELECT SalesOrderID, OrderDate, CustomerID, TotalDue 
    FROM Sales.SalesOrderHeader;
END;
```

### Execution
```sql
EXEC GetSalesOrders;
```

## Stored Procedure with Parameters
We can define parameters to filter data dynamically.

```sql
CREATE PROCEDURE GetSalesOrdersByCustomer
    @CustomerID INT
AS
BEGIN
    SELECT SalesOrderID, OrderDate, TotalDue 
    FROM Sales.SalesOrderHeader
    WHERE CustomerID = @CustomerID;
END;
```

### Execution
```sql
EXEC GetSalesOrdersByCustomer @CustomerID = 29825;
```

## Stored Procedure with Output Parameters
Output parameters return values from the procedure.

```sql
CREATE PROCEDURE GetTotalSalesForCustomer
    @CustomerID INT,
    @TotalSales MONEY OUTPUT
AS
BEGIN
    SELECT @TotalSales = SUM(TotalDue)
    FROM Sales.SalesOrderHeader
    WHERE CustomerID = @CustomerID;
END;
```

### Execution
```sql
DECLARE @Total MONEY;
EXEC GetTotalSalesForCustomer @CustomerID = 29825, @TotalSales = @Total OUTPUT;
PRINT @Total;
```

## Modifying and Dropping a Stored Procedure

### Modify Procedure
```sql
ALTER PROCEDURE GetSalesOrders
AS
BEGIN
    SELECT SalesOrderID, OrderDate, CustomerID, TotalDue, Status 
    FROM Sales.SalesOrderHeader;
END;
```

### Drop Procedure
```sql
DROP PROCEDURE GetSalesOrders;
```

## Error Handling in Stored Procedures
Using `TRY...CATCH` for error handling.

```sql
CREATE PROCEDURE SafeInsertOrder
    @CustomerID INT,
    @OrderDate DATE,
    @TotalDue MONEY
AS
BEGIN
    BEGIN TRY
        INSERT INTO Sales.SalesOrderHeader (CustomerID, OrderDate, TotalDue)
        VALUES (@CustomerID, @OrderDate, @TotalDue);
    END TRY
    BEGIN CATCH
        PRINT 'An error occurred';
    END CATCH;
END;
```

## Using Transactions in Stored Procedures
Ensuring data consistency with transactions.

```sql
CREATE PROCEDURE ProcessOrder
    @SalesOrderID INT
AS
BEGIN
    BEGIN TRANSACTION;
    BEGIN TRY
        UPDATE Sales.SalesOrderHeader
        SET Status = 5  -- Mark as processed
        WHERE SalesOrderID = @SalesOrderID;
        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION;
    END CATCH;
END;
```

## Real-Time Use Cases

### Automating Order Processing
A stored procedure can be used to mark an order as shipped and update inventory levels automatically.

```sql
CREATE PROCEDURE ProcessShippedOrder
    @SalesOrderID INT
AS
BEGIN
    BEGIN TRANSACTION;
    BEGIN TRY
        UPDATE Sales.SalesOrderHeader
        SET Status = 5  -- Shipped
        WHERE SalesOrderID = @SalesOrderID;
        
        UPDATE Production.ProductInventory
        SET Quantity = Quantity - (
            SELECT SUM(OrderQty) FROM Sales.SalesOrderDetail WHERE SalesOrderID = @SalesOrderID)
        WHERE ProductID IN (SELECT ProductID FROM Sales.SalesOrderDetail WHERE SalesOrderID = @SalesOrderID);
        
        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION;
        PRINT 'Error processing order';
    END CATCH;
END;
```

### Monthly Sales Report Generation
A stored procedure can generate a monthly sales summary report.

```sql
CREATE PROCEDURE GetMonthlySalesReport
    @Year INT, @Month INT
AS
BEGIN
    SELECT SalesOrderID, OrderDate, CustomerID, TotalDue 
    FROM Sales.SalesOrderHeader
    WHERE YEAR(OrderDate) = @Year AND MONTH(OrderDate) = @Month;
END;
```

### Customer Loyalty Points Calculation
A stored procedure can calculate and update loyalty points based on customer purchases.

```sql
CREATE PROCEDURE UpdateLoyaltyPoints
    @CustomerID INT
AS
BEGIN
    UPDATE Person.Person
    SET LoyaltyPoints = LoyaltyPoints + (
        SELECT SUM(TotalDue) * 0.05 FROM Sales.SalesOrderHeader WHERE CustomerID = @CustomerID)
    WHERE BusinessEntityID = @CustomerID;
END;
```

## Best Practices
- Use schema-qualified names (`Sales.SalesOrderHeader`).
- Minimize dynamic SQL to prevent SQL injection.
- Avoid using `SELECT *` for better performance.
- Handle errors properly using `TRY...CATCH`.
- Regularly review and optimize stored procedures.


