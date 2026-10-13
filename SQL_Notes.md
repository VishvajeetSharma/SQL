# SQL Interview Notes

## 1. Database Fundamentals

### What is a database?
A **database** is an organized collection of data that can be stored, retrieved, and managed efficiently.

### What is a DBMS?
A **Database Management System (DBMS)** is software used to create, store, retrieve, update, and manage data, including access control and data integrity.

### What is an RDBMS?
A **Relational Database Management System (RDBMS)** is a type of DBMS that organizes data into relations, commonly represented as tables made up of rows and columns.

### What is SQL?
**SQL (Structured Query Language)** is used to define, query, manipulate, and control data in relational database systems.

### What is a table?
A table organizes data into:
- **Rows / records / tuples:** individual entries.
- **Columns / fields / attributes:** properties of those entries.

### DBMS vs. file system

| Area | DBMS | File system |
|---|---|---|
| Redundancy | Can reduce and control duplicate data | Duplication may be harder to control |
| Consistency | Constraints and transactions help maintain consistency | Often depends on application logic |
| Security | Supports users, roles, and permissions | Depends on file and application security |
| Backup and recovery | Commonly provides database backup/recovery facilities | Often needs separate processes |
| Concurrent access | Provides mechanisms for simultaneous access | Concurrent updates can be harder to coordinate |
| Integrity | Supports constraints such as primary and foreign keys | No built-in relational constraints |
| Querying | SQL and query optimization | Often requires custom code |
| Transactions | Transaction support is available in transactional systems | Not generally provided as database transactions |

### Common uses of databases
Banking, healthcare, e-commerce, education, social media, telecommunications, government systems, transport, airlines, retail, and inventory management.

### Interview questions
**Q. What is the difference between DBMS and RDBMS?**  
A DBMS is the general category of software for managing databases. An RDBMS is a type of DBMS that organizes data relationally, usually in tables, and supports relationships and constraints.

**Q. What is data redundancy?**  
Data redundancy means storing the same data unnecessarily in multiple places.

**Q. What is data integrity?**  
Data integrity means maintaining the accuracy, validity, and consistency of data.

---

## 2. Types of SQL Commands

| Category | Full form | Purpose | Common commands |
|---|---|---|---|
| DDL | Data Definition Language | Defines or changes database objects | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| DML | Data Manipulation Language | Adds, changes, or removes rows | `INSERT`, `UPDATE`, `DELETE` |
| DQL | Data Query Language | Retrieves data | `SELECT` |
| DCL | Data Control Language | Controls permissions | `GRANT`, `REVOKE` |
| TCL | Transaction Control Language | Controls transactions | `BEGIN TRANSACTION`, `COMMIT`, `ROLLBACK` |

**Note:** Some learning resources classify `SELECT` under DML; many teach it separately as DQL. Exact command behavior and transaction syntax vary by database.

- `CREATE`: creates a database object.
- `ALTER`: changes an object's structure.
- `DROP`: removes a database object.
- `TRUNCATE`: removes all rows while retaining the table structure.
- `INSERT`: adds rows.
- `UPDATE`: changes existing values.
- `DELETE`: removes rows.
- `SELECT`: retrieves data.
- `GRANT`: grants privileges.
- `REVOKE`: removes previously granted privileges.
- `COMMIT`: saves the current transaction's changes.
- `ROLLBACK`: reverses uncommitted transaction work, subject to database and transaction state.

### Interview question
**Q. What is the difference between DDL and DML?**  
DDL defines or changes database objects and their structure; DML manipulates the data stored in those objects.

---

## 3. SQL Data Types

A **data type** defines what kind of values a column can store. Availability and limits vary by database system.

### Numeric types

| Type | Purpose |
|---|---|
| `SMALLINT` | Small-range whole numbers |
| `INT` | Whole numbers |
| `BIGINT` | Larger-range whole numbers |
| `DECIMAL(p,s)` | Exact decimal values; useful for money and precise calculations |
| `FLOAT` / `REAL` | Approximate numeric values |
| `MONEY` | Monetary values in SQL Server |

For precise financial calculations, prefer an appropriate `DECIMAL(p,s)` type rather than approximate types such as `FLOAT`.

### String types in SQL Server

| Type | Purpose |
|---|---|
| `CHAR(n)` | Fixed-length, non-Unicode character data |
| `NCHAR(n)` | Fixed-length Unicode character data |
| `VARCHAR(n)` | Variable-length, non-Unicode character data |
| `NVARCHAR(n)` | Variable-length Unicode character data |
| `VARCHAR(MAX)` | Larger variable-length non-Unicode text |
| `NVARCHAR(MAX)` | Larger variable-length Unicode text |

