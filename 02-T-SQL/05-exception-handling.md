# Exception Handling

## Index
- [Introduction to Exception Handling](#introduction-to-exception-handling)
- [Using TRY...CATCH](#using-trycatch)
- [Capturing Error Information](#capturing-error-information)
- [Handling Transactions in TRY...CATCH](#handling-transactions-in-trycatch)
- [Throwing Custom Errors with THROW](#throwing-custom-errors-with-throw)
- [Using RAISERROR for Custom Messages](#using-raiserror-for-custom-messages)
- [Real-Time Use Cases](#real-time-use-cases)
  - [Logging Errors in an Audit Table](#logging-errors-in-an-audit-table)
  - [Graceful Handling of Insert Failures](#graceful-handling-of-insert-failures)
  - [Transaction Rollback in Case of Failure](#transaction-rollback-in-case-of-failure)
- [Best Practices](#best-practices)

## Introduction to Exception Handling
Exception handling in T-SQL ensures that errors are managed effectively, preventing unexpected failures and maintaining data integrity.

## Using TRY...CATCH
T-SQL provides a structured way to handle errors using TRY...CATCH.

```sql
BEGIN TRY
    -- Code that may cause an error
    SELECT 1/0;  -- This will cause a divide by zero error
END TRY
BEGIN CATCH
    PRINT 'An error occurred';
END CATCH;
```

## Capturing Error Information
Inside the CATCH block, system functions can capture detailed error information:

```sql
BEGIN TRY
    SELECT 1/0;
END TRY
BEGIN CATCH
    PRINT 'Error Number: ' + CAST(ERROR_NUMBER() AS NVARCHAR(10));
    PRINT 'Error Message: ' + ERROR_MESSAGE();
    PRINT 'Error Severity: ' + CAST(ERROR_SEVERITY() AS NVARCHAR(10));
    PRINT 'Error State: ' + CAST(ERROR_STATE() AS NVARCHAR(10));
    PRINT 'Error Line: ' + CAST(ERROR_LINE() AS NVARCHAR(10));
END CATCH;
```

## Handling Transactions in TRY...CATCH
Ensuring transactions are rolled back in case of failure:

```sql
BEGIN TRANSACTION;
BEGIN TRY
    INSERT INTO Sales.SalesOrderHeader (CustomerID, OrderDate, TotalDue)
    VALUES (29825, GETDATE(), 500.00);
    
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    ROLLBACK TRANSACTION;
    PRINT 'Transaction rolled back due to error';
END CATCH;
```

## Throwing Custom Errors with THROW
`THROW` raises an error and stops execution.

```sql
BEGIN TRY
    IF NOT EXISTS (SELECT 1 FROM Sales.Customer WHERE CustomerID = 99999)
        THROW 50001, 'Customer does not exist', 1;
END TRY
BEGIN CATCH
    PRINT ERROR_MESSAGE();
END CATCH;
```

## Using RAISERROR for Custom Messages
`RAISERROR` allows customized error messages with severity levels.

```sql
RAISERROR ('Invalid order ID', 16, 1);
```

## Real-Time Use Cases

### Logging Errors in an Audit Table
Stores error details for debugging.

```sql
CREATE TABLE ErrorLog (
    ErrorID INT IDENTITY PRIMARY KEY,
    ErrorMessage NVARCHAR(4000),
    ErrorDate DATETIME DEFAULT GETDATE()
);

BEGIN TRY
    EXEC SomeProcedure;
END TRY
BEGIN CATCH
    INSERT INTO ErrorLog (ErrorMessage)
    VALUES (ERROR_MESSAGE());
END CATCH;
```

### Graceful Handling of Insert Failures
Prevents partial inserts from corrupting data.

```sql
BEGIN TRY
    INSERT INTO Sales.SalesOrderHeader (CustomerID, OrderDate, TotalDue)
    VALUES (NULL, GETDATE(), 500.00);
END TRY
BEGIN CATCH
    PRINT 'Insert failed: ' + ERROR_MESSAGE();
END CATCH;
```

### Transaction Rollback in Case of Failure
Ensures data consistency when multiple inserts depend on each other.

```sql
BEGIN TRANSACTION;
BEGIN TRY
    INSERT INTO Sales.SalesOrderHeader (CustomerID, OrderDate, TotalDue)
    VALUES (29825, GETDATE(), 500.00);
    
    INSERT INTO Sales.SalesOrderDetail (SalesOrderID, ProductID, OrderQty)
    VALUES (SCOPE_IDENTITY(), 776, 2);
    
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    ROLLBACK TRANSACTION;
    PRINT 'Order processing failed: ' + ERROR_MESSAGE();
END CATCH;
```

## Best Practices
- Always use `TRY...CATCH` for error handling in stored procedures.
- Use `ERROR_MESSAGE()` and related functions to log errors.
- Ensure transactions are properly committed or rolled back.
- Prefer `THROW` over `RAISERROR` in new development.
- Use error logging to capture error details for debugging.


