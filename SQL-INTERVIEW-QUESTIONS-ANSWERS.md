# SQL Interview Questions & Answers

A practical SQL interview preparation guide with concise answers, examples, scenarios, and query-based questions.

## 1. What is SQL?
**Answer:** SQL (Structured Query Language) is used to create, read, update, and delete data in relational databases.

**Common operations:** SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER, DROP.

---

## 2. What is the difference between WHERE and HAVING?
**Answer:** `WHERE` filters rows before grouping, while `HAVING` filters groups after `GROUP BY`.

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees
WHERE salary > 30000
GROUP BY department_id
HAVING COUNT(*) > 5;
```

---

## 3. What is a primary key?
**Answer:** A primary key uniquely identifies each row in a table. It cannot contain NULL values and a table can have only one primary-key constraint (which may contain multiple columns).

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100)
);
```

---

## 4. What is a foreign key?
**Answer:** A foreign key creates a relationship between a column in one table and a candidate/primary key in another table.

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

---

## 5. DELETE vs TRUNCATE vs DROP

| Command | Purpose | WHERE allowed? | Table structure |
|---|---|---:|---|
| DELETE | Removes selected rows | Yes | Retained |
| TRUNCATE | Removes all rows | No | Retained |
| DROP | Removes the table | No | Removed |

> Transaction and identity-reset behavior can vary by database engine.

---

## 6. What is normalization?
**Answer:** Normalization organizes data to reduce redundancy and improve data integrity.

Common forms:
- 1NF — atomic values
- 2NF — 1NF + no partial dependency on a composite key
- 3NF — 2NF + no transitive dependency
- BCNF — every determinant is a candidate key

---

## 7. What is a JOIN?
**Answer:** A JOIN combines rows from two or more tables using a related condition.

Common joins:
- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL OUTER JOIN
- CROSS JOIN
- SELF JOIN

---

## 8. INNER JOIN example

```sql
SELECT e.employee_name, d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;
```

**Result:** Only employees with a matching department are returned.

---

## 9. LEFT JOIN example

```sql
SELECT e.employee_name, d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

**Result:** All employees are returned, including employees without a matching department.

---

## 10. Find duplicate records

```sql
SELECT email, COUNT(*) AS duplicate_count
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;
```

---

## 11. Find the second-highest salary

### Using DENSE_RANK

```sql
SELECT employee_id, employee_name, salary
FROM (
    SELECT employee_id,
           employee_name,
           salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
    FROM employees
) x
WHERE salary_rank = 2;
```

**Why DENSE_RANK?** It correctly handles tied salaries.

---

## 12. Find the Nth-highest salary

```sql
SELECT employee_id, employee_name, salary
FROM (
    SELECT employee_id,
           employee_name,
           salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees
) x
WHERE rnk = 5;
```

Change `5` to the required rank.

---

## 13. Find employees earning more than their department average

```sql
SELECT e.employee_id, e.employee_name, e.salary, e.department_id
FROM employees e
JOIN (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) a
ON e.department_id = a.department_id
WHERE e.salary > a.avg_salary;
```

---

## 14. What is GROUP BY?
**Answer:** GROUP BY groups rows with the same values so aggregate functions can be applied.

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id;
```

---

## 15. What are aggregate functions?
Common aggregate functions include:
- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()

Example:

```sql
SELECT department_id,
       COUNT(*) AS total_employees,
       AVG(salary) AS average_salary,
       MAX(salary) AS highest_salary
FROM employees
GROUP BY department_id;
```

---

## 16. COUNT(*) vs COUNT(column)
**Answer:** `COUNT(*)` counts rows. `COUNT(column)` counts non-NULL values in that column.

---

## 17. What is NULL?
**Answer:** NULL represents a missing, unknown, or not-applicable value. It is not the same as zero or an empty string.

Use:

