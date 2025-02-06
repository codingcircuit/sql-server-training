# Cursors

## Index
- [Introduction to Cursors](#introduction-to-cursors)
- [Types of Cursors](#types-of-cursors)
  - [Static Cursor](#static-cursor)
  - [Dynamic Cursor](#dynamic-cursor)
  - [Forward-Only Cursor](#forward-only-cursor)
  - [Keyset-Driven Cursor](#keyset-driven-cursor)
- [Declaring and Using a Cursor](#declaring-and-using-a-cursor)
- [Fetching Data Using a Cursor](#fetching-data-using-a-cursor)
- [Closing and Deallocating Cursors](#closing-and-deallocating-cursors)
- [Performance Considerations](#performance-considerations)
- [Alternative Approaches](#alternative-approaches)
- [Real-Time Use Cases](#real-time-use-cases)

## Introduction to Cursors
Cursors in T-SQL allow row-by-row processing of query results. They are useful when operations cannot be performed using set-based queries.

## Types of Cursors
T-SQL supports multiple cursor types:

### Static Cursor
- Stores the result set in `tempdb`.
- Does not reflect changes made to the base table after cursor declaration.

```sql
DECLARE sales_cursor CURSOR STATIC
FOR SELECT SalesOrderID, TotalDue FROM Sales.SalesOrderHeader;
```

### Dynamic Cursor
- Reflects real-time changes in the underlying data.

```sql
DECLARE sales_cursor CURSOR DYNAMIC
FOR SELECT SalesOrderID, TotalDue FROM Sales.SalesOrderHeader;
```

### Forward-Only Cursor
- Fetches rows in sequence without backward movement.
- Uses fewer resources than other cursor types.

```sql
DECLARE sales_cursor CURSOR FORWARD_ONLY
FOR SELECT SalesOrderID, TotalDue FROM Sales.SalesOrderHeader;
```

### Keyset-Driven Cursor
- Uses a keyset to track rows.
- Changes to data are visible, but new rows are not included.

```sql
DECLARE sales_cursor CURSOR KEYSET
FOR SELECT SalesOrderID, TotalDue FROM Sales.SalesOrderHeader;
```

## Declaring and Using a Cursor
To use a cursor:
1. Declare the cursor.
2. Open the cursor.
3. Fetch rows into variables.
4. Close and deallocate the cursor.

```sql
DECLARE @OrderID INT, @TotalDue MONEY;
DECLARE sales_cursor CURSOR FOR
SELECT SalesOrderID, TotalDue FROM Sales.SalesOrderHeader;

OPEN sales_cursor;
FETCH NEXT FROM sales_cursor INTO @OrderID, @TotalDue;

WHILE @@FETCH_STATUS = 0
BEGIN
    PRINT 'Order ID: ' + CAST(@OrderID AS VARCHAR) + ' - Total Due: ' + CAST(@TotalDue AS VARCHAR);
    FETCH NEXT FROM sales_cursor INTO @OrderID, @TotalDue;
END;

CLOSE sales_cursor;
DEALLOCATE sales_cursor;
```

## Fetching Data Using a Cursor
Cursors allow row-by-row retrieval using:
- `FETCH NEXT`: Moves to the next row.
- `FETCH PRIOR`: Moves to the previous row (not for forward-only cursors).
- `FETCH FIRST`: Moves to the first row.
- `FETCH LAST`: Moves to the last row.

Example:
```sql
FETCH LAST FROM sales_cursor INTO @OrderID, @TotalDue;
```

## Closing and Deallocating Cursors
To release resources:

```sql
CLOSE sales_cursor;
DEALLOCATE sales_cursor;
```

## Performance Considerations
- Cursors are slower than set-based queries.
- They consume more memory and processing power.
- They should be avoided unless necessary.

## Alternative Approaches
Instead of cursors, use:
- **Set-based operations**:

```sql
SELECT SalesOrderID, TotalDue FROM Sales.SalesOrderHeader;
```
- **Common Table Expressions (CTEs)**:

```sql
WITH SalesData AS (
    SELECT SalesOrderID, TotalDue FROM Sales.SalesOrderHeader
)
SELECT * FROM SalesData;
```

## Real-Time Use Cases
### Processing Orders Sequentially
A cursor can process orders one by one and update their status.

```sql
DECLARE @OrderID INT;
DECLARE order_cursor CURSOR FOR
SELECT SalesOrderID FROM Sales.SalesOrderHeader WHERE Status = 1;

OPEN order_cursor;
FETCH NEXT FROM order_cursor INTO @OrderID;

WHILE @@FETCH_STATUS = 0
BEGIN
    UPDATE Sales.SalesOrderHeader SET Status = 5 WHERE SalesOrderID = @OrderID;
    FETCH NEXT FROM order_cursor INTO @OrderID;
END;

CLOSE order_cursor;
DEALLOCATE order_cursor;
```
