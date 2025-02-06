# Transactions in T-SQL

## Index
- [Introduction to Transactions](#introduction-to-transactions)
- [ACID Properties](#acid-properties)
- [Types of Transactions](#types-of-transactions)
  - [Implicit Transactions](#implicit-transactions)
  - [Explicit Transactions](#explicit-transactions)
  - [Autocommit Transactions](#autocommit-transactions)
  - [Batch Scoped Transactions](#batch-scoped-transactions)
- [Transaction Control Commands](#transaction-control-commands)
  - [BEGIN TRANSACTION](#begin-transaction)
  - [COMMIT TRANSACTION](#commit-transaction)
  - [ROLLBACK TRANSACTION](#rollback-transaction)
  - [SAVE TRANSACTION](#save-transaction)
- [Error Handling with TRY...CATCH](#error-handling-with-trycatch)
- [Deadlocks and Concurrency](#deadlocks-and-concurrency)
- [Examples and Use Cases](#examples-and-use-cases)
  - [Basic Explicit Transaction](#basic-explicit-transaction)
  - [Using SAVEPOINTS](#using-savepoints)
  - [Handling Errors](#handling-errors)
  - [Dealing with Deadlocks](#dealing-with-deadlocks)
- [Best Practices](#best-practices)

## Introduction to Transactions
A transaction in T-SQL ensures that a sequence of operations is performed completely or not at all, maintaining database integrity. Transactions are used to group multiple SQL operations into a single unit of work. This ensures data consistency and allows recovery in case of failure.

## ACID Properties
Transactions adhere to the ACID properties:
- **Atomicity**: Ensures all operations in a transaction complete successfully or none execute.
- **Consistency**: Ensures data integrity before and after a transaction.
- **Isolation**: Ensures transactions do not interfere with each other.
- **Durability**: Ensures committed transactions persist even in case of system failure.

### Example:
```sql
BEGIN TRANSACTION;
UPDATE Accounts SET Balance = Balance - 100 WHERE AccountID = 1;
UPDATE Accounts SET Balance = Balance + 100 WHERE AccountID = 2;
COMMIT;
```

## Types of Transactions

### Implicit Transactions
Automatically starts a new transaction after the previous one completes. The transaction remains active until a `COMMIT` or `ROLLBACK` statement is issued.
```sql
SET IMPLICIT_TRANSACTIONS ON;
INSERT INTO Orders (OrderID, CustomerID) VALUES (1, 'C001');
COMMIT;
```

### Explicit Transactions
Manually controlled transactions using `BEGIN TRANSACTION`, `COMMIT`, and `ROLLBACK`.
```sql
BEGIN TRANSACTION;
INSERT INTO Orders (OrderID, CustomerID) VALUES (2, 'C002');
COMMIT TRANSACTION;
```

### Autocommit Transactions
Each statement is treated as a transaction and committed automatically by default.
```sql
INSERT INTO Orders (OrderID, CustomerID) VALUES (3, 'C003');
```

### Batch Scoped Transactions
Transactions that span multiple statements within a batch or stored procedure.
```sql
BEGIN TRANSACTION;
UPDATE Inventory SET Stock = Stock - 1 WHERE ProductID = 101;
UPDATE Sales SET Quantity = Quantity + 1 WHERE ProductID = 101;
COMMIT;
```

## Transaction Control Commands

### BEGIN TRANSACTION
Starts a new explicit transaction.
```sql
BEGIN TRANSACTION;
```

### COMMIT TRANSACTION
Commits the transaction and saves changes.
```sql
COMMIT TRANSACTION;
```

### ROLLBACK TRANSACTION
Rolls back the transaction, undoing changes.
```sql
ROLLBACK TRANSACTION;
```

### SAVE TRANSACTION
Creates a savepoint for partial rollbacks.
```sql
BEGIN TRANSACTION;
INSERT INTO Orders (OrderID, CustomerID) VALUES (3, 'C003');
SAVE TRANSACTION SavePoint1;
DELETE FROM Orders WHERE OrderID = 3;
ROLLBACK TRANSACTION SavePoint1;
COMMIT;
```

## Error Handling with TRY...CATCH
Using TRY...CATCH to handle errors and rollback transactions upon failure.
```sql
BEGIN TRANSACTION;
BEGIN TRY
    INSERT INTO Orders (OrderID, CustomerID) VALUES (4, 'C004');
    COMMIT;
END TRY
BEGIN CATCH
    ROLLBACK;
END CATCH;
```

## Deadlocks and Concurrency
Deadlocks occur when two transactions hold locks that the other needs, leading to a cycle. To minimize deadlocks:
- Use proper indexing.
- Keep transactions short.
- Use `SET LOCK_TIMEOUT` to avoid indefinite waits.

```sql
SET LOCK_TIMEOUT 5000; -- Timeout in milliseconds
BEGIN TRANSACTION;
UPDATE Customers SET Balance = Balance - 50 WHERE CustomerID = 1;
UPDATE Orders SET Status = 'Paid' WHERE OrderID = 10;
COMMIT;
```

## Examples and Use Cases

### Basic Explicit Transaction
```sql
BEGIN TRANSACTION;
UPDATE Products SET Price = Price * 1.1 WHERE Category = 'Electronics';
COMMIT;
```

### Using SAVEPOINTS
```sql
BEGIN TRANSACTION;
UPDATE Products SET Price = 100 WHERE ProductID = 1;
SAVE TRANSACTION SavePoint1;
DELETE FROM Products WHERE ProductID = 2;
ROLLBACK TRANSACTION SavePoint1;
COMMIT;
```

### Handling Errors
```sql
BEGIN TRANSACTION;
BEGIN TRY
    INSERT INTO Customers (CustomerID, Name) VALUES (5, 'John Doe');
    COMMIT;
END TRY
BEGIN CATCH
    ROLLBACK;
END CATCH;
```

### Dealing with Deadlocks
Using `NOLOCK` to avoid blocking and setting deadlock priority.
```sql
SET DEADLOCK_PRIORITY LOW;
SELECT * FROM Orders WITH (NOLOCK) WHERE OrderDate > '2024-01-01';
```

## Best Practices
- Keep transactions short.
- Use proper indexing to reduce deadlocks.
- Handle errors gracefully with TRY...CATCH.
- Avoid unnecessary locks.
