
# SQL Functions 

## 1. String / Text Functions

String functions are used to manipulate, search, extract, format, and combine text values.

### 1.1 Basic String Operations

| Function | Parameters | Description | Example | Result |
|---|---:|---|---|---|
| `LENGTH(str)` | 1 | Returns the length of a string; supported in MySQL and other systems | `LENGTH('Hello')` | `5` |
| `UPPER(str)` | 1 | Converts text to uppercase | `UPPER('hello')` | `'HELLO'` |
| `LOWER(str)` | 1 | Converts text to lowercase | `LOWER('HELLO')` | `'hello'` |
| `TRIM(str)` | 1 | Removes leading and trailing spaces | `TRIM(' Hi ')` | `'Hi'` |
| `LTRIM(str)` | 1 | Removes leading spaces | `LTRIM(' Hi')` | `'Hi'` |
| `RTRIM(str)` | 1 | Removes trailing spaces | `RTRIM('Hi ')` | `'Hi'` |

**SQL Server note:** Use `LEN()` to get a string's length in SQL Server. Unlike `LENGTH()` in many other databases, `LEN()` does not count trailing spaces.

```sql
SELECT UPPER('hello') AS upper_text;
SELECT LOWER('HELLO') AS lower_text;
SELECT TRIM('  Hello  ') AS trimmed_text;
SELECT LEN('Hello') AS string_length; -- SQL Server
```

### 1.2 String Extraction and Replacement

| Function | Parameters | Description | Example | Result |
|---|---:|---|---|---|
| `SUBSTRING(str, start, len)` | 3 | Extracts part of a string | `SUBSTRING('Hello', 2, 3)` | `'ell'` |
| `LEFT(str, length)` | 2 | Returns leftmost characters | `LEFT('Hello', 2)` | `'He'` |
| `RIGHT(str, length)` | 2 | Returns rightmost characters | `RIGHT('Hello', 3)` | `'llo'` |
| `CHARINDEX(sub, str)` | 2 | Finds the position of a substring in SQL Server | `CHARINDEX('l', 'Hello')` | `3` |
| `REPLACE(str, old, new)` | 3 | Replaces occurrences of a substring | `REPLACE('Hello', 'l', 'x')` | `'Hexxo'` |

**Examples:**

```sql
SELECT SUBSTRING('Hello World', 1, 5) AS extracted_text;
SELECT LEFT('Hello', 2) AS left_text;
SELECT RIGHT('Hello', 3) AS right_text;
SELECT CHARINDEX('l', 'Hello') AS character_position;
SELECT REPLACE('Hello', 'l', 'x') AS replaced_text;
```

### 1.3 String Manipulation

| Function | Description | Example | Result |
|---|---|---|---|
| `CONCAT(str1, str2, ...)` | Combines strings | `CONCAT('Hello', ' ', 'World')` | `'Hello World'` |
| `CONCAT_WS(sep, str1, str2, ...)` | Combines strings using a separator | `CONCAT_WS('-', '2024', '12')` | `'2024-12'` |
| `REVERSE(str)` | Reverses a string | `REVERSE('Hello')` | `'olleH'` |
| `REPEAT(str, count)` | Repeats a string; available in MySQL | `REPEAT('Ha', 3)` | `'HaHaHa'` |
| `REPLICATE(str, count)` | Repeats a string in SQL Server | `REPLICATE('0', 5)` | `'00000'` |

**Examples:**

```sql
SELECT CONCAT('Hello', ' ', 'World') AS full_text;

SELECT CONCAT_WS('-', '2024', '12', '17') AS formatted_text;

SELECT REVERSE('Hello') AS reversed_text;

SELECT REPLICATE('0', 5) AS repeated_text; -- SQL Server
```

**Note:** `CONCAT_WS()` skips `NULL` values in supported database systems. Check your database's documentation for exact behavior.

### 1.4 String Padding and Formatting