`TEXT` and `NTEXT` are deprecated in SQL Server; use `VARCHAR(MAX)` or `NVARCHAR(MAX)` where appropriate.

### Boolean-like type
SQL Server uses `BIT`, typically represented as `0`, `1`, or `NULL`. Other systems may support a native `BOOLEAN` type.

### Date and time types

| Type | Purpose |
|---|---|
| `DATE` | Date only |
| `TIME` | Time only |
| `DATETIME` | Date and time |
| `DATETIME2` | Date and time with greater precision/range than legacy `DATETIME` in SQL Server |

A common unambiguous date representation is `YYYY-MM-DD`.

### Interview question
**Q. What is the difference between `CHAR` and `VARCHAR`?**  
`CHAR(n)` is fixed-length, while `VARCHAR(n)` stores variable-length character data. In SQL Server, `NCHAR` and `NVARCHAR` are Unicode types.

---

## 4. ER Diagrams and Entities

An **Entity-Relationship (ER) diagram** visually represents entities, their attributes, and the relationships between them.

- **Entity:** a real-world object or concept about which data is stored, such as a Student or Department.
- **Attribute:** a property of an entity, such as `student_id` or `student_name`.
- **Relationship:** an association between entities, such as a Student enrolling in a Course.

Common ER-diagram notation in traditional Chen notation:
- Rectangle: entity.
- Oval: attribute.
- Diamond: relationship.
- Underlined attribute: key attribute.
- Double rectangle: weak entity.
- Double oval: multivalued attribute.

Notation can vary between ER modeling tools and styles.

---

## 5. Creating Databases and Tables

### Create a database

```sql
CREATE DATABASE database_name;
```

### Create a table

```sql
CREATE TABLE students (
    student_id INT,
    student_name VARCHAR(100),
    marks DECIMAL(5,2)
);
```

`CREATE` can create database objects such as databases, tables, views, indexes, procedures, functions, and triggers, depending on the database system.

---

## 6. INSERT — Adding Data

`INSERT` adds new rows to a table.

### Insert values for specified columns

```sql
INSERT INTO students (student_id, student_name, marks)
VALUES (1, 'Aman', 85.50);
```

### Insert multiple rows (supported by SQL Server)

```sql
INSERT INTO students (student_id, student_name, marks)
VALUES
    (2, 'Riya', 91.00),
    (3, 'Kabir', 78.50);
```

**Best practice:** specify column names. Text values are normally enclosed in single quotes; numeric values normally are not.

### Interview question
**Q. Why should we specify column names in an `INSERT` statement?**  
It makes the statement clearer and less dependent on the table's column order. It also allows omitted columns to receive defaults or `NULL` where permitted.

---

## 7. Constraints

Constraints are rules enforced by the database to help validate data and maintain integrity.

| Constraint | Meaning |
|---|---|
| `NOT NULL` | Prevents `NULL` values in a column |
| `DEFAULT` | Supplies a default value when a value is omitted or `DEFAULT` is specified |
| `CHECK` | Requires values to satisfy a condition |
| `PRIMARY KEY` | Uniquely identifies each row; key values cannot be `NULL` |
| `UNIQUE` | Prevents duplicate values for the constrained column or column combination |
| `FOREIGN KEY` | Enforces a reference to a key in another table or, where supported, the same table |

### Example

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE,
    salary DECIMAL(10,2) CHECK (salary >= 0),
    status VARCHAR(20) DEFAULT 'Active'
);
```

Constraints can be defined while creating a table. Many constraints can also be added or removed using `ALTER TABLE`; exact syntax varies.

**Important:** handling of `NULL` values in `UNIQUE` constraints varies across database systems.

---

## 8. Keys in DBMS

| Key | Definition |
|---|---|
| Primary key | The selected key used to uniquely identify each row |
| Foreign key | Column(s) referencing a key in another table, or a supported self-reference |
| Candidate key | A minimal set of column(s) that can uniquely identify a row |
| Superkey | Any set of column(s) that uniquely identifies a row; it may contain extra columns |
| Unique constraint/key | Enforces uniqueness for the constrained column(s) |

### Primary key vs. unique constraint

- A primary key uniquely identifies each row and does not allow `NULL`.
- A table has one primary-key constraint, which may consist of multiple columns (a composite primary key).
- A table can have multiple unique constraints.
- The way unique constraints treat `NULL` differs by database system.

### Foreign key example

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    department_id INT,
    FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);
```