```sql
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

Not:

```sql
WHERE manager_id = NULL
```

---

## 18. What is a subquery?
**Answer:** A subquery is a query nested inside another SQL statement.

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

---

## 19. What is a CTE?
**Answer:** A Common Table Expression (CTE) creates a temporary named result set for one SQL statement.

```sql
WITH department_avg AS (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT e.employee_name, e.salary
FROM employees e
JOIN department_avg a
    ON e.department_id = a.department_id
WHERE e.salary > a.avg_salary;
```

---

## 20. What are window functions?
**Answer:** Window functions calculate values across related rows without collapsing the result into one row per group.

Common functions:
- ROW_NUMBER()
- RANK()
- DENSE_RANK()
- LAG()
- LEAD()
- SUM() OVER()
- AVG() OVER()

---

## 21. ROW_NUMBER vs RANK vs DENSE_RANK

For salaries 100, 100, 90:

| Salary | ROW_NUMBER | RANK | DENSE_RANK |
|---:|---:|---:|---:|
| 100 | 1 | 1 | 1 |
| 100 | 2 | 1 | 1 |
| 90 | 3 | 3 | 2 |

**Key point:** RANK leaves gaps after ties; DENSE_RANK does not.

---

## 22. Find the latest record for each customer

```sql
SELECT customer_id, transaction_id, transaction_date, amount
FROM (
    SELECT t.*,
           ROW_NUMBER() OVER (
               PARTITION BY customer_id
               ORDER BY transaction_date DESC
           ) AS rn
    FROM transactions t
) x
WHERE rn = 1;
```

---

## 23. Find customers with no transactions

```sql
SELECT c.customer_id, c.customer_name
FROM customers c
LEFT JOIN transactions t
    ON c.customer_id = t.customer_id
WHERE t.customer_id IS NULL;
```

---

## 24. What is an index?
**Answer:** An index is a database structure that can speed up data retrieval by providing an efficient access path to rows.

```sql
CREATE INDEX idx_employee_department
ON employees(department_id);
```

**Trade-off:** Indexes consume storage and can increase INSERT, UPDATE, and DELETE cost.

---

## 25. Clustered vs non-clustered index
**Answer:** The exact implementation depends on the database engine.

- A clustered index generally determines how table rows are physically/clustered in storage.
- A non-clustered index is a separate access structure that points to table rows.

A database may implement these concepts differently, so always verify engine-specific behavior.

---

## 26. What is a view?
**Answer:** A view is a stored query that behaves like a virtual table.

```sql
CREATE VIEW active_customers AS
SELECT customer_id, customer_name
FROM customers
WHERE status = 'ACTIVE';
```

---

## 27. What is a stored procedure?
**Answer:** A stored procedure is a named database program containing SQL and, depending on the database, procedural logic.

---

## 28. What is a transaction?
**Answer:** A transaction is a logical unit of database work that should follow ACID properties.

### ACID
- **Atomicity** — all or nothing
- **Consistency** — preserves valid database rules
- **Isolation** — concurrent transactions are appropriately separated
- **Durability** — committed changes survive failures

---

## 29. COMMIT vs ROLLBACK
- **COMMIT:** permanently commits the current transaction.
- **ROLLBACK:** undoes uncommitted transaction changes.

---

## 30. What are isolation levels?
Common SQL isolation levels are:
- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ
- SERIALIZABLE

Some database systems also provide additional levels or different implementations.

---

## 31. What is a deadlock?
**Answer:** A deadlock occurs when transactions wait on resources held by each other, so neither can proceed.

**Typical prevention techniques:**
1. Access tables/resources in a consistent order.
2. Keep transactions short.
3. Avoid unnecessary locks.
4. Implement retry logic where appropriate.
5. Monitor blocking and deadlock reports.

---

## 32. EXISTS vs IN
**Answer:** Both can test membership, but their behavior and performance depend on the query and database optimizer.

```sql
SELECT c.*
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM transactions t
    WHERE t.customer_id = c.customer_id
);
```

Prefer the form that clearly expresses the requirement, then verify performance with the execution plan.

---

## 33. UNION vs UNION ALL
- **UNION:** combines result sets and removes duplicates.
- **UNION ALL:** combines result sets without duplicate elimination and is generally cheaper.

---

## 34. Find the top 3 salaries in each department

```sql
SELECT employee_id, employee_name, department_id, salary
FROM (
    SELECT e.*,
           DENSE_RANK() OVER (
               PARTITION BY department_id
               ORDER BY salary DESC
           ) AS rnk
    FROM employees e
) x
WHERE rnk <= 3;
```

---

## 35. Find consecutive duplicate status records

A common approach is to use LAG():

```sql
SELECT customer_id,
       status_date,
       status,
       LAG(status) OVER (
           PARTITION BY customer_id
           ORDER BY status_date
       ) AS previous_status
