# SQL Server Data Definition Language (DDL)

## Table of Contents

1. [Introduction to DDL](#introduction-to-ddl)
2. [SQL Server DDL Commands](#sql-server-ddl-commands)
   - [CREATE](#create)
   - [ALTER](#alter)
   - [DROP](#drop)
   - [TRUNCATE](#truncate)
   - [RENAME](#rename)
   - [Examples of DDL Commands](#examples-of-ddl-commands)
3. [Use Cases and Examples](#use-cases-and-examples)
   - [Use Case 1: Creating a New Table](#use-case-1-creating-a-new-table)
   - [Use Case 2: Modifying an Existing Table](#use-case-2-modifying-an-existing-table)
   - [Use Case 3: Dropping a Table](#use-case-3-dropping-a-table)
   - [Use Case 4: Truncating a Table](#use-case-4-truncating-a-table)
   - [Use Case 5: Renaming a Table](#use-case-5-renaming-a-table)

---

## Introduction to DDL

Data Definition Language (DDL) is a subset of SQL commands used to define and manage database structures such as tables, indexes, and schemas. DDL commands are responsible for the creation, modification, and deletion of these structures.

### Key DDL Commands:
- **`CREATE`**: Used to create new database objects such as tables, indexes, and schemas.
- **`ALTER`**: Used to modify the structure of existing database objects.
- **`DROP`**: Used to delete database objects.
- **`TRUNCATE`**: Used to remove all records from a table, while maintaining the table structure.
- **`RENAME`**: Used to rename database objects.

---

## SQL Server DDL Commands

### CREATE

The `CREATE` command is used to create a new table, index, or other database objects.

```sql
CREATE TABLE Employees (
    EmployeeID int PRIMARY KEY,
    FirstName varchar(50),
    LastName varchar(50),
    HireDate datetime
);
```

### ALTER

The `ALTER` command is used to modify an existing database object, such as adding or dropping a column from a table.

```sql
ALTER TABLE Employees
ADD MiddleName varchar(50);
```

### DROP

The `DROP` command is used to delete an existing database object permanently.

```sql
DROP TABLE Employees;
```

### TRUNCATE

The `TRUNCATE` command removes all records from a table but keeps the table structure intact.

```sql
TRUNCATE TABLE Employees;
```

### RENAME

The `RENAME` command is used to change the name of a database object.

```sql
EXEC sp_rename 'Employees', 'Staff';
```

### Examples of DDL Commands

```sql
-- Create a New Table
CREATE TABLE Departments (
    DepartmentID int PRIMARY KEY,
    DepartmentName varchar(50)
);

-- Add a New Column to an Existing Table
ALTER TABLE Departments
ADD Location varchar(100);

-- Drop a Table
DROP TABLE Departments;

-- Truncate a Table
TRUNCATE TABLE Departments;

-- Rename a Table
EXEC sp_rename 'Departments', 'Divisions';
```

---

## Use Cases and Examples

In this section, we explore real-world scenarios where DDL commands are used in SQL Server, with examples.

### Use Case 1: Creating a New Table

#### Example: Using CREATE Command

```sql
CREATE TABLE Projects (
    ProjectID int PRIMARY KEY,
    ProjectName varchar(100),
    StartDate datetime,
    EndDate datetime
);
```

### Use Case 2: Modifying an Existing Table

#### Example: Using ALTER Command

```sql
ALTER TABLE Projects
ADD Budget decimal(18, 2);
```

### Use Case 3: Dropping a Table

#### Example: Using DROP Command

```sql
DROP TABLE Projects;
```

### Use Case 4: Truncating a Table

#### Example: Using TRUNCATE Command

```sql
TRUNCATE TABLE Projects;
```

### Use Case 5: Renaming a Table

#### Example: Using RENAME Command

```sql
EXEC sp_rename 'Projects', 'Initiatives';
```