### IDENTITY in SQL Server

`IDENTITY(seed, increment)` generates numeric values automatically when rows are inserted.

```sql
CREATE TABLE students (
    student_id INT IDENTITY(1,1) PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL
);
```

`IDENTITY(1,1)` starts at 1 and increments by 1. Other databases use their own identity or auto-increment syntax.

---

## 9. SELECT — Retrieving Data

`SELECT` retrieves data and returns a **result set**.

### Select all columns

```sql
SELECT *
FROM students;
```

### Select specific columns

```sql
SELECT student_name, marks
FROM students;
```

**Best practice:** select only the columns required by the query instead of using `SELECT *` in production code when unnecessary.

---

## 10. WHERE and SQL Operators

`WHERE` filters rows according to a condition.

```sql
SELECT student_name, marks
FROM students
WHERE marks >= 80;
```

### Common operators

| Operator | Purpose |
|---|---|
| `=` | Equal to |
| `<>` or `!=` | Not equal to; `!=` support varies |
| `>` / `<` | Greater than / less than |
| `>=` / `<=` | Greater than or equal to / less than or equal to |
| `AND` | Both conditions must be true |
| `OR` | At least one condition must be true |
| `IN` | Matches a value in a list or subquery |
| `NOT IN` | Excludes listed values; be careful if the list contains `NULL` |
| `BETWEEN` | Checks an inclusive range |
| `IS NULL` | Checks for `NULL` |
| `IS NOT NULL` | Checks for non-`NULL` values |
| `LIKE` | Pattern matching |

### Examples

```sql
SELECT *
FROM students
WHERE marks BETWEEN 70 AND 90;

SELECT *
FROM students
WHERE student_name IN ('Aman', 'Riya');

SELECT *
FROM students
WHERE marks IS NOT NULL;
```

**Important:** use `IS NULL` or `IS NOT NULL` to test for `NULL`; do not use `= NULL`.

**Important:** if a `NOT IN` list or subquery returns `NULL`, the result may be surprising because of SQL's three-valued logic. `NOT EXISTS` can be a safer alternative in some cases.

---

## 11. LIKE — Pattern Matching

In SQL Server, `LIKE` supports these common pattern symbols:

| Pattern | Meaning |
|---|---|
| `%` | Zero or more characters |
| `_` | Exactly one character |
| `[abc]` | One character from the specified set |
| `[a-f]` | One character in the specified range |
| `[^a-f]` | One character not in the specified range |

### Examples

```sql
-- Starts with 'Mr.'
SELECT * FROM employees
WHERE employee_name LIKE 'Mr.%';

-- Ends with 'Kumar'
SELECT * FROM employees
WHERE employee_name LIKE '%Kumar';

-- Contains 'Kumar'
SELECT * FROM employees
WHERE employee_name LIKE '%Kumar%';

-- Exactly two characters: starts with 'a'
SELECT * FROM employees
WHERE employee_code LIKE 'a_';

-- Exactly five characters
SELECT * FROM employees
WHERE employee_code LIKE '_____';

-- Starts with a, b, or c (SQL Server pattern syntax)
SELECT * FROM employees
WHERE employee_name LIKE '[abc]%';
```

Bracket patterns such as `[abc]` are SQL Server-specific. Other databases may use different syntax. For date filtering, prefer date ranges or date functions instead of matching formatted dates with `LIKE`.

---

## 12. ORDER BY — Sorting Results

`ORDER BY` sorts a result set by one or more columns.

```sql
SELECT student_name, marks
FROM students
ORDER BY marks DESC;
```

- `ASC`: ascending order; default in SQL Server.
- `DESC`: descending order.

Multiple sort columns:

```sql
SELECT *
FROM employees
ORDER BY department_id ASC, salary DESC;
```

---

## 13. Aggregate Functions

Aggregate functions calculate a value from a set of rows.

| Function | Purpose |
|---|---|
| `MIN()` | Smallest value |
| `MAX()` | Largest value |
| `SUM()` | Sum of numeric values |
| `AVG()` | Average of numeric values |
| `COUNT(*)` | Counts rows |
| `COUNT(column_name)` | Counts non-`NULL` values in that column |

### Example

```sql
SELECT
    COUNT(*) AS total_students,
    AVG(marks) AS average_marks,
    MAX(marks) AS highest_marks,
    MIN(marks) AS lowest_marks
FROM students;
```

