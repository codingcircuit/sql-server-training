# SQL Server Joins

## Introduction
SQL Server joins are used to combine records from two or more tables in a database based on a related column between them. Joins are a powerful tool for querying relational databases and can be used to retrieve data from multiple tables in a single query.

## Index of Joins

1. [Types of Joins](#types-of-joins)
   - [INNER JOIN](#inner-join)
   - [LEFT JOIN](#left-join)
   - [RIGHT JOIN](#right-join)
   - [FULL OUTER JOIN](#full-outer-join)
   - [CROSS JOIN](#cross-join)
   - [SELF JOIN](#self-join)
2. [Use Cases](#use-cases)
   - [Example 1: Combining Data from Two Tables](#example-1-combining-data-from-two-tables)
   - [Example 2: Finding Non-Matching Records](#example-2-finding-non-matching-records)
   - [Example 3: Cartesian Product with CROSS JOIN](#example-3-cartesian-product-with-cross-join)
   - [Example 4: Joining a Table to Itself](#example-4-joining-a-table-to-itself)

## Types of Joins

### INNER JOIN
- **Description:** Returns records that have matching values in both tables.
- **Syntax:** `SELECT columns FROM table1 INNER JOIN table2 ON table1.column = table2.column;`
- **Example:** 
    ```sql
    SELECT Employees.EmployeeID, Employees.Name, Departments.DepartmentName
    FROM Employees
    INNER JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
    ```
- **Result:** Returns employees along with their respective department names.

### LEFT JOIN
- **Description:** Returns all records from the left table and the matched records from the right table. If there is no match, NULL values are returned for columns from the right table.
- **Syntax:** `SELECT columns FROM table1 LEFT JOIN table2 ON table1.column = table2.column;`
- **Example:** 
    ```sql
    SELECT Employees.EmployeeID, Employees.Name, Departments.DepartmentName
    FROM Employees
    LEFT JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
    ```
- **Result:** Returns all employees, including those who are not assigned to any department (with NULL in the department name).

### RIGHT JOIN
- **Description:** Returns all records from the right table and the matched records from the left table. If there is no match, NULL values are returned for columns from the left table.
- **Syntax:** `SELECT columns FROM table1 RIGHT JOIN table2 ON table1.column = table2.column;`
- **Example:** 
    ```sql
    SELECT Employees.EmployeeID, Employees.Name, Departments.DepartmentName
    FROM Employees
    RIGHT JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
    ```
- **Result:** Returns all departments, including those that have no employees assigned to them (with NULL in the employee name).

### FULL OUTER JOIN
- **Description:** Returns all records when there is a match in either left or right table. If there is no match, NULL values are returned for columns from the table without a match.
- **Syntax:** `SELECT columns FROM table1 FULL OUTER JOIN table2 ON table1.column = table2.column;`
- **Example:** 
    ```sql
    SELECT Employees.EmployeeID, Employees.Name, Departments.DepartmentName
    FROM Employees
    FULL OUTER JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
    ```
- **Result:** Returns all employees and all departments, including those without a match in the other table.

### CROSS JOIN
- **Description:** Returns the Cartesian product of two tables, meaning all possible combinations of rows.
- **Syntax:** `SELECT columns FROM table1 CROSS JOIN table2;`
- **Example:** 
    ```sql
    SELECT Employees.Name, Departments.DepartmentName
    FROM Employees
    CROSS JOIN Departments;
    ```
- **Result:** Returns every possible pairing of an employee and a department.

### SELF JOIN
- **Description:** A self join is a regular join but the table is joined with itself.
- **Syntax:** `SELECT a.column, b.column FROM table a INNER JOIN table b ON a.column = b.column;`
- **Example:** 
    ```sql
    SELECT e1.Name AS Employee, e2.Name AS Manager
    FROM Employees e1
    INNER JOIN Employees e2 ON e1.ManagerID = e2.EmployeeID;
    ```
- **Result:** Returns a list of employees along with their respective managers.

## Use Cases

### Example 1: Combining Data from Two Tables
- **Scenario:** You want to retrieve a list of employees along with their department names.
- **Solution:**
    ```sql
    SELECT Employees.Name, Departments.DepartmentName
    FROM Employees
    INNER JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
    ```

### Example 2: Finding Non-Matching Records
- **Scenario:** You want to find employees who are not assigned to any department.
- **Solution:**
    ```sql
    SELECT Employees.Name
    FROM Employees
    LEFT JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID
    WHERE Departments.DepartmentID IS NULL;
    ```

### Example 3: Cartesian Product with CROSS JOIN
- **Scenario:** You need to generate all possible combinations of employees and departments for an internal report.
- **Solution:**
    ```sql
    SELECT Employees.Name, Departments.DepartmentName
    FROM Employees
    CROSS JOIN Departments;
    ```

### Example 4: Joining a Table to Itself
- **Scenario:** You want to list all employees along with their managers.
- **Solution:**
    ```sql
    SELECT e1.Name AS Employee, e2.Name AS Manager
    FROM Employees e1
    INNER JOIN Employees e2 ON e1.ManagerID = e2.EmployeeID;
    ```
