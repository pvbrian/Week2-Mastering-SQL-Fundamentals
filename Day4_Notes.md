# Table of contents:
1) [SubQuery](#1-subquery)
2) [CTEs](#2-common-table-expressions)
3) [When To Use a SubQuery or CTE](#3-when-to-choose)

## 1. SubQuery
- A subquery is a SELECT statement nested inside another SQL statement.

Types of sub-queries:
A. Scalar SubQuery
B. Row SubQuery
C. Column SubQUery
D. Correlated SubQuery

A. Scalar SubQuery
- What it is: A subquery that returns exactly one row and one column (a single value).

- Where it's used: Anywhere a single value is expected (e.g., in SELECT, WHERE, SET).

- Syntax & Example:
```sql
-- Find all employees whose salary is above the company average.
SELECT 
     first_name, 
     salary
FROM employees
WHERE salary > (
    SELECT AVG(salary) -- This returns one single value
    FROM employees
);
```

B. Row SubQuery
- What it is: A subquery that returns one row with multiple columns.

- Where it's used: Often with row constructors.

- Syntax & Example:
```sql
-- Find the employee with the highest salary and their department.
SELECT 
     first_name, 
     department_id
FROM employees
WHERE (salary, department_id) = (
    SELECT MAX(salary), department_id -- Returns one row: (max_salary, dept_id)
    FROM employees
    WHERE department_id = 60
);
```

C. Column SubQuery
- What it is: A subquery that returns one column with multiple rows.

- Where it's used: Often with operators like IN, ANY, ALL.

- Syntax & Example:
```sql
-- Find all employees who work in a department located in London.
SELECT 
     first_name, 
     last_name
FROM employees
WHERE department_id IN (
    SELECT department_id -- Returns a list of IDs: e.g., (40, 70, 80)
    FROM departments
    WHERE location_id = 'LONDON'
);
```

D. Correlated SuQuery
- What it is: A subquery that depends on the outer query for its values. 
- It is executed once for each row processed by the outer query. This can be slow.

- Syntax & Example:
```sql
-- Find all employees who earn more than the average salary in THEIR OWN department.
SELECT 
     first_name, 
     salary, 
     department_id
FROM employees e1
WHERE salary > (
    SELECT AVG(salary)
    FROM employees e2
    WHERE e2.department_id = e1.department_id -- Correlates with the outer row
);
```


## 2. Common Table Expressions

Definition

- A CTE (Common Table Expression) is a temporary named result set used to simplify complex queries, especially those with subqueries.

Syntax:
```sql
WITH cte_name (optional_column_names) AS (
    SELECT ...
    FROM ...
    WHERE ...
)
SELECT * 
FROM cte_name;
```

Benefits
- Simplifies nested queries
- Improves readability and reusability
- Can be recursive
- Better performance than subqueries in some cases
- Allows for modular query construction

Examples

Example 1: Students Scoring Above 85
```sql
WITH TopStudents AS (
    SELECT name, 
          score 
    FROM students 
    WHERE score > 85
)
SELECT * 
FROM TopStudents 
ORDER BY score DESC;
```

Example 2: Average Salary by Department
```sql
WITH AvgDeptSalary AS (
    SELECT 
         department_id, 
         AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT 
     d.department_name, 
     ads.avg_salary
FROM AvgDeptSalary ads
INNER JOIN departments d 
ON ads.department_id = d.department_id;
```

Recursive CTEs
```sql
WITH RECURSIVE employee_hierarchy AS (
    -- Base case: top-level managers
    SELECT 
          employee_id, 
          name, 
          manager_id, 
          1 as level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive case: employees reporting to previous level
    SELECT 
         e.employee_id, 
         e.name, 
         e.manager_id, 
         eh.level + 1
    FROM employees e
    JOIN employee_hierarchy eh 
    ON e.manager_id = eh.employee_id
)
SELECT * 
FROM employee_hierarchy 
ORDER BY level, name;
```

Multiple CTEs
```sql
WITH 
high_performers AS (
    SELECT 
          employee_id, 
          name, 
          performance_score
    FROM employees
    WHERE performance_score > 4.5
),
recent_hires AS (
    SELECT 
         employee_id, 
         name, 
         hire_date
    FROM employees
    WHERE hire_date > '2023-01-01'
)
SELECT 
     hp.name, 
     hp.performance_score, 
     rh.hire_date
FROM high_performers hp
JOIN recent_hires rh 
ON hp.employee_id = rh.employee_id;
```

## 3. When to Choose
✅ Use a Subquery when…

- It’s small and simple → quick filtering or calculation.

- You only need it once in the query.

- You want to embed it inside WHERE, FROM, or SELECT.

✅ Use a CTE when…

- Query is long or complex, and breaking it into steps makes it readable.

- You need the result more than once in the query.

- You’re working with recursive logic (hierarchies, trees, sequences).

- You want to make queries easier for teammates to understand.

