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


# Extended SQL Interview Questions

## SQL Basics

### 41. What is SQL?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

### 42. What is an RDBMS?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

### 43. What is a table?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

### 44. What is a row?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

### 45. What is a column?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

### 46. What is a schema?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

### 47. What is a primary key?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

### 48. What is a foreign key?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

### 49. What is a candidate key?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

### 50. What is a composite key?
**Answer:** It is a relational database concept used to structure, identify, and relate stored data.

## DDL and DML

### 51. What is DDL?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

### 52. What is DML?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

### 53. What is DCL?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

### 54. What is TCL?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

### 55. What does CREATE do?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

### 56. What does ALTER do?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

### 57. What does DROP do?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

### 58. What does TRUNCATE do?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

### 59. What does INSERT do?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

### 60. What does UPDATE do?
**Answer:** It is a SQL command category used to define or manipulate database objects/data.

## Filtering

### 61. What does WHERE do?
**Answer:** It controls which rows or values qualify for a query result.

### 62. What does HAVING do?
**Answer:** It controls which rows or values qualify for a query result.

### 63. What does DISTINCT do?
**Answer:** It controls which rows or values qualify for a query result.

### 64. What does BETWEEN do?
**Answer:** It controls which rows or values qualify for a query result.

### 65. What does IN do?
**Answer:** It controls which rows or values qualify for a query result.

### 66. What does NOT IN do?
**Answer:** It controls which rows or values qualify for a query result.

### 67. What does LIKE do?
**Answer:** It controls which rows or values qualify for a query result.

### 68. What does IS NULL do?
**Answer:** It controls which rows or values qualify for a query result.

### 69. What does CASE do?
**Answer:** It controls which rows or values qualify for a query result.

### 70. What does COALESCE do?
**Answer:** It controls which rows or values qualify for a query result.

## Aggregation

### 71. What is GROUP BY?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

### 72. What are aggregate functions?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

### 73. COUNT(*) vs COUNT(column)?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

### 74. What is conditional aggregation?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

### 75. Can GROUP BY use multiple columns?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

### 76. Can HAVING be used without GROUP BY?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

### 77. What does COUNT(DISTINCT) do?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

### 78. How do you count conditional rows?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

### 79. How do you calculate an average?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

### 80. How do you calculate a running total?
**Answer:** It summarizes rows using grouping and aggregate functions such as COUNT, SUM, and AVG.

## Joins

### 81. What is INNER JOIN?
**Answer:** It combines related rows from multiple table sources using a matching condition.

### 82. What is LEFT JOIN?
**Answer:** It combines related rows from multiple table sources using a matching condition.

### 83. What is RIGHT JOIN?
**Answer:** It combines related rows from multiple table sources using a matching condition.

### 84. What is FULL OUTER JOIN?
**Answer:** It combines related rows from multiple table sources using a matching condition.

### 85. What is CROSS JOIN?
**Answer:** It combines related rows from multiple table sources using a matching condition.

### 86. What is SELF JOIN?
**Answer:** It combines related rows from multiple table sources using a matching condition.

### 87. What is a many-to-many join?
**Answer:** It combines related rows from multiple table sources using a matching condition.

### 88. What is a bridge table?
**Answer:** It combines related rows from multiple table sources using a matching condition.

### 89. Why do joins create duplicates?
**Answer:** It combines related rows from multiple table sources using a matching condition.

### 90. How do you find unmatched rows?
**Answer:** It combines related rows from multiple table sources using a matching condition.

## Subqueries

### 91. What is a subquery?
**Answer:** It is a query nested inside another SQL statement.

### 92. What is a scalar subquery?
**Answer:** It is a query nested inside another SQL statement.

### 93. What is a correlated subquery?
**Answer:** It is a query nested inside another SQL statement.

### 94. What is EXISTS?
**Answer:** It is a query nested inside another SQL statement.

### 95. What is NOT EXISTS?
**Answer:** It is a query nested inside another SQL statement.

### 96. EXISTS vs IN?
**Answer:** It is a query nested inside another SQL statement.

### 97. NOT EXISTS vs NOT IN?
**Answer:** It is a query nested inside another SQL statement.

### 98. What is a derived table?
**Answer:** It is a query nested inside another SQL statement.

