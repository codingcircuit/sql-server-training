# T-SQL Indexes and Performance Optimization (AdventureWorks 2022)

## Index
- [Introduction to Indexes](#introduction-to-indexes)
- [Types of Indexes](#types-of-indexes)
  - [Clustered Index](#clustered-index)
  - [Non-Clustered Index](#non-clustered-index)
  - [Unique Index](#unique-index)
  - [Filtered Index](#filtered-index)
  - [Covering Index](#covering-index)
- [Index Maintenance](#index-maintenance)
  - [Rebuilding Indexes](#rebuilding-indexes)
  - [Reorganizing Indexes](#reorganizing-indexes)
- [Execution Plans and Query Optimization](#execution-plans-and-query-optimization)
- [Indexing Best Practices](#indexing-best-practices)

## Introduction to Indexes
Indexes in T-SQL improve query performance by allowing the database engine to find data more efficiently. They function like an index in a book, enabling quick lookups.

### AdventureWorks 2022 Sample Data Setup
AdventureWorks 2022 is a Microsoft-provided sample database that contains real-world business data. We will use the `Sales.SalesOrderDetail` and `Person.Person` tables to demonstrate index usage.

## Types of Indexes

### Clustered Index
A clustered index sorts and stores data rows in the table based on the index key. A table can have only one clustered index.
By default, primary keys in AdventureWorks have clustered indexes.

#### Why Clustered Index Works Best
In `Sales.SalesOrderHeader`, the primary key `SalesOrderID` already has a clustered index, ensuring that queries searching for specific sales orders are highly optimized. Creating a clustered index on `OrderDate` helps when filtering sales data by date efficiently.

```sql
-- Checking existing clustered index on SalesOrderDetail
SELECT name, type_desc FROM sys.indexes WHERE object_id = OBJECT_ID('Sales.SalesOrderDetail');

-- Creating a clustered index on OrderDate in SalesOrderHeader
CREATE CLUSTERED INDEX IX_SalesOrderHeader_OrderDate ON Sales.SalesOrderHeader (OrderDate);
```

### Non-Clustered Index
A non-clustered index creates a separate structure that stores pointers to the actual data rows.

#### Why Non-Clustered Index Works Best
In the `Person.Person` table, searching by `LastName` is common but not unique. A non-clustered index speeds up searches without affecting the table's physical order.

```sql
CREATE NONCLUSTERED INDEX IX_Person_LastName ON Person.Person (LastName);
```

### Unique Index
Ensures that all values in the index column(s) are unique.

#### Why Unique Index Works Best
In `Sales.SalesOrderDetail`, the combination of `SalesOrderID` and `SalesOrderDetailID` is always unique, making it an ideal candidate for a unique index.

```sql
CREATE UNIQUE INDEX IX_SalesOrderDetail_Unique ON Sales.SalesOrderDetail (SalesOrderID, SalesOrderDetailID);
```

### Filtered Index
Applies an index only to a subset of data, improving performance for specific queries.

#### Why Filtered Index Works Best
If we often query orders with `OrderQty > 5`, a filtered index ensures that only relevant rows are indexed, reducing storage and improving performance.

```sql
CREATE INDEX IX_SalesOrderDetail_OnlineOrders ON Sales.SalesOrderDetail (ProductID) WHERE OrderQty > 5;
```

### Covering Index
Includes all the columns needed by a query, reducing the need for lookups.

#### Why Covering Index Works Best
A query that retrieves `ProductID`, `OrderQty`, `UnitPrice`, and `LineTotal` from `Sales.SalesOrderDetail` benefits from a covering index, preventing additional lookups.

```sql
CREATE NONCLUSTERED INDEX IX_SalesOrderDetail_Covering ON Sales.SalesOrderDetail (ProductID) INCLUDE (OrderQty, UnitPrice, LineTotal);
```

## Index Maintenance
### Rebuilding Indexes
Rebuilds the index structure, removing fragmentation and improving performance.

```sql
ALTER INDEX ALL ON Sales.SalesOrderDetail REBUILD;
```

### Reorganizing Indexes
Defragments an index while keeping it online.

```sql
ALTER INDEX ALL ON Sales.SalesOrderDetail REORGANIZE;
```

## Execution Plans and Query Optimization
Understanding execution plans helps identify inefficient queries and suggest index improvements.

```sql
SET SHOWPLAN_XML ON;
SELECT * FROM Sales.SalesOrderDetail WHERE ProductID = 870;
SET SHOWPLAN_XML OFF;
```

## Indexing Best Practices
- Use clustered indexes on frequently searched columns.
- Avoid over-indexing as it affects insert/update performance.
- Regularly rebuild or reorganize fragmented indexes.
- Use `INCLUDE` columns in non-clustered indexes for covering queries.