| Function | Description | Example | Result |
|---|---|---|---|
| `LPAD(str, len, pad)` | Pads the left side of a string | `LPAD('5', 3, '0')` | `'005'` |
| `RPAD(str, len, pad)` | Pads the right side of a string | `RPAD('5', 3, '0')` | `'500'` |
| `INITCAP(str)` | Capitalizes the first letter of words in supported systems | `INITCAP('hello world')` | `'Hello World'` |
| `SPACE(count)` | Returns a specified number of spaces in SQL Server | `SPACE(3)` | Three spaces |

**Important:** `LPAD()`, `RPAD()`, and `INITCAP()` are not supported in every database system. In SQL Server, use `REPLICATE()` and string operations when equivalent padding is needed.

### 1.5 Special String Functions

| Function | Description | Example | Result |
|---|---|---|---|
| `ASCII(char)` | Returns the ASCII code of a character | `ASCII('A')` | `65` |
| `CHAR(code)` | Returns the character for a code in SQL Server | `CHAR(65)` | `'A'` |
| `SOUNDEX(str)` | Returns a phonetic representation | `SOUNDEX('Smith')` | `'S530'` |
| `DIFFERENCE(str1, str2)` | Compares Soundex values in SQL Server | `DIFFERENCE('Smith', 'Smyth')` | `4` |
| `FORMAT(value, format)` | Formats a value for display in SQL Server | `FORMAT(1234.5, 'C', 'en-US')` | Currency-formatted text |

**Example:**

```sql
SELECT ASCII('A') AS ascii_code;
SELECT CHAR(65) AS character_value;
SELECT SOUNDEX('Smith') AS soundex_value;
SELECT DIFFERENCE('Smith', 'Smyth') AS soundex_difference;
SELECT FORMAT(1234.5, 'C', 'en-US') AS formatted_currency;
```

**Interview point:** `FORMAT()` is mainly for presentation. Avoid relying on formatted strings for numerical calculations or sorting when the underlying numeric value is available.

---

## 2. Date and Time Functions

Date and time functions help retrieve the current date/time, extract date parts, perform date calculations, and format or convert date values.

### 2.1 Current Date and Time

| Function | Database / Purpose | Description |
|---|---|---|
| `GETDATE()` | SQL Server | Returns the current date and time |
| `SYSDATETIME()` | SQL Server | Returns current date and time with higher fractional-second precision |
| `GETUTCDATE()` | SQL Server | Returns the current UTC date and time |
| `NOW()` | MySQL | Returns the current date and time |
| `CURRENT_TIMESTAMP` | Standard SQL; supported by many systems | Returns the current date and time |
| `CURRENT_DATE` | Supported by MySQL and other systems | Returns the current date |
| `CURRENT_TIME` | Supported by MySQL and other systems | Returns the current time |
| `SYSDATE` | Oracle | Returns the system date and time |

**Examples:**

```sql
-- SQL Server
SELECT GETDATE() AS current_datetime;
SELECT SYSDATETIME() AS precise_datetime;
SELECT GETUTCDATE() AS current_utc_datetime;

-- MySQL
SELECT NOW() AS current_datetime;
SELECT CURRENT_DATE AS current_date;
SELECT CURRENT_TIME AS current_time;

-- Oracle
SELECT SYSDATE FROM dual;
```

The returned date and time depend on the server, session settings, data type, and database system.

### 2.2 Date Extraction Functions

| Function | Description | Example | Result |
|---|---|---|---|
| `YEAR(date)` | Extracts the year | `YEAR('2024-12-17')` | `2024` |
| `MONTH(date)` | Extracts the month | `MONTH('2024-12-17')` | `12` |
| `DAY(date)` | Extracts the day of the month | `DAY('2024-12-17')` | `17` |
| `DATEPART(part, date)` | Extracts a specified date part in SQL Server | `DATEPART(MONTH, '2024-12-17')` | `12` |
| `DATENAME(part, date)` | Returns the name of a date part in SQL Server | `DATENAME(MONTH, '2024-12-17')` | `'December'` |
| `EXTRACT(part FROM date)` | Extracts a date part in supported systems | `EXTRACT(YEAR FROM date)` | `2024` |