FROM customer_status_history;
```

Then filter where the current and previous statuses match.

---

## 36. Find customers who made transactions on consecutive days

```sql
SELECT DISTINCT customer_id
FROM (
    SELECT customer_id,
           transaction_date,
           LAG(transaction_date) OVER (
               PARTITION BY customer_id
               ORDER BY transaction_date
           ) AS previous_date
    FROM transactions
) x
WHERE transaction_date = previous_date + INTERVAL '1' DAY;
```

> Date arithmetic syntax varies by database.

---

## 37. How do you optimize a slow SQL query?
A practical approach:

1. Check the execution plan.
2. Identify full scans, expensive joins, sorts, and spills.
3. Verify indexes and join/filter columns.
4. Return only required columns.
5. Filter early when appropriate.
6. Avoid unnecessary DISTINCT.
7. Review data types and implicit conversions.
8. Check statistics/cardinality estimates.
9. Reduce unnecessary rows before expensive operations.
10. Re-test with representative production-sized data.

---

## 38. What is an execution plan?
**Answer:** An execution plan shows how the database optimizer intends to execute a query, including scans, seeks, joins, sorts, estimated rows, and costs.

---

## 39. What is a self join?
**Answer:** A self join joins a table to itself.

Example: employee-manager relationship.

```sql
SELECT e.employee_name AS employee,
       m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

---

## 40. What is a correlated subquery?
**Answer:** A correlated subquery references a column from the outer query and is logically evaluated with respect to each outer row.

---

# Scenario-Based Interview Questions

## Scenario 1 — Duplicate customer records
**Question:** A customer table contains duplicate email addresses. How do you identify them?

```sql
SELECT email, COUNT(*) AS count_per_email
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;
```

**Follow-up:** Before deleting duplicates, decide which record is authoritative and preserve the required business/audit data.

---

## Scenario 2 — Third-highest salary
**Question:** Return the third-highest distinct salary.

```sql
SELECT salary
FROM (
    SELECT salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees
) x
WHERE rnk = 3;
```

---

## Scenario 3 — Missing transactions
**Question:** Find customers who registered but never made a transaction.

**Answer:** Use a LEFT JOIN and filter unmatched transaction rows.

---

## Scenario 4 — Daily transaction totals
```sql
SELECT transaction_date,
       SUM(amount) AS total_amount
FROM transactions
GROUP BY transaction_date
ORDER BY transaction_date;
```

---

## Scenario 5 — Monthly revenue
```sql
SELECT
    EXTRACT(YEAR FROM transaction_date) AS year_num,
    EXTRACT(MONTH FROM transaction_date) AS month_num,
    SUM(amount) AS revenue
FROM transactions
GROUP BY
    EXTRACT(YEAR FROM transaction_date),
    EXTRACT(MONTH FROM transaction_date)
ORDER BY year_num, month_num;
```

> Date extraction syntax varies across database platforms.

---

# Quick Interview Revision

### Must-know topics
- SELECT / WHERE / ORDER BY
- GROUP BY / HAVING
- JOINs
- Subqueries
- CTEs
- Window functions
- Primary and foreign keys
- Constraints
- Indexes
- Views
- Transactions
- ACID
- Isolation levels
- Deadlocks
- Normalization
- Query optimization
- Execution plans
- NULL handling
- DELETE / TRUNCATE / DROP
- UNION / UNION ALL
- EXISTS / IN

### Interview tip
For every SQL problem, explain:
1. **What the query needs to return**
2. **Which tables are required**
3. **How tables are joined**
4. **How rows are filtered**
5. **How grouping/ranking is handled**
6. **How NULLs and duplicates are handled**
7. **Why your approach is correct**
8. **How you would validate performance**

---

## License

This repository is intended for SQL learning and interview preparation.
