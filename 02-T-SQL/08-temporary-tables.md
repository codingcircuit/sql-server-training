# Temporary Tables
## Index
- [Introduction to Temporary Tables](#introduction-to-temporary-tables)
- [Types of Temporary Tables](#types-of-temporary-tables)
  - [Local Temporary Tables](#local-temporary-tables)
  - [Global Temporary Tables](#global-temporary-tables)
- [Temporary Table vs Table Variable](#temporary-table-vs-table-variable)
- [Using Temporary Tables in Stored Procedures](#using-temporary-tables-in-stored-procedures)
- [Real-Time Use Cases](#real-time-use-cases)

## Introduction to Temporary Tables
Temporary tables store intermediate results temporarily and are automatically deleted when no longer needed. They are useful for storing and manipulating transient data.

## Types of Temporary Tables
T-SQL supports two types of temporary tables: local and global.

### Local Temporary Tables
Local temporary tables (`#TempTable`) are visible only within the session that created them.

```sql
CREATE TABLE #TempSales (
    SalesOrderID INT,
    CustomerID INT,
    OrderDate DATE
);

INSERT INTO #TempSales
SELECT SalesOrderID, CustomerID, OrderDate FROM Sales.SalesOrderHeader;

SELECT * FROM #TempSales;
```

### Global Temporary Tables
Global temporary tables (`##TempTable`) are visible across multiple sessions but are dropped when the last session referencing them is closed.

```sql
CREATE TABLE ##GlobalTempSales (
    SalesOrderID INT,
    CustomerID INT,
    OrderDate DATE
);

INSERT INTO ##GlobalTempSales
SELECT SalesOrderID, CustomerID, OrderDate FROM Sales.SalesOrderHeader;

SELECT * FROM ##GlobalTempSales;
```

## Temporary Table vs Table Variable
| Feature            | Temporary Table (`#TempTable`) | Table Variable (`@TableVar`) |
|--------------------|--------------------------------|------------------------------|
| Scope             | Session-wide                   | Batch/Procedure-wide        |
| Transaction Logs  | Logged                         | Minimal Logging             |
| Indexing          | Supports indexes              | Limited Indexing            |

## Using Temporary Tables in Stored Procedures
Temporary tables can improve performance and simplify complex queries in stored procedures.

```sql
CREATE PROCEDURE GetRecentSales
AS
BEGIN
    CREATE TABLE #RecentSales (
        SalesOrderID INT,
        CustomerID INT,
        OrderDate DATE
    );

    INSERT INTO #RecentSales
    SELECT SalesOrderID, CustomerID, OrderDate
    FROM Sales.SalesOrderHeader
    WHERE OrderDate > DATEADD(MONTH, -1, GETDATE());

    SELECT * FROM #RecentSales;
END;
```

## Real-Time Use Cases
### Caching Data for Reports
Temporary tables allow caching frequently accessed data for reporting.

```sql
SELECT CustomerID, SUM(TotalDue) AS TotalSpent
INTO #CustomerSpending
FROM Sales.SalesOrderHeader
GROUP BY CustomerID;

SELECT * FROM #CustomerSpending WHERE TotalSpent > 10000;
```

### Storing Intermediate Results
Breaking down complex queries into multiple steps improves performance.

```sql
SELECT * INTO #TopCustomers
FROM (
    SELECT TOP 10 CustomerID, COUNT(*) AS OrderCount
    FROM Sales.SalesOrderHeader
    GROUP BY CustomerID
    ORDER BY OrderCount DESC
) AS SubQuery;

SELECT * FROM #TopCustomers;
```