**Examples in SQL Server:**

```sql
SELECT YEAR('2024-12-17') AS year_value;
SELECT MONTH('2024-12-17') AS month_value;
SELECT DAY('2024-12-17') AS day_value;

SELECT DATEPART(MONTH, '2024-12-17') AS month_number;
SELECT DATENAME(MONTH, '2024-12-17') AS month_name;
```

**Important:** `DATEPART(WEEK, date)` can depend on SQL Server settings. Week-number results may differ between database systems or configurations.

### 2.3 Time Extraction Functions

| Function | Description | Example | Result |
|---|---|---|---|
| `HOUR(time)` | Extracts the hour in supported systems | `HOUR('14:30:45')` | `14` |
| `MINUTE(time)` | Extracts the minute in supported systems | `MINUTE('14:30:45')` | `30` |
| `SECOND(time)` | Extracts the second in supported systems | `SECOND('14:30:45')` | `45` |
| `DATEPART(hour, datetime)` | Extracts the hour in SQL Server | `DATEPART(HOUR, GETDATE())` | Current hour |

**SQL Server example:**

```sql
SELECT DATEPART(HOUR, GETDATE()) AS current_hour;
SELECT DATEPART(MINUTE, GETDATE()) AS current_minute;
SELECT DATEPART(SECOND, GETDATE()) AS current_second;
```

### 2.4 Date Arithmetic

Date arithmetic is used to add or subtract intervals and calculate date differences.

| Function | Database | Description |
|---|---|---|
| `DATEADD(part, number, date)` | SQL Server | Adds or subtracts a specified date interval |
| `DATEDIFF(part, start, end)` | SQL Server | Counts date-part boundaries between two values |
| `DATE_ADD(date, INTERVAL ...)` | MySQL | Adds an interval to a date |
| `DATE_SUB(date, INTERVAL ...)` | MySQL | Subtracts an interval from a date |
| `ADD_MONTHS(date, months)` | Oracle | Adds a number of months to a date |

**SQL Server examples:**

```sql
-- Add 7 days
SELECT DATEADD(DAY, 7, '2024-12-17') AS future_date;

-- Subtract 7 days
SELECT DATEADD(DAY, -7, '2024-12-17') AS previous_date;

-- Add 3 months
SELECT DATEADD(MONTH, 3, '2024-12-17') AS future_month;

-- Calculate day-boundary difference
SELECT DATEDIFF(DAY, '2024-12-10', '2024-12-17') AS day_difference;
```

**MySQL examples:**

```sql
SELECT DATE_ADD('2024-12-17', INTERVAL 7 DAY) AS future_date;

SELECT DATE_SUB('2024-12-17', INTERVAL 7 DAY) AS previous_date;

SELECT DATE_ADD('2024-12-17', INTERVAL 1 MONTH) AS future_month;
```

**Interview point:** In SQL Server, `DATEDIFF()` counts the specified date-part boundaries crossed; it does not always represent a complete elapsed duration. Choose the date part carefully.

### 2.5 Date Formatting and Conversion

| Function | Database | Description |
|---|---|---|
| `CONVERT(type, value, style)` | SQL Server | Converts a value to a specified data type; style can control formatting |
| `CAST(value AS type)` | Standard SQL; supported by many systems | Converts a value to another data type |
| `FORMAT(value, format)` | SQL Server | Formats a value as text |
| `TO_DATE(str, format)` | Oracle and some other systems | Converts a string to a date |
| `TO_CHAR(value, format)` | Oracle and some other systems | Converts a date or number to formatted text |
| `STR_TO_DATE(str, format)` | MySQL | Parses a string into a date/time value |

**SQL Server examples:**

```sql
-- Convert a string to DATE
SELECT CAST('2024-12-17' AS DATE) AS converted_date;

-- Format date as dd/MM/yyyy
SELECT FORMAT(GETDATE(), 'dd/MM/yyyy') AS formatted_date;

-- Convert date to dd/MM/yyyy style
SELECT CONVERT(VARCHAR(10), GETDATE(), 103) AS formatted_date;
```

