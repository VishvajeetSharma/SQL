# SQL JOIN — Complete Notes

These notes use **SQL Server syntax**, consistent with `IDENTITY`, `BIT`, and `OFFSET/FETCH` in the original SQL notes.

## 1. What is JOIN in SQL?

**Definition:** A JOIN in SQL retrieves and combines related data from two or more tables based on a specified condition between their columns.

When required data is stored in multiple tables, JOIN helps fetch that data in one result set.

**Example:** A `tbl_country` table stores countries, and a `tbl_city` table stores cities. JOIN can return a city's name along with its country name.

## 2. Sample tables

### Parent table: `tbl_country`

```sql
CREATE TABLE tbl_country (
    country_id INT PRIMARY KEY IDENTITY(1,1),
    country_name VARCHAR(50) NOT NULL
);

INSERT INTO tbl_country (country_name)
VALUES
('India'),
('USA'),
('Japan'),
('France');
```

### Child table: `tbl_city`

```sql
CREATE TABLE tbl_city (
    city_id INT PRIMARY KEY IDENTITY(1,1),
    city_name VARCHAR(50) NOT NULL,
    country_id INT NULL,
    FOREIGN KEY (country_id)
        REFERENCES tbl_country(country_id)
);

INSERT INTO tbl_city (city_name, country_id)
VALUES
('Lucknow', 1),
('Delhi', 1),
('New York', 2),
('Tokyo', 3),
('Unknown City', NULL);
```

Relationship:

- `tbl_country.country_id` is the **primary key (PK)**.
- `tbl_city.country_id` is the **foreign key (FK)**.
- One country can have multiple cities.
- The foreign key may be `NULL` here because the column allows it.

**Important:** A foreign key constraint is not mandatory for JOIN. Suitable columns and a correct `ON` condition are enough to join tables.

## 3. Types of JOIN

1. **INNER JOIN** — returns rows with matching values in both tables.
2. **LEFT JOIN / LEFT OUTER JOIN** — returns all rows from the left table and matching rows from the right table.
3. **RIGHT JOIN / RIGHT OUTER JOIN** — returns all rows from the right table and matching rows from the left table.
4. **FULL OUTER JOIN** — returns all matching and non-matching rows from both tables.
5. **CROSS JOIN** — returns every possible combination of rows from both tables.
6. **SELF JOIN** — joins a table to itself, usually using aliases.

## 4. INNER JOIN

**Definition:** INNER JOIN returns only records that match the join condition in both tables.

### Syntax

```sql
SELECT column_list
FROM table1
INNER JOIN table2
    ON table1.common_column = table2.common_column;
```

### Example

```sql
SELECT
    c.country_name,
    ct.city_name
FROM tbl_country c
INNER JOIN tbl_city ct
    ON c.country_id = ct.country_id;
```

### Expected output

| country_name | city_name |
|---|---|
| India | Lucknow |
| India | Delhi |
| USA | New York |
| Japan | Tokyo |

**Observation:**

- France is excluded because it has no matching city.
- `Unknown City` is excluded because its `country_id` is `NULL`.
- India appears twice because it has two matching cities.

## 5. LEFT JOIN

**Definition:** LEFT JOIN returns all rows from the left table and matching rows from the right table. If no match exists, right-side columns contain `NULL`.

```sql
SELECT
    c.country_name,
    ct.city_name
FROM tbl_country c
LEFT JOIN tbl_city ct
    ON c.country_id = ct.country_id;
```

### Expected output

| country_name | city_name |
|---|---|
| India | Lucknow |
| India | Delhi |
| USA | New York |
| Japan | Tokyo |
| France | NULL |

**Key point:** A row from the left table is preserved even when no matching row exists in the right table.

**Example use case:** Fetch all employees and their departments, including employees who have not been assigned a department.

## 6. RIGHT JOIN

**Definition:** RIGHT JOIN returns all rows from the right table and matching rows from the left table.

```sql
SELECT
    c.country_name,
    ct.city_name
FROM tbl_country c
RIGHT JOIN tbl_city ct
    ON c.country_id = ct.country_id;
```

### Expected output

| country_name | city_name |
|---|---|
| India | Lucknow |
| India | Delhi |
| USA | New York |
| Japan | Tokyo |
| NULL | Unknown City |

`Unknown City` is preserved because all rows from the right-side table, `tbl_city`, are returned.

**Note:** A RIGHT JOIN can be rewritten as a LEFT JOIN by reversing the table order.

## 7. FULL OUTER JOIN

**Definition:** FULL OUTER JOIN returns all matching and non-matching records from both tables.

```sql
SELECT
    c.country_name,
    ct.city_name
FROM tbl_country c
FULL OUTER JOIN tbl_city ct
    ON c.country_id = ct.country_id;
```

The output includes:

- All matching country-city pairs.
- France with `city_name = NULL`.
- `Unknown City` with `country_name = NULL`.

Both tables' unmatched records are preserved.

## 8. CROSS JOIN

