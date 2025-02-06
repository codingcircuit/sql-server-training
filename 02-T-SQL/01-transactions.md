# Transactions in T-SQL

## Table of Contents
1. [Introduction](#introduction)
2. [Transactions in T-SQL](#1-transactions-in-t-sql)
   - [What is a Transaction?](#11-what-is-a-transaction)
   - [Types of Transactions](#12-types-of-transactions)
   - [Explicit Transaction Example](#13-explicit-transaction-example)
3. [Error Handling in T-SQL](#2-error-handling-in-t-sql)
   - [Using TRY...CATCH for Error Handling](#21-using-trycatch-for-error-handling)
   - [Common Error Functions](#22-common-error-functions)
4. [Use Cases & Real-time Scenarios](#3-use-cases--real-time-scenarios)
   - [Banking Transaction (Money Transfer)](#31-banking-transaction-money-transfer)
   - [Inventory Management (Stock Update)](#32-inventory-management-stock-update)
   - [Payroll Processing](#33-payroll-processing)
5. [Best Practices for Transactions & Error Handling](#4-best-practices-for-transactions--error-handling)
6. [Conclusion](#5-conclusion)

---

## Introduction
Transactions in SQL Server ensure data integrity by allowing multiple operations to be executed as a single unit of work. If any part of the transaction fails, the entire transaction can be rolled back to maintain consistency. Error handling mechanisms like `TRY...CATCH` help in managing exceptions and ensuring smooth execution.

---

## 1. Transactions in T-SQL

### 1.1 What is a Transaction?
A transaction is a sequence of operations performed as a single unit of work. Transactions follow the **ACID** properties:
- **Atomicity**: Ensures that all operations within a transaction are completed successfully; otherwise, the entire transaction is rolled back.
- **Consistency**: Ensures data remains in a valid state before and after the transaction.
- **Isolation**: Ensures transactions are executed independently of one another.
- **Durability**: Ensures committed transactions are permanently stored in the database.

### 1.2 Types of Transactions
1. **Explicit Transactions** (Manually controlled using `BEGIN TRAN`, `COMMIT`, `ROLLBACK`)
2. **Implicit Transactions** (Automatically starts a transaction for each DML statement when `SET IMPLICIT_TRANSACTIONS ON` is enabled)
3. **Autocommit Transactions** (Each individual statement is treated as a transaction)

### 1.3 Explicit Transaction Example
```sql
BEGIN TRANSACTION;
UPDATE Accounts SET Balance = Balance - 500 WHERE AccountID = 1;
UPDATE Accounts SET Balance = Balance + 500 WHERE AccountID = 2;
COMMIT TRANSACTION;
```

If an error occurs in the second update, the transaction can be rolled back:
```sql
BEGIN TRANSACTION;
UPDATE Accounts SET Balance = Balance - 500 WHERE AccountID = 1;
IF @@ERROR <> 0
    ROLLBACK TRANSACTION;
UPDATE Accounts SET Balance = Balance + 500 WHERE AccountID = 2;
COMMIT TRANSACTION;
```

---

## 2. Error Handling in T-SQL

### 2.1 Using TRY...CATCH for Error Handling
The `TRY...CATCH` block captures errors and prevents abrupt termination of queries.

#### Example:
```sql
BEGIN TRANSACTION;
BEGIN TRY
    UPDATE Accounts SET Balance = Balance - 500 WHERE AccountID = 1;
    UPDATE Accounts SET Balance = Balance + 500 WHERE AccountID = 2;
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    ROLLBACK TRANSACTION;
    PRINT 'Transaction failed: ' + ERROR_MESSAGE();
END CATCH;
```

### 2.2 Common Error Functions
- `ERROR_NUMBER()` - Returns the error number.
- `ERROR_MESSAGE()` - Returns the full error message.
- `ERROR_SEVERITY()` - Returns the severity level.
- `ERROR_STATE()` - Returns the state number.
- `ERROR_PROCEDURE()` - Returns the name of the stored procedure where the error occurred.
- `ERROR_LINE()` - Returns the line number where the error occurred.

#### Example:
```sql
BEGIN TRY
    INSERT INTO Orders (OrderID, CustomerID) VALUES (1, NULL);
END TRY
BEGIN CATCH
    PRINT 'Error Number: ' + CAST(ERROR_NUMBER() AS VARCHAR);
    PRINT 'Error Message: ' + ERROR_MESSAGE();
    PRINT 'Error Severity: ' + CAST(ERROR_SEVERITY() AS VARCHAR);
END CATCH;
```

---

## 3. Use Cases & Real-time Scenarios

### 3.1 Banking Transaction (Money Transfer)
**Scenario:** If a money transfer fails in the middle of the process, it should rollback to prevent incorrect deductions.

#### Solution:
```sql
BEGIN TRANSACTION;
BEGIN TRY
    UPDATE Accounts SET Balance = Balance - 1000 WHERE AccountID = 101;
    UPDATE Accounts SET Balance = Balance + 1000 WHERE AccountID = 102;
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    ROLLBACK TRANSACTION;
    PRINT 'Transaction failed: ' + ERROR_MESSAGE();
END CATCH;
```

### 3.2 Inventory Management (Stock Update)
**Scenario:** If a stock update process encounters an error while updating multiple tables, all changes should be rolled back.

#### Solution:
```sql
BEGIN TRANSACTION;
BEGIN TRY
    UPDATE Products SET Stock = Stock - 5 WHERE ProductID = 10;
    INSERT INTO StockLog (ProductID, ChangeQty, ChangeDate) VALUES (10, -5, GETDATE());
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    ROLLBACK TRANSACTION;
    PRINT 'Stock update failed: ' + ERROR_MESSAGE();
END CATCH;
```

### 3.3 Payroll Processing
**Scenario:** A company processes salaries in bulk. If any record fails, no salary should be processed.

#### Solution:
```sql
BEGIN TRANSACTION;
BEGIN TRY
    UPDATE EmployeeSalaries SET SalaryPaid = 1 WHERE Month = '2024-02';
    INSERT INTO PayrollLog (LogDate, Status) VALUES (GETDATE(), 'Success');
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    ROLLBACK TRANSACTION;
    PRINT 'Payroll processing failed: ' + ERROR_MESSAGE();
END CATCH;
```

---

## 4. Best Practices for Transactions & Error Handling

- Always use `TRY...CATCH` blocks for critical operations.
- Minimize transaction duration to avoid locking issues.
- Use `SAVEPOINT` for partial rollbacks.
- Handle deadlocks by retrying transactions.
- Log errors using a dedicated error log table.

---

## 5. Conclusion
Transactions and error handling in T-SQL are crucial for maintaining **data integrity** and **reliability**. Using proper **ACID-compliant** transaction mechanisms with `TRY...CATCH` ensures that failures are handled gracefully in real-world applications.