**Oracle examples:**

```sql
SELECT TO_DATE('17-12-2024', 'DD-MM-YYYY') FROM dual;

SELECT TO_CHAR(SYSDATE, 'DD-MON-YYYY') FROM dual;
```

**MySQL example:**

```sql
SELECT STR_TO_DATE('01,5,2013', '%d,%m,%Y') AS converted_date;
```

**Interview point:** A date value and a formatted date string are different. Keep values in date/time data types for filtering, comparison, and calculations; format them as text mainly when displaying results.

### 2.6 Special Date Functions

| Function | Database | Description | Example |
|---|---|---|---|
| `GETUTCDATE()` | SQL Server | Returns current UTC date and time | `GETUTCDATE()` |
| `EOMONTH(date)` | SQL Server | Returns the last day of the month | `EOMONTH('2024-12-17')` |
| `LAST_DAY(date)` | MySQL and Oracle | Returns the last day of the month | `LAST_DAY('2024-12-17')` |
| `ISDATE(expr)` | SQL Server | Checks whether an expression can be interpreted as a date/time value | `ISDATE('2024-12-17')` |
| `DATEFROMPARTS(y,m,d)` | SQL Server | Creates a date from year, month, and day | `DATEFROMPARTS(2024,12,17)` |
| `TIMEFROMPARTS(h,m,s, fractions, precision)` | SQL Server | Creates a time from its components | `TIMEFROMPARTS(14,30,45,0,0)` |

**Examples:**

```sql
-- SQL Server
SELECT EOMONTH('2024-12-17') AS month_end;

SELECT ISDATE('2024-12-17') AS is_valid_date;

SELECT DATEFROMPARTS(2024, 12, 17) AS created_date;

SELECT TIMEFROMPARTS(14, 30, 45, 0, 0) AS created_time;

-- MySQL
SELECT LAST_DAY('2024-12-17') AS month_end;
```

**Note:** `ISDATE()` behavior depends on SQL Server settings and input format. For robust date validation, use explicit conversions and appropriate error handling.

---

## 3. Mathematical Functions

Mathematical functions perform numeric calculations, rounding, power operations, logarithms, trigonometry, and other numeric tasks.

### 3.1 Basic Arithmetic and Rounding

| Function | Description | Example | Result |
|---|---|---|---|
| `ABS(x)` | Returns the absolute value | `ABS(-10)` | `10` |
| `CEILING(x)` | Returns the smallest integer greater than or equal to `x` | `CEILING(4.3)` | `5` |
| `FLOOR(x)` | Returns the largest integer less than or equal to `x` | `FLOOR(4.7)` | `4` |
| `ROUND(x, d)` | Rounds to `d` decimal places | `ROUND(4.567, 2)` | `4.57` |
| `TRUNCATE(x, d)` | Truncates to `d` decimal places in supported systems | `TRUNCATE(4.567, 2)` | `4.56` |

**Examples:**

```sql
SELECT ABS(-10) AS absolute_value;
SELECT CEILING(4.3) AS ceiling_value;
SELECT FLOOR(4.7) AS floor_value;
SELECT ROUND(4.567, 2) AS rounded_value;
```

**Note:** `TRUNCATE()` is supported in MySQL. SQL Server does not use that function name; use `ROUND(number, decimals, 1)` for truncation behavior.

```sql
-- SQL Server: third argument 1 truncates rather than rounds
SELECT ROUND(4.567, 2, 1) AS truncated_value;
```

### 3.2 Power, Roots, and Logarithms

| Function | Description | Example | Result |
|---|---|---|---|
| `POWER(x, y)` | Raises `x` to the power `y` | `POWER(2, 3)` | `8` |
| `SQRT(x)` | Returns the square root | `SQRT(16)` | `4` |
| `SQUARE(x)` | Returns the square of a number in SQL Server | `SQUARE(4)` | `16` |
| `EXP(x)` | Returns `e` raised to the power `x` | `EXP(1)` | Approximately `2.71828` |
| `LOG(x)` | Returns the natural logarithm in SQL Server | `LOG(10)` | Approximately `2.30259` |
| `LOG10(x)` | Returns the base-10 logarithm | `LOG10(100)` | `2` |
| `LOG(x, b)` or another base syntax | Logarithm with a specified base; syntax varies by database | Depends on database | Depends on input |

