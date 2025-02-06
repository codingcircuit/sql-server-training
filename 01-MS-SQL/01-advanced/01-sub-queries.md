# SQL Server Subqueries

## Introduction
A subquery is a query nested inside another query. In SQL Server, subqueries are often used to perform operations that involve comparisons, filtering, and retrieving data based on specific criteria. Subqueries can return single values, multiple rows, or even tables.

## Index of Subqueries

1. [Types of Subqueries](#types-of-subqueries)
   - [Single-Row Subquery](#single-row-subquery)
   - [Multiple-Row Subquery](#multiple-row-subquery)
   - [Correlated Subquery](#correlated-subquery)
   - [Nested Subquery](#nested-subquery)
2. [Use Cases](#use-cases)
   - [Example 1: Using a Single-Row Subquery](#example-1-using-a-single-row-subquery)
   - [Example 2: Using a Multiple-Row Subquery](#example-2-using-a-multiple-row-subquery)
   - [Example 3: Using a Correlated Subquery](#example-3-using-a-correlated-subquery)
   - [Example 4: Using a Nested Subquery](#example-4-using-a-nested-subquery)

## Types of Subqueries

### Single-Row Subquery
- **Description:** A subquery that returns a single row and is typically used with comparison operators like `=`, `>`, `<`, etc.
- **Syntax:** 
    ```sql
    SELECT column_name 
    FROM table_name 
    WHERE column_name = (SELECT column_name FROM table_name WHERE condition);
    ```
- **Example:**
    ```sql
    SELECT Name, Salary 
    FROM Employees 
    WHERE Salary = (SELECT MAX(Salary) FROM Employees);
    ```
- **Result:** Returns the employee(s) with the highest salary.

### Multiple-Row Subquery
- **Description:** A subquery that returns multiple rows and is typically used with operators like `IN`, `ANY`, `ALL`, etc.
- **Syntax:** 
    ```sql
    SELECT column_name 
    FROM table_name 
    WHERE column_name IN (SELECT column_name FROM table_name WHERE condition);
    ```
- **Example:**
    ```sql
    SELECT Name 
    FROM Employees 
    WHERE DepartmentID IN (SELECT DepartmentID FROM Departments WHERE Location = 'New York');
    ```
- **Result:** Returns employees who work in departments located in New York.

### Correlated Subquery
- **Description:** A subquery that references columns from the outer query. It is executed once for each row selected by the outer query.
- **Syntax:** 
    ```sql
    SELECT column_name 
    FROM table_name outer_alias 
    WHERE column_name operator (SELECT column_name FROM table_name inner_alias WHERE outer_alias.column_name = inner_alias.column_name);
    ```
- **Example:**
    ```sql
    SELECT e1.Name, e1.Salary 
    FROM Employees e1 
    WHERE e1.Salary > (SELECT AVG(e2.Salary) FROM Employees e2 WHERE e2.DepartmentID = e1.DepartmentID);
    ```
- **Result:** Returns employees whose salary is above the average salary of their department.

### Nested Subquery
- **Description:** A subquery within another subquery, allowing for complex queries and data retrieval.
- **Syntax:** 
    ```sql
    SELECT column_name 
    FROM table_name 
    WHERE column_name = (SELECT column_name FROM table_name WHERE column_name = (SELECT column_name FROM table_name WHERE condition));
    ```
- **Example:**
    ```sql
    SELECT Name 
    FROM Employees 
    WHERE Salary > (SELECT AVG(Salary) 
                    FROM Employees 
                    WHERE DepartmentID = (SELECT DepartmentID 
                                          FROM Departments 
                                          WHERE DepartmentName = 'HR'));
    ```
- **Result:** Returns employees with salaries above the average salary in the HR department.

## Use Cases

### Example 1: Using a Single-Row Subquery
- **Scenario:** You want to find the employee with the highest salary.
- **Solution:**
    ```sql
    SELECT Name, Salary 
    FROM Employees 
    WHERE Salary = (SELECT MAX(Salary) FROM Employees);
    ```

### Example 2: Using a Multiple-Row Subquery
- **Scenario:** You need to find all employees working in a specific set of departments.
- **Solution:**
    ```sql
    SELECT Name 
    FROM Employees 
    WHERE DepartmentID IN (SELECT DepartmentID FROM Departments WHERE Location = 'New York');
    ```

### Example 3: Using a Correlated Subquery
- **Scenario:** You want to list employees who earn more than the average salary in their department.
- **Solution:**
    ```sql
    SELECT Name, Salary 
    FROM Employees e1 
    WHERE Salary > (SELECT AVG(Salary) FROM Employees e2 WHERE e2.DepartmentID = e1.DepartmentID);
    ```

### Example 4: Using a Nested Subquery
- **Scenario:** You need to find employees with salaries higher than the average salary of a specific department.
- **Solution:**
    ```sql
    SELECT Name 
    FROM Employees 
    WHERE Salary > (SELECT AVG(Salary) 
                    FROM Employees 
                    WHERE DepartmentID = (SELECT DepartmentID 
                                          FROM Departments 
                                          WHERE DepartmentName = 'HR'));
    ```
