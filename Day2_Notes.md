## Table of Contents:
1) [SQL Math Functions](1-sql-math-functions)
2) [SQL Aggregate Functions](2-sql-aggregate-functions)
3) [SQL String Functions](3-sql-string-functions)
4) [SQL Date Functions](4-sql-date-functions)
5) [GROUP BY](5-group-by) 
6) [HAVING](6-having)
7) [ORDER BY, LIMIT, OFFSET (and FETCH)](7-order-by)

### 1. SQL Math Functions
Math functions perform operations on numerical data to manipulate values or calculate new ones. They are used primarily in the SELECT clause or the WHERE clause.

Common Math Functions:

- ROUND(column, decimals): Rounds a number to a specified number of decimal places.

- CEIL() / CEILING(): Rounds a number up to the nearest integer.

- FLOOR(): Rounds a number down to the nearest integer.

- ABS(column): Returns the absolute value of a number.

- POWER(column, exponent): Raises a number to the power of the specified exponent.

- SQRT(column): Returns the square root of a number.

- Basic operators: +, -, *, /

```sql
-- Calculate a 15% tip on a bill
SELECT bill_amount, 
       bill_amount * 0.15 AS tip 
FROM orders;

-- Round the average salary to 2 decimal places
SELECT ROUND(AVG(salary), 2) AS avg_salary 
FROM employees;

-- Find the square root of a value
SELECT SQRT(49) AS root; -- Returns 7
```


### 2. SQL Aggregate Functions
Aggregate functions perform a calculation on a set of rows and return a single summary value. They are almost always used with the GROUP BY clause.

Common Aggregate Functions:

- COUNT(): Returns the number of rows.

   - COUNT(*) counts all rows, including those with NULLs.

   - COUNT(column) counts non-NULL values in a specific column.

- SUM(): Returns the sum of all values in a column.

- AVG(): Returns the average value of a column.

- MIN(): Returns the smallest value in a column.

- MAX(): Returns the largest value in a column.

Key Point: Aggregate functions ignore NULL values (except for COUNT(*)).

```sql
-- How many employees are there?
SELECT COUNT(employee_id) AS total_employees 
FROM employees;

-- What is the total salary paid to all employees?
SELECT SUM(salary) AS total_salary_budget 
FROM employees;

-- What is the highest and lowest salary?
SELECT MAX(salary) AS highest_salary,
       MIN(salary) AS lowest_salary 
FROM employees;
```


### 3. SQL String Functions
String functions manipulate and return information about text data.

Common String Functions:

- CONCAT(str1, str2, ...): Combines two or more strings into one.

- LENGTH(str) / LEN(str): Returns the number of characters in a string.

- UPPER(str) / LOWER(str): Converts a string to upper/lower case.

- TRIM(str): Removes leading and trailing spaces.

- SUBSTRING(str, start, length) / SUBSTR(str, start, length): Extracts a substring from a string.

- REPLACE(str, old_text, new_text): Replaces all occurrences of a substring.

```sql
-- Combine first and last name into a full name
SELECT CONCAT(first_name, ' ', last_name) AS full_name 
FROM employees;

-- Find the length of each product name
SELECT product_name, LENGTH(product_name) AS name_length 
FROM products;

-- Get the domain from an email (e.g., from 'alice@github.com')
SELECT email,
       SUBSTRING(email FROM '@(.+)$') AS domain -- Syntax may vary by database
FROM users;

-- Standard SQL version might use SUBSTRING(email, POSITION('@' IN email) + 1)
```

### 4. SQL Date Functions
Part 1: Getting the Current Date, Time , Date Time
```sql
SELECT CURRENT_DATE; -- today's date
SELECT CURRENT_TIME; -- current time
SELECT NOW(); -- current date and time
```

Part 2: Extracting parts of a date
```sql
SELECT EXTRACT(YEAR FROM date_column);     -- 2025
SELECT EXTRACT(MONTH FROM date_column);    -- 9
SELECT TO_CHAR(date_column, 'Month');   -- September
SELECT EXTRACT(DAY FROM date_column);      -- 9
SELECT EXTRACT(DOW FROM date_column);      -- 2 (0=Sunday, 1=Monday...)
```

