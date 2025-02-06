# Query Optimization Techniques

## Index
- [Introduction to Query Optimization](#introduction-to-query-optimization)
- [Understanding Execution Plans](#understanding-execution-plans)
- [Indexing Strategies](#indexing-strategies)
  - [Clustered vs. Non-Clustered Indexes](#clustered-vs-non-clustered-indexes)
  - [Covering Indexes](#covering-indexes)
  - [Filtered Indexes](#filtered-indexes)
- [Common Performance Bottlenecks](#common-performance-bottlenecks)
- [Optimizing Joins](#optimizing-joins)
- [Using Statistics for Optimization](#using-statistics-for-optimization)
- [Reducing I/O Operations](#reducing-io-operations)
- [Best Practices](#best-practices)

## Introduction to Query Optimization
Query optimization in SQL Server ensures that database queries execute efficiently by minimizing resource consumption and execution time. Optimization techniques involve indexing, execution plans, and reducing I/O operations.

## Understanding Execution Plans
Execution plans provide insights into how SQL Server executes queries. To view an execution plan:

```sql
SET STATISTICS IO ON;
SET STATISTICS TIME ON;
GO
EXPLAIN SELECT * FROM Sales.SalesOrderHeader;
```

## Indexing Strategies
Proper indexing significantly improves query performance.

### Clustered vs. Non-Clustered Indexes
- **Clustered Index**: Defines the physical order of data in a table.
- **Non-Clustered Index**: Stores pointers to the actual data, useful for speeding up lookups.

Example:
```sql
CREATE CLUSTERED INDEX IX_SalesOrder ON Sales.SalesOrderHeader(OrderDate);
CREATE NONCLUSTERED INDEX IX_Customer ON Sales.SalesOrderHeader(CustomerID);
```

### Covering Indexes
A covering index includes all columns required by a query to avoid table scans.

```sql
CREATE NONCLUSTERED INDEX IX_SalesOrder_Covering
ON Sales.SalesOrderDetail (SalesOrderID, ProductID)
INCLUDE (OrderQty, UnitPrice);
```

### Filtered Indexes
Filtered indexes improve performance by indexing only a subset of rows.

```sql
CREATE NONCLUSTERED INDEX IX_SalesOrder_OpenOrders
ON Sales.SalesOrderHeader(Status)
WHERE Status = 1;
```

## Common Performance Bottlenecks
- **Missing or redundant indexes**
- **Poorly written joins and subqueries**
- **Excessive use of SELECT ***
- **Blocking and deadlocks**

## Optimizing Joins
Using appropriate join types and indexes speeds up queries.

```sql
SELECT s.SalesOrderID, c.CustomerID
FROM Sales.SalesOrderHeader s
INNER LOOP JOIN Sales.Customer c ON s.CustomerID = c.CustomerID;
```

## Using Statistics for Optimization
SQL Server uses statistics to estimate query costs and choose the best execution plan. Updating statistics can improve performance:

```sql
UPDATE STATISTICS Sales.SalesOrderHeader;
```

## Reducing I/O Operations
- Use **pagination** to limit result sets:

```sql
SELECT * FROM Sales.SalesOrderHeader
ORDER BY OrderDate
OFFSET 0 ROWS FETCH NEXT 10 ROWS ONLY;
```

- Avoid **functions in WHERE clauses** as they prevent index usage:

```sql
-- Avoid
SELECT * FROM Sales.SalesOrderHeader WHERE YEAR(OrderDate) = 2022;

-- Use
SELECT * FROM Sales.SalesOrderHeader WHERE OrderDate >= '2022-01-01' AND OrderDate < '2023-01-01';
```

## Best Practices
- Always **analyze execution plans** to identify slow queries.
- **Use indexed views** where applicable.
- **Partition large tables** to improve query performance.
- **Avoid cursors**; use set-based operations instead.
- Regularly **update statistics and rebuild indexes**.


