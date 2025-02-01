# Temporary Tables in SQL Server

## Table of Contents
1. [Introduction](#introduction)
2. [Types of Temporary Tables](#types-of-temporary-tables)
   - [Local Temporary Tables](#local-temporary-tables)
   - [Global Temporary Tables](#global-temporary-tables)
3. [Creating Temporary Tables](#creating-temporary-tables)
   - [Using `CREATE TABLE`](#using-create-table)
   - [Using `SELECT INTO`](#using-select-into)
4. [Working with Temporary Tables](#working-with-temporary-tables)
   - [Inserting Data](#inserting-data)
   - [Selecting Data](#selecting-data)
   - [Updating Data](#updating-data)
   - [Dropping Temporary Tables](#dropping-temporary-tables)
5. [Scope and Lifetime of Temporary Tables](#scope-and-lifetime-of-temporary-tables)
   - [Scope of Local Temporary Tables](#scope-of-local-temporary-tables)
   - [Scope of Global Temporary Tables](#scope-of-global-temporary-tables)
   - [Lifetime and Cleanup](#lifetime-and-cleanup)
6. [Use Cases for Temporary Tables](#use-cases-for-temporary-tables)
   - [Staging Data for Complex Queries](#staging-data-for-complex-queries)
   - [Breaking Down Large Queries](#breaking-down-large-queries)
   - [Storing Intermediate Results](#storing-intermediate-results)
   - [Handling Large Data Sets](#handling-large-data-sets)
7. [Examples](#examples)
   - [Example 1: Creating a Local Temporary Table](#example-1-creating-a-local-temporary-table)
   - [Example 2: Using `SELECT INTO` to Create a Temporary Table](#example-2-using-select-into-to-create-a-temporary-table)
   - [Example 3: Updating Data in a Temporary Table](#example-3-updating-data-in-a-temporary-table)
   - [Example 4: Using Temporary Tables in Joins](#example-4-using-temporary-tables-in-joins)
   - [Example 5: Using Global Temporary Tables](#example-5-using-global-temporary-tables)
8. [Temporary Tables vs CTEs vs Table Variables](#temporary-tables-vs-ctes-vs-table-variables)
9. [Best Practices and Considerations](#best-practices-and-considerations)
   - [Performance Considerations](#performance-considerations)
   - [Managing Storage](#managing-storage)
     

## Introduction
Temporary tables in SQL Server are a valuable tool for handling intermediate data in complex queries, performing data transformations, or breaking down large operations. They offer flexibility for data manipulation without affecting the main database tables.

## Types of Temporary Tables

### Local Temporary Tables
Local temporary tables are prefixed with a single `#` symbol and are visible only to the session that created them. They are automatically dropped when the session ends.

**Example:**
```sql
CREATE TABLE #TempTable (ID INT, Name NVARCHAR(50));
```

### Global Temporary Tables
Global temporary tables are prefixed with `##` and are visible to all sessions. They remain available until the session that created them ends and there are no active references to them by other sessions.

**Example:**
```sql
CREATE TABLE ##GlobalTempTable (ID INT, Name NVARCHAR(50));
```

## Creating Temporary Tables

### Using `CREATE TABLE`
You can create a temporary table just like a regular table using the `CREATE TABLE` statement, specifying the columns and data types.

**Example:**
```sql
CREATE TABLE #TempTable
(
    ID INT PRIMARY KEY,
    Name NVARCHAR(50),
    CreatedDate DATETIME
);
```

### Using `SELECT INTO`
Another method to create a temporary table is by using `SELECT INTO`, which creates the table and inserts data into it in a single operation.

**Example:**
```sql
SELECT ID, Name, GETDATE() AS CreatedDate
INTO #TempTable
FROM Employees
WHERE DepartmentID = 1;
```

## Working with Temporary Tables

### Inserting Data
You can insert data into a temporary table using the `INSERT INTO` statement.

**Example:**
```sql
INSERT INTO #TempTable (ID, Name, CreatedDate)
VALUES (1, 'John Doe', GETDATE());
```

### Selecting Data
Query data from a temporary table just like any other table.

**Example:**
```sql
SELECT * FROM #TempTable;
```

### Updating Data
You can update records in a temporary table using the `UPDATE` statement.

**Example:**
```sql
UPDATE #TempTable
SET Name = 'Jane Doe'
WHERE ID = 1;
```

### Dropping Temporary Tables
It’s a good practice to manually drop temporary tables when they are no longer needed.

**Example:**
```sql
DROP TABLE #TempTable;
```

## Scope and Lifetime of Temporary Tables

### Scope of Local Temporary Tables
Local temporary tables are only accessible within the session that created them. They cannot be accessed by other sessions or users.

### Scope of Global Temporary Tables
Global temporary tables can be accessed by any session or user, but they are dropped when the session that created them ends and there are no active references to them.

### Lifetime and Cleanup
- **Local Temporary Tables:** Automatically dropped when the session ends.
- **Global Temporary Tables:** Dropped when the session that created them ends and no other sessions are using them.

## Use Cases for Temporary Tables

### Staging Data for Complex Queries
Temporary tables are useful for staging data before performing complex operations like joins, aggregations, or data transformations.

### Breaking Down Large Queries
Large queries can be broken into smaller, more manageable steps by storing intermediate results in temporary tables.

### Storing Intermediate Results
Temporary tables are ideal for storing intermediate results that can be used in subsequent queries within the same session.

### Handling Large Data Sets
Temporary tables can help manage large data sets by filtering, sorting, or aggregating data before further processing.

## Examples

### Example 1: Creating a Local Temporary Table
```sql
CREATE TABLE #EmployeeTemp
(
    EmployeeID INT,
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50)
);
```
**Use Case:** Storing employee data temporarily for a specific report.

### Example 2: Using `SELECT INTO` to Create a Temporary Table
```sql
SELECT EmployeeID, FirstName, LastName
INTO #EmployeeTemp
FROM Employees
WHERE DepartmentID = 2;
```
**Use Case:** Creating a temporary table with data from a specific department.

### Example 3: Updating Data in a Temporary Table
```sql
UPDATE #EmployeeTemp
SET LastName = 'Smith'
WHERE EmployeeID = 1;
```
**Use Case:** Correcting data in the temporary table before final processing.

### Example 4: Using Temporary Tables in Joins
```sql
SELECT e.EmployeeID, e.FirstName, d.DepartmentName
FROM #EmployeeTemp e
JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```
**Use Case:** Joining temporary table data with another table to retrieve additional information.

### Example 5: Using Global Temporary Tables
```sql
CREATE TABLE ##AllEmployees
(
    EmployeeID INT,
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50)
);
```
**Use Case:** Storing employee data accessible to multiple sessions or users.

## Temporary Tables vs CTEs vs Table Variables

- **Temporary Tables:**
  - Suitable for larger datasets.
  - Can have indexes and constraints.
  - Persist for the session and can be referenced multiple times.

- **CTEs:**
  - Ideal for readability and recursion.
  - Defined and used within a single query.
  - Do not persist after the query execution.

- **Table Variables:**
  - Similar to temporary tables but are stored in memory.
  - Limited scope and are generally faster for small datasets.
  - Do not participate in transactions.

## Best Practices and Considerations

### Performance Considerations
- Indexing temporary tables can improve performance, especially for large datasets.
- Be mindful of the tempdb system database's capacity, as temporary tables are stored there.

### Managing Storage
- Regularly drop temporary tables when they are no longer needed to free up space in tempdb.
- Monitor tempdb usage to avoid potential performance issues.

