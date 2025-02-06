# Records and Collections

## Index
- [Introduction to Records and Collections](#introduction-to-records-and-collections)
- [Table Variables](#table-variables)
- [Temporary Tables](#temporary-tables)
- [Common Table Expressions (CTEs)](#common-table-expressions-ctes)
- [JSON Data Handling](#json-data-handling)
- [XML Data Handling](#xml-data-handling)
- [Real-Time Use Cases](#real-time-use-cases)

## Introduction to Records and Collections
Records and collections in T-SQL allow handling multiple rows of data within a session efficiently. Techniques include table variables, temporary tables, CTEs, JSON, and XML data structures.

## Table Variables
Table variables store temporary data within a batch or stored procedure. They are useful for small datasets.

```sql
DECLARE @Sales TABLE (
    SalesOrderID INT,
    CustomerID INT,
    OrderDate DATE
);

INSERT INTO @Sales
SELECT SalesOrderID, CustomerID, OrderDate FROM Sales.SalesOrderHeader;

SELECT * FROM @Sales;
```

## Temporary Tables
Temporary tables (`#TempTable`) persist within a session and allow indexing.

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

## Common Table Expressions (CTEs)
CTEs simplify complex queries by allowing recursion and structured processing.

```sql
WITH RecentOrders AS (
    SELECT SalesOrderID, CustomerID, OrderDate
    FROM Sales.SalesOrderHeader
    WHERE OrderDate > '2023-01-01'
)
SELECT * FROM RecentOrders;
```

## JSON Data Handling
T-SQL supports JSON for semi-structured data storage and manipulation.

```sql
DECLARE @json NVARCHAR(MAX) =
    '{ "SalesOrderID": 1, "CustomerID": 1001, "OrderDate": "2023-05-01" }';

SELECT * FROM OPENJSON(@json)
WITH (
    SalesOrderID INT,
    CustomerID INT,
    OrderDate DATE
);
```

## XML Data Handling
SQL Server allows storing and querying XML data using `FOR XML`.

```sql
SELECT SalesOrderID, CustomerID, OrderDate
FROM Sales.SalesOrderHeader
FOR XML AUTO;
```

## Real-Time Use Cases
### Caching Data in Table Variables
Using table variables to store query results for later reuse.

```sql
DECLARE @CustomerData TABLE (
    CustomerID INT,
    CustomerName NVARCHAR(100)
);

INSERT INTO @CustomerData
SELECT CustomerID, FirstName + ' ' + LastName FROM Person.Person;

SELECT * FROM @CustomerData;
```

### Using JSON for API Integration
Fetching sales data in JSON format for API responses.

```sql
SELECT SalesOrderID, CustomerID, OrderDate
FROM Sales.SalesOrderHeader
FOR JSON AUTO;
```
