# SQL Server Set Operators

## Introduction
Set operators in SQL Server are used to combine the results of two or more queries into a single result set. The primary set operators are `UNION`, `UNION ALL`, `INTERSECT`, and `EXCEPT`. These operators are used to perform various operations on the results of multiple SELECT statements.

## Index of Set Operators

1. [Types of Set Operators](#types-of-set-operators)
   - [UNION](#union)
   - [UNION ALL](#union-all)
   - [INTERSECT](#intersect)
   - [EXCEPT](#except)
2. [Use Cases](#use-cases)
   - [Example 1: Combining Results with UNION](#example-1-combining-results-with-union)
   - [Example 2: Including Duplicates with UNION ALL](#example-2-including-duplicates-with-union-all)
   - [Example 3: Finding Common Records with INTERSECT](#example-3-finding-common-records-with-intersect)
   - [Example 4: Finding Differences with EXCEPT](#example-4-finding-differences-with-except)

## Types of Set Operators

### UNION
- **Description:** Combines the result sets of two or more queries into a single result set and removes duplicates.
- **Syntax:**
    ```sql
    SELECT column_name(s) FROM table1
    UNION
    SELECT column_name(s) FROM table2;
    ```
- **Example:**
    ```sql
    SELECT Name FROM Employees
    UNION
    SELECT Name FROM Clients;
    ```
- **Result:** Returns a list of names that are either in the Employees or Clients table, with duplicates removed.

### UNION ALL
- **Description:** Combines the result sets of two or more queries into a single result set, including all duplicates.
- **Syntax:**
    ```sql
    SELECT column_name(s) FROM table1
    UNION ALL
    SELECT column_name(s) FROM table2;
    ```
- **Example:**
    ```sql
    SELECT Name FROM Employees
    UNION ALL
    SELECT Name FROM Clients;
    ```
- **Result:** Returns a list of names that are either in the Employees or Clients table, including any duplicates.

### INTERSECT
- **Description:** Returns only the records that are common to both result sets.
- **Syntax:**
    ```sql
    SELECT column_name(s) FROM table1
    INTERSECT
    SELECT column_name(s) FROM table2;
    ```
- **Example:**
    ```sql
    SELECT Name FROM Employees
    INTERSECT
    SELECT Name FROM Clients;
    ```
- **Result:** Returns a list of names that are present in both the Employees and Clients tables.

### EXCEPT
- **Description:** Returns the records from the first result set that do not appear in the second result set.
- **Syntax:**
    ```sql
    SELECT column_name(s) FROM table1
    EXCEPT
    SELECT column_name(s) FROM table2;
    ```
- **Example:**
    ```sql
    SELECT Name FROM Employees
    EXCEPT
    SELECT Name FROM Clients;
    ```
- **Result:** Returns a list of names that are in the Employees table but not in the Clients table.

## Use Cases

### Example 1: Combining Results with UNION
- **Scenario:** You need to create a list of all distinct names from two tables: Employees and Clients.
- **Solution:**
    ```sql
    SELECT Name FROM Employees
    UNION
    SELECT Name FROM Clients;
    ```

### Example 2: Including Duplicates with UNION ALL
- **Scenario:** You need to combine the names from Employees and Clients, including duplicates, to see all entries.
- **Solution:**
    ```sql
    SELECT Name FROM Employees
    UNION ALL
    SELECT Name FROM Clients;
    ```

### Example 3: Finding Common Records with INTERSECT
- **Scenario:** You want to find names that are listed as both Employees and Clients.
- **Solution:**
    ```sql
    SELECT Name FROM Employees
    INTERSECT
    SELECT Name FROM Clients;
    ```

### Example 4: Finding Differences with EXCEPT
- **Scenario:** You need to find names that are Employees but not Clients.
- **Solution:**
    ```sql
    SELECT Name FROM Employees
    EXCEPT
    SELECT Name FROM Clients;
    ```
