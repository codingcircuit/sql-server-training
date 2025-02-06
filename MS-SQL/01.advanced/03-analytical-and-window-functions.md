# SQL Server Analytical and Window Functions

## Introduction
Analytical and window functions in SQL Server are used to perform calculations across a set of rows related to the current row. Unlike aggregate functions, which return a single value for a group of rows, window functions return a value for each row in the query result. These functions are powerful for tasks like ranking, running totals, and moving averages.

## Index of Analytical and Window Functions

1. [Types of Analytical and Window Functions](#types-of-analytical-and-window-functions)
   - [ROW_NUMBER()](#row_number)
   - [RANK()](#rank)
   - [DENSE_RANK()](#dense_rank)
   - [NTILE()](#ntile)
   - [LAG()](#lag)
   - [LEAD()](#lead)
   - [FIRST_VALUE()](#first_value)
   - [LAST_VALUE()](#last_value)
2. [Use Cases](#use-cases)
   - [Example 1: Using ROW_NUMBER() for Row Numbering](#example-1-using-row_number-for-row-numbering)
   - [Example 2: Using RANK() for Ranking with Gaps](#example-2-using-rank-for-ranking-with-gaps)
   - [Example 3: Using DENSE_RANK() for Continuous Ranking](#example-3-using-dense_rank-for-continuous-ranking)
   - [Example 4: Using NTILE() for Dividing Data into Buckets](#example-4-using-ntile-for-dividing-data-into-buckets)
   - [Example 5: Using LAG() for Accessing Previous Rows](#example-5-using-lag-for-accessing-previous-rows)
   - [Example 6: Using LEAD() for Accessing Subsequent Rows](#example-6-using-lead-for-accessing-subsequent-rows)
   - [Example 7: Using FIRST_VALUE() to Retrieve First Value in a Window](#example-7-using-first_value-to-retrieve-first-value-in-a-window)
   - [Example 8: Using LAST_VALUE() to Retrieve Last Value in a Window](#example-8-using-last_value-to-retrieve-last-value-in-a-window)

## Types of Analytical and Window Functions

### ROW_NUMBER()
- **Description:** Assigns a unique sequential integer to rows within a partition of a result set, starting at 1 for the first row in each partition.
- **Syntax:**
    ```sql
    SELECT column_name, 
           ROW_NUMBER() OVER(PARTITION BY partition_column ORDER BY sort_column) AS RowNum
    FROM table_name;
    ```
- **Example:**
    ```sql
    SELECT Name, DepartmentID,
           ROW_NUMBER() OVER(PARTITION BY DepartmentID ORDER BY Salary DESC) AS RowNum
    FROM Employees;
    ```
- **Result:** Returns employees with a row number assigned within each department, ordered by salary.

### RANK()
- **Description:** Assigns a rank to each row within a partition of a result set, with gaps in ranking if there are ties.
- **Syntax:**
    ```sql
    SELECT column_name, 
           RANK() OVER(PARTITION BY partition_column ORDER BY sort_column) AS Rank
    FROM table_name;
    ```
- **Example:**
    ```sql
    SELECT Name, DepartmentID,
           RANK() OVER(PARTITION BY DepartmentID ORDER BY Salary DESC) AS Rank
    FROM Employees;
    ```
- **Result:** Returns employees ranked by salary within each department, with gaps if there are ties.

### DENSE_RANK()
- **Description:** Similar to RANK(), but without gaps in ranking values.
- **Syntax:**
    ```sql
    SELECT column_name, 
           DENSE_RANK() OVER(PARTITION BY partition_column ORDER BY sort_column) AS DenseRank
    FROM table_name;
    ```
- **Example:**
    ```sql
    SELECT Name, DepartmentID,
           DENSE_RANK() OVER(PARTITION BY DepartmentID ORDER BY Salary DESC) AS DenseRank
    FROM Employees;
    ```
- **Result:** Returns employees ranked by salary within each department, without gaps in ranking.

### NTILE()
- **Description:** Divides the result set into a specified number of roughly equal parts and assigns a number to each row indicating which part it belongs to.
- **Syntax:**
    ```sql
    SELECT column_name, 
           NTILE(num_buckets) OVER(PARTITION BY partition_column ORDER BY sort_column) AS NTile
    FROM table_name;
    ```
- **Example:**
    ```sql
    SELECT Name, DepartmentID,
           NTILE(4) OVER(PARTITION BY DepartmentID ORDER BY Salary DESC) AS Quartile
    FROM Employees;
    ```
- **Result:** Divides employees in each department into four quartiles based on salary.

### LAG()
- **Description:** Provides access to a value from a previous row in the result set without using a self-join.
- **Syntax:**
    ```sql
    SELECT column_name, 
           LAG(column_name, offset, default_value) OVER(PARTITION BY partition_column ORDER BY sort_column) AS PreviousValue
    FROM table_name;
    ```
- **Example:**
    ```sql
    SELECT Name, Salary,
           LAG(Salary, 1, 0) OVER(ORDER BY Salary DESC) AS PreviousSalary
    FROM Employees;
    ```
- **Result:** Returns the salary of the previous row based on the descending salary order.

### LEAD()
- **Description:** Provides access to a value from a subsequent row in the result set without using a self-join.
- **Syntax:**
    ```sql
    SELECT column_name, 
           LEAD(column_name, offset, default_value) OVER(PARTITION BY partition_column ORDER BY sort_column) AS NextValue
    FROM table_name;
    ```
- **Example:**
    ```sql
    SELECT Name, Salary,
           LEAD(Salary, 1, 0) OVER(ORDER BY Salary DESC) AS NextSalary
    FROM Employees;
    ```
- **Result:** Returns the salary of the next row based on the descending salary order.

### FIRST_VALUE()
- **Description:** Returns the first value in an ordered set of values.
- **Syntax:**
    ```sql
    SELECT column_name, 
           FIRST_VALUE(column_name) OVER(PARTITION BY partition_column ORDER BY sort_column) AS FirstValue
    FROM table_name;
    ```
- **Example:**
    ```sql
    SELECT Name, Salary,
           FIRST_VALUE(Salary) OVER(ORDER BY Salary DESC) AS HighestSalary
    FROM Employees;
    ```
- **Result:** Returns the highest salary in the result set.

### LAST_VALUE()
- **Description:** Returns the last value in an ordered set of values.
- **Syntax:**
    ```sql
    SELECT column_name, 
           LAST_VALUE(column_name) OVER(PARTITION BY partition_column ORDER BY sort_column) AS LastValue
    FROM table_name;
    ```
- **Example:**
    ```sql
    SELECT Name, Salary,
           LAST_VALUE(Salary) OVER(ORDER BY Salary DESC ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS LowestSalary
    FROM Employees;
    ```
- **Result:** Returns the lowest salary in the result set.

## Use Cases

### Example 1: Using ROW_NUMBER() for Row Numbering
- **Scenario:** You need to assign a unique row number to each employee within their department.
- **Solution:**
    ```sql
    SELECT Name, DepartmentID,
           ROW_NUMBER() OVER(PARTITION BY DepartmentID ORDER BY Salary DESC) AS RowNum
    FROM Employees;
    ```

### Example 2: Using RANK() for Ranking with Gaps
- **Scenario:** You want to rank employees by salary within each department, with ties having the same rank.
- **Solution:**
    ```sql
    SELECT Name, DepartmentID,
           RANK() OVER(PARTITION BY DepartmentID ORDER BY Salary DESC) AS Rank
    FROM Employees;
    ```

### Example 3: Using DENSE_RANK() for Continuous Ranking
- **Scenario:** You want to rank employees by salary within each department without gaps in the rank numbers.
- **Solution:**
    ```sql
    SELECT Name, DepartmentID,
           DENSE_RANK() OVER(PARTITION BY DepartmentID ORDER BY Salary DESC) AS DenseRank
    FROM Employees;
    ```

### Example 4: Using NTILE() for Dividing Data into Buckets
- **Scenario:** You need to divide employees into four salary quartiles within each department.
- **Solution:**
    ```sql
    SELECT Name, DepartmentID,
           NTILE(4) OVER(PARTITION BY DepartmentID ORDER BY Salary DESC) AS Quartile
    FROM Employees;
    ```

### Example 5: Using LAG() for Accessing Previous Rows
- **Scenario:** You want to compare each employee's salary to the previous employee's salary in the result set.
- **Solution:**
    ```sql
    SELECT Name, Salary,
           LAG(Salary, 1, 0) OVER(ORDER BY Salary DESC) AS PreviousSalary
    FROM Employees;
    ```

### Example 6: Using LEAD() for Accessing Subsequent Rows
- **Scenario:** You want to compare each employee's salary to the next employee's salary in the result set.
- **Solution:**
    ```sql
    SELECT Name, Salary,
           LEAD(Salary, 1, 0)

 OVER(ORDER BY Salary DESC) AS NextSalary
    FROM Employees;
    ```

### Example 7: Using FIRST_VALUE() to Retrieve First Value in a Window
- **Scenario:** You want to retrieve the highest salary in the result set for each employee.
- **Solution:**
    ```sql
    SELECT Name, Salary,
           FIRST_VALUE(Salary) OVER(ORDER BY Salary DESC) AS HighestSalary
    FROM Employees;
    ```

### Example 8: Using LAST_VALUE() to Retrieve Last Value in a Window
- **Scenario:** You want to retrieve the lowest salary in the result set for each employee.
- **Solution:**
    ```sql
    SELECT Name, Salary,
           LAST_VALUE(Salary) OVER(ORDER BY Salary DESC ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS LowestSalary
    FROM Employees;
    ```
