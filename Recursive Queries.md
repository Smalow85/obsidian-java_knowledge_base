A recursive query in [[SQL]] is a query that references itself. Recursive queries have a minimum of two queries, an anchor member (runs only once), and a recursive member (runs repeatedly). Include a UNION ALL statement between these queries.

Here’s a sample of a recursive CTE:

```sql
WITH RECURSIVE ancestors AS (
  SELECT employee_id, manager_id, full_name
  FROM employees
  WHERE manager_id IS NULL
  
  UNION ALL

  SELECT e.employee_id, e.manager_id, e.full_name
  FROM employees e
  INNER JOIN ancestors a ON a.employee_id = e.manager_id
)
SELECT * FROM ancestors;
```

In this code snippet, the first query is the anchor member that fetches the employees with no manager. The second part is the recursive member, continuously fetching managers until none are left.
## Syntax of Recursive CTE

Here’s the general structure of a recursive CTE:

```sql
WITH RECURSIVE cte_name (column_list) AS (
  
  -- Anchor member
  SELECT column_list
  FROM table_name
  WHERE condition
  
  UNION ALL
  
  -- Recursive member
  SELECT column_list
  FROM table_name
  INNER JOIN cte_name ON condition
)
SELECT * FROM cte_name;
```

Note: some database systems such as MySQL, PostgreSQL, and SQLite use `WITH RECURSIVE` for recursive queries. Others like SQL Server, Oracle, and DB2 use just `WITH`.

Remember to be careful when setting the conditions for your recursive query to avoid infinite loops.