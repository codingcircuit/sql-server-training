# SQL Server Data Manipulation Language (DML)

## Table of Contents

1. [Introduction to DML](#introduction-to-dml)
2. [SQL Server DML Commands](#sql-server-dml-commands)
   - [INSERT](#insert)
   - [UPDATE](#update)
   - [DELETE](#delete)
   - [MERGE](#merge)
   - [SELECT](#select)
   - [Examples of DML Commands](#examples-of-dml-commands)
3. [Use Cases and Examples](#use-cases-and-examples)
   - [Use Case 1: Inserting Data into a Table](#use-case-1-inserting-data-into-a-table)
   - [Use Case 2: Updating Data in a Table](#use-case-2-updating-data-in-a-table)
   - [Use Case 3: Deleting Data from a Table](#use-case-3-deleting-data-from-a-table)
   - [Use Case 4: Merging Data from Two Tables](#use-case-4-merging-data-from-two-tables)
   - [Use Case 5: Retrieving Data from a Table](#use-case-5-retrieving-data-from-a-table)

---

## Introduction to DML

Data Manipulation Language (DML) is a subset of SQL commands used to manipulate data stored in database objects like tables. DML commands allow you to perform tasks such as inserting, updating, deleting, and retrieving data.

### Key DML Commands:
- **`INSERT`**: Used to insert new rows of data into a table.
- **`UPDATE`**: Used to modify existing rows of data in a table.
- **`DELETE`**: Used to remove rows of data from a table.
- **`MERGE`**: Used to perform insert, update, or delete operations based on conditions between two tables.
- **`SELECT`**: Used to query and retrieve data from a table.

---

## SQL Server DML Commands

### INSERT

The `INSERT` command is used to add new rows to a table.

```sql
INSERT INTO Employees (EmployeeID, FirstName, LastName, HireDate)
VALUES (1, 'John', 'Doe', '2023-08-27');
```

### UPDATE

The `UPDATE` command is used to modify existing data in a table.

```sql
UPDATE Employees
SET LastName = 'Smith'
WHERE EmployeeID = 1;
```

### DELETE

The `DELETE` command is used to remove rows from a table.

```sql
DELETE FROM Employees
WHERE EmployeeID = 1;
```

### MERGE

The `MERGE` command is used to perform insert, update, or delete operations based on conditions between two tables.

```sql
MERGE INTO Employees AS target
USING (SELECT EmployeeID, FirstName FROM NewEmployees) AS source
ON (target.EmployeeID = source.EmployeeID)
WHEN MATCHED THEN
    UPDATE SET target.FirstName = source.FirstName
WHEN NOT MATCHED BY TARGET THEN
    INSERT (EmployeeID, FirstName)
    VALUES (source.EmployeeID, source.FirstName);
```

### SELECT

The `SELECT` command is used to retrieve data from a table.

```sql
SELECT EmployeeID, FirstName, LastName
FROM Employees
WHERE HireDate > '2023-01-01';
```

### Examples of DML Commands

```sql
-- Insert Data into a Table
INSERT INTO Departments (DepartmentID, DepartmentName)
VALUES (1, 'HR'), (2, 'IT');

-- Update Data in a Table
UPDATE Departments
SET DepartmentName = 'Human Resources'
WHERE DepartmentID = 1;

-- Delete Data from a Table
DELETE FROM Departments
WHERE DepartmentID = 2;

-- Merge Data from Two Tables
MERGE INTO Departments AS target
USING (SELECT DepartmentID, DepartmentName FROM NewDepartments) AS source
ON (target.DepartmentID = source.DepartmentID)
WHEN MATCHED THEN
    UPDATE SET target.DepartmentName = source.DepartmentName
WHEN NOT MATCHED BY TARGET THEN
    INSERT (DepartmentID, DepartmentName)
    VALUES (source.DepartmentID, source.DepartmentName);

-- Retrieve Data from a Table
SELECT DepartmentID, DepartmentName
FROM Departments;
```

---

## Use Cases and Examples

In this section, we explore real-world scenarios where DML commands are used in SQL Server, with examples.

### Use Case 1: Inserting Data into a Table

#### Example: Using INSERT Command

```sql
INSERT INTO Products (ProductID, ProductName, Price)
VALUES (1, 'Laptop', 999.99), (2, 'Mouse', 19.99);
```

### Use Case 2: Updating Data in a Table

#### Example: Using UPDATE Command

```sql
UPDATE Products
SET Price = 899.99
WHERE ProductID = 1;
```

### Use Case 3: Deleting Data from a Table

#### Example: Using DELETE Command

```sql
DELETE FROM Products
WHERE ProductID = 2;
```

### Use Case 4: Merging Data from Two Tables

#### Example: Using MERGE Command

```sql
MERGE INTO Products AS target
USING (SELECT ProductID, ProductName, Price FROM NewProducts) AS source
ON (target.ProductID = source.ProductID)
WHEN MATCHED THEN
    UPDATE SET target.ProductName = source.ProductName, target.Price = source.Price
WHEN NOT MATCHED BY TARGET THEN
    INSERT (ProductID, ProductName, Price)
    VALUES (source.ProductID, source.ProductName, source.Price);
```

### Use Case 5: Retrieving Data from a Table

#### Example: Using SELECT Command

```sql
SELECT ProductID, ProductName, Price
FROM Products
WHERE Price > 500;
```
