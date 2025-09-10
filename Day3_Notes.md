## Table of Contents:
1) [INNER JOIN](#1-inner-join)
2) [LEFT JOIN](#2-left-join) 
3) [RIGHT JOIN](#3-right-join)
4) [FULL JOIN](#4-full-join)
5) [SELF JOIN](#5-self-join)
6) [UNIONS](#6-unions)
7) [UNION ALL](#7-union-all)

### 1. INNER JOIN
The INNER JOIN is the most common type of join. It returns only the rows where there is a match in both tables based on the join condition.

-Think of the  intersection of two circles.

- Result: Only records with matching values in both tables are included. Non-matching rows from both tables are excluded.

- Syntax: SELECT ... FROM table1 INNER JOIN table2 ON table1.column = table2.column;

- Key Point: INNER is optional. You can often just write JOIN, as it is the default.

**Query:**

```sql
-- Get all employees and their department names, but only if they are assigned to a department
SELECT 
      e.emp_name,
      d.dept_name
FROM employees e
INNER JOIN departments d 
ON e.dept_id = d.dept_id;
```


### 2. LEFT JOIN
Returns all records from the left table (the first table mentioned), and the matched records from the right table. If no match exists, NULL values are returned for columns from the right table.

- Venn Diagram: The entire left circle, plus the intersection with the right circle.

- Use Case: Essential for finding "orphaned" records in the left table (e.g., employees with no department).

**Query**
```sql
-- Get all employees, and show their department name if they have one
SELECT 
     e.emp_name,
     d.dept_name
FROM employees e
LEFT JOIN departments d 
ON e.dept_id = d.dept_id;
```

### 3. RIGHT JOIN
Returns all records from the right table (the second table mentioned), and the matched records from the left table. If no match exists, NULL values are returned for columns from the left table.

- Venn Diagram: The entire right circle, plus the intersection with the left circle.

- Note: RIGHT JOIN is less common. You can usually achieve the same result by switching the table order in a LEFT JOIN.

**Query**
```sql
-- Get all departments, and show any employees in them
SELECT 
     e.emp_name, 
     d.dept_name
FROM employees e
RIGHT JOIN departments d 
ON e.dept_id = d.dept_id;
```

### 4. FULL JOIN
A FULL JOIN (or FULL OUTER JOIN) returns all records when there is a match in either the left or right table. It combines the results of both LEFT and RIGHT joins.

- Venn Diagram: The entire union of both circles.

- Use Case: Useful for seeing the complete picture from both tables, including all matched and unmatched records.

**Query**
```sql
-- Get a complete list of all employees and all departments, matching where possible
SELECT 
     e.emp_name, 
     d.dept_name
FROM employees e
FULL JOIN departments d 
ON e.dept_id = d.dept_id;
```

### 5. SELF JOIN
A SELF JOIN is a regular join, but the table is joined with itself. It's used to query hierarchical data or compare rows within the same table.

- How it works: You treat the same table as two different tables by giving it two different aliases (e.g., e for employee, m for manager).

- Use Cases: Organizational charts, finding duplicates, comparing rows within a table.


**Query**
```sql
-- Find all employees and the name of their manager
SELECT 
      e.emp_name AS employee_name,
      m.emp_name AS manager_name
FROM employees e -- Alias 'e' for the employee
LEFT JOIN employees m -- Alias 'm' for the manager
       ON e.manager_id = m.emp_id;
```

**Explanation:** We join the employee's manager_id to the manager's emp_id.

### 6. UNIONS
- Combines results (row-wise)
- Removes duplicates (keeps only unique rows)

Example:
```sql
-- Example: Customers from Kenya and Uganda
SELECT customer_name, country
FROM customers
WHERE country = 'Kenya'

UNION

SELECT customer_name, country
FROM customers
WHERE country = 'Uganda';
```
- This will return a single list of customers, with no duplicates.

### 7. UNION ALL
- Combines results
- Keeps duplicates (does not remove them)
```sql
-- Example with UNION ALL
SELECT customer_name, country
FROM customers
WHERE country = 'Kenya'

UNION ALL

SELECT customer_name, country
FROM customers
WHERE country = 'Uganda';
```
- This will return all customers from Kenya and Uganda, including duplicates if a customer exists in both lists.

3. Key Rules for UNION / UNION ALL

- Each SELECT must have same number of columns.

- Columns must have compatible data types.

- Order of columns must match.