**Definition:** CROSS JOIN returns the Cartesian product of two tables, combining every row of the first table with every row of the second table.

```sql
SELECT
    c.country_name,
    ct.city_name
FROM tbl_country c
CROSS JOIN tbl_city ct;
```

If the first table has 4 rows and the second has 5 rows:

**Number of result rows = 4 × 5 = 20**

CROSS JOIN does not require an `ON` condition.

**Use case:** Generate all combinations of products and sizes, or other possible combinations.

## 9. SELF JOIN

**Definition:** SELF JOIN joins a table to itself. Aliases distinguish the two instances of the same table.

### Example: Employee and manager

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    manager_id INT NULL
);
```

```sql
SELECT
    e.employee_name AS Employee,
    m.employee_name AS Manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

- `e` represents the employee.
- `m` represents the manager.
- `manager_id` references another employee's `employee_id`.
- `LEFT JOIN` preserves employees who do not have a manager.

## 10. Joining multiple tables

In real projects, one query often joins more than two tables.

Example: Orders, users, and products.

```sql
SELECT
    o.order_id,
    u.user_name,
    p.product_name,
    o.quantity
FROM tbl_order o
INNER JOIN tbl_user u
    ON o.uid = u.uid
INNER JOIN tbl_product p
    ON o.pid = p.pid;
```

In this query:

1. `tbl_order` provides order details.
2. `tbl_user` provides the user's name.
3. `tbl_product` provides the product's name.

**Important:** Each JOIN should have an appropriate `ON` condition that matches the intended columns.

## 11. JOIN vs UNION

| JOIN | UNION |
|---|---|
| Combines columns from related tables horizontally. | Combines query results vertically. |
| Often matches rows using a key or condition. | Combines rows from compatible result sets. |
| Can return columns from multiple tables. | Corresponding columns form one result structure. |
| Example: Country + City. | Example: Current employees + former employees. |

For `UNION`, each query must return the same number of columns, with compatible data types in corresponding positions. `UNION` removes duplicate rows; `UNION ALL` retains them.

## 12. Important interview questions

### Q1. What is the difference between INNER JOIN and LEFT JOIN?

INNER JOIN returns only matching rows. LEFT JOIN returns all rows from the left table, including rows without a match in the right table.

### Q2. Is a foreign key mandatory for JOIN?

No. A foreign key constraint is not mandatory. Tables can be joined by comparing suitable columns in the `ON` condition.

### Q3. What happens when there is no matching record in LEFT JOIN?

Columns from the right table return `NULL` for the unmatched row.

### Q4. Can we join more than two tables?

Yes. Multiple JOIN clauses can be used to join three or more tables.

### Q5. What is the difference between LEFT JOIN and RIGHT JOIN?

LEFT JOIN preserves all rows from the left table. RIGHT JOIN preserves all rows from the right table.

### Q6. What is a SELF JOIN?

A SELF JOIN joins a table to itself, such as when finding employees and their managers from one employee table.

### Q7. What is a CROSS JOIN?

CROSS JOIN generates the Cartesian product. If the tables contain `m` and `n` rows, the result has `m × n` rows.

### Q8. Can JOIN produce duplicate rows?

Yes. If one row matches multiple rows in the other table, the result includes multiple rows. This is normal JOIN behavior and does not necessarily mean the data is duplicated incorrectly.

### Q9. What is the difference between ON and WHERE in JOIN?

- `ON` specifies the matching condition between tables.
- `WHERE` filters rows in the resulting query.
- With outer joins, moving a condition between `ON` and `WHERE` can change the output.

### Q10. Can we use aliases in JOIN?

Yes. Aliases make queries shorter and easier to read, especially in multi-table joins and SELF JOINs.

## 13. Practice tasks

Try writing the SQL query for each task:

1. Display country names with city names, only for matching records.
2. Display all countries, including countries without any city.
3. Display all cities, including cities without a matching country.
4. Display all countries and cities, whether matching or unmatched.
5. Generate every possible country-city combination.
6. Display each employee with their manager's name from the same table.
7. Display each order with its user name and product name.
8. Display countries that have no matching cities.

### Practice task 8: Hint

```sql
SELECT c.country_name
FROM tbl_country c
LEFT JOIN tbl_city ct
    ON c.country_id = ct.country_id
WHERE ct.city_id IS NULL;
```

This uses a LEFT JOIN and filters for rows with no matching city.

## Quick revision

| JOIN type | What it returns |
|---|---|
| INNER JOIN | Matching rows only |
| LEFT JOIN | All left rows + matching right rows |
| RIGHT JOIN | All right rows + matching left rows |
| FULL OUTER JOIN | All rows from both tables |
| CROSS JOIN | Every possible row combination |
| SELF JOIN | A table joined to itself |

**Revision tip:** First practise INNER JOIN and LEFT JOIN by manually predicting their output. Then practise RIGHT JOIN and FULL OUTER JOIN, focusing on what happens to unmatched rows.
