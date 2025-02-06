# Triggers

## Index
- [Introduction to Triggers](#introduction-to-triggers)
- [Types of Triggers](#types-of-triggers)
  - [AFTER Triggers](#after-triggers)
  - [INSTEAD OF Triggers](#instead-of-triggers)
- [Creating an AFTER Trigger](#creating-an-after-trigger)
- [Creating an INSTEAD OF Trigger](#creating-an-instead-of-trigger)
- [Using Triggers for Auditing](#using-triggers-for-auditing)
- [Preventing Unauthorized Deletions](#preventing-unauthorized-deletions)
- [Real-Time Use Cases](#real-time-use-cases)
  - [Auditing Sales Order Changes](#auditing-sales-order-changes)
  - [Maintaining Inventory Levels](#maintaining-inventory-levels)
  - [Enforcing Business Rules](#enforcing-business-rules)
- [Best Practices](#best-practices)

## Introduction to Triggers
A trigger in SQL Server is a special type of stored procedure that is automatically executed when an event occurs in a table. Triggers are commonly used for enforcing business rules, maintaining data integrity, and auditing changes.

## Types of Triggers
SQL Server supports two main types of triggers:

### AFTER Triggers
- Fired **after** an `INSERT`, `UPDATE`, or `DELETE` operation.
- Used for logging, auditing, and enforcing constraints.

### INSTEAD OF Triggers
- Fired **instead of** an `INSERT`, `UPDATE`, or `DELETE` operation.
- Used when a direct modification needs to be intercepted or prevented.

## Creating an AFTER Trigger
The following trigger logs order updates to an audit table.

```sql
CREATE TABLE Sales.SalesOrderAudit (
    AuditID INT IDENTITY PRIMARY KEY,
    SalesOrderID INT,
    ModifiedDate DATETIME DEFAULT GETDATE(),
    OldTotalDue MONEY,
    NewTotalDue MONEY
);

CREATE TRIGGER trg_AfterOrderUpdate
ON Sales.SalesOrderHeader
AFTER UPDATE
AS
BEGIN
    INSERT INTO Sales.SalesOrderAudit (SalesOrderID, OldTotalDue, NewTotalDue)
    SELECT i.SalesOrderID, d.TotalDue, i.TotalDue
    FROM inserted i
    JOIN deleted d ON i.SalesOrderID = d.SalesOrderID;
END;
```

### Testing the Trigger
```sql
UPDATE Sales.SalesOrderHeader
SET TotalDue = TotalDue * 1.1
WHERE SalesOrderID = 43659;
```

### Checking the Audit Log
```sql
SELECT * FROM Sales.SalesOrderAudit;
```

## Creating an INSTEAD OF Trigger
The following trigger prevents deletions from the `Sales.SalesOrderHeader` table.

```sql
CREATE TRIGGER trg_PreventOrderDelete
ON Sales.SalesOrderHeader
INSTEAD OF DELETE
AS
BEGIN
    PRINT 'Deletion is not allowed!';
    ROLLBACK TRANSACTION;
END;
```

### Testing the Trigger
```sql
DELETE FROM Sales.SalesOrderHeader WHERE SalesOrderID = 43659;
```

## Using Triggers for Auditing
Triggers can be used to log changes to sensitive tables for auditing purposes.

```sql
CREATE TABLE HumanResources.EmployeeAudit (
    AuditID INT IDENTITY PRIMARY KEY,
    EmployeeID INT,
    ModifiedBy NVARCHAR(50),
    ModifiedDate DATETIME DEFAULT GETDATE(),
    ChangeType NVARCHAR(10)
);

CREATE TRIGGER trg_AuditEmployeeChanges
ON HumanResources.Employee
AFTER INSERT, UPDATE, DELETE
AS
BEGIN
    INSERT INTO HumanResources.EmployeeAudit (EmployeeID, ModifiedBy, ChangeType)
    SELECT COALESCE(i.BusinessEntityID, d.BusinessEntityID), SUSER_NAME(),
           CASE WHEN i.BusinessEntityID IS NOT NULL AND d.BusinessEntityID IS NOT NULL THEN 'UPDATE'
                WHEN i.BusinessEntityID IS NOT NULL THEN 'INSERT'
                WHEN d.BusinessEntityID IS NOT NULL THEN 'DELETE'
           END
    FROM inserted i
    FULL OUTER JOIN deleted d ON i.BusinessEntityID = d.BusinessEntityID;
END;
```

## Preventing Unauthorized Deletions
Triggers can help enforce data protection rules.

```sql
CREATE TRIGGER trg_PreventEmployeeDelete
ON HumanResources.Employee
INSTEAD OF DELETE
AS
BEGIN
    PRINT 'Employees cannot be deleted!'
    ROLLBACK TRANSACTION;
END;
```

## Real-Time Use Cases

### Auditing Sales Order Changes
- Automatically logs order modifications for compliance and tracking.
- Example: Keeping a record of total order value changes.

### Maintaining Inventory Levels
- Ensures stock levels are updated after each sale.
- Example: Decrement product inventory when an order is placed.

```sql
CREATE TRIGGER trg_UpdateInventory
ON Sales.SalesOrderDetail
AFTER INSERT
AS
BEGIN
    UPDATE Production.ProductInventory
    SET Quantity = Quantity - i.OrderQty
    FROM inserted i
    JOIN Production.ProductInventory p ON i.ProductID = p.ProductID;
END;
```

### Enforcing Business Rules
- Prevents invalid operations based on business logic.
- Example: Blocking orders from inactive customers.

```sql
CREATE TRIGGER trg_PreventInactiveCustomerOrder
ON Sales.SalesOrderHeader
BEFORE INSERT
AS
BEGIN
    IF EXISTS (
        SELECT 1 FROM inserted i
        JOIN Sales.Customer c ON i.CustomerID = c.CustomerID
        WHERE c.Status = 'Inactive')
    BEGIN
        PRINT 'Cannot place order for inactive customers!';
        ROLLBACK TRANSACTION;
    END;
END;
```

## Best Practices
- **Avoid complex logic** in triggers to prevent performance bottlenecks.
- **Use AFTER triggers** instead of INSTEAD OF triggers unless necessary.
- **Minimize trigger execution time** to reduce blocking issues.
- **Ensure proper error handling** using `TRY...CATCH`.