Part 3: Adding and Subtracting Dates
- Key Concept: Use the INTERVAL keyword to define a period of time.
```sql
SELECT date_column + INTERVAL '7 days';    -- 7 days later
SELECT date_column - INTERVAL '1 month';   -- 1 month before
SELECT DATE_ADD(order_date, INTERVAL 30 DAY) as due_date,
SELECT DATEDIFF(CURRENT_DATE, order_date) as days_since_order
-- Add 1 year, 2 months, and 5 days to today
SELECT NOW() + INTERVAL '1 year 2 months 5 days' AS future_datetime;
SELECT payment_date - order_date AS days_to_pay
```

Part 4: Casting strings to date
```sql
SELECT '2025-09-09'::date;   -- 2025-09-09
SELECT TO_DATE('09-09-2025', 'DD-MM-YYYY'); -- 2025-09-09
```


### 5. GROUP BY Clause
The GROUP BY clause groups rows that have the same values in specified columns into summary rows. It is essential for using aggregate functions effectively.

- What it does: Collapses multiple rows into a single group for each unique combination of values in the GROUP BY columns.

- Key Rule: Every column in your SELECT statement that is not inside an aggregate function must be listed in the GROUP BY clause.

```sql
-- Count the number of employees in each department
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;

-- Find the total sales for each product
SELECT product_id, SUM(sale_amount) AS total_revenue
FROM sales
GROUP BY product_id;

-- Find the average salary by department and country
SELECT department, country, AVG(salary) AS avg_salary
FROM employees
GROUP BY department, country; -- Groups by unique dept/country pairs
```


### 6. HAVING Clause
The HAVING clause is used to filter groups created by the GROUP BY clause. It is like a WHERE clause, but for aggregated data.

- WHERE vs. HAVING:

   - WHERE filters individual rows before they are grouped.

   - HAVING filters groups after the aggregation has occurred.

- Key Point: Because HAVING works on aggregated data, you can use aggregate functions in its condition.

```sql
-- Find departments with more than 10 employees
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 10; -- Filter on the aggregated result

-- Find products with total revenue over $10,000
SELECT product_id, SUM(sale_amount) AS total_revenue
FROM sales
GROUP BY product_id
HAVING SUM(sale_amount) > 10000;

-- Find countries where the average salary is over 50000
SELECT country, AVG(salary) AS avg_salary
FROM employees
GROUP BY country
HAVING AVG(salary) > 50000;
```


### 7. ORDER BY, LIMIT, OFFSET (and FETCH)
These clauses control the final presentation of your result set and are executed at the very end of the query.

ORDER BY
- Purpose: Sorts the final result set by one or more columns.

- Default: Ascending order (ASC). Use DESC for descending order.

- You can sort by: column names, column aliases, or even the numerical position of the column in the SELECT list (e.g., ORDER BY 1).

LIMIT / OFFSET (and FETCH)
- LIMIT n: Restricts the number of rows returned to n. Extremely useful for pagination or getting top-N results.

- OFFSET m: Skips the first m rows before starting to return rows. Used with LIMIT for pagination.

- Standard SQL Equivalent: The SQL standard uses OFFSET ... FETCH ... which is more verbose but supported in databases like PostgreSQL and SQL Server.

  - LIMIT 5 OFFSET 10 is equivalent to OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY.

```sql
-- Get the top 10 highest-paid employees
SELECT first_name, last_name, salary
FROM employees
ORDER BY salary DESC
LIMIT 10;

-- Pagination: Get rows 11 to 20 (e.g., page 2 of results with 10 items per page)
SELECT product_name, price
FROM products
ORDER BY product_name
LIMIT 10 OFFSET 10; -- Skip first 10, return next 10

-- Standard SQL version of pagination
SELECT product_name, price
FROM products
ORDER BY product_name
OFFSET 10 ROWS FETCH NEXT 10 ROWS ONLY;

-- Sort by an alias defined in the SELECT clause
SELECT department, AVG(salary) AS avg_sal
FROM employees
GROUP BY department
ORDER BY avg_sal DESC;
```



