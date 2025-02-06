# Pivot and Unpivot

## Index
- [Introduction to Pivot and Unpivot](#introduction-to-pivot-and-unpivot)
- [Understanding PIVOT](#understanding-pivot)
- [PIVOT Example: Sales by Year](#pivot-example-sales-by-year)
- [Understanding UNPIVOT](#understanding-unpivot)
- [UNPIVOT Example: Converting Columns to Rows](#unpivot-example-converting-columns-to-rows)
- [Real-Time Use Cases](#real-time-use-cases)
  - [Sales Report Pivoting](#sales-report-pivoting)
  - [Transforming Survey Data](#transforming-survey-data)
- [Best Practices](#best-practices)

## Introduction to Pivot and Unpivot
T-SQL provides `PIVOT` and `UNPIVOT` operators to transform data for reporting and analysis:
- **PIVOT** converts row-based data into columnar format.
- **UNPIVOT** performs the reverse transformation, converting columns into rows.

## Understanding PIVOT
The `PIVOT` operator aggregates and rotates data, making it useful for summarizing information.

## PIVOT Example: Sales by Year
Transform sales data from row format to a column-based report.

### Sample Data (Sales.SalesOrderHeader)
| SalesOrderID | CustomerID | OrderYear | TotalDue |
|-------------|-----------|-----------|----------|
| 1           | 1001      | 2022      | 500.00   |
| 2           | 1001      | 2023      | 750.00   |
| 3           | 1002      | 2022      | 300.00   |

### Pivot Query
```sql
SELECT CustomerID, [2022] AS Sales_2022, [2023] AS Sales_2023
FROM (
    SELECT CustomerID, YEAR(OrderDate) AS OrderYear, TotalDue
    FROM Sales.SalesOrderHeader
) AS SourceTable
PIVOT (
    SUM(TotalDue) FOR OrderYear IN ([2022], [2023])
) AS PivotTable;
```

### Output
| CustomerID | Sales_2022 | Sales_2023 |
|-----------|------------|------------|
| 1001      | 500.00     | 750.00     |
| 1002      | 300.00     | NULL       |

## Understanding UNPIVOT
The `UNPIVOT` operator converts columns into row-based format.

## UNPIVOT Example: Converting Columns to Rows
Reverting the pivoted table back to its original format.

```sql
SELECT CustomerID, OrderYear, TotalDue
FROM (
    SELECT CustomerID, Sales_2022, Sales_2023
    FROM PivotTable
) AS PivotedData
UNPIVOT (
    TotalDue FOR OrderYear IN (Sales_2022, Sales_2023)
) AS UnpivotedData;
```

### Output
| CustomerID | OrderYear | TotalDue |
|-----------|----------|----------|
| 1001      | Sales_2022 | 500.00   |
| 1001      | Sales_2023 | 750.00   |
| 1002      | Sales_2022 | 300.00   |

## Real-Time Use Cases

### Sales Report Pivoting
A business wants to generate an annual sales summary for customers using `PIVOT`.

### Transforming Survey Data
Survey responses stored as columns (Yes/No/Maybe) need to be converted into row-based format for analysis using `UNPIVOT`.

## Best Practices
- Use `PIVOT` when you need to aggregate data dynamically into columns.
- Use `UNPIVOT` when normalizing data for further processing.
- Always specify column names explicitly in `PIVOT` and `UNPIVOT` operations.