Most aggregate functions ignore `NULL` values; `COUNT(*)` counts rows regardless of whether individual columns contain `NULL`.

---

## 14. GROUP BY — Grouping Rows

`GROUP BY` groups rows with the same value(s), allowing aggregate calculations per group.

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id;
```

When using `GROUP BY`, selected expressions generally must be grouped expressions or aggregates (subject to database-specific rules).

---

## 15. HAVING — Filtering Groups

`HAVING` filters groups, often using aggregate functions. `WHERE` filters individual rows before grouping.

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5
ORDER BY employee_count DESC;
```

### WHERE vs. HAVING

| `WHERE` | `HAVING` |
|---|---|
| Filters rows before grouping | Filters groups after grouping |
| Normally used for row-level conditions | Commonly used with aggregate conditions |

---

## 16. UPDATE — Modifying Data

`UPDATE` changes existing values.

```sql
UPDATE employees
SET salary = 50000
WHERE employee_id = 10;
```

### Update multiple columns

```sql
UPDATE employees
SET department_id = 2,
    salary = 55000
WHERE employee_id = 10;
```

**Safety warning:** without a `WHERE` clause, every row in the table is targeted. For important updates, verify the filter with a `SELECT` first and use a transaction where appropriate.

---

## 17. DELETE, TRUNCATE, and DROP

### DELETE

Removes selected rows.

```sql
DELETE FROM employees
WHERE employee_id = 10;
```

Remove all rows but retain the table structure:

```sql
DELETE FROM employees;
```

Without `WHERE`, all rows are targeted.

### TRUNCATE TABLE

Removes all rows while retaining the table structure.

```sql
TRUNCATE TABLE employees;
```

In SQL Server, `TRUNCATE TABLE` has restrictions, resets an identity counter to its seed, and is transactional in supported transaction contexts. It is not interchangeable with `DELETE` in every situation.

### DROP

Removes a database object.

```sql
DROP TABLE employees;
DROP DATABASE database_name;
```

Dropping a table removes its structure and data.

### DELETE vs. TRUNCATE vs. DROP

| Feature | `DELETE` | `TRUNCATE TABLE` | `DROP TABLE` |
|---|---|---|---|
| Removes rows | Yes | All rows | Removes the table itself |
| Supports `WHERE` | Yes | No | No |
| Keeps table structure | Yes | Yes | No |
| Can remove selected rows | Yes | No | No |
| Identity behavior in SQL Server | Usually does not reset identity | Resets identity to seed | Table is removed |

Recovery depends on the database, transaction state, configuration, and whether the changes have been committed. Do not assume a destructive operation is recoverable.

---

## 18. ALTER TABLE — Changing Structure

`ALTER TABLE` changes an existing table's structure.

### Add a column

```sql
ALTER TABLE employees
ADD email VARCHAR(150);
```

### Drop a column

```sql
ALTER TABLE employees
DROP COLUMN email;
```

### Change a column's type in SQL Server

```sql
ALTER TABLE employees
ALTER COLUMN employee_name VARCHAR(200);
```

`ALTER TABLE` can also add or drop constraints. Exact syntax and restrictions depend on the database system and the existing data.

---

## 19. DISTINCT — Removing Duplicate Results

`DISTINCT` removes duplicate rows from the query result.

```sql
SELECT DISTINCT department_id
FROM employees;
```

With multiple columns, it returns unique combinations of the selected columns:

```sql
SELECT DISTINCT department_id, job_title
FROM employees;
```

`DISTINCT` applies to the combination of selected expressions, not to one selected column independently when several columns are selected.

---

## 20. Subqueries

A **subquery** is a query nested inside another SQL statement. Its result can be used for filtering, comparison, or calculation.

### Example: values above the average

```sql
SELECT student_name, marks
FROM students
WHERE marks > (
    SELECT AVG(marks)
    FROM students
);
```

This returns students whose marks are above the overall average.

### Common types

- **Scalar subquery:** returns a single value.
- **Multi-row subquery:** returns multiple rows.
- **Correlated subquery:** refers to a value from the outer query.
- **Nested subquery:** a subquery inside another subquery.

Common operators used with subqueries include `IN`, `EXISTS`, `ANY`, `ALL`, and comparison operators such as `=`.

### Example using IN

```sql
SELECT employee_name
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE department_name = 'IT'
);
```

### Example using EXISTS