### 99. When should a subquery be replaced by a join?
**Answer:** It is a query nested inside another SQL statement.

### 100. What causes a scalar subquery error?
**Answer:** It is a query nested inside another SQL statement.

## CTEs

### 101. What is a CTE?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

### 102. What is a recursive CTE?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

### 103. When should you use a CTE?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

### 104. CTE vs temporary table?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

### 105. Can a CTE be referenced multiple times?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

### 106. Can a CTE modify data?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

### 107. What is a recursive CTE base case?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

### 108. How do you stop recursive CTE cycles?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

### 109. What is query factoring?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

### 110. What is CTE materialization?
**Answer:** It defines a named query result for use within a statement; recursive CTEs can traverse hierarchies.

## Window Functions

### 111. What is a window function?
**Answer:** It calculates across related rows without collapsing them into one row per group.

### 112. What does OVER() do?
**Answer:** It calculates across related rows without collapsing them into one row per group.

### 113. What is PARTITION BY?
**Answer:** It calculates across related rows without collapsing them into one row per group.

### 114. What is window ORDER BY?
**Answer:** It calculates across related rows without collapsing them into one row per group.

### 115. ROW_NUMBER vs RANK?
**Answer:** It calculates across related rows without collapsing them into one row per group.

### 116. RANK vs DENSE_RANK?
**Answer:** It calculates across related rows without collapsing them into one row per group.

### 117. What is LAG()?
**Answer:** It calculates across related rows without collapsing them into one row per group.

### 118. What is LEAD()?
**Answer:** It calculates across related rows without collapsing them into one row per group.

### 119. What is FIRST_VALUE()?
**Answer:** It calculates across related rows without collapsing them into one row per group.

### 120. What is NTILE()?
**Answer:** It calculates across related rows without collapsing them into one row per group.

## Ranking Problems

### 121. How do you find the second-highest salary?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

### 122. How do you find the third-highest salary?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

### 123. How do you find the Nth-highest salary?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

### 124. How do you find top 3 per department?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

### 125. How do you find the latest row per customer?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

### 126. How do you find the first row per group?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

### 127. How do you find duplicate records?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

### 128. How do you remove duplicates safely?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

### 129. How do you find tied rankings?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

### 130. How do you make ranking deterministic?
**Answer:** Use window ranking functions such as ROW_NUMBER, RANK, or DENSE_RANK, then filter the desired rank.

## Transactions

### 131. What is a transaction?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

### 132. What is ACID?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

### 133. What is COMMIT?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

### 134. What is ROLLBACK?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

### 135. What is SAVEPOINT?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

### 136. What is atomicity?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

### 137. What is consistency?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

### 138. What is isolation?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

### 139. What is durability?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

### 140. What is autocommit?
**Answer:** A transaction groups database changes into a controlled unit that can commit or roll back.

## Isolation and Locking

### 141. What is READ UNCOMMITTED?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

### 142. What is READ COMMITTED?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

### 143. What is REPEATABLE READ?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

### 144. What is SERIALIZABLE?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

### 145. What is a dirty read?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

### 146. What is a non-repeatable read?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

### 147. What is a phantom read?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

### 148. What is blocking?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

### 149. What is a deadlock?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

### 150. How do you prevent deadlocks?
**Answer:** These mechanisms control concurrent access and visibility between transactions.

## Indexes

### 151. What is an index?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

### 152. Why are indexes useful?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

### 153. What is a B-tree index?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

### 154. What is a hash index?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

### 155. What is a composite index?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

### 156. What is a unique index?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

### 157. What is a covering index?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

### 158. What is a partial index?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

### 159. What is an expression index?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

### 160. Why can too many indexes hurt?
**Answer:** An index provides an alternate access path that can speed suitable searches, joins, and ordering.

## Performance

### 161. What is an execution plan?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

### 162. What is a full table scan?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

### 163. What is an index seek?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

### 164. What is cardinality estimation?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

### 165. What are optimizer statistics?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

### 166. What is sargability?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

### 167. What is predicate pushdown?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

### 168. Why can SELECT * hurt performance?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

### 169. Why can functions on indexed columns hurt?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

### 170. How do you optimize a slow query?
**Answer:** Use execution plans, statistics, indexes, sargable predicates, and representative workload testing.

