# INFORMATION_SCHEMA in SQL Server

## Table of Contents
1. [Introduction](#introduction)
2. [Overview of INFORMATION_SCHEMA](#overview-of-information_schema)
3. [Common INFORMATION_SCHEMA Views](#common-information_schema-views)
   - [INFORMATION_SCHEMA.TABLES](#information_schema.tables)
   - [INFORMATION_SCHEMA.COLUMNS](#information_schema.columns)
   - [INFORMATION_SCHEMA.CONSTRAINTS](#information_schema.constraints)
   - [INFORMATION_SCHEMA.KEY_COLUMN_USAGE](#information_schema.key_column_usage)
   - [INFORMATION_SCHEMA.VIEWS](#information_schema.views)
   - [INFORMATION_SCHEMA.ROUTINES](#information_schema.routines)
4. [Using INFORMATION_SCHEMA in Queries](#using-information_schema-in-queries)
   - [Querying Table Metadata](#querying-table-metadata)
   - [Retrieving Column Information](#retrieving-column-information)
   - [Working with Constraints](#working-with-constraints)
   - [Listing Views](#listing-views)
   - [Fetching Stored Procedures and Functions](#fetching-stored-procedures-and-functions)
5. [Examples](#examples)
   - [Example 1: List All Tables in a Database](#example-1-list-all-tables-in-a-database)
   - [Example 2: Retrieve Columns for a Specific Table](#example-2-retrieve-columns-for-a-specific-table)
   - [Example 3: List All Primary Keys](#example-3-list-all-primary-keys)
   - [Example 4: Find All Views](#example-4-find-all-views)
   - [Example 5: List All Stored Procedures](#example-5-list-all-stored-procedures)
6. [INFORMATION_SCHEMA vs System Catalog Views](#information_schema-vs-system-catalog-views)
7. [Best Practices and Considerations](#best-practices-and-considerations)
   

## Introduction
`INFORMATION_SCHEMA` is a set of read-only views in SQL Server that provide metadata about database objects such as tables, columns, constraints, and more. These views are part of the SQL standard and offer a consistent way to access metadata across different database systems.

## Overview of INFORMATION_SCHEMA
The `INFORMATION_SCHEMA` views are a special schema provided by SQL Server that allows you to query metadata about the objects in your database. These views present a standardized interface to retrieve details about database structure, such as table names, column definitions, constraints, and more.

## Common INFORMATION_SCHEMA Views

### INFORMATION_SCHEMA.TABLES
The `INFORMATION_SCHEMA.TABLES` view provides information about all tables and views within the database.

**Columns:**
- `TABLE_CATALOG`: The database name.
- `TABLE_SCHEMA`: The schema name.
- `TABLE_NAME`: The name of the table or view.
- `TABLE_TYPE`: The type of object (e.g., `BASE TABLE` or `VIEW`).

**Example:**
```sql
SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE = 'BASE TABLE';
```

### INFORMATION_SCHEMA.COLUMNS
The `INFORMATION_SCHEMA.COLUMNS` view provides information about the columns in each table or view.

**Columns:**
- `TABLE_CATALOG`: The database name.
- `TABLE_SCHEMA`: The schema name.
- `TABLE_NAME`: The name of the table or view.
- `COLUMN_NAME`: The name of the column.
- `DATA_TYPE`: The data type of the column.
- `IS_NULLABLE`: Whether the column allows NULL values.

**Example:**
```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'Employees';
```

### INFORMATION_SCHEMA.CONSTRAINTS
The `INFORMATION_SCHEMA.CONSTRAINTS` view contains information about table constraints, such as primary keys, foreign keys, and unique constraints.

**Columns:**
- `CONSTRAINT_CATALOG`: The database name.
- `CONSTRAINT_SCHEMA`: The schema name.
- `CONSTRAINT_NAME`: The name of the constraint.
- `CONSTRAINT_TYPE`: The type of constraint (e.g., `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`).

**Example:**
```sql
SELECT * FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS WHERE TABLE_NAME = 'Orders';
```

### INFORMATION_SCHEMA.KEY_COLUMN_USAGE
This view provides information about which columns are used in constraints (e.g., primary keys, foreign keys).

**Columns:**
- `CONSTRAINT_CATALOG`: The database name.
- `CONSTRAINT_SCHEMA`: The schema name.
- `CONSTRAINT_NAME`: The name of the constraint.
- `TABLE_NAME`: The name of the table.
- `COLUMN_NAME`: The name of the column.

**Example:**
```sql
SELECT * FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE WHERE TABLE_NAME = 'Employees';
```

### INFORMATION_SCHEMA.VIEWS
The `INFORMATION_SCHEMA.VIEWS` view provides information about all views in the database.

**Columns:**
- `TABLE_CATALOG`: The database name.
- `TABLE_SCHEMA`: The schema name.
- `TABLE_NAME`: The name of the view.
- `CHECK_OPTION`: Indicates the level of check option (e.g., `NONE`).
- `IS_UPDATABLE`: Whether the view is updatable.

**Example:**
```sql
SELECT * FROM INFORMATION_SCHEMA.VIEWS;
```

### INFORMATION_SCHEMA.ROUTINES
This view contains information about stored procedures and functions in the database.

**Columns:**
- `ROUTINE_CATALOG`: The database name.
- `ROUTINE_SCHEMA`: The schema name.
- `ROUTINE_NAME`: The name of the routine (stored procedure or function).
- `ROUTINE_TYPE`: The type of routine (`PROCEDURE` or `FUNCTION`).
- `DATA_TYPE`: The return data type (for functions).

**Example:**
```sql
SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_TYPE = 'PROCEDURE';
```

## Using INFORMATION_SCHEMA in Queries

### Querying Table Metadata
You can query the `INFORMATION_SCHEMA.TABLES` view to get details about the tables in a specific database.

**Example:**
```sql
SELECT TABLE_NAME, TABLE_TYPE
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA = 'dbo';
```

### Retrieving Column Information
The `INFORMATION_SCHEMA.COLUMNS` view is useful for obtaining information about columns in a specific table, such as data types and whether they allow null values.

**Example:**
```sql
SELECT COLUMN_NAME, DATA_TYPE, IS_NULLABLE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'Employees';
```

### Working with Constraints
To retrieve information about constraints (e.g., primary keys, foreign keys) on a table, you can query the `INFORMATION_SCHEMA.TABLE_CONSTRAINTS` and `INFORMATION_SCHEMA.KEY_COLUMN_USAGE` views.

**Example:**
```sql
SELECT CONSTRAINT_NAME, CONSTRAINT_TYPE
FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS
WHERE TABLE_NAME = 'Orders';
```

### Listing Views
You can list all the views in a database using the `INFORMATION_SCHEMA.VIEWS` view.

**Example:**
```sql
SELECT TABLE_NAME
FROM INFORMATION_SCHEMA.VIEWS;
```

### Fetching Stored Procedures and Functions
Use the `INFORMATION_SCHEMA.ROUTINES` view to list all stored procedures and functions in a database.

**Example:**
```sql
SELECT ROUTINE_NAME, ROUTINE_TYPE
FROM INFORMATION_SCHEMA.ROUTINES;
```

## Examples

### Example 1: List All Tables in a Database
This query retrieves all tables in the current database.

```sql
SELECT TABLE_NAME
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_TYPE = 'BASE TABLE';
```

### Example 2: Retrieve Columns for a Specific Table
This query lists all columns for the `Employees` table along with their data types and nullability.

```sql
SELECT COLUMN_NAME, DATA_TYPE, IS_NULLABLE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'Employees';
```

### Example 3: List All Primary Keys
This query lists all primary keys in the database.

```sql
SELECT CONSTRAINT_NAME, TABLE_NAME
FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS
WHERE CONSTRAINT_TYPE = 'PRIMARY KEY';
```

### Example 4: Find All Views
This query retrieves the names of all views in the database.

```sql
SELECT TABLE_NAME
FROM INFORMATION_SCHEMA.VIEWS;
```

### Example 5: List All Stored Procedures
This query lists all stored procedures in the database.

```sql
SELECT ROUTINE_NAME
FROM INFORMATION_SCHEMA.ROUTINES
WHERE ROUTINE_TYPE = 'PROCEDURE';
```

## INFORMATION_SCHEMA vs System Catalog Views
- **INFORMATION_SCHEMA Views:**
  - Part of the SQL standard.
  - Portable across different database systems.
  - Limited in scope and does not cover all SQL Server features.

- **System Catalog Views:**
  - SQL Server-specific.
  - Provide more detailed and comprehensive metadata.
  - Include additional features like indexes, triggers, and permissions.

**Example of a System Catalog View:**
```sql
SELECT name, object_id
FROM sys.objects
WHERE type = 'U';  -- 'U' stands for user-defined tables
```

## Best Practices and Considerations
- Use `INFORMATION_SCHEMA` views for cross-database compatibility and when you need standard-compliant metadata access.
- For more detailed and SQL Server-specific metadata, use the system catalog views like `sys.objects`, `sys.columns`, and `sys.indexes`.
- Be aware that `INFORMATION_SCHEMA` views may not expose all features or the latest SQL Server enhancements.