```sql
SELECT d.department_id, d.department_name
FROM departments AS d
WHERE EXISTS (
    SELECT 1
    FROM employees AS e
    WHERE e.department_id = d.department_id
);
```

A subquery is not guaranteed to execute first in every physical execution plan. The database optimizer may transform the query.

---

## 21. OFFSET and FETCH — Pagination

In SQL Server, `OFFSET` and `FETCH` support pagination and require `ORDER BY`.

```sql
SELECT employee_id, employee_name
FROM employees
ORDER BY employee_id
OFFSET 10 ROWS
FETCH NEXT 10 ROWS ONLY;
```

- `OFFSET 10 ROWS`: skips the first 10 rows.
- `FETCH NEXT 10 ROWS ONLY`: returns the next 10 rows.

A stable, unique ordering is recommended for reliable pagination. For example, use a unique ID as the final sort column when other sort columns may contain duplicate values.

---

## 22. CTE — Common Table Expression

A **Common Table Expression (CTE)** names a query result for use by the immediately following statement. It can improve query readability and organization.

```sql
WITH high_salary_employees AS (
    SELECT employee_id, employee_name, salary
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary_employees;
```

A CTE is not automatically a stored or persistent table; its scope is limited to the statement that follows it.

### CTE vs. subquery

- A CTE gives a query a name and can make complex logic easier to read.
- A subquery is embedded directly inside another query.
- Neither automatically means the result is permanently stored.
- Performance depends on the query and database optimizer; a CTE is not automatically faster.

---

## 23. Logical Processing Order of a SELECT Query

A useful simplified logical processing order is:

`FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY` → `OFFSET/FETCH`

This is the **logical** order used to understand query semantics, not necessarily the physical order chosen by the query optimizer.

---

## 24. High-Value Interview Questions for Revision

### Q1. What is the difference between `WHERE` and `HAVING`?
`WHERE` filters rows before grouping; `HAVING` filters groups, often using aggregate functions.

### Q2. What is the difference between `COUNT(*)` and `COUNT(column)`?
`COUNT(*)` counts rows. `COUNT(column)` counts only rows where that column is not `NULL`.

### Q3. What is the difference between `DELETE` and `DROP`?
`DELETE` removes rows while keeping the table structure. `DROP` removes the table object and its structure.

### Q4. What is the difference between `PRIMARY KEY` and `FOREIGN KEY`?
A primary key uniquely identifies rows in its own table. A foreign key enforces a reference to a key in another table or a supported self-reference.

### Q5. What is the difference between `GROUP BY` and `ORDER BY`?
`GROUP BY` groups rows for aggregate calculations. `ORDER BY` sorts the resulting rows.

### Q6. What is a subquery?
A query nested inside another SQL statement whose result is used by the outer statement.

### Q7. What is a CTE?
A named query expression available to the immediately following statement, commonly used to improve readability.

### Q8. Why should you be careful with `UPDATE` and `DELETE`?
Without a `WHERE` clause, all rows may be affected. Check the target rows before running destructive statements.

### Q9. How do you check for `NULL`?
Use `IS NULL` or `IS NOT NULL`, not `= NULL`.

### Q10. What is pagination?
Pagination returns a limited portion of a result set, often using `ORDER BY` with `OFFSET` and `FETCH` in SQL Server.

---

## 25. Topics Not Covered in This File

These topics were listed as pending in the source notes or maintained separately:

- String/text, numeric, and date functions.
- Stored procedures.
- User-defined functions.
- Triggers.
- Views.
- Indexes.
- **JOIN:** kept in the separate `SQL_JOIN_Complete_Notes.md` file.
- **Set operators:** kept in the separate `SQL_SET_Operators_Notes.md` file.

---

## Quick Revision Checklist

- [ ] Database, DBMS, RDBMS, and SQL definitions
- [ ] DDL, DML, DQL, DCL, and TCL
- [ ] SQL data types
- [ ] ER diagrams and entities
- [ ] `CREATE` and `INSERT`
- [ ] Constraints and keys
- [ ] `SELECT`, `WHERE`, and operators
- [ ] `LIKE` patterns
- [ ] `ORDER BY` and aggregate functions
- [ ] `GROUP BY` and `HAVING`
- [ ] `UPDATE`, `DELETE`, `TRUNCATE`, `DROP`, and `ALTER`
- [ ] `DISTINCT`
- [ ] Subqueries, `EXISTS`, and `IN`
- [ ] `OFFSET` / `FETCH`
- [ ] CTEs
- [ ] Logical query processing order