## Constraints

### 171. What is NOT NULL?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

### 172. What is UNIQUE?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

### 173. What is CHECK?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

### 174. What is DEFAULT?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

### 175. What is referential integrity?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

### 176. Can a foreign key be NULL?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

### 177. Can a table have multiple UNIQUE constraints?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

### 178. Can a table have two primary keys?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

### 179. What is a composite foreign key?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

### 180. What are cascading actions?
**Answer:** They enforce data-integrity rules such as uniqueness, required values, valid ranges, and relationships.

## Normalization

### 181. What is normalization?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

### 182. What is 1NF?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

### 183. What is 2NF?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

### 184. What is 3NF?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

### 185. What is BCNF?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

### 186. What is denormalization?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

### 187. What is an update anomaly?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

### 188. What is an insertion anomaly?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

### 189. What is a deletion anomaly?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

### 190. When is denormalization useful?
**Answer:** It organizes relational data to reduce redundancy and update anomalies.

## Views and Routines

### 191. What is a view?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

### 192. What is a materialized view?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

### 193. What is a stored procedure?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

### 194. What is a function?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

### 195. What is a trigger?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

### 196. What is a cursor?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

### 197. What is an updatable view?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

### 198. Can a view have indexes?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

### 199. What is a database sequence?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

### 200. What is an identity column?
**Answer:** They encapsulate queries or database logic for reuse, abstraction, security, or automation.

## Set Operations

### 201. What is UNION?
**Answer:** They combine or compare compatible query result sets.

### 202. What is UNION ALL?
**Answer:** They combine or compare compatible query result sets.

### 203. What is INTERSECT?
**Answer:** They combine or compare compatible query result sets.

### 204. What is EXCEPT?
**Answer:** They combine or compare compatible query result sets.

### 205. UNION vs UNION ALL?
**Answer:** They combine or compare compatible query result sets.

### 206. What columns are required for UNION?
**Answer:** They combine or compare compatible query result sets.

### 207. Can set operations preserve duplicates?
**Answer:** They combine or compare compatible query result sets.

### 208. How do you find rows in A but not B?
**Answer:** They combine or compare compatible query result sets.

### 209. How do you find rows common to A and B?
**Answer:** They combine or compare compatible query result sets.

### 210. How do you compare two tables?
**Answer:** They combine or compare compatible query result sets.

## Dates and Time

### 211. How do you filter a timestamp range?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

### 212. Why use half-open date ranges?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

### 213. How do you find today's records?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

### 214. How do you group by month?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

### 215. How do you group by year?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

### 216. How do you calculate date differences?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

### 217. How do you find consecutive dates?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

### 218. How do you find missing dates?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

### 219. How do you handle time zones?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

### 220. How do you calculate month-over-month change?
**Answer:** Use explicit boundaries, correct timezone semantics, and database-appropriate date functions.

## NULL and Data Types

### 221. What is NULL?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

### 222. Why is NULL not equal to zero?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

### 223. Why use IS NULL?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

### 224. How does NULL affect aggregates?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

### 225. What is NULLIF?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

### 226. What is CAST?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

### 227. What is implicit conversion?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

### 228. VARCHAR vs CHAR?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

### 229. DATE vs TIMESTAMP?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

### 230. DECIMAL vs floating point?
**Answer:** NULL represents missing/unknown data; data types determine how values are stored and compared.

## Advanced SQL

### 231. What is a recursive query?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

### 232. What are gaps and islands?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

### 233. What is sessionization?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

### 234. What is a top-N-per-group query?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

### 235. What is a pivot?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

### 236. What is an unpivot?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

### 237. What is a lateral join?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

### 238. What is a range join?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

### 239. What is effective dating?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

### 240. What is a temporal query?
**Answer:** These techniques solve hierarchical, sequential, analytical, and complex relational problems.

## Data Warehousing

### 241. What is OLTP?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

### 242. What is OLAP?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

### 243. What is a data warehouse?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

### 244. What is a fact table?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

### 245. What is a dimension table?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

### 246. What is a star schema?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

### 247. What is a snowflake schema?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

### 248. What is fact grain?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

### 249. What is a slowly changing dimension?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

