# Dynamic SQL

## Index
- [Introduction to Dynamic SQL](#introduction-to-dynamic-sql)
- [Advantages and Disadvantages](#advantages-and-disadvantages)
- [Basic Dynamic SQL Execution](#basic-dynamic-sql-execution)
- [Using Parameters in Dynamic SQL](#using-parameters-in-dynamic-sql)
- [Handling SQL Injection](#handling-sql-injection)
- [Executing Dynamic SQL with `sp_executesql`](#executing-dynamic-sql-with-sp_executesql)
- [Dynamic Table and Column Selection](#dynamic-table-and-column-selection)
- [Real-Time Use Cases](#real-time-use-cases)
  - [Dynamic Search Queries](#dynamic-search-queries)
  - [Automating Table Partition Queries](#automating-table-partition-queries)
  - [Dynamic Report Generation](#dynamic-report-generation)
- [Best Practices](#best-practices)

## Introduction to Dynamic SQL
Dynamic SQL refers to SQL statements that are generated and executed at runtime. This allows flexibility in constructing queries dynamically based on input parameters or business logic.

## Advantages and Disadvantages
### Advantages:
- **Flexible Query Building**: Allows building queries dynamically based on conditions.
- **Improved Performance in Some Cases**: Can avoid large static queries with many conditional branches.
- **Supports Dynamic Objects**: Enables selecting tables and columns dynamically.

### Disadvantages:
- **SQL Injection Risk**: If not handled correctly, it can be vulnerable to attacks.
- **Performance Issues**: Execution plans are not cached as effectively as static SQL.
- **Complexity**: Harder to debug and maintain.

## Basic Dynamic SQL Execution
Dynamic SQL can be executed using `EXEC` or `sp_executesql`. Here’s an example using `EXEC`:

```sql
DECLARE @SQL NVARCHAR(MAX);
SET @SQL = 'SELECT TOP 10 * FROM Sales.SalesOrderHeader';
EXEC(@SQL);
```

## Using Parameters in Dynamic SQL
To pass parameters securely, use `sp_executesql`:

```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @CustomerID INT = 29825;

SET @SQL = 'SELECT * FROM Sales.SalesOrderHeader WHERE CustomerID = @CID';
EXEC sp_executesql @SQL, N'@CID INT', @CustomerID;
```

## Handling SQL Injection
To prevent SQL injection, always use parameterized queries instead of concatenating strings:

**Vulnerable Code:** ❌
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @CustomerID NVARCHAR(50) = '29825 OR 1=1';

SET @SQL = 'SELECT * FROM Sales.SalesOrderHeader WHERE CustomerID = ' + @CustomerID;
EXEC(@SQL);
```

**Secure Code:** ✅
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @CustomerID INT = 29825;

SET @SQL = 'SELECT * FROM Sales.SalesOrderHeader WHERE CustomerID = @CID';
EXEC sp_executesql @SQL, N'@CID INT', @CustomerID;
```

## Executing Dynamic SQL with `sp_executesql`
`sp_executesql` improves security and performance by allowing query plan reuse.

```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @StartDate DATE = '2023-01-01';
DECLARE @EndDate DATE = '2023-12-31';

SET @SQL = 'SELECT * FROM Sales.SalesOrderHeader WHERE OrderDate BETWEEN @Start AND @End';
EXEC sp_executesql @SQL, N'@Start DATE, @End DATE', @StartDate, @EndDate;
```

## Dynamic Table and Column Selection
Dynamic SQL can be used when table names or column names need to be dynamically specified.

```sql
DECLARE @TableName NVARCHAR(100) = 'Sales.SalesOrderHeader';
DECLARE @ColumnName NVARCHAR(100) = 'TotalDue';
DECLARE @SQL NVARCHAR(MAX);

SET @SQL = 'SELECT TOP 10 ' + QUOTENAME(@ColumnName) + ' FROM ' + QUOTENAME(@TableName);
EXEC(@SQL);
```

## Real-Time Use Cases

### Dynamic Search Queries
Used to build flexible search filters.

```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @CustomerID INT = NULL;
DECLARE @OrderDate DATE = '2023-01-01';

SET @SQL = 'SELECT * FROM Sales.SalesOrderHeader WHERE 1=1';
IF @CustomerID IS NOT NULL
    SET @SQL = @SQL + ' AND CustomerID = @CID';
IF @OrderDate IS NOT NULL
    SET @SQL = @SQL + ' AND OrderDate >= @ODate';

EXEC sp_executesql @SQL, N'@CID INT, @ODate DATE', @CustomerID, @OrderDate;
```

### Automating Table Partition Queries
Used for querying data from partitioned tables dynamically.

```sql
DECLARE @Year INT = 2023;
DECLARE @SQL NVARCHAR(MAX);

SET @SQL = 'SELECT * FROM Sales.Sales_' + CAST(@Year AS NVARCHAR(4));
EXEC(@SQL);
```

### Dynamic Report Generation
Used to create dynamic reports based on user selection.

```sql
DECLARE @ReportType NVARCHAR(50) = 'Sales';
DECLARE @SQL NVARCHAR(MAX);

IF @ReportType = 'Sales'
    SET @SQL = 'SELECT CustomerID, SUM(TotalDue) AS SalesTotal FROM Sales.SalesOrderHeader GROUP BY CustomerID';
ELSE IF @ReportType = 'Orders'
    SET @SQL = 'SELECT COUNT(*) AS OrderCount FROM Sales.SalesOrderHeader';

EXEC(@SQL);
```

## Best Practices
- **Always use `sp_executesql` with parameters** to prevent SQL injection.
- **Avoid dynamic SQL when static SQL can be used** for performance benefits.
- **Use `QUOTENAME()`** when dealing with table or column names to avoid syntax errors.
- **Minimize the use of dynamic SQL inside loops** to prevent excessive recompilation.