**Examples:**

```sql
SELECT POWER(2, 3) AS power_value;
SELECT SQRT(16) AS square_root;
SELECT SQUARE(4) AS square_value; -- SQL Server
SELECT EXP(1) AS exponential_value;
SELECT LOG(10) AS natural_logarithm; -- SQL Server
SELECT LOG10(100) AS base10_logarithm;
```

**Important:** The syntax for logarithms with a custom base differs by database. Check the target database before using a two-argument `LOG()` call.

### 3.3 Trigonometric Functions

Trigonometric functions are used for mathematical calculations involving angles. Most SQL implementations expect angles in **radians**.

| Function | Description | Example | Result |
|---|---|---|---|
| `SIN(x)` | Sine of `x` | `SIN(PI()/2)` | `1` |
| `COS(x)` | Cosine of `x` | `COS(0)` | `1` |
| `TAN(x)` | Tangent of `x` | `TAN(PI()/4)` | Approximately `1` |
| `ASIN(x)` | Inverse sine | `ASIN(1)` | Approximately `1.5708` |
| `ACOS(x)` | Inverse cosine | `ACOS(1)` | `0` |
| `ATAN(x)` | Inverse tangent | `ATAN(1)` | Approximately `0.7854` |
| `ATAN2(y, x)` | Angle determined from two coordinates | `ATAN2(1, 1)` | Approximately `0.7854` |

**Examples:**

```sql
SELECT SIN(PI() / 2) AS sine_value;
SELECT COS(0) AS cosine_value;
SELECT TAN(PI() / 4) AS tangent_value;
SELECT ASIN(1) AS inverse_sine;
SELECT ACOS(1) AS inverse_cosine;
SELECT ATAN(1) AS inverse_tangent;
```

Function availability and argument conventions may differ by database.

### 3.4 Sign and Modulo

| Function / Operator | Description | Example | Result |
|---|---|---|---|
| `SIGN(x)` | Returns `-1`, `0`, or `1` based on the sign of `x` | `SIGN(-10)` | `-1` |
| `MOD(x, y)` | Returns the remainder in supported systems | `MOD(10, 3)` | `1` |
| `%` | Modulo operator in SQL Server and several other systems | `10 % 3` | `1` |
| `DIV` | Integer-division operator in MySQL | `10 DIV 3` | `3` |

**Examples:**

```sql
SELECT SIGN(-10) AS sign_value;
SELECT 10 % 3 AS remainder; -- SQL Server
SELECT 10 DIV 3 AS integer_quotient; -- MySQL
```

**Interview point:** Modulo returns the remainder, while integer division returns the integer quotient according to the database's division rules.

### 3.5 Random Numbers and Constants

| Function | Description | Example |
|---|---|---|
| `RAND()` | Returns a pseudo-random number; exact range and behavior depend on the database | `RAND()` |
| `RAND(seed)` | Uses a seed in systems that support this syntax | `RAND(123)` |
| `PI()` | Returns the value of π | `PI()` |
| `DEGREES(x)` | Converts radians to degrees | `DEGREES(PI())` → `180` |
| `RADIANS(x)` | Converts degrees to radians | `RADIANS(180)` → Approximately `3.14159` |

**Examples:**

```sql
SELECT RAND() AS random_value;
SELECT PI() AS pi_value;
SELECT DEGREES(PI()) AS degrees_value;
SELECT RADIANS(180) AS radians_value;
```

**Important:** Random-number behavior, seeding, and repeatability vary by database and query execution context. Do not use ordinary SQL pseudo-random functions for security-sensitive random tokens.

---

## 4. Quick Interview Revision

