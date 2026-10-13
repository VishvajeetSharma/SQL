# SQL Set Operators — Complete Notes

These notes cover the set operators listed in the supplied SQL notes: `UNION`, `UNION ALL`, `INTERSECT`, and `EXCEPT` (called `MINUS` in some database systems).

## 1. What are Set Operators?

**Definition:** Set operators combine or compare the result sets of two `SELECT` queries and return one result set.

Set operators work vertically: they combine rows from query results rather than adding columns side by side.

## 2. Requirements

For set operators:

1. Both queries must return the same number of columns.
2. Corresponding columns must have compatible data types.
3. The columns are matched by position, not by their names.
4. The final column names are generally taken from the first `SELECT`.
5. Use `ORDER BY` at the end to sort the combined result.

## 3. Sample data

Assume the following two result sets:

**Result set R1**

| value |
|---:|
| 1 |
| 2 |
| 3 |
| 4 |

**Result set R2**

| value |
|---:|
| 1 |
| 3 |
| 5 |
| 6 |

## 4. UNION

**Definition:** `UNION` combines the results of two queries and removes duplicate rows.

```sql
SELECT value FROM TableA
UNION
SELECT value FROM TableB;
```

For the sample result sets:

`R1 UNION R2` → `1, 2, 3, 4, 5, 6`

Example:

```sql
SELECT country_code AS code
FROM current_customers

UNION

SELECT country_code AS code
FROM archived_customers;
```

Use `UNION` when the combined result should contain distinct rows.

## 5. UNION ALL

**Definition:** `UNION ALL` combines the results of two queries without removing duplicate rows.

```sql
SELECT value FROM TableA
UNION ALL
SELECT value FROM TableB;
```

For the sample result sets, the combined rows are:

`1, 2, 3, 4, 1, 3, 5, 6`

Example:

```sql
SELECT country_code AS code
FROM current_customers

UNION ALL

SELECT country_code AS code
FROM archived_customers;
```

Use `UNION ALL` when duplicates should be retained. It often avoids the duplicate-removal work required by `UNION`.

### UNION vs UNION ALL

| `UNION` | `UNION ALL` |
|---|---|
| Removes duplicate rows from the combined result | Retains duplicate rows |
| May require extra work to remove duplicates | Often faster when duplicate removal is unnecessary |
| Use for distinct combined results | Use when all rows and duplicates are needed |

## 6. INTERSECT

**Definition:** `INTERSECT` returns distinct rows that appear in both query results.

```sql
SELECT value FROM TableA
INTERSECT
SELECT value FROM TableB;
```

For the sample result sets:

`R1 INTERSECT R2` → `1, 3`

Example: Find country codes present in both current and archived customer records.

```sql
SELECT country_code
FROM current_customers

INTERSECT

SELECT country_code
FROM archived_customers;
```

`INTERSECT` returns only values/rows common to both results.

## 7. EXCEPT / MINUS

**Definition:** `EXCEPT` returns distinct rows present in the first query result but not in the second query result.

```sql
SELECT value FROM TableA
EXCEPT
SELECT value FROM TableB;
```

For the sample result sets:

`R1 EXCEPT R2` → `2, 4`

Example: Find country codes present in current customers but absent from archived customers.

```sql
SELECT country_code
FROM current_customers

EXCEPT

SELECT country_code
FROM archived_customers;
```

**Database-specific naming:**

- SQL Server and PostgreSQL use `EXCEPT`.
- Oracle uses `MINUS`.
- Check the syntax supported by your database.

The order of the queries matters: `A EXCEPT B` is not generally the same as `B EXCEPT A`.

## 8. Quick comparison

| Operator | Result |
|---|---|
| `UNION` | Rows from both results, with duplicates removed |
| `UNION ALL` | Rows from both results, including duplicates |
| `INTERSECT` | Distinct rows common to both results |
| `EXCEPT` / `MINUS` | Distinct rows in the first result but not the second |

## 9. Important interview questions

### Q1. What is the difference between JOIN and UNION?

JOIN combines columns from tables based on a matching condition. UNION combines rows from compatible query results.

### Q2. What is the difference between UNION and UNION ALL?

`UNION` removes duplicate rows; `UNION ALL` keeps them.

### Q3. What conditions must be met to use UNION?

Both queries must return the same number of columns, and corresponding columns must have compatible data types.

### Q4. Does INTERSECT return duplicates?

In standard usage, `INTERSECT` returns distinct rows common to both results.

### Q5. What does EXCEPT return?

It returns distinct rows found in the first query but not in the second. In Oracle, the equivalent operator is named `MINUS`.

### Q6. Does the order of queries matter in EXCEPT?

Yes. `A EXCEPT B` returns rows from A missing from B, while `B EXCEPT A` reverses that comparison.

### Q7. Can ORDER BY be used with set operators?

Yes. Put `ORDER BY` at the end of the combined query. It sorts the final result.

Example:

```sql
SELECT value FROM TableA
UNION
SELECT value FROM TableB
ORDER BY value;
```

## 10. Practice tasks

Using two tables or queries of your choice, write SQL to:

1. Combine two lists and remove duplicates.
2. Combine two lists while retaining duplicates.
3. Find values present in both lists.
4. Find values present in the first list but not the second.
5. Sort the combined result in ascending order.
6. Combine customer email addresses from current and archived customer tables.
7. Find products listed in one result but not in another.

## Quick revision

- `UNION` = combine + remove duplicates.
- `UNION ALL` = combine + keep duplicates.
- `INTERSECT` = common rows.
- `EXCEPT` / `MINUS` = rows in the first result but not the second.
