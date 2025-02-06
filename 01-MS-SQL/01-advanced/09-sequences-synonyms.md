# Sequences and Synonyms

## Index
- [Introduction to Sequences and Synonyms](#introduction-to-sequences-and-synonyms)
- [Sequences](#sequences)
  - [Creating a Sequence](#creating-a-sequence)
  - [Using a Sequence](#using-a-sequence)
  - [Resetting a Sequence](#resetting-a-sequence)
- [Synonyms](#synonyms)
  - [Creating a Synonym](#creating-a-synonym)
  - [Using Synonyms](#using-synonyms)
- [Real-Time Use Cases](#real-time-use-cases)

## Introduction to Sequences and Synonyms
Sequences and synonyms are features in T-SQL that help manage identity values and abstract object names for better maintainability.

## Sequences
A sequence is a user-defined object that generates numeric values sequentially. Unlike identity columns, sequences are independent of tables.

### Creating a Sequence
```sql
CREATE SEQUENCE SalesOrderSeq
    AS INT
    START WITH 1
    INCREMENT BY 1;
```

### Using a Sequence
```sql
SELECT NEXT VALUE FOR SalesOrderSeq;
```

### Resetting a Sequence
```sql
ALTER SEQUENCE SalesOrderSeq
    RESTART WITH 1;
```

## Synonyms
Synonyms provide an alias for database objects, simplifying query writing and maintenance.

### Creating a Synonym
```sql
CREATE SYNONYM SalesOrdersSyn
FOR Sales.SalesOrderHeader;
```

### Using Synonyms
```sql
SELECT * FROM SalesOrdersSyn;
```

## Real-Time Use Cases
### Generating Unique Order Numbers
Sequences can help maintain a custom order numbering system.

```sql
INSERT INTO Sales.SalesOrderHeader (SalesOrderID, CustomerID, OrderDate)
VALUES (NEXT VALUE FOR SalesOrderSeq, 1001, GETDATE());
```

### Simplifying Cross-Database Queries
Synonyms allow referencing objects across databases without hardcoding database names.

```sql
SELECT * FROM SalesOrdersSyn;
```