### String functions
- `UPPER()` — converts text to uppercase.
- `LOWER()` — converts text to lowercase.
- `TRIM()` — removes leading and trailing spaces.
- `SUBSTRING()` — extracts part of a string.
- `LEFT()` / `RIGHT()` — extracts characters from either end.
- `REPLACE()` — replaces occurrences of text.
- `CONCAT()` — combines strings.
- `CHARINDEX()` — finds a substring's position in SQL Server.
- `LEN()` — returns string length in SQL Server, excluding trailing spaces.
- `LENGTH()` — returns string length in MySQL and other supported systems.

### Date and time functions
- `GETDATE()` — current date/time in SQL Server.
- `YEAR()`, `MONTH()`, `DAY()` — extract date components.
- `DATEADD()` — adds an interval in SQL Server.
- `DATEDIFF()` — counts date-part boundaries in SQL Server.
- `EOMONTH()` — returns the last day of a month in SQL Server.
- `CAST()` / `CONVERT()` — convert data types in SQL Server.
- `FORMAT()` — formats a value as text in SQL Server.

### Mathematical functions
- `ABS()` — absolute value.
- `CEILING()` — rounds upward to an integer.
- `FLOOR()` — rounds downward to an integer.
- `ROUND()` — rounds a number.
- `POWER()` — raises a number to a power.
- `SQRT()` — square root.
- `SUM()` / `AVG()` — aggregate functions covered in aggregate-function notes, rather than scalar mathematical functions.
- `%` — modulo operator in SQL Server.
- `PI()` — returns π.

---

## 5. Important Interview Questions

### Q1. What is a SQL function?
A SQL function performs an operation on input values and returns a result. Functions may manipulate strings, work with dates, perform mathematical calculations, or aggregate data.

### Q2. What is the difference between `LEN()` and `LENGTH()`?
`LEN()` is used in SQL Server and excludes trailing spaces. `LENGTH()` is used in MySQL and other database systems, with behavior depending on the system and character set.

### Q3. What is the difference between `CAST()` and `CONVERT()`?
Both can convert data types in SQL Server. `CONVERT()` also supports a style argument for certain conversions, such as date formatting. `CAST()` uses standard SQL-style syntax.

### Q4. What is the difference between `DATEADD()` and `DATEDIFF()`?
`DATEADD()` adds or subtracts a specified date interval. `DATEDIFF()` counts the specified date-part boundaries crossed between two values in SQL Server.

### Q5. What is the difference between `ROUND()` and truncation?
`ROUND()` rounds a value to a specified precision. Truncation removes digits beyond the specified precision without rounding them upward or downward.

### Q6. What is the difference between `CONCAT()` and `CONCAT_WS()`?
`CONCAT()` joins values together. `CONCAT_WS()` joins values using a separator between them, where supported.

### Q7. Why should dates be stored in date/time data types instead of formatted strings?
Date/time data types support proper date comparisons, sorting, filtering, and calculations. Formatted strings are primarily useful for display.

### Q8. Are SQL functions identical in SQL Server, MySQL, and Oracle?
No. Function names, parameters, return types, formatting rules, and supported syntax can differ. Always check the target database's documentation.

---

## 6. Database-Specific Syntax Reminder

| Task | SQL Server | MySQL | Oracle |
|---|---|---|---|
| Current date/time | `GETDATE()` | `NOW()` | `SYSDATE` |
| String length | `LEN()` | `LENGTH()` | `LENGTH()` |
| Add date interval | `DATEADD()` | `DATE_ADD()` | `ADD_MONTHS()` for months |
| Format date as text | `FORMAT()` / `CONVERT()` | `DATE_FORMAT()` | `TO_CHAR()` |
| Parse string as date | `CAST()` / `CONVERT()` | `STR_TO_DATE()` | `TO_DATE()` |
| Last day of month | `EOMONTH()` | `LAST_DAY()` | `LAST_DAY()` |
| Repeat string | `REPLICATE()` | `REPEAT()` | Syntax differs |

**Final interview tip:** First identify which database the interviewer is asking about. Then give the function's purpose, syntax for that database, and one short example.
