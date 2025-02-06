# Common Table Expressions (CTEs) in SQL Server

## Table of Contents
1. [Introduction](#introduction)
2. [Basic Syntax of CTE](#basic-syntax-of-cte)
3. [Types of CTEs](#types-of-ctes)
   - [Simple CTE](#simple-cte)
   - [Recursive CTE](#recursive-cte)
4. [Use Cases for CTEs](#use-cases-for-ctes)
5. [Examples](#examples)
   - [Example 1: Simple CTE](#example-1-simple-cte)
   - [Example 2: Recursive CTE for Hierarchical Data](#example-2-recursive-cte-for-hierarchical-data)
   - [Example 3: CTE with Multiple Expressions](#example-3-cte-with-multiple-expressions)
   - [Example 4: Using CTEs with INSERT, UPDATE, DELETE](#example-4-using-ctes-with-insert-update-delete)
6. [CTE vs Temporary Tables vs Subqueries](#cte-vs-temporary-tables-vs-subqueries)
7. [Best Practices and Limitations](#best-practices-and-limitations)

## Introduction
Common Table Expressions (CTEs) are a powerful feature in SQL Server that allows you to define temporary result sets that can be referenced within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement. CTEs improve readability and maintainability of complex queries, making SQL code easier to understand and manage.

## Basic Syntax of CTE
A CTE is defined using the `WITH` keyword followed by the CTE name and the query defining the result set. You can reference the CTE within the main query as if it were a table.

**Syntax:**
```sql
WITH cte_name (column1, column2, ...)
AS
(
    -- CTE query
    SELECT ...
)
-- Main query using CTE
SELECT ...
FROM cte_name;
```

## Types of CTEs

### Simple CTE
A simple CTE is a single, non-recursive query that can be used to simplify complex SQL queries by breaking them into logical parts.

### Recursive CTE
A recursive CTE references itself and is used for queries that involve hierarchical data, such as organizational charts or tree structures. It consists of an anchor member and a recursive member.

**Syntax for Recursive CTE:**
```sql
WITH cte_name (column1, column2, ...)
AS
(
    -- Anchor member
    SELECT ...
    UNION ALL
    -- Recursive member
    SELECT ...
    FROM cte_name
    WHERE ...
)
SELECT ...
FROM cte_name;
```

## Use Cases for CTEs
- Simplifying complex queries by breaking them down into smaller, more manageable pieces.
- Creating recursive queries for hierarchical data, such as employee-manager relationships.
- Performing operations that require multiple passes over the data, like calculating running totals or cumulative sums.
- Making the SQL code more readable and maintainable, especially in scenarios with multiple joins or nested subqueries.

## Examples

### Example 1: Simple CTE
This example demonstrates the basic use of a CTE to simplify a query that selects employees with salaries above a certain threshold.

```sql
WITH HighSalaries AS
(
    SELECT EmployeeID, FirstName, LastName, Salary
    FROM Employees
    WHERE Salary > 60000
)
SELECT *
FROM HighSalaries;
```

**Explanation:** The CTE `HighSalaries` selects employees with salaries greater than 60,000, and the main query retrieves all the columns from this CTE.

### Example 2: Recursive CTE for Hierarchical Data
This example shows how to use a recursive CTE to retrieve all employees in an organizational hierarchy starting from a specific manager.

```sql
WITH EmployeeHierarchy AS
(
    -- Anchor member: Select the manager
    SELECT EmployeeID, ManagerID, FirstName, LastName, 0 AS Level
    FROM Employees
    WHERE ManagerID IS NULL
    
    UNION ALL
    
    -- Recursive member: Select employees reporting to the current level
    SELECT e.EmployeeID, e.ManagerID, e.FirstName, e.LastName, eh.Level + 1
    FROM Employees e
    INNER JOIN EmployeeHierarchy eh ON e.ManagerID = eh.EmployeeID
)
SELECT *
FROM EmployeeHierarchy
ORDER BY Level, EmployeeID;
```

**Explanation:** The CTE `EmployeeHierarchy` starts with the top-level manager and recursively selects all employees reporting to each level.

### Example 3: CTE with Multiple Expressions
This example demonstrates how to define and use multiple CTEs in a single query.

```sql
WITH SalesData AS
(
    SELECT SalesPersonID, SUM(SalesAmount) AS TotalSales
    FROM Sales
    GROUP BY SalesPersonID
),
HighSales AS
(
    SELECT SalesPersonID, TotalSales
    FROM SalesData
    WHERE TotalSales > 100000
)
SELECT *
FROM HighSales;
```

**Explanation:** The first CTE `SalesData` aggregates sales by salesperson, and the second CTE `HighSales` filters out salespeople with total sales greater than 100,000.

### Example 4: Using CTEs with INSERT, UPDATE, DELETE
CTEs can also be used in conjunction with `INSERT`, `UPDATE`, or `DELETE` operations.

```sql
WITH TopSalespeople AS
(
    SELECT SalesPersonID
    FROM Sales
    GROUP BY SalesPersonID
    HAVING SUM(SalesAmount) > 100000
)
UPDATE Employees
SET Bonus = 5000
WHERE EmployeeID IN (SELECT SalesPersonID FROM TopSalespeople);
```

**Explanation:** This example updates the `Bonus` column for employees who are top salespeople based on their sales performance.

## CTE vs Temporary Tables vs Subqueries
- **CTEs:** Ideal for readability and recursive queries. They are more straightforward for complex, nested queries and are only valid for the statement they are defined in.
- **Temporary Tables:** Useful for complex operations across multiple queries. They persist for the session and can store large intermediate results.
- **Subqueries:** Can be more concise but harder to read and maintain, especially with deep nesting. Subqueries are best for simple, one-off transformations.

## Best Practices and Limitations
- **Best Practices:**
  - Use CTEs to break down complex queries into more readable parts.
  - Utilize recursive CTEs for hierarchical data.
  - Keep CTEs focused and avoid excessive complexity within a single CTE.

- **Limitations:**
  - CTEs are only valid for the duration of the statement they are defined in.
  - Recursive CTEs can be performance-intensive, especially with deep recursion or large data sets.
  - SQL Server imposes a limit of 32767 recursions for a recursive CTE.
