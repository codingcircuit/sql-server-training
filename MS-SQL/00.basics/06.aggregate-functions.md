# SQL Server Aggregate Functions

## Introduction
SQL Server aggregate functions perform a calculation on a set of values and return a single value. These functions are often used with the `GROUP BY` clause in SQL queries to summarize data. Additionally, SQL Server provides advanced features like `HAVING`, `VAR`, `STDEV`, `CUBE`, `ROLLUP`, `GROUPING`, and `GROUPING SETS` for more complex data analysis.

---

## Index of Aggregate Functions

1. [Aggregate Functions](#aggregate-functions)
   - [COUNT()](#count)
   - [SUM()](#sum)
   - [AVG()](#avg)
   - [MIN()](#min)
   - [MAX()](#max)
   - [VAR()](#var)
   - [STDEV()](#stdev)
   - [GROUPING()](#grouping)
   - [GROUPING SETS](#grouping-sets)
   - [CUBE](#cube)
   - [ROLLUP](#rollup)
2. [Use Cases](#use-cases)
   - [Example 1: Counting Rows](#example-1-counting-rows)
   - [Example 2: Summing Values](#example-2-summing-values)
   - [Example 3: Finding Averages](#example-3-finding-averages)
   - [Example 4: Finding Minimum and Maximum Values](#example-4-finding-minimum-and-maximum-values)
   - [Example 5: Using HAVING Clause](#example-5-using-having-clause)
   - [Example 6: Using VAR()](#example-6-using-var)
   - [Example 7: Using STDEV()](#example-7-using-stdev)
   - [Example 8: Using GROUPING()](#example-8-using-grouping)
   - [Example 9: Using GROUPING SETS](#example-9-using-grouping-sets)
   - [Example 10: Using CUBE](#example-10-using-cube)
   - [Example 11: Using ROLLUP](#example-11-using-rollup)

---

## Aggregate Functions

### COUNT()
- **Description:** Returns the number of rows that match a specified condition.
- **Syntax:** `COUNT(expression)`
- **Example:**
    ```sql
    SELECT COUNT(*) AS TotalRows FROM Employees;
    ```
- **Sample Input:** Employees Table
    | EmployeeID | Name       | Department  | JobTitle       | Salary |
    |------------|------------|-------------|----------------|--------|
    | 1          | John Doe   | HR          | Manager        | 70000  |
    | 2          | Jane Smith | HR          | Assistant      | 50000  |
    | 3          | Alice Brown| IT          | Developer      | 60000  |
    | 4          | Bob Johnson| IT          | Developer      | 65000  |
    | 5          | Charlie Lee| Finance     | Analyst        | 55000  |
    | 6          | David Green| Finance     | Manager        | 75000  |
    | 7          | Eve White  | HR          | Assistant      | 48000  |
    | 8          | Frank Black| IT          | Manager        | 80000  |
- **Sample Output:**
    | TotalRows |
    |-----------|
    | 8         |

---

### SUM()
- **Description:** Returns the total sum of a numeric column.
- **Syntax:** `SUM(expression)`
- **Example:**
    ```sql
    SELECT SUM(Salary) AS TotalSalary FROM Employees;
    ```
- **Sample Output:**
    | TotalSalary |
    |-------------|
    | 513000      |

---

### AVG()
- **Description:** Returns the average value of a numeric column.
- **Syntax:** `AVG(expression)`
- **Example:**
    ```sql
    SELECT AVG(Salary) AS AverageSalary FROM Employees;
    ```
- **Sample Output:**
    | AverageSalary |
    |---------------|
    | 64125         |

---

### MIN()
- **Description:** Returns the smallest value in a set of values.
- **Syntax:** `MIN(expression)`
- **Example:**
    ```sql
    SELECT MIN(Salary) AS MinimumSalary FROM Employees;
    ```
- **Sample Output:**
    | MinimumSalary |
    |---------------|
    | 48000         |

---

### MAX()
- **Description:** Returns the largest value in a set of values.
- **Syntax:** `MAX(expression)`
- **Example:**
    ```sql
    SELECT MAX(Salary) AS MaximumSalary FROM Employees;
    ```
- **Sample Output:**
    | MaximumSalary |
    |---------------|
    | 80000         |

---

### VAR()
- **Description:** Returns the statistical variance of all values in a numeric column.
- **Syntax:** `VAR(expression)`
- **Example:**
    ```sql
    SELECT VAR(Salary) AS SalaryVariance FROM Employees;
    ```
- **Sample Output:**
    | SalaryVariance |
    |----------------|
    | 12250000       |

---

### STDEV()
- **Description:** Returns the statistical standard deviation of all values in a numeric column.
- **Syntax:** `STDEV(expression)`
- **Example:**
    ```sql
    SELECT STDEV(Salary) AS SalaryStandardDeviation FROM Employees;
    ```
- **Sample Output:**
    | SalaryStandardDeviation |
    |-------------------------|
    | 3500                    |

---

### GROUPING()
- **Description:** Used with `CUBE` or `ROLLUP` to identify summary rows.
- **Syntax:** `GROUPING(column_name)`
- **Example:**
    ```sql
    SELECT Department, JobTitle, SUM(Salary) AS TotalSalary,
           GROUPING(Department) AS IsDepartmentGrouped,
           GROUPING(JobTitle) AS IsJobTitleGrouped
    FROM Employees
    GROUP BY CUBE (Department, JobTitle);
    ```
- **Sample Output:**
    | Department | JobTitle   | TotalSalary | IsDepartmentGrouped | IsJobTitleGrouped |
    |------------|------------|-------------|----------------------|-------------------|
    | HR         | Manager    | 70000       | 0                    | 0                 |
    | HR         | Assistant  | 98000       | 0                    | 0                 |
    | IT         | Developer  | 125000      | 0                    | 0                 |
    | IT         | Manager    | 80000       | 0                    | 0                 |
    | Finance    | Analyst    | 55000       | 0                    | 0                 |
    | Finance    | Manager    | 75000       | 0                    | 0                 |
    | HR         | NULL       | 168000      | 0                    | 1                 |
    | IT         | NULL       | 205000      | 0                    | 1                 |
    | Finance    | NULL       | 130000      | 0                    | 1                 |
    | NULL       | NULL       | 513000      | 1                    | 1                 |

---

### GROUPING SETS
- **Description:** Allows multiple grouping sets in a single query.
- **Syntax:** `GROUP BY GROUPING SETS ((column1, column2), (column1), ())`
- **Example:**
    ```sql
    SELECT Department, JobTitle, SUM(Salary) AS TotalSalary
    FROM Employees
    GROUP BY GROUPING SETS ((Department, JobTitle), (Department), ());
    ```
- **Sample Output:**
    | Department | JobTitle   | TotalSalary |
    |------------|------------|-------------|
    | HR         | Manager    | 70000       |
    | HR         | Assistant  | 98000       |
    | IT         | Developer  | 125000      |
    | IT         | Manager    | 80000       |
    | Finance    | Analyst    | 55000       |
    | Finance    | Manager    | 75000       |
    | HR         | NULL       | 168000      |
    | IT         | NULL       | 205000      |
    | Finance    | NULL       | 130000      |
    | NULL       | NULL       | 513000      |

---

### CUBE
- **Description:** Generates all possible grouping combinations.
- **Syntax:** `GROUP BY CUBE (column1, column2)`
- **Example:**
    ```sql
    SELECT Department, JobTitle, SUM(Salary) AS TotalSalary
    FROM Employees
    GROUP BY CUBE (Department, JobTitle);
    ```
- **Sample Output:** Same as `GROUPING SETS` example above.

---

### ROLLUP
- **Description:** Generates subtotals and a grand total.
- **Syntax:** `GROUP BY ROLLUP (column1, column2)`
- **Example:**
    ```sql
    SELECT Department, JobTitle, SUM(Salary) AS TotalSalary
    FROM Employees
    GROUP BY ROLLUP (Department, JobTitle);
    ```
- **Sample Output:**
    | Department | JobTitle   | TotalSalary |
    |------------|------------|-------------|
    | HR         | Manager    | 70000       |
    | HR         | Assistant  | 98000       |
    | HR         | NULL       | 168000      |
    | IT         | Developer  | 125000      |
    | IT         | Manager    | 80000       |
    | IT         | NULL       | 205000      |
    | Finance    | Analyst    | 55000       |
    | Finance    | Manager    | 75000       |
    | Finance    | NULL       | 130000      |
    | NULL       | NULL       | 513000      |

---

## Use Cases

### Example 1: Counting Rows
- **Scenario:** You want to know the total number of employees in the company.
- **Solution:**
    ```sql
    SELECT COUNT(*) AS TotalEmployees FROM Employees;
    ```
- **Sample Output:**
    | TotalEmployees |
    |----------------|
    | 8              |

---

### Example 2: Summing Values
- **Scenario:** You need to calculate the total budget spent on salaries.
- **Solution:**
    ```sql
    SELECT SUM(Salary) AS TotalSalaries FROM Employees;
    ```
- **Sample Output:**
    | TotalSalaries |
    |---------------|
    | 513000        |

---

### Example 3: Finding Averages
- **Scenario:** You want to find the average salary of employees in the company.
- **Solution:**
    ```sql
    SELECT AVG(Salary) AS AverageSalary FROM Employees;
    ```
- **Sample Output:**
    | AverageSalary |
    |---------------|
    | 64125         |

---

### Example 4: Finding Minimum and Maximum Values
- **Scenario:** You need to find the lowest and highest salaries in the company.
- **Solution:**
    ```sql
    SELECT MIN(Salary) AS MinimumSalary, MAX(Salary) AS MaximumSalary FROM Employees;
    ```
- **Sample Output:**
    | MinimumSalary | MaximumSalary |
    |---------------|---------------|
    | 48000         | 80000         |

---

### Example 5: Using HAVING Clause
- **Scenario:** You want to find departments where the average salary is greater than $50,000.
- **Solution:**
    ```sql
    SELECT Department, AVG(Salary) AS AverageSalary
    FROM Employees
    GROUP BY Department
    HAVING AVG(Salary) > 50000;
    ```
- **Sample Output:**
    | Department | AverageSalary |
    |------------|---------------|
    | HR         | 56000         |
    | IT         | 68333         |
    | Finance    | 65000         |

---

### Example 6: Using VAR()
- **Scenario:** You want to find the variance in salaries within each department.
- **Solution:**
    ```sql
    SELECT Department, VAR(Salary) AS SalaryVariance
    FROM Employees
    GROUP BY Department;
    ```
- **Sample Output:**
    | Department | SalaryVariance |
    |------------|----------------|
    | HR         | 12250000       |
    | IT         | 8333333        |
    | Finance    | 10000000       |

---

### Example 7: Using STDEV()
- **Scenario:** You want to find the standard deviation of salaries within each department.
- **Solution:**
    ```sql
    SELECT Department, STDEV(Salary) AS SalaryStandardDeviation
    FROM Employees
    GROUP BY Department;
    ```
- **Sample Output:**
    | Department | SalaryStandardDeviation |
    |------------|-------------------------|
    | HR         | 3500                    |
    | IT         | 2886                    |
    | Finance    | 3162                    |

---

### Example 8: Using GROUPING()
- **Scenario:** You want to analyze the total salary by department and job title using `CUBE`, and identify summary rows.
- **Solution:**
    ```sql
    SELECT Department, JobTitle, SUM(Salary) AS TotalSalary,
           GROUPING(Department) AS IsDepartmentGrouped,
           GROUPING(JobTitle) AS IsJobTitleGrouped
    FROM Employees
    GROUP BY CUBE (Department, JobTitle);
    ```
- **Sample Output:**
    | Department | JobTitle   | TotalSalary | IsDepartmentGrouped | IsJobTitleGrouped |
    |------------|------------|-------------|----------------------|-------------------|
    | HR         | Manager    | 70000       | 0                    | 0                 |
    | HR         | Assistant  | 98000       | 0                    | 0                 |
    | IT         | Developer  | 125000      | 0                    | 0                 |
    | IT         | Manager    | 80000       | 0                    | 0                 |
    | Finance    | Analyst    | 55000       | 0                    | 0                 |
    | Finance    | Manager    | 75000       | 0                    | 0                 |
    | HR         | NULL       | 168000      | 0                    | 1                 |
    | IT         | NULL       | 205000      | 0                    | 1                 |
    | Finance    | NULL       | 130000      | 0                    | 1                 |
    | NULL       | NULL       | 513000      | 1                    | 1                 |

---

### Example 9: Using GROUPING SETS
- **Scenario:** You want to analyze the total salary by department and job title, by department alone, and also get a grand total in a single query.
- **Solution:**
    ```sql
    SELECT Department, JobTitle, SUM(Salary) AS TotalSalary
    FROM Employees
    GROUP BY GROUPING SETS ((Department, JobTitle), (Department), ());
    ```
- **Sample Output:**
    | Department | JobTitle   | TotalSalary |
    |------------|------------|-------------|
    | HR         | Manager    | 70000       |
    | HR         | Assistant  | 98000       |
    | IT         | Developer  | 125000      |
    | IT         | Manager    | 80000       |
    | Finance    | Analyst    | 55000       |
    | Finance    | Manager    | 75000       |
    | HR         | NULL       | 168000      |
    | IT         | NULL       | 205000      |
    | Finance    | NULL       | 130000      |
    | NULL       | NULL       | 513000      |

---

### Example 10: Using CUBE
- **Scenario:** You want to analyze the total salary by department and job title, including all possible subtotals and a grand total.
- **Solution:**
    ```sql
    SELECT Department, JobTitle, SUM(Salary) AS TotalSalary
    FROM Employees
    GROUP BY CUBE (Department, JobTitle);
    ```
- **Sample Output:** Same as `GROUPING SETS` example above.

---

### Example 11: Using ROLLUP
- **Scenario:** You want to analyze the total salary by department and job title, including subtotals for each department and a grand total.
- **Solution:**
    ```sql
    SELECT Department, JobTitle, SUM(Salary) AS TotalSalary
    FROM Employees
    GROUP BY ROLLUP (Department, JobTitle);
    ```
- **Sample Output:**
    | Department | JobTitle   | TotalSalary |
    |------------|------------|-------------|
    | HR         | Manager    | 70000       |
    | HR         | Assistant  | 98000       |
    | HR         | NULL       | 168000      |
    | IT         | Developer  | 125000      |
    | IT         | Manager    | 80000       |
    | IT         | NULL       | 205000      |
    | Finance    | Analyst    | 55000       |
    | Finance    | Manager    | 75000       |
    | Finance    | NULL       | 130000      |
    | NULL       | NULL       | 513000      |

---
