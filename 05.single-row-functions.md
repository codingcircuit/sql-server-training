# SQL Server Single Row Functions

## Introduction
SQL Server single row functions are used to manipulate data at the row level. These functions operate on individual rows and return a single result per row. They can be categorized into string functions, numeric functions, date functions, conversion functions, logical functions, system functions, metadata functions, and mathematical functions.

## Index of Single Row Functions

1. [String Functions](#string-functions)
   - [LEN()](#len)
   - [SUBSTRING()](#substring)
   - [UPPER()](#upper)
   - [LOWER()](#lower)
   - [REPLACE()](#replace)
   - [LEFT()](#left)
   - [RIGHT()](#right)
   - [LTRIM()](#ltrim)
   - [RTRIM()](#rtrim)
   - [CHARINDEX()](#charindex)
2. [Numeric Functions](#numeric-functions)
   - [ROUND()](#round)
   - [CEILING()](#ceiling)
   - [FLOOR()](#floor)
   - [ABS()](#abs)
   - [SQRT()](#sqrt)
   - [POWER()](#power)
   - [RAND()](#rand)
3. [Date Functions](#date-functions)
   - [GETDATE()](#getdate)
   - [DATEADD()](#dateadd)
   - [DATEDIFF()](#datediff)
   - [CONVERT()](#convert)
   - [DATEPART()](#datepart)
   - [YEAR()](#year)
   - [MONTH()](#month)
   - [DAY()](#day)
4. [Conversion Functions](#conversion-functions)
   - [CAST()](#cast)
   - [TRY_CAST()](#try_cast)
   - [TRY_CONVERT()](#try_convert)
5. [Logical Functions](#logical-functions)
   - [IIF()](#iif)
   - [CHOOSE()](#choose)
   - [COALESCE()](#coalesce)
   - [NULLIF()](#nullif)
6. [System Functions](#system-functions)
   - [@@VERSION](#version)
   - [HOST_NAME()](#host_name)
   - [DB_NAME()](#db_name)
   - [USER_NAME()](#user_name)
   - [ISNULL()](#isnull)
   - [ISNUMERIC()](#isnumeric)
7. [Metadata Functions](#metadata-functions)
   - [COL_LENGTH()](#col_length)
   - [OBJECT_ID()](#object_id)
   - [OBJECT_NAME()](#object_name)
8. [Mathematical Functions](#mathematical-functions)
   - [SIN()](#sin)
   - [COS()](#cos)
   - [TAN()](#tan)
   - [LOG()](#log)
   - [EXP()](#exp)
   - [PI()](#pi)
9. [Use Cases](#use-cases)  
   - [Example 1: String Manipulation](#example-1-string-manipulation)  
   - [Example 2: Rounding Financial Figures](#example-2-rounding-financial-figures)  
   - [Example 3: Date Calculations for Employee Tenure](#example-3-date-calculations-for-employee-tenure)  
   - [Example 4: Extracting Domain from Email Addresses](#example-4-extracting-domain-from-email-addresses)  
   - [Example 5: Calculating Age from Birthdate](#example-5-calculating-age-from-birthdate)  
   - [Example 6: Generating Random Passwords](#example-6-generating-random-passwords)  
   - [Example 7: Formatting Dates for Reports](#example-7-formatting-dates-for-reports)  
   - [Example 8: Handling NULL Values in Calculations](#example-8-handling-null-values-in-calculations)  
   - [Example 9: Extracting Year and Month from Dates](#example-9-extracting-year-and-month-from-dates)  
   - [Example 10: Calculating the Square Root of a Column](#example-10-calculating-the-square-root-of-a-column)  
   - [Example 11: Using COALESCE to Handle Missing Data](#example-11-using-coalesce-to-handle-missing-data)  
   - [Example 12: Calculating the Power of a Number](#example-12-calculating-the-power-of-a-number)  
   - [Example 13: Using IIF for Conditional Logic](#example-13-using-iif-for-conditional-logic)  
   - [Example 14: Using CHOOSE to Map Numbers to Text](#example-14-using-choose-to-map-numbers-to-text)  
   - [Example 15: Using NULLIF to Avoid Division by Zero](#example-15-using-nullif-to-avoid-division-by-zero)  
   - [Example 16: Using Metadata Functions to Check Column Length](#example-16-using-metadata-functions-to-check-column-length)  
   - [Example 17: Using Mathematical Functions for Trigonometry](#example-17-using-mathematical-functions-for-trigonometry)  

---

## String Functions

### LEN()
- **Description:** Returns the number of characters in a string, excluding trailing spaces.
- **Syntax:** `LEN(expression)`
- **Example:** 
    ```sql
    SELECT LEN('Hello World') AS Length;
    ```
- **Result:** `11`

### SUBSTRING()
- **Description:** Extracts a substring from a string.
- **Syntax:** `SUBSTRING(expression, start, length)`
- **Example:** 
    ```sql
    SELECT SUBSTRING('Hello World', 1, 5) AS Substring;
    ```
- **Result:** `Hello`

### UPPER()
- **Description:** Converts all characters in a string to uppercase.
- **Syntax:** `UPPER(expression)`
- **Example:** 
    ```sql
    SELECT UPPER('Hello World') AS Uppercase;
    ```
- **Result:** `HELLO WORLD`

### LOWER()
- **Description:** Converts all characters in a string to lowercase.
- **Syntax:** `LOWER(expression)`
- **Example:** 
    ```sql
    SELECT LOWER('Hello World') AS Lowercase;
    ```
- **Result:** `hello world`

### REPLACE()
- **Description:** Replaces all occurrences of a specified substring within a string with another substring.
- **Syntax:** `REPLACE(expression, find, replace)`
- **Example:** 
    ```sql
    SELECT REPLACE('Hello World', 'World', 'SQL') AS Replaced;
    ```
- **Result:** `Hello SQL`

### LEFT()
- **Description:** Returns the left part of a string with the specified number of characters.
- **Syntax:** `LEFT(expression, length)`
- **Example:** 
    ```sql
    SELECT LEFT('Hello World', 5) AS LeftPart;
    ```
- **Result:** `Hello`

### RIGHT()
- **Description:** Returns the right part of a string with the specified number of characters.
- **Syntax:** `RIGHT(expression, length)`
- **Example:** 
    ```sql
    SELECT RIGHT('Hello World', 5) AS RightPart;
    ```
- **Result:** `World`

### LTRIM()
- **Description:** Removes leading spaces from a string.
- **Syntax:** `LTRIM(expression)`
- **Example:** 
    ```sql
    SELECT LTRIM('   Hello World') AS Trimmed;
    ```
- **Result:** `Hello World`

### RTRIM()
- **Description:** Removes trailing spaces from a string.
- **Syntax:** `RTRIM(expression)`
- **Example:** 
    ```sql
    SELECT RTRIM('Hello World   ') AS Trimmed;
    ```
- **Result:** `Hello World`

### CHARINDEX()
- **Description:** Returns the starting position of a substring within a string.
- **Syntax:** `CHARINDEX(find, expression)`
- **Example:** 
    ```sql
    SELECT CHARINDEX('World', 'Hello World') AS Position;
    ```
- **Result:** `7`

---

## Numeric Functions

### ROUND()
- **Description:** Rounds a number to a specified number of decimal places.
- **Syntax:** `ROUND(expression, decimal_places)`
- **Example:** 
    ```sql
    SELECT ROUND(123.4567, 2) AS Rounded;
    ```
- **Result:** `123.46`

### CEILING()
- **Description:** Returns the smallest integer greater than or equal to the given number.
- **Syntax:** `CEILING(expression)`
- **Example:** 
    ```sql
    SELECT CEILING(123.45) AS CeilValue;
    ```
- **Result:** `124`

### FLOOR()
- **Description:** Returns the largest integer less than or equal to the given number.
- **Syntax:** `FLOOR(expression)`
- **Example:** 
    ```sql
    SELECT FLOOR(123.45) AS FloorValue;
    ```
- **Result:** `123`

### ABS()
- **Description:** Returns the absolute value of a number.
- **Syntax:** `ABS(expression)`
- **Example:** 
    ```sql
    SELECT ABS(-123.45) AS AbsoluteValue;
    ```
- **Result:** `123.45`

### SQRT()
- **Description:** Returns the square root of a number.
- **Syntax:** `SQRT(expression)`
- **Example:** 
    ```sql
    SELECT SQRT(16) AS SquareRoot;
    ```
- **Result:** `4`

### POWER()
- **Description:** Raises a number to the power of another number.
- **Syntax:** `POWER(expression, power)`
- **Example:** 
    ```sql
    SELECT POWER(2, 3) AS PowerResult;
    ```
- **Result:** `8`

### RAND()
- **Description:** Returns a random float value between 0 and 1.
- **Syntax:** `RAND()`
- **Example:** 
    ```sql
    SELECT RAND() AS RandomValue;
    ```
- **Result:** `0.123456789` (random value)

---

## Date Functions

### GETDATE()
- **Description:** Returns the current date and time.
- **Syntax:** `GETDATE()`
- **Example:** 
    ```sql
    SELECT GETDATE() AS CurrentDateTime;
    ```
- **Result:** `2024-08-27 10:45:23.000`

### DATEADD()
- **Description:** Adds a specified number of units to a date.
- **Syntax:** `DATEADD(datepart, number, date)`
- **Example:** 
    ```sql
    SELECT DATEADD(day, 5, '2024-08-27') AS NewDate;
    ```
- **Result:** `2024-09-01`

### DATEDIFF()
- **Description:** Returns the difference between two dates in terms of the specified date part.
- **Syntax:** `DATEDIFF(datepart, startdate, enddate)`
- **Example:** 
    ```sql
    SELECT DATEDIFF(day, '2024-08-27', '2024-09-01') AS DateDifference;
    ```
- **Result:** `5`

### CONVERT()
- **Description:** Converts an expression of one data type to another. Commonly used to format dates.
- **Syntax:** `CONVERT(data_type, expression, style)`
- **Example:** 
    ```sql
    SELECT CONVERT(VARCHAR, GETDATE(), 103) AS FormattedDate;
    ```
- **Result:** `27/08/2024`

### DATEPART()
- **Description:** Returns a specific part of a date (e.g., year, month, day).
- **Syntax:** `DATEPART(datepart, date)`
- **Example:** 
    ```sql
    SELECT DATEPART(year, '2024-08-27') AS YearPart;
    ```
- **Result:** `2024`

### YEAR()
- **Description:** Returns the year part of a date.
- **Syntax:** `YEAR(date)`
- **Example:** 
    ```sql
    SELECT YEAR('2024-08-27') AS Year;
    ```
- **Result:** `2024`

### MONTH()
- **Description:** Returns the month part of a date.
- **Syntax:** `MONTH(date)`
- **Example:** 
    ```sql
    SELECT MONTH('2024-08-27') AS Month;
    ```
- **Result:** `8`

### DAY()
- **Description:** Returns the day part of a date.
- **Syntax:** `DAY(date)`
- **Example:** 
    ```sql
    SELECT DAY('2024-08-27') AS Day;
    ```
- **Result:** `27`

---

## Conversion Functions

### CAST()
- **Description:** Converts an expression to a specified data type.
- **Syntax:** `CAST(expression AS data_type)`
- **Example:** 
    ```sql
    SELECT CAST(123.45 AS INT) AS IntegerValue;
    ```
- **Result:** `123`

### TRY_CAST()
- **Description:** Similar to `CAST()`, but returns `NULL` if the conversion fails.
- **Syntax:** `TRY_CAST(expression AS data_type)`
- **Example:** 
    ```sql
    SELECT TRY_CAST('ABC' AS INT) AS IntegerValue;
    ```
- **Result:** `NULL`

### TRY_CONVERT()
- **Description:** Similar to `CONVERT()`, but returns `NULL` if the conversion fails.
- **Syntax:** `TRY_CONVERT(data_type, expression)`
- **Example:** 
    ```sql
    SELECT TRY_CONVERT(INT, 'ABC') AS IntegerValue;
    ```
- **Result:** `NULL`

---

## Logical Functions

### IIF()
- **Description:** Returns one of two values based on a condition.
- **Syntax:** `IIF(condition, true_value, false_value)`
- **Example:** 
    ```sql
    SELECT IIF(1 > 2, 'True', 'False') AS Result;
    ```
- **Result:** `False`

### CHOOSE()
- **Description:** Returns a value from a list based on an index.
- **Syntax:** `CHOOSE(index, value1, value2, ...)`
- **Example:** 
    ```sql
    SELECT CHOOSE(2, 'Apple', 'Banana', 'Cherry') AS Fruit;
    ```
- **Result:** `Banana`

### COALESCE()
- **Description:** Returns the first non-null value in a list of arguments.
- **Syntax:** `COALESCE(expression1, expression2, ...)`
- **Example:** 
    ```sql
    SELECT COALESCE(NULL, 'SQL', 'Server') AS Result;
    ```
- **Result:** `SQL`

### NULLIF()
- **Description:** Returns `NULL` if two expressions are equal; otherwise, returns the first expression.
- **Syntax:** `NULLIF(expression1, expression2)`
- **Example:** 
    ```sql
    SELECT NULLIF(10, 10) AS Result;
    ```
- **Result:** `NULL`

---

## System Functions

### @@VERSION
- **Description:** Returns the version of SQL Server.
- **Syntax:** `@@VERSION`
- **Example:** 
    ```sql
    SELECT @@VERSION AS Version;
    ```
- **Result:** `Microsoft SQL Server 2022 (RTM) - 16.0.1000.6`

### HOST_NAME()
- **Description:** Returns the name of the current host computer.
- **Syntax:** `HOST_NAME()`
- **Example:** 
    ```sql
    SELECT HOST_NAME() AS HostName;
    ```
- **Result:** `MyComputer`

### DB_NAME()
- **Description:** Returns the name of the current database.
- **Syntax:** `DB_NAME()`
- **Example:** 
    ```sql
    SELECT DB_NAME() AS DatabaseName;
    ```
- **Result:** `MyDatabase`

### USER_NAME()
- **Description:** Returns the name of the current user.
- **Syntax:** `USER_NAME()`
- **Example:** 
    ```sql
    SELECT USER_NAME() AS UserName;
    ```
- **Result:** `dbo`

### ISNULL()
- **Description:** Replaces `NULL` with a specified value.
- **Syntax:** `ISNULL(expression, replacement_value)`
- **Example:** 
    ```sql
    SELECT ISNULL(NULL, 'SQL Server') AS Result;
    ```
- **Result:** `SQL Server`

### ISNUMERIC()
- **Description:** Checks if an expression is a valid numeric type.
- **Syntax:** `ISNUMERIC(expression)`
- **Example:** 
    ```sql
    SELECT ISNUMERIC('123') AS IsNumeric;
    ```
- **Result:** `1` (true)

---

## Metadata Functions

### COL_LENGTH()
- **Description:** Returns the defined length of a column.
- **Syntax:** `COL_LENGTH(table_name, column_name)`
- **Example:** 
    ```sql
    SELECT COL_LENGTH('Employees', 'Salary') AS ColumnLength;
    ```
- **Result:** `8` (length of the `Salary` column)

### OBJECT_ID()
- **Description:** Returns the database object ID for a specified object name.
- **Syntax:** `OBJECT_ID(object_name)`
- **Example:** 
    ```sql
    SELECT OBJECT_ID('Employees') AS ObjectID;
    ```
- **Result:** `123456` (object ID of the `Employees` table)

### OBJECT_NAME()
- **Description:** Returns the database object name for a specified object ID.
- **Syntax:** `OBJECT_NAME(object_id)`
- **Example:** 
    ```sql
    SELECT OBJECT_NAME(123456) AS ObjectName;
    ```
- **Result:** `Employees`

---

## Mathematical Functions

### SIN()
- **Description:** Returns the sine of an angle in radians.
- **Syntax:** `SIN(expression)`
- **Example:** 
    ```sql
    SELECT SIN(1.0) AS SineValue;
    ```
- **Result:** `0.8414709848078965`

### COS()
- **Description:** Returns the cosine of an angle in radians.
- **Syntax:** `COS(expression)`
- **Example:** 
    ```sql
    SELECT COS(1.0) AS CosineValue;
    ```
- **Result:** `0.5403023058681398`

### TAN()
- **Description:** Returns the tangent of an angle in radians.
- **Syntax:** `TAN(expression)`
- **Example:** 
    ```sql
    SELECT TAN(1.0) AS TangentValue;
    ```
- **Result:** `1.5574077246549023`

### LOG()
- **Description:** Returns the natural logarithm of a number.
- **Syntax:** `LOG(expression)`
- **Example:** 
    ```sql
    SELECT LOG(10) AS NaturalLog;
    ```
- **Result:** `2.302585092994046`

### EXP()
- **Description:** Returns the exponential value of a number.
- **Syntax:** `EXP(expression)`
- **Example:** 
    ```sql
    SELECT EXP(1.0) AS ExponentialValue;
    ```
- **Result:** `2.718281828459045`

### PI()
- **Description:** Returns the value of π (pi).
- **Syntax:** `PI()`
- **Example:** 
    ```sql
    SELECT PI() AS PiValue;
    ```
- **Result:** `3.141592653589793`

---

## Use Cases

### Example 1: String Manipulation
- **Scenario:** You want to standardize the casing of usernames in a database by converting them to lowercase and remove any leading or trailing spaces.
- **Solution:**
    ```sql
    UPDATE Users
    SET Username = LOWER(LTRIM(RTRIM(Username)));
    ```
- **Explanation:** 
  - `LTRIM()` and `RTRIM()` remove leading and trailing spaces.
  - `LOWER()` converts the username to lowercase.

---

### Example 2: Rounding Financial Figures
- **Scenario:** You need to round off financial figures (e.g., salaries) to two decimal places for accurate reporting.
- **Solution:**
    ```sql
    SELECT EmployeeID, ROUND(Salary, 2) AS RoundedSalary
    FROM Employees;
    ```
- **Explanation:** 
  - `ROUND()` is used to round the salary to two decimal places.

---

### Example 3: Date Calculations for Employee Tenure
- **Scenario:** You need to calculate the number of years each employee has worked at the company based on their hire date.
- **Solution:**
    ```sql
    SELECT EmployeeID, 
           DATEDIFF(year, HireDate, GETDATE()) AS YearsOfService
    FROM Employees;
    ```
- **Explanation:** 
  - `DATEDIFF()` calculates the difference in years between the `HireDate` and the current date (`GETDATE()`).

---

### Example 4: Extracting Domain from Email Addresses
- **Scenario:** You want to extract the domain part of email addresses (e.g., `example.com` from `user@example.com`).
- **Solution:**
    ```sql
    SELECT Email,
           SUBSTRING(Email, CHARINDEX('@', Email) + 1, LEN(Email)) AS Domain
    FROM Users;
    ```
- **Explanation:** 
  - `CHARINDEX()` finds the position of the `@` symbol.
  - `SUBSTRING()` extracts the domain part starting from the character after `@`.

---

### Example 5: Calculating Age from Birthdate
- **Scenario:** You need to calculate the age of users based on their birthdate.
- **Solution:**
    ```sql
    SELECT UserID, 
           DATEDIFF(year, Birthdate, GETDATE()) - 
           CASE WHEN DATEADD(year, DATEDIFF(year, Birthdate, GETDATE()), Birthdate) > GETDATE() 
                THEN 1 
                ELSE 0 
           END AS Age
    FROM Users;
    ```
- **Explanation:** 
  - `DATEDIFF()` calculates the difference in years between the birthdate and the current date.
  - The `CASE` statement adjusts the age if the user's birthday hasn't occurred yet this year.

---

### Example 6: Generating Random Passwords
- **Scenario:** You need to generate random passwords for new users.
- **Solution:**
    ```sql
    SELECT LEFT(NEWID(), 8) AS RandomPassword;
    ```
- **Explanation:** 
  - `NEWID()` generates a unique identifier (GUID).
  - `LEFT()` extracts the first 8 characters of the GUID to create a random password.

---

### Example 7: Formatting Dates for Reports
- **Scenario:** You need to format dates in a specific style (e.g., `DD/MM/YYYY`) for a report.
- **Solution:**
    ```sql
    SELECT OrderID, 
           CONVERT(VARCHAR, OrderDate, 103) AS FormattedDate
    FROM Orders;
    ```
- **Explanation:** 
  - `CONVERT()` is used to format the `OrderDate` in the `DD/MM/YYYY` style (style code `103`).

---

### Example 8: Handling NULL Values in Calculations
- **Scenario:** You need to calculate the total price of orders, but some prices are `NULL`.
- **Solution:**
    ```sql
    SELECT OrderID, 
           ISNULL(Price, 0) * Quantity AS TotalPrice
    FROM Orders;
    ```
- **Explanation:** 
  - `ISNULL()` replaces `NULL` prices with `0` to avoid errors in calculations.

---

### Example 9: Extracting Year and Month from Dates
- **Scenario:** You need to group sales data by year and month.
- **Solution:**
    ```sql
    SELECT YEAR(OrderDate) AS OrderYear, 
           MONTH(OrderDate) AS OrderMonth, 
           SUM(TotalAmount) AS TotalSales
    FROM Orders
    GROUP BY YEAR(OrderDate), MONTH(OrderDate);
    ```
- **Explanation:** 
  - `YEAR()` and `MONTH()` extract the year and month from the `OrderDate`.
  - The data is grouped by year and month for aggregation.

---

### Example 10: Calculating the Square Root of a Column
- **Scenario:** You need to calculate the square root of values in a column (e.g., for statistical analysis).
- **Solution:**
    ```sql
    SELECT Value, 
           SQRT(Value) AS SquareRoot
    FROM DataTable;
    ```
- **Explanation:** 
  - `SQRT()` calculates the square root of the `Value` column.

---

### Example 11: Using COALESCE to Handle Missing Data
- **Scenario:** You need to display a default value if a column contains `NULL`.
- **Solution:**
    ```sql
    SELECT ProductID, 
           COALESCE(ProductName, 'Unknown') AS ProductName
    FROM Products;
    ```
- **Explanation:** 
  - `COALESCE()` returns the first non-null value. If `ProductName` is `NULL`, it returns `'Unknown'`.

---

### Example 12: Calculating the Power of a Number
- **Scenario:** You need to calculate the square or cube of a number for mathematical computations.
- **Solution:**
    ```sql
    SELECT Number, 
           POWER(Number, 2) AS Square, 
           POWER(Number, 3) AS Cube
    FROM NumbersTable;
    ```
- **Explanation:** 
  - `POWER()` raises the `Number` to the specified power (e.g., 2 for square, 3 for cube).

---

### Example 13: Using IIF for Conditional Logic
- **Scenario:** You need to categorize employees as "Senior" or "Junior" based on their years of experience.
- **Solution:**
    ```sql
    SELECT EmployeeID, 
           IIF(YearsOfExperience > 5, 'Senior', 'Junior') AS EmployeeLevel
    FROM Employees;
    ```
- **Explanation:** 
  - `IIF()` checks if `YearsOfExperience` is greater than 5 and returns `'Senior'` or `'Junior'`.

---

### Example 14: Using CHOOSE to Map Numbers to Text
- **Scenario:** You need to map numeric status codes (e.g., 1, 2, 3) to their corresponding text descriptions.
- **Solution:**
    ```sql
    SELECT OrderID, 
           CHOOSE(Status, 'Pending', 'Shipped', 'Delivered') AS StatusDescription
    FROM Orders;
    ```
- **Explanation:** 
  - `CHOOSE()` maps the `Status` code to its corresponding text description.

---

### Example 15: Using NULLIF to Avoid Division by Zero
- **Scenario:** You need to calculate the ratio of two columns but want to avoid division by zero errors.
- **Solution:**
    ```sql
    SELECT Numerator, 
           Denominator, 
           Numerator / NULLIF(Denominator, 0) AS Ratio
    FROM Calculations;
    ```
- **Explanation:** 
  - `NULLIF()` returns `NULL` if the denominator is `0`, preventing division by zero errors.

---

### Example 16: Using Metadata Functions to Check Column Length
- **Scenario:** You need to verify the length of a column in a table.
- **Solution:**
    ```sql
    SELECT COL_LENGTH('Employees', 'FirstName') AS FirstNameLength;
    ```
- **Explanation:** 
  - `COL_LENGTH()` returns the defined length of the `FirstName` column in the `Employees` table.

---

### Example 17: Using Mathematical Functions for Trigonometry
- **Scenario:** You need to calculate the sine, cosine, and tangent of angles stored in a table.
- **Solution:**
    ```sql
    SELECT Angle, 
           SIN(Angle) AS Sine, 
           COS(Angle) AS Cosine, 
           TAN(Angle) AS Tangent
    FROM AnglesTable;
    ```
- **Explanation:** 
  - `SIN()`, `COS()`, and `TAN()` calculate the trigonometric values of the `Angle`.

---
