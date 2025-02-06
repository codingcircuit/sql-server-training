# SQL Server Data Retrieval Language (DRL)

## Table of Contents

1. [Introduction to DRL](#introduction-to-drl)
2. [SQL Server DRL Commands](#sql-server-drl-commands)
   - [SELECT](#select)
   - [WHERE](#where)
   - [ORDER BY](#order-by)
   - [GROUP BY](#group-by)
   - [HAVING](#having)
   - [JOIN](#join)
   - [LIKE](#like)
   - [REGEX](#regex)
   - [AND, OR](#and-or)
   - [DISTINCT](#distinct)
   - [IN](#in)
   - [Examples of DRL Commands](#examples-of-drl-commands)
3. [Use Cases and Examples](#use-cases-and-examples)
   - [Use Case 1: Retrieving Specific Columns from a Table](#use-case-1-retrieving-specific-columns-from-a-table)
   - [Use Case 2: Filtering Data Using WHERE and Logical Operators](#use-case-2-filtering-data-using-where-and-logical-operators)
   - [Use Case 3: Searching for Patterns Using LIKE and REGEX](#use-case-3-searching-for-patterns-using-like-and-regex)
   - [Use Case 4: Sorting Data Using ORDER BY Clause](#use-case-4-sorting-data-using-order-by-clause)
   - [Use Case 5: Grouping Data Using GROUP BY and Filtering Using HAVING Clause](#use-case-5-grouping-data-using-group-by-and-filtering-using-having-clause)
   - [Use Case 6: Retrieving Unique Records Using DISTINCT](#use-case-6-retrieving-unique-records-using-distinct)
   - [Use Case 7: Filtering Data Using IN Clause](#use-case-7-filtering-data-using-in-clause)
   - [Use Case 8: Joining Multiple Tables](#use-case-8-joining-multiple-tables)

---

## Introduction to DRL

Data Retrieval Language (DRL) is a subset of SQL commands used to query and retrieve data from database objects such as tables. DRL commands allow you to filter, sort, group, and join data from one or more tables, and provide functionalities for pattern matching and retrieving distinct records.

### Key DRL Commands:
- **`SELECT`**: Used to retrieve data from a table.
- **`WHERE`**: Used to filter data based on specific conditions.
- **`ORDER BY`**: Used to sort data in ascending or descending order.
- **`GROUP BY`**: Used to group data based on one or more columns.
- **`HAVING`**: Used to filter groups based on specific conditions.
- **`JOIN`**: Used to retrieve data from multiple tables based on related columns.
- **`LIKE`**: Used to search for patterns in data.
- **`REGEX`**: Used for advanced pattern matching (note: limited support in SQL Server).
- **`AND`, `OR`**: Logical operators used to combine multiple conditions in a `WHERE` clause.
- **`DISTINCT`**: Used to retrieve unique records.
- **`IN`**: Used to filter data based on a set of values.

---

## SQL Server DRL Commands

### SELECT

The `SELECT` command is used to retrieve data from one or more columns in a table.

```sql
SELECT EmployeeID, FirstName, LastName
FROM Employees;
```

### WHERE

The `WHERE` clause is used to filter data based on specific conditions.

```sql
SELECT EmployeeID, FirstName, LastName
FROM Employees
WHERE DepartmentID = 1;
```

### ORDER BY

The `ORDER BY` clause is used to sort data in ascending (default) or descending order.

```sql
SELECT EmployeeID, FirstName, LastName
FROM Employees
ORDER BY LastName ASC;
```

### GROUP BY

The `GROUP BY` clause is used to group data based on one or more columns.

```sql
SELECT DepartmentID, COUNT(EmployeeID) AS EmployeeCount
FROM Employees
GROUP BY DepartmentID;
```

### HAVING

The `HAVING` clause is used to filter groups of data, often in combination with the `GROUP BY` clause.

```sql
SELECT DepartmentID, COUNT(EmployeeID) AS EmployeeCount
FROM Employees
GROUP BY DepartmentID
HAVING COUNT(EmployeeID) > 10;
```

### JOIN

The `JOIN` clause is used to retrieve data from multiple tables based on related columns.

```sql
SELECT Employees.EmployeeID, Employees.FirstName, Departments.DepartmentName
FROM Employees
JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
```

### LIKE

The `LIKE` operator is used to search for a specified pattern in a column.

```sql
SELECT FirstName, LastName
FROM Employees
WHERE FirstName LIKE 'J%';
```

### REGEX

SQL Server supports basic pattern matching with the `LIKE` operator, but regular expressions (regex) require CLR integration or other workarounds for full support.

```sql
-- Example of basic pattern matching with LIKE
SELECT Email
FROM Employees
WHERE Email LIKE '%@gmail.com';
```

### AND, OR

The `AND` and `OR` operators are used to combine multiple conditions in a `WHERE` clause.

```sql
SELECT FirstName, LastName
FROM Employees
WHERE DepartmentID = 1 AND HireDate > '2023-01-01';

SELECT FirstName, LastName
FROM Employees
WHERE DepartmentID = 1 OR HireDate > '2023-01-01';
```

### DISTINCT

The `DISTINCT` keyword is used to return unique records from a query.

```sql
SELECT DISTINCT DepartmentID
FROM Employees;
```

### IN

The `IN` operator is used to filter data based on a set of values.

```sql
SELECT FirstName, LastName
FROM Employees
WHERE DepartmentID IN (1, 2, 3);
```

### Examples of DRL Commands

```sql
-- Retrieve Specific Columns from a Table
SELECT ProductID, ProductName, Price
FROM Products;

-- Filter Data Using WHERE and Logical Operators
SELECT ProductID, ProductName, Price
FROM Products
WHERE Price > 50 AND CategoryID = 2;

-- Search for Patterns Using LIKE
SELECT ProductID, ProductName
FROM Products
WHERE ProductName LIKE 'Lap%';

-- Sort Data Using ORDER BY Clause
SELECT ProductID, ProductName, Price
FROM Products
ORDER BY Price DESC;

-- Group Data Using GROUP BY Clause
SELECT CategoryID, COUNT(ProductID) AS ProductCount
FROM Products
GROUP BY CategoryID;

-- Filter Groups Using HAVING Clause
SELECT CategoryID, COUNT(ProductID) AS ProductCount
FROM Products
GROUP BY CategoryID
HAVING COUNT(ProductID) > 5;

-- Retrieve Unique Records Using DISTINCT
SELECT DISTINCT CategoryID
FROM Products;

-- Filter Data Using IN Clause
SELECT ProductID, ProductName
FROM Products
WHERE CategoryID IN (1, 3, 5);

-- Retrieve Data from Multiple Tables Using JOIN
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

---

## Use Cases and Examples

In this section, we explore real-world scenarios where DRL commands are used in SQL Server, with examples.

### Use Case 1: Retrieving Specific Columns from a Table

#### Example: Using SELECT Command

```sql
SELECT ProductID, ProductName, Price
FROM Products;
```

### Use Case 2: Filtering Data Using WHERE and Logical Operators

#### Example: Using WHERE with AND, OR

```sql
SELECT ProductID, ProductName, Price
FROM Products
WHERE Price > 50 AND CategoryID = 2;

SELECT ProductID, ProductName, Price
FROM Products
WHERE Price > 50 OR CategoryID = 2;
```

### Use Case 3: Searching for Patterns Using LIKE and REGEX

#### Example: Using LIKE for Pattern Matching

```sql
SELECT ProductID, ProductName
FROM Products
WHERE ProductName LIKE '%Pro%';
```

### Use Case 4: Sorting Data Using ORDER BY Clause

#### Example: Using ORDER BY Clause

```sql
SELECT ProductID, ProductName, Price
FROM Products
ORDER BY Price DESC;
```

### Use Case 5: Grouping Data Using GROUP BY and Filtering Using HAVING Clause

#### Example: Using GROUP BY and HAVING

```sql
SELECT CategoryID, AVG(Price) AS AveragePrice
FROM Products
GROUP BY CategoryID
HAVING AVG(Price) > 100;
```

### Use Case 6: Retrieving Unique Records Using DISTINCT

#### Example: Using DISTINCT

```sql
SELECT DISTINCT CategoryID
FROM Products;
```

### Use Case 7: Filtering Data Using IN Clause

#### Example: Using IN Clause

```sql
SELECT ProductID, ProductName
FROM Products
WHERE CategoryID IN (1, 3, 5);
```

### Use Case 8: Joining Multiple Tables

#### Example: Using JOIN Clause

```sql
SELECT Orders.OrderID, Customers.CustomerName, Products.ProductName
FROM Orders
JOIN Customers ON Orders.CustomerID = Customers.CustomerID
JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
JOIN Products ON OrderDetails.ProductID = Products.ProductID;
```
