# SQL Server Conditional Statements

## Introduction
Conditional statements in SQL Server allow you to implement logic within your queries or stored procedures. These statements enable you to execute different actions based on conditions, making your SQL scripts more dynamic and adaptable to various scenarios.

## Index of Conditional Statements

1. [Types of Conditional Statements](#types-of-conditional-statements)
   - [CASE Statement](#case-statement)
   - [IIF() Function](#iif-function)
2. [Use Cases](#use-cases)
   - [Example 1: Using CASE for Conditional Logic](#example-1-using-case-for-conditional-logic)
   - [Example 2: Using IIF() for Simple Conditions](#example-2-using-iif-for-simple-conditions)

## Types of Conditional Statements

### CASE Statement
- **Description:** The `CASE` statement evaluates a list of conditions and returns one of multiple possible result expressions.
- **Syntax:**
    ```sql
    CASE
        WHEN condition1 THEN result1
        WHEN condition2 THEN result2
        ...
        ELSE resultN
    END
    ```
- **Example:**
    ```sql
    SELECT Name, Salary,
           CASE 
               WHEN Salary > 50000 THEN 'High'
               WHEN Salary BETWEEN 30000 AND 50000 THEN 'Medium'
               ELSE 'Low'
           END AS SalaryRange
    FROM Employees;
    ```
- **Result:** Categorizes employees into 'High', 'Medium', or 'Low' salary ranges.


### IIF() Function
- **Description:** The `IIF()` function is a shorthand way to write simple conditional logic in SQL Server. It evaluates a condition and returns one of two values.
- **Syntax:**
    ```sql
    IIF(condition, true_value, false_value)
    ```
- **Example:**
    ```sql
    SELECT Name, Salary,
           IIF(Salary > 50000, 'High', 'Low') AS SalaryRange
    FROM Employees;
    ```
- **Result:** Labels employees as 'High' or 'Low' based on their salary.

## Use Cases

### Example 1: Using CASE for Conditional Logic
- **Scenario:** You need to categorize employees based on their salary into 'High', 'Medium', or 'Low'.
- **Solution:**
    ```sql
    SELECT Name, Salary,
           CASE 
               WHEN Salary > 50000 THEN 'High'
               WHEN Salary BETWEEN 30000 AND 50000 THEN 'Medium'
               ELSE 'Low'
           END AS SalaryRange
    FROM Employees;
    ```


### Example 2: Using IIF() for Simple Conditions
- **Scenario:** You need a quick way to label employees' salaries as 'High' or 'Low'.
- **Solution:**
    ```sql
    SELECT Name, Salary,
           IIF(Salary > 50000, 'High', 'Low') AS SalaryRange
    FROM Employees;
    ```