### 250. What is a Type 2 dimension?
**Answer:** These concepts support analytical models optimized for reporting and historical analysis.

## ETL and Data Quality

### 251. What is ETL?
**Answer:** They move, transform, reconcile, and validate data between systems.

### 252. What is ELT?
**Answer:** They move, transform, reconcile, and validate data between systems.

### 253. What is a staging table?
**Answer:** They move, transform, reconcile, and validate data between systems.

### 254. What is an incremental load?
**Answer:** They move, transform, reconcile, and validate data between systems.

### 255. What is a full refresh?
**Answer:** They move, transform, reconcile, and validate data between systems.

### 256. What is CDC?
**Answer:** They move, transform, reconcile, and validate data between systems.

### 257. What is a watermark?
**Answer:** They move, transform, reconcile, and validate data between systems.

### 258. What is data reconciliation?
**Answer:** They move, transform, reconcile, and validate data between systems.

### 259. What is data deduplication?
**Answer:** They move, transform, reconcile, and validate data between systems.

### 260. What is an idempotent load?
**Answer:** They move, transform, reconcile, and validate data between systems.

## Security

### 261. What is SQL injection?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

### 262. How do parameterized queries prevent injection?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

### 263. What is least privilege?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

### 264. What is GRANT?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

### 265. What is REVOKE?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

### 266. What is role-based access control?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

### 267. What is row-level security?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

### 268. What is data masking?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

### 269. What is encryption in transit?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

### 270. Why should passwords not be hardcoded?
**Answer:** Use parameterization, least privilege, encryption, auditing, and controlled access to protect SQL systems.

## Production Support

### 271. How do you troubleshoot a slow SQL query?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

### 272. How do you troubleshoot blocking?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

### 273. How do you troubleshoot a deadlock?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

### 274. How do you find expensive queries?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

### 275. How do you inspect an execution plan?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

### 276. How do you validate a data fix?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

### 277. How do you safely run a production UPDATE?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

### 278. How do you safely run a production DELETE?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

### 279. What should a SQL incident runbook contain?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

### 280. How do you perform root-cause analysis for SQL?
**Answer:** Start with impact, query text, execution plan, locks, waits, resource usage, and recent changes.

## Real Interview Scenarios

### 281. Find customers with no orders.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.

### 282. Find employees above department average.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.

### 283. Find the latest transaction per customer.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.

### 284. Find duplicate emails.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.

### 285. Find top 3 salaries per department.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.

### 286. Find customers inactive for 90 days.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.

### 287. Find transactions on consecutive days.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.

### 288. Find employees with no department.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.

### 289. Find departments with more than 10 employees.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.

### 290. Find the month with highest revenue.
**Answer:** Clarify the required result and grain, choose joins/window functions/aggregation, then validate edge cases and performance.


### 291. How do you calculate a cumulative total per customer?
**Answer:** Use SUM(amount) OVER (PARTITION BY customer_id ORDER BY transaction_date, transaction_id).

### 292. How do you find the first purchase per customer?
**Answer:** Use ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY purchase_date, purchase_id) and filter for 1.

### 293. How do you find customers with more than five orders?
**Answer:** GROUP BY customer_id and use HAVING COUNT(*) > 5.

### 294. How do you find the highest order per customer?
**Answer:** Use ROW_NUMBER or MAX according to whether you need the complete order row.

### 295. How do you find products never sold?
**Answer:** LEFT JOIN products to order lines and filter for a NULL matching order-line key, or use NOT EXISTS.

### 296. How do you find departments with no employees?
**Answer:** LEFT JOIN departments to employees and filter for NULL employee keys.

### 297. How do you find duplicate records using ROW_NUMBER?
**Answer:** Partition by the columns defining duplication, order deterministically, and mark rows where ROW_NUMBER() > 1.

### 298. How do you compare two query results for equality?
**Answer:** Compare both directions with EXCEPT/NOT EXISTS and account for duplicate semantics when required.

### 299. How do you find the percentage of customers who placed an order?
**Answer:** Divide the distinct ordering-customer count by the total customer count, using decimal arithmetic and zero protection.

### 300. What should you mention when an interviewer asks for a SQL optimization?
**Answer:** Explain the execution plan, data volume, predicates, joins, indexes, statistics, and measured before/after performance.
