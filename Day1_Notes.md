## Table of Contents:
1) [SELECT Statement](#1-select-statement)
2) [DISINCT Function](#2-distinct-function)
3) [Aliases](#3-aliases)
4) [WHERE](#4-where)
5) [Wild Cards (LIKE operators)](#5-wild-cards)

### 1. SELECT statement
- The SELECT statement is the most fundamental and used SQL command.
- It's basic function is to retrieve columns from tables stored in a database.

- Syntax: 
```shell
SELECT 
     column1, 
     column2,
     column X
FROM table_name;
```

Key Points:

- You can select specific columns by name, separated by commas.

- The asterisk * is a wildcard that means "all columns".

- Although SELECT is written first, it is not the first clause to be executed logically (execution starts with the FROM clause).

```sql
-- Select all columns from the 'employees' table
SELECT * FROM employees;

-- Select only the 'name' and 'salary' columns from the 'employees' table
SELECT full_name, salary FROM employees;
```

### 2. DISTINCT Function
- The DISTINCT keyword is used to remove duplicate rows from the result set. It ensures that all values in the specified column(s) are unique.

- What it does: Filters out duplicate values, returning only distinct (different) values.

- Syntax: SELECT DISTINCT column1, column2, ... FROM table_name;

Key Points:

- It operates on the combination of values from all columns listed after it.

- DISTINCT is applied after the SELECT clause processes the raw data.

```sql
-- Find all unique job titles in the company
SELECT DISTINCT job_title FROM employees;

-- Find all unique combinations of department and country
SELECT DISTINCT department, country FROM offices;
```

### 3. Aliases (AS)
SQL aliases are used to give a table or a column a temporary, more readable name for the duration of a query.

- What it does: Renames a column or table in the output.

- Syntax for a Column: SELECT column_name AS alias_name FROM table_name;
*The AS keyword is optional but recommended for clarity.*

- Syntax for a Table: SELECT column_name FROM table_name AS alias_name;

Key Points:

- The alias only exists for the duration of the query and does not change the original name in the database.

- Aliases are crucial when using functions or calculations.

- If an alias contains spaces, it must be enclosed in double quotes " " or square brackets [ ] (Full Name).


### 4. WHERE
The WHERE clause is used to filter records. It extracts only those records that fulfill a specified condition.

- What it does: Acts as a filter on the rows returned from the FROM clause.

- Syntax: SELECT column1, column2 FROM table_name WHERE condition;

Key Points:

- It is executed after the FROM clause but before the SELECT clause.

- You cannot use column aliases from the SELECT clause in the WHERE clause because the WHERE clause is processed first.

- Use comparison operators: =, <> or != (not equal), >, <, >=, <=.

- Use logical operators: AND, OR, NOT.


### 5. Wild Cards (LIKE operator)
The LIKE operator is used in a WHERE clause to search for a specified pattern in a column. It is always used with wildcard characters.

- What it does: Enables pattern matching instead of exact matching.

Wildcard Symbols:

- % : The percent sign represents zero, one, or multiple characters.

- _ : The underscore represents a single character.

- Syntax: SELECT column1, column2 FROM table_name WHERE columnN LIKE pattern;

Key Points:

- LIKE is not case-sensitive in many database systems (like MySQL), but it is case-sensitive in others (like PostgreSQL). Always check your database's documentation.

- Pattern matching can be slower than other filters on large datasets.

```sql
-- Select all customers with a name starting with 'A'
SELECT full_name FROM customers WHERE customer_name LIKE 'A%';

-- Select all products whose name ends with 'phone'
SELECT product_id, product_name FROM products WHERE product_name LIKE '%phone';

-- Find any name that has 'or' in any position
SELECT full_name FROM employees WHERE name LIKE '%or%';

-- Select all customers with a name that has 'r' as the second letter
SELECT full_name FROM customers WHERE customer_name LIKE '_r%';

-- Find names that start with 'J' and are at least 3 characters long
SELECT full_name FROM employees WHERE name LIKE 'J__%';
```


