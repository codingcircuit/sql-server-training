# SQL Server Cardinality with Data

## Table of Contents
1. [Introduction to Cardinality](#1-introduction-to-cardinality)
2. [Types of Cardinality](#2-types-of-cardinality)
   - 2.1 [One-to-One (1:1)](#21-one-to-one-11)
   - 2.2 [One-to-Many (1:N)](#22-one-to-many-1n)
   - 2.3 [Many-to-Many (M:N)](#23-many-to-many-mn)
3. [Cardinality in SQL Server](#3-cardinality-in-sql-server)
4. [Cardinality Estimation](#4-cardinality-estimation)
   - 4.1 [How SQL Server Estimates Cardinality](#41-how-sql-server-estimates-cardinality)
   - 4.2 [Cardinality Estimator Versions](#42-cardinality-estimator-versions)
5. [Examples and Use Cases](#5-examples-and-use-cases)
   - 5.1 [One-to-One Relationship](#51-one-to-one-relationship)
   - 5.2 [One-to-Many Relationship](#52-one-to-many-relationship)
   - 5.3 [Many-to-Many Relationship](#53-many-to-many-relationship)
6. [Best Practices](#6-best-practices)
7. [Troubleshooting Cardinality Issues](#7-troubleshooting-cardinality-issues)
8. [Conclusion](#8-conclusion)
9. [References](#9-references)

---

## 1. Introduction to Cardinality
Cardinality refers to the uniqueness of data values contained in a column or a set of columns. It is an important concept in database design and query optimization, as it affects the performance and accuracy of queries. High cardinality means the data has a large number of unique values, while low cardinality means fewer unique values.

## 2. Types of Cardinality

### 2.1 One-to-One (1:1)
In a one-to-one relationship, each row in a table is linked to one and only one row in another table. This type of relationship is less common and is often used when you need to split a table for security or organizational reasons.

### 2.2 One-to-Many (1:N)
The most common relationship type, where a single row in one table can be related to multiple rows in another table. For example, a single customer can have multiple orders.

### 2.3 Many-to-Many (M:N)
In a many-to-many relationship, rows in one table can relate to multiple rows in another table, and vice versa. This relationship typically requires a junction table to resolve.

## 3. Cardinality in SQL Server
In SQL Server, cardinality plays a crucial role in query execution plans. The SQL Server query optimizer uses cardinality estimates to decide on the best execution plan for a query. Misestimations can lead to inefficient queries, resulting in slower performance.

## 4. Cardinality Estimation

### 4.1 How SQL Server Estimates Cardinality
SQL Server uses statistical data to estimate the cardinality of a query result. This estimation is based on the number of rows, distribution of data, and relationships between tables.

### 4.2 Cardinality Estimator Versions
SQL Server introduced a new cardinality estimator in SQL Server 2014. This new estimator improves query performance by better handling modern workloads, but it may cause regressions in some queries. It is important to test and compare the performance of your queries with both estimators.

## 5. Examples and Use Cases

### 5.1 One-to-One Relationship
**SQL Script:**
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name VARCHAR(100),
    Position VARCHAR(100)
);

CREATE TABLE EmployeeDetails (
    EmployeeID INT PRIMARY KEY,
    Address VARCHAR(255),
    Phone VARCHAR(20),
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);

-- Insert data into Employees table
INSERT INTO Employees (EmployeeID, Name, Position)
VALUES (1, 'John Doe', 'Manager'),
       (2, 'Jane Smith', 'Developer'),
       (3, 'Emily Davis', 'Analyst');

-- Insert data into EmployeeDetails table
INSERT INTO EmployeeDetails (EmployeeID, Address, Phone)
VALUES (1, '123 Main St', '555-1234'),
       (2, '456 Maple Ave', '555-5678'),
       (3, '789 Oak Dr', '555-9012');
```

**Use Case:** 
This relationship could be used to store sensitive information like employee details separately from general employee data.

### 5.2 One-to-Many Relationship
**SQL Script:**
```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100)
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    OrderDate DATE,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

-- Insert data into Customers table
INSERT INTO Customers (CustomerID, CustomerName)
VALUES (1, 'Acme Corp'),
       (2, 'Global Industries'),
       (3, 'Tech Solutions');

-- Insert data into Orders table
INSERT INTO Orders (OrderID, OrderDate, CustomerID)
VALUES (1, '2024-01-15', 1),
       (2, '2024-02-20', 1),
       (3, '2024-03-10', 2),
       (4, '2024-03-15', 3),
       (5, '2024-04-01', 3);
```

**Use Case:** 
This setup is ideal for scenarios like an e-commerce platform where a customer can place multiple orders.

### 5.3 Many-to-Many Relationship
**SQL Script:**
```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(100)
);

CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(100)
);

CREATE TABLE StudentCourses (
    StudentID INT,
    CourseID INT,
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);

-- Insert data into Students table
INSERT INTO Students (StudentID, StudentName)
VALUES (1, 'Alice Johnson'),
       (2, 'Bob Brown'),
       (3, 'Charlie Wilson');

-- Insert data into Courses table
INSERT INTO Courses (CourseID, CourseName)
VALUES (101, 'Mathematics'),
       (102, 'Physics'),
       (103, 'Chemistry');

-- Insert data into StudentCourses table
INSERT INTO StudentCourses (StudentID, CourseID)
VALUES (1, 101),
       (1, 102),
       (2, 101),
       (2, 103),
       (3, 102),
       (3, 103);
```

**Use Case:** 
Used in educational databases where students can enroll in multiple courses, and each course can have multiple students.

## 6. Best Practices
- Regularly update statistics to ensure accurate cardinality estimates.
- Use indexed views to improve query performance in cases of complex joins.
- Consider the impact of query hints on cardinality estimates.

## 7. Troubleshooting Cardinality Issues
- **Symptom:** Slow query performance.
  **Solution:** Check the execution plan to see if there are any cardinality estimation issues. Consider updating statistics or rewriting the query.
  
- **Symptom:** Incorrect results due to stale statistics.
  **Solution:** Use `UPDATE STATISTICS` or `sp_updatestats` to refresh the statistics.

## 8. Conclusion
Understanding cardinality is essential for database design and query optimization. Proper management of cardinality through statistics and indexing can significantly improve the performance of your SQL Server databases.

## 9. References
- [SQL Server Documentation](https://docs.microsoft.com/en-us/sql/sql-server/)
- [Cardinality Estimation in SQL Server](https://docs.microsoft.com/en-us/sql/relational-databases/sql-server-query-optimizer-cardinality-estimation)
