# SQL Server Data Types and Constraints

## Table of Contents

1. [SQL Server Data Types](#sql-server-data-types)
   - [Numeric Data Types](#numeric-data-types)
   - [Character Data Types](#character-data-types)
   - [Date and Time Data Types](#date-and-time-data-types)
   - [Binary Data Types](#binary-data-types)
   - [Other Data Types](#other-data-types)
   - [Examples of Data Types](#examples-of-data-types)
2. [SQL Server Constraints](#sql-server-constraints)
   - [Primary Key Constraint](#primary-key-constraint)
   - [Foreign Key Constraint](#foreign-key-constraint)
   - [Unique Constraint](#unique-constraint)
   - [Check Constraint](#check-constraint)
   - [Default Constraint](#default-constraint)
   - [Not Null Constraint](#not-null-constraint)
   - [Examples of Constraints](#examples-of-constraints)
3. [Use Cases and Examples](#use-cases-and-examples)
   - [Use Case 1: Enforcing Data Integrity](#use-case-1-enforcing-data-integrity)
   - [Use Case 2: Managing Relationships Between Tables](#use-case-2-managing-relationships-between-tables)
   - [Use Case 3: Ensuring Unique Usernames](#use-case-3-ensuring-unique-usernames)
   - [Use Case 4: Providing Default Values](#use-case-4-providing-default-values)
   - [Use Case 5: Preventing Null Values](#use-case-5-preventing-null-values)

---

## SQL Server Data Types

SQL Server supports a variety of data types to store different kinds of data. Understanding these data types is essential for designing databases efficiently.

### Numeric Data Types

- **`int`**: Integer data from -2,147,483,648 to 2,147,483,647.
- **`decimal(p, s)`**: Fixed precision and scale numbers.
- **`float(n)`**: Floating precision numbers.

### Character Data Types

- **`char(n)`**: Fixed-length non-Unicode character data.
- **`varchar(n)`**: Variable-length non-Unicode character data.
- **`text`**: Variable-length non-Unicode data with a maximum length of 2^31-1 characters.

### Date and Time Data Types

- **`date`**: Stores date data only.
- **`datetime`**: Stores date and time data.
- **`time`**: Stores time data only.

### Binary Data Types

- **`binary(n)`**: Fixed-length binary data.
- **`varbinary(n)`**: Variable-length binary data.

### Other Data Types

- **`bit`**: Integer data with either a 0 or 1 value.
- **`uniqueidentifier`**: A globally unique identifier (GUID).

### Examples of Data Types

```sql
-- Numeric Data Type Example
DECLARE @Quantity int = 100;
DECLARE @Price decimal(10, 2) = 19.99;

-- Character Data Type Example
DECLARE @FirstName varchar(50) = 'John';

-- Date and Time Data Type Example
DECLARE @OrderDate datetime = GETDATE();

-- Binary Data Type Example
DECLARE @Image varbinary(max) = 0xFFD8FFE000104A464946000101;

-- Uniqueidentifier Data Type Example
DECLARE @ID uniqueidentifier = NEWID();
```

---

## SQL Server Constraints

Constraints are rules enforced on data columns to maintain the integrity and accuracy of the data in the database.

### Primary Key Constraint

Uniquely identifies each record in a table.

- Example: `PRIMARY KEY (EmployeeID)`

### Foreign Key Constraint

Ensures referential integrity by linking two tables.

- Example: `FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)`

### Unique Constraint

Ensures all values in a column are unique.

- Example: `UNIQUE (Email)`

### Check Constraint

Ensures the value in a column meets a specific condition.

- Example: `CHECK (Salary > 0)`

### Default Constraint

Sets a default value for a column when no value is specified.

- Example: `DEFAULT GETDATE()`

### Not Null Constraint

Ensures that a column cannot have a NULL value.

- Example: `NOT NULL`

### Examples of Constraints

```sql
-- Primary Key Constraint Example
CREATE TABLE Employees (
    EmployeeID int PRIMARY KEY,
    FirstName varchar(50),
    LastName varchar(50)
);

-- Foreign Key Constraint Example
CREATE TABLE Orders (
    OrderID int PRIMARY KEY,
    EmployeeID int FOREIGN KEY REFERENCES Employees(EmployeeID)
);

-- Unique Constraint Example
CREATE TABLE Users (
    UserID int PRIMARY KEY,
    Email varchar(100) UNIQUE
);

-- Check Constraint Example
CREATE TABLE Products (
    ProductID int PRIMARY KEY,
    Price decimal(10, 2) CHECK (Price > 0)
);

-- Default Constraint Example
CREATE TABLE Customers (
    CustomerID int PRIMARY KEY,
    SignupDate datetime DEFAULT GETDATE()
);

-- Not Null Constraint Example
CREATE TABLE Invoices (
    InvoiceID int PRIMARY KEY,
    TotalAmount decimal(10, 2) NOT NULL
);
```

---

## Use Cases and Examples

In this section, we explore real-world scenarios where data types and constraints are used in SQL Server, with examples.

### Use Case 1: Enforcing Data Integrity

#### Example: Using Check Constraint

```sql
CREATE TABLE Employees (
    EmployeeID int PRIMARY KEY,
    Age int CHECK (Age >= 18 AND Age <= 65)
);
```

### Use Case 2: Managing Relationships Between Tables

#### Example: Using Foreign Key Constraint

```sql
CREATE TABLE Departments (
    DepartmentID int PRIMARY KEY,
    DepartmentName varchar(50)
);

CREATE TABLE Employees (
    EmployeeID int PRIMARY KEY,
    DepartmentID int,
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);
```

### Use Case 3: Ensuring Unique Usernames

#### Example: Using Unique Constraint

```sql
CREATE TABLE Users (
    UserID int PRIMARY KEY,
    Username varchar(50) UNIQUE
);
```

### Use Case 4: Providing Default Values

#### Example: Using Default Constraint

```sql
CREATE TABLE Orders (
    OrderID int PRIMARY KEY,
    OrderDate datetime DEFAULT GETDATE()
);
```

### Use Case 5: Preventing Null Values

#### Example: Using Not Null Constraint

```sql
CREATE TABLE Products (
    ProductID int PRIMARY KEY,
    ProductName varchar(100) NOT NULL,
    Price decimal(10, 2) NOT NULL
);
```
