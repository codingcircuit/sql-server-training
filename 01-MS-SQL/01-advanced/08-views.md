# Views

## Index
- [Introduction to Views](#introduction-to-views)
- [Advantages of Using Views](#advantages-of-using-views)
- [Creating a Basic View](#creating-a-basic-view)
- [Updating Data Through Views](#updating-data-through-views)
- [Indexed Views](#indexed-views)
- [Using Views for Security](#using-views-for-security)
- [Real-Time Use Cases](#real-time-use-cases)
  - [Customer Order Summary](#customer-order-summary)
  - [Product Inventory Overview](#product-inventory-overview)
  - [Employee Department Report](#employee-department-report)
- [Best Practices](#best-practices)

## Introduction to Views
A view in SQL Server is a virtual table that encapsulates a query. It simplifies data access, improves security, and abstracts complex queries into reusable components.

## Advantages of Using Views
- **Simplifies Complex Queries**: Encapsulates complex joins and aggregations.
- **Enhances Security**: Restricts access to specific columns or rows.
- **Improves Performance**: Indexed views can enhance query execution speed.
- **Promotes Maintainability**: Simplifies modifications by centralizing query logic.

## Creating a Basic View
This view retrieves customer orders from the `Sales.SalesOrderHeader` table.

```sql
CREATE VIEW v_CustomerOrders
AS
SELECT SalesOrderID, CustomerID, OrderDate, TotalDue
FROM Sales.SalesOrderHeader;
```

### Using the View
```sql
SELECT * FROM v_CustomerOrders WHERE CustomerID = 29825;
```

## Updating Data Through Views
Views can allow updates if they reference a single table without aggregations.

```sql
CREATE VIEW v_UpdatableCustomers
AS
SELECT BusinessEntityID, FirstName, LastName
FROM Person.Person;
```

### Updating Data Using the View
```sql
UPDATE v_UpdatableCustomers
SET LastName = 'Smith'
WHERE BusinessEntityID = 1001;
```

## Indexed Views
Indexed views can improve query performance by materializing results.

```sql
CREATE VIEW v_SalesSummary
WITH SCHEMABINDING
AS
SELECT CustomerID, COUNT(SalesOrderID) AS OrderCount, SUM(TotalDue) AS TotalSales
FROM Sales.SalesOrderHeader
GROUP BY CustomerID;
```

### Creating an Index on the View
```sql
CREATE UNIQUE CLUSTERED INDEX IX_v_SalesSummary ON v_SalesSummary (CustomerID);
```

## Using Views for Security
Views can restrict access to sensitive data.

```sql
CREATE VIEW v_PublicEmployeeData
AS
SELECT BusinessEntityID, FirstName, LastName, JobTitle
FROM HumanResources.Employee;
```

## Real-Time Use Cases

### Customer Order Summary
A view to provide a summarized order history for customers.

```sql
CREATE VIEW v_CustomerOrderSummary
AS
SELECT CustomerID, COUNT(SalesOrderID) AS OrderCount, SUM(TotalDue) AS TotalSpent
FROM Sales.SalesOrderHeader
GROUP BY CustomerID;
```

### Product Inventory Overview
A view that provides a real-time inventory snapshot.

```sql
CREATE VIEW v_ProductInventory
AS
SELECT ProductID, SUM(Quantity) AS TotalStock
FROM Production.ProductInventory
GROUP BY ProductID;
```

### Employee Department Report
A view that retrieves employees along with their department names.

```sql
CREATE VIEW v_EmployeeDepartment
AS
SELECT e.BusinessEntityID, p.FirstName, p.LastName, d.Name AS Department
FROM HumanResources.Employee e
JOIN Person.Person p ON e.BusinessEntityID = p.BusinessEntityID
JOIN HumanResources.Department d ON e.DepartmentID = d.DepartmentID;
```

## Best Practices
- Use `WITH SCHEMABINDING` for indexed views.
- Avoid using `SELECT *` in views.
- Restrict permissions on underlying tables and expose data through views.
- Ensure indexed views are used only when necessary due to maintenance overhead.
