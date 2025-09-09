# VIEWS

1. What is a View?

- A view is like a saved SQL query that you can treat as a virtual table.

- It does not store data itself (by default).

- It just stores the SQL logic, and every time you query it, PostgreSQL runs the underlying query.

- You can select from it like a normal table.


2. Why Use Views?

✅ Make complex queries easier to reuse

✅ Improve readability (give a name to long SQL)

✅ Provide security (hide certain columns from users)

✅ Centralize business logic (avoid rewriting the same query everywhere)


3. Syntax and Example

```sql
-- Create a view for total sales per customer
CREATE VIEW customer_sales AS
SELECT customer_id, SUM(amount) AS total_amount
FROM sales
GROUP BY customer_id;

-- Use the view
SELECT * FROM customer_sales
WHERE total_amount > 1000;

-- Drop View
DROP VIEW IF EXISTS customer_sales;
```