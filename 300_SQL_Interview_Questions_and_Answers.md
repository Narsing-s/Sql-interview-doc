# 300 Real SQL Interview Questions & Answers

> **Interview Practice Book** · 300 Questions · SQL Queries · Answers · Explanations · Real-World Scenarios
>
> Examples primarily use **PostgreSQL-style SQL**. Translate dialect-specific syntax when interviewing on MySQL, SQL Server, or Oracle.

## Contents

| # | Section | Questions |
|---|---|---:|
| 01 | SQL Fundamentals | 30 |
| 02 | Filtering, Sorting & Expressions | 25 |
| 03 | JOINs | 35 |
| 04 | Aggregation & GROUP BY | 30 |
| 05 | Subqueries | 25 |
| 06 | CTEs & Recursive SQL | 20 |
| 07 | Window Functions | 35 |
| 08 | Dates, Strings & NULL Handling | 25 |
| 09 | Advanced Data Modification | 20 |
| 10 | Views, Procedures, Functions & Transactions | 20 |
| 11 | Performance & Optimization | 20 |
| 12 | Real-World Interview Scenarios | 15 |
| **Total** | | **300** |

---

## SQL Fundamentals

### 1. Select all columns from employees.

**Difficulty:** `Easy`  
**Concept:** `SELECT`

**SQL**

```sql
SELECT *
FROM employees;
```

**Answer / Explanation**

Returns every column and row from the employees table.

---

### 2. Select only employee names and salaries.

**Difficulty:** `Easy`  
**Concept:** `Projection`

**SQL**

```sql
SELECT employee_name, salary
FROM employees;
```

**Answer / Explanation**

Selecting named columns keeps the result focused and avoids unnecessary data.

---

### 3. Return unique department IDs.

**Difficulty:** `Easy`  
**Concept:** `DISTINCT`

**SQL**

```sql
SELECT DISTINCT department_id
FROM employees;
```

**Answer / Explanation**

DISTINCT removes duplicate department IDs from the result.

---

### 4. Find employees whose salary is greater than 80000.

**Difficulty:** `Easy`  
**Concept:** `WHERE`

**SQL**

```sql
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary > 80000;
```

**Answer / Explanation**

WHERE filters rows before they are returned.

---

### 5. Find employees hired after January 1, 2024.

**Difficulty:** `Easy`  
**Concept:** `Date filtering`

**SQL**

```sql
SELECT employee_id, employee_name, hire_date
FROM employees
WHERE hire_date > DATE '2024-01-01';
```

**Answer / Explanation**

The date predicate keeps only employees hired after the specified date.

---

### 6. Find employees in departments 10, 20, or 30.

**Difficulty:** `Easy`  
**Concept:** `IN`

**SQL**

```sql
SELECT employee_id, employee_name, department_id
FROM employees
WHERE department_id IN (10, 20, 30);
```

**Answer / Explanation**

IN is a concise alternative to multiple OR conditions.

---

### 7. Find employees whose salary is between 50000 and 90000.

**Difficulty:** `Easy`  
**Concept:** `BETWEEN`

**SQL**

```sql
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary BETWEEN 50000 AND 90000;
```

**Answer / Explanation**

BETWEEN includes both boundary values in standard SQL.

---

### 8. Find employees whose name starts with 'A'.

**Difficulty:** `Easy`  
**Concept:** `LIKE`

**SQL**

```sql
SELECT employee_id, employee_name
FROM employees
WHERE employee_name LIKE 'A%';
```

**Answer / Explanation**

The percent wildcard matches any sequence of characters after A.

---

### 9. Find employees whose name contains 'son'.

**Difficulty:** `Easy`  
**Concept:** `LIKE`

**SQL**

```sql
SELECT employee_id, employee_name
FROM employees
WHERE employee_name LIKE '%son%';
```

**Answer / Explanation**

The pattern matches names containing the requested substring.

---

### 10. Find rows where manager_id is missing.

**Difficulty:** `Easy`  
**Concept:** `NULL`

**SQL**

```sql
SELECT employee_id, employee_name
FROM employees
WHERE manager_id IS NULL;
```

**Answer / Explanation**

NULL must be tested with IS NULL rather than = NULL.

---

### 11. Replace NULL commission with zero.

**Difficulty:** `Easy`  
**Concept:** `COALESCE`

**SQL**

```sql
SELECT employee_id, salary,
 COALESCE(commission, 0) AS commission
FROM employees;
```

**Answer / Explanation**

COALESCE returns the first non-NULL expression.

---

### 12. Create a full-name column from first and last names.

**Difficulty:** `Easy`  
**Concept:** `Concatenation`

**SQL**

```sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees;
```

**Answer / Explanation**

The concatenation operator combines the two name columns.

---

### 13. Give salary a readable alias.

**Difficulty:** `Easy`  
**Concept:** `Aliases`

**SQL**

```sql
SELECT employee_name, salary AS annual_salary
FROM employees;
```

**Answer / Explanation**

An alias gives the output column a clearer business name.

---

### 14. Sort employees by salary from highest to lowest.

**Difficulty:** `Easy`  
**Concept:** `ORDER BY`

**SQL**

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC;
```

**Answer / Explanation**

DESC places the highest salary first.

---

### 15. Return the five highest-paid employees.

**Difficulty:** `Easy`  
**Concept:** `Top-N`

**SQL**

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC
FETCH FIRST 5 ROWS ONLY;
```

**Answer / Explanation**

Sorting descending and limiting the result returns the top five employees.

---

### 16. Count all employees.

**Difficulty:** `Easy`  
**Concept:** `COUNT`

**SQL**

```sql
SELECT COUNT(*) AS employee_count
FROM employees;
```

**Answer / Explanation**

COUNT(*) counts rows, including rows containing NULL values.

---

### 17. Count employees with a recorded commission.

**Difficulty:** `Easy`  
**Concept:** `COUNT(column)`

**SQL**

```sql
SELECT COUNT(commission) AS employees_with_commission
FROM employees;
```

**Answer / Explanation**

COUNT(column) ignores NULL values.

---

### 18. Calculate the average salary.

**Difficulty:** `Easy`  
**Concept:** `AVG`

**SQL**

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```

**Answer / Explanation**

AVG calculates the arithmetic mean of non-NULL salary values.

---

### 19. Find the minimum and maximum salary.

**Difficulty:** `Easy`  
**Concept:** `MIN/MAX`

**SQL**

```sql
SELECT MIN(salary) AS min_salary,
 MAX(salary) AS max_salary
FROM employees;
```

**Answer / Explanation**

MIN and MAX identify the lowest and highest non-NULL values.

---

### 20. Calculate total salary expense.

**Difficulty:** `Easy`  
**Concept:** `SUM`

**SQL**

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

**Answer / Explanation**

SUM adds all non-NULL salary values.

---

### 21. Create a salary band using CASE.

**Difficulty:** `Easy`  
**Concept:** `CASE`

**SQL**

```sql
SELECT employee_name, salary,
 CASE
 WHEN salary >= 100000 THEN 'High'
 WHEN salary >= 60000 THEN 'Medium'
 ELSE 'Low'
 END AS salary_band
FROM employees;
```

**Answer / Explanation**

CASE converts numeric salary ranges into business-friendly categories.

---

### 22. Find employees who are not in department 10.

**Difficulty:** `Easy`  
**Concept:** `Comparison`

**SQL**

```sql
SELECT employee_id, employee_name
FROM employees
WHERE department_id <> 10;
```

**Answer / Explanation**

The not-equal predicate excludes department 10.

---

### 23. Find employees with salary greater than 70000 and department 20.

**Difficulty:** `Easy`  
**Concept:** `AND`

**SQL**

```sql
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary > 70000
 AND department_id = 20;
```

**Answer / Explanation**

AND requires both conditions to be true.

---

### 24. Find employees in department 10 or earning over 100000.

**Difficulty:** `Easy`  
**Concept:** `OR`

**SQL**

```sql
SELECT employee_id, employee_name, salary, department_id
FROM employees
WHERE department_id = 10
 OR salary > 100000;
```

**Answer / Explanation**

OR keeps a row when either predicate is true.

---

### 25. Return the current date.

**Difficulty:** `Easy`  
**Concept:** `Date function`

**SQL**

```sql
SELECT CURRENT_DATE AS today;
```

**Answer / Explanation**

CURRENT_DATE returns the database session's current date.

---

### 26. Return the current timestamp.

**Difficulty:** `Easy`  
**Concept:** `Timestamp`

**SQL**

```sql
SELECT CURRENT_TIMESTAMP AS current_time;
```

**Answer / Explanation**

CURRENT_TIMESTAMP returns the current date and time.

---

### 27. Remove duplicate employee/dept combinations.

**Difficulty:** `Easy`  
**Concept:** `DISTINCT`

**SQL**

```sql
SELECT DISTINCT employee_id, department_id
FROM employee_department;
```

**Answer / Explanation**

DISTINCT applies to the complete selected combination.

---

### 28. Find employees with salaries outside 50000–90000.

**Difficulty:** `Easy`  
**Concept:** `NOT BETWEEN`

**SQL**

```sql
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary NOT BETWEEN 50000 AND 90000;
```

**Answer / Explanation**

NOT BETWEEN excludes values inside the specified inclusive range.

---

### 29. Find employees whose names match either Smith or Jones exactly.

**Difficulty:** `Easy`  
**Concept:** `IN`

**SQL**

```sql
SELECT employee_id, employee_name
FROM employees
WHERE employee_name IN ('Smith', 'Jones');
```

**Answer / Explanation**

IN compares the column against a finite list of allowed values.

---

## Filtering, Sorting & Expressions

### 30. Find employees hired in 2025 using a half-open date range.

**Difficulty:** `Medium`  
**Concept:** `Date range`

**SQL**

```sql
SELECT employee_id, employee_name
FROM employees
WHERE hire_date >= DATE '2025-01-01'
 AND hire_date < DATE '2026-01-01';
```

**Answer / Explanation**

The half-open range is safe for both DATE and TIMESTAMP columns.

---

### 31. Find names whose second character is 'a'.

**Difficulty:** `Easy`  
**Concept:** `LIKE wildcard`

**SQL**

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE '_a%';
```

**Answer / Explanation**

Underscore matches exactly one character.

---

### 32. Find names ending in 'y'.

**Difficulty:** `Easy`  
**Concept:** `LIKE`

**SQL**

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE '%y';
```

**Answer / Explanation**

The percent wildcard before y allows any preceding characters.

---

### 33. Find employees whose salary is not NULL and exceeds 75000.

**Difficulty:** `Easy`  
**Concept:** `NULL + filter`

**SQL**

```sql
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary IS NOT NULL
 AND salary > 75000;
```

**Answer / Explanation**

The explicit NULL test documents the intended data-quality condition.

---

### 34. Sort by department ascending and salary descending.

**Difficulty:** `Medium`  
**Concept:** `Multi-column ORDER BY`

**SQL**

```sql
SELECT employee_name, department_id, salary
FROM employees
ORDER BY department_id ASC, salary DESC;
```

**Answer / Explanation**

SQL sorts by the first key, then uses the second key to break ties.

---

### 35. Put NULL commissions last.

**Difficulty:** `Medium`  
**Concept:** `NULL ordering`

**SQL**

```sql
SELECT employee_name, commission
FROM employees
ORDER BY commission NULLS LAST;
```

**Answer / Explanation**

NULLS LAST explicitly controls where missing commission values appear.

---

### 36. Calculate monthly salary from annual salary.

**Difficulty:** `Easy`  
**Concept:** `Arithmetic`

**SQL**

```sql
SELECT employee_name, salary / 12.0 AS monthly_salary
FROM employees;
```

**Answer / Explanation**

Dividing by 12 converts annual salary to a monthly estimate.

---

### 37. Calculate a 10% salary increase.

**Difficulty:** `Easy`  
**Concept:** `Arithmetic`

**SQL**

```sql
SELECT employee_name, salary,
 salary * 1.10 AS adjusted_salary
FROM employees;
```

**Answer / Explanation**

Multiplying by 1.10 adds ten percent to the salary.

---

### 38. Round salary to the nearest thousand.

**Difficulty:** `Medium`  
**Concept:** `ROUND`

**SQL**

```sql
SELECT employee_name,
 ROUND(salary, -3) AS rounded_salary
FROM employees;
```

**Answer / Explanation**

ROUND can be used with a negative precision to round large units.

---

### 39. Convert employee names to uppercase.

**Difficulty:** `Easy`  
**Concept:** `UPPER`

**SQL**

```sql
SELECT UPPER(employee_name) AS employee_name
FROM employees;
```

**Answer / Explanation**

UPPER converts alphabetic characters to uppercase.

---

### 40. Trim spaces from department names.

**Difficulty:** `Easy`  
**Concept:** `TRIM`

**SQL**

```sql
SELECT TRIM(department_name) AS department_name
FROM departments;
```

**Answer / Explanation**

TRIM removes leading and trailing spaces.

---

### 41. Find the length of every employee name.

**Difficulty:** `Easy`  
**Concept:** `String length`

**SQL**

```sql
SELECT employee_name,
 LENGTH(employee_name) AS name_length
FROM employees;
```

**Answer / Explanation**

LENGTH returns the number of characters for common SQL dialects.

---

### 42. Extract the first three characters of each employee name.

**Difficulty:** `Medium`  
**Concept:** `SUBSTRING`

**SQL**

```sql
SELECT employee_name,
 SUBSTRING(employee_name FROM 1 FOR 3) AS prefix
FROM employees;
```

**Answer / Explanation**

SUBSTRING extracts a defined section of a string.

---

### 43. Replace hyphens in employee codes with slashes.

**Difficulty:** `Easy`  
**Concept:** `REPLACE`

**SQL**

```sql
SELECT REPLACE(employee_code, '-', '/') AS formatted_code
FROM employees;
```

**Answer / Explanation**

REPLACE substitutes every occurrence of the target text.

---

### 44. Filter case-insensitively for names containing 'ann'.

**Difficulty:** `Medium`  
**Concept:** `LOWER + LIKE`

**SQL**

```sql
SELECT employee_name
FROM employees
WHERE LOWER(employee_name) LIKE '%ann%';
```

**Answer / Explanation**

Normalizing both the search value and column makes the match case-insensitive.

---

### 45. Sort employees by the length of their name.

**Difficulty:** `Medium`  
**Concept:** `Expression in ORDER BY`

**SQL**

```sql
SELECT employee_name
FROM employees
ORDER BY LENGTH(employee_name), employee_name;
```

**Answer / Explanation**

Expressions can be used as sort keys.

---

### 46. Return a label for active/inactive employees.

**Difficulty:** `Easy`  
**Concept:** `CASE`

**SQL**

```sql
SELECT employee_name,
 CASE WHEN status = 'ACTIVE' THEN 'Active'
 ELSE 'Inactive' END AS status_label
FROM employees;
```

**Answer / Explanation**

CASE maps stored status codes to readable output.

---

### 47. Use NULLIF to avoid division by zero.

**Difficulty:** `Medium`  
**Concept:** `NULLIF`

**SQL**

```sql
SELECT employee_id,
 sales_amount / NULLIF(order_count, 0) AS average_order_value
FROM customer_metrics;
```

**Answer / Explanation**

NULLIF converts zero to NULL so the division does not raise a zero-divide error.

---

### 48. Use COALESCE to select a preferred phone number.

**Difficulty:** `Medium`  
**Concept:** `COALESCE`

**SQL**

```sql
SELECT customer_id,
 COALESCE(mobile_phone, work_phone, home_phone, 'N/A') AS phone
FROM customers;
```

**Answer / Explanation**

COALESCE chooses the first available phone value.

---

### 49. Find employees whose salary is exactly the department maximum.

**Difficulty:** `Hard`  
**Concept:** `Correlated subquery`

**SQL**

```sql
SELECT e.employee_id, e.employee_name, e.salary
FROM employees e
WHERE e.salary = (
 SELECT MAX(e2.salary)
 FROM employees e2
 WHERE e2.department_id = e.department_id
);
```

**Answer / Explanation**

The correlated subquery calculates the maximum salary separately for each employee's department.

---

### 50. Return the second-highest distinct salary.

**Difficulty:** `Medium`  
**Concept:** `Subquery`

**SQL**

```sql
SELECT MAX(salary) AS second_highest_salary
FROM employees


WHERE salary < (SELECT MAX(salary) FROM employees);
```

**Answer / Explanation**

Filtering below the overall maximum and taking MAX gives the second distinct value.

---

### 51. Find employees whose salary is a multiple of 5000.

**Difficulty:** `Medium`  
**Concept:** `MOD`

**SQL**

```sql
SELECT employee_id, employee_name, salary
FROM employees
WHERE MOD(salary, 5000) = 0;
```

**Answer / Explanation**

MOD returns the remainder; zero means the salary divides evenly by 5000.

---

### 52. Find records created in the last 30 days.

**Difficulty:** `Medium`  
**Concept:** `Relative dates`

**SQL**

```sql
SELECT *
FROM orders
WHERE created_at >= CURRENT_TIMESTAMP - INTERVAL '30' DAY;
```

**Answer / Explanation**

Subtracting an interval creates a rolling 30-day filter.

---

### 53. Find orders placed during business hours.

**Difficulty:** `Medium`  
**Concept:** `Time filtering`

**SQL**

```sql
SELECT order_id, order_date
FROM orders
WHERE CAST(order_date AS TIME) BETWEEN TIME '09:00:00' AND TIME '17:00:00';
```

**Answer / Explanation**

Casting to TIME allows the time-of-day portion to be compared.

---

### 54. Use a CASE expression to classify order value.

**Difficulty:** `Easy`  
**Concept:** `CASE`

**SQL**

```sql
SELECT order_id, total_amount,
 CASE WHEN total_amount >= 1000 THEN 'Large'
 WHEN total_amount >= 500 THEN 'Medium'
 ELSE 'Small' END AS order_size
FROM orders;
```

**Answer / Explanation**

CASE implements ordered business rules directly in SQL.

---

## JOINs

### 55. List employees with their department names.

**Difficulty:** `Easy`  
**Concept:** `INNER JOIN`

**SQL**

```sql
SELECT e.employee_name, d.department_name
FROM employees e
JOIN departments d
 ON d.department_id = e.department_id;
```

**Answer / Explanation**

An inner join returns employees that have a matching department.

---

### 56. List all employees, including those without a department.

**Difficulty:** `Easy`  
**Concept:** `LEFT JOIN`

**SQL**

```sql
SELECT e.employee_name, d.department_name
FROM employees e
LEFT JOIN departments d
 ON d.department_id = e.department_id;
```

**Answer / Explanation**

LEFT JOIN preserves every employee even when no department matches.

---

### 57. Find departments with no employees.

**Difficulty:** `Medium`  
**Concept:** `Anti-join`

**SQL**

```sql
SELECT d.department_id, d.department_name
FROM departments d
LEFT JOIN employees e
 ON e.department_id = d.department_id
WHERE e.employee_id IS NULL;
```

**Answer / Explanation**

The NULL check identifies departments with no matching employee.

---

### 58. Find employees who have received a bonus.

**Difficulty:** `Easy`  
**Concept:** `JOIN + DISTINCT`

**SQL**

```sql
SELECT DISTINCT e.employee_id, e.employee_name
FROM employees e
JOIN bonuses b
 ON b.employee_id = e.employee_id;
```

**Answer / Explanation**

The join finds bonus recipients; DISTINCT prevents duplicates when multiple bonuses exist.

---

### 59. Find employees who never received a bonus.

**Difficulty:** `Medium`  
**Concept:** `Anti-join`

**SQL**

```sql
SELECT e.employee_id, e.employee_name
FROM employees e


LEFT JOIN bonuses b
 ON b.employee_id = e.employee_id
WHERE b.employee_id IS NULL;
```

**Answer / Explanation**

A left join plus a NULL match identifies employees with no bonus record.

---

### 60. Show each employee beside their manager.

**Difficulty:** `Medium`  
**Concept:** `Self join`

**SQL**

```sql
SELECT e.employee_name AS employee,
 m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
 ON m.employee_id = e.manager_id;
```

**Answer / Explanation**

The same employees table is joined twice to map an employee to a manager.

---

### 61. Find employees whose manager earns more than they do.

**Difficulty:** `Medium`  
**Concept:** `Self join`

**SQL**

```sql
SELECT e.employee_name AS employee,
 m.employee_name AS manager
FROM employees e
JOIN employees m
 ON m.employee_id = e.manager_id
WHERE m.salary > e.salary;
```

**Answer / Explanation**

The self join provides the manager's salary for direct comparison.

---

### 62. Show customers and their orders, including customers with no orders.

**Difficulty:** `Easy`  
**Concept:** `LEFT JOIN`

**SQL**

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM customers c
LEFT JOIN orders o
 ON o.customer_id = c.customer_id;
```

**Answer / Explanation**

Customers remain in the result even when their order side is NULL.

---

### 63. Find customers who placed at least one order.

**Difficulty:** `Easy`  
**Concept:** `Semi-join pattern`

**SQL**

```sql
SELECT DISTINCT c.customer_id, c.customer_name
FROM customers c
JOIN orders o
 ON o.customer_id = c.customer_id;
```

**Answer / Explanation**

An inner join identifies customers with at least one matching order.

---

### 64. Find orders with product details.

**Difficulty:** `Medium`  
**Concept:** `Multi-table join`

**SQL**

```sql
SELECT o.order_id, p.product_name, oi.quantity
FROM orders o
JOIN order_items oi ON oi.order_id = o.order_id
JOIN products p ON p.product_id = oi.product_id;
```

**Answer / Explanation**

The order header joins to items, then items join to the product catalog.

---

### 65. Calculate order totals from line items.

**Difficulty:** `Medium`  
**Concept:** `JOIN + aggregation`

**SQL**

```sql
SELECT o.order_id,
 SUM(oi.quantity * oi.unit_price) AS order_total
FROM orders o
JOIN order_items oi ON oi.order_id = o.order_id
GROUP BY o.order_id;
```

**Answer / Explanation**

Line values are calculated first and then summed per order.

---

### 66. Find products that have never been ordered.

**Difficulty:** `Medium`  
**Concept:** `Anti-join`

**SQL**

```sql
SELECT p.product_id, p.product_name
FROM products p
LEFT JOIN order_items oi
 ON oi.product_id = p.product_id
WHERE oi.product_id IS NULL;
```

**Answer / Explanation**

Products with no matching line item are never ordered.

---

### 67. Find customers who ordered products from category 5.

**Difficulty:** `Medium`  
**Concept:** `Multi-table filter`

**SQL**

```sql
SELECT DISTINCT o.customer_id
FROM orders o
JOIN order_items oi ON oi.order_id = o.order_id
JOIN products p ON p.product_id = oi.product_id
WHERE p.category_id = 5;
```

**Answer / Explanation**

Joining through order_items connects customers to product categories.

---

### 68. Find customers who ordered from more than one category.

**Difficulty:** `Medium`  
**Concept:** `JOIN + HAVING`

**SQL**

```sql
SELECT o.customer_id
FROM orders o
JOIN order_items oi ON oi.order_id = o.order_id
JOIN products p ON p.product_id = oi.product_id
GROUP BY o.customer_id
HAVING COUNT(DISTINCT p.category_id) > 1;
```

**Answer / Explanation**

COUNT(DISTINCT category_id) measures category variety per customer.

---

### 69. Show every department and its employee count, including zero.

**Difficulty:** `Medium`  
**Concept:** `LEFT JOIN + COUNT`

**SQL**

```sql
SELECT d.department_id, d.department_name,
 COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e ON e.department_id = d.department_id
GROUP BY d.department_id, d.department_name;
```

**Answer / Explanation**

COUNT(employee_id) returns zero for departments with no matching employee.

---

### 70. Join orders to customers using two columns.

**Difficulty:** `Medium`  
**Concept:** `Composite join`

**SQL**

```sql
SELECT o.order_id, c.customer_name
FROM orders o
JOIN customers c
 ON c.customer_id = o.customer_id
 AND c.region = o.region;
```

**Answer / Explanation**

Both keys must match, which is useful when a relationship is defined by multiple columns.

---

### 71. Find duplicate customer records by email.

**Difficulty:** `Hard`  
**Concept:** `Self join`

**SQL**

```sql
SELECT c1.customer_id, c2.customer_id, c1.email
FROM customers c1
JOIN customers c2
 ON c1.email = c2.email
 AND c1.customer_id < c2.customer_id;
```

**Answer / Explanation**

The ordered ID comparison prevents returning each duplicate pair twice.

---

### 72. Find employees who work in the same department as employee 101.

**Difficulty:** `Medium`  
**Concept:** `Subquery`

**SQL**

```sql
SELECT e.employee_id, e.employee_name
FROM employees e
WHERE e.department_id = (
 SELECT department_id FROM employees WHERE employee_id = 101
);
```

**Answer / Explanation**

The subquery identifies the target department and the outer query finds peers.

---

### 73. Find customers who have orders but no returned items.

**Difficulty:** `Hard`  
**Concept:** `Anti-join`

**SQL**

```sql
SELECT DISTINCT c.customer_id, c.customer_name
FROM customers c
JOIN orders o ON o.customer_id = c.customer_id
LEFT JOIN returns r ON r.order_id = o.order_id
WHERE r.order_id IS NULL;
```

**Answer / Explanation**

The left join to returns isolates orders with no return record.

---

### 74. Join employees to the latest salary record.

**Difficulty:** `Hard`  
**Concept:** `Latest-row join`

**SQL**

```sql
SELECT e.employee_id, e.employee_name, s.salary
FROM employees e
JOIN employee_salary_history s
 ON s.employee_id = e.employee_id
WHERE s.effective_date = (
 SELECT MAX(s2.effective_date)
 FROM employee_salary_history s2
 WHERE s2.employee_id = s.employee_id
);
```

**Answer / Explanation**

The correlated MAX identifies the most recent effective salary for each employee.

---

### 75. Find orders whose customer is inactive.

**Difficulty:** `Easy`  
**Concept:** `JOIN filter`

**SQL**

```sql
SELECT o.order_id, o.customer_id
FROM orders o
JOIN customers c ON c.customer_id = o.customer_id
WHERE c.status = 'INACTIVE';
```

**Answer / Explanation**

The customer status is available after joining the customer table.

---

### 76. Find employees and their department location.

**Difficulty:** `Medium`  
**Concept:** `Three-table join`

**SQL**

```sql
SELECT e.employee_name, d.department_name, l.city
FROM employees e
JOIN departments d ON d.department_id = e.department_id
JOIN locations l ON l.location_id = d.location_id;
```

**Answer / Explanation**

Each join follows the foreign-key relationship from employee to location.

---

### 77. Find customers with both a completed order and an open support ticket.

**Difficulty:** `Hard`  
**Concept:** `Multiple predicates`

**SQL**

```sql
SELECT DISTINCT c.customer_id, c.customer_name
FROM customers c
JOIN orders o ON o.customer_id = c.customer_id AND o.status = 'COMPLETED'
JOIN support_tickets t ON t.customer_id = c.customer_id AND t.status = 'OPEN';
```

**Answer / Explanation**

Both relationships must exist, so inner joins with status predicates are appropriate.

---

### 78. Find products purchased by every customer in region 'WEST'.

**Difficulty:** `Hard`  
**Concept:** `Relational division`

**SQL**

```sql
SELECT oi.product_id
FROM orders o
JOIN order_items oi ON oi.order_id = o.order_id
JOIN customers c ON c.customer_id = o.customer_id
WHERE c.region = 'WEST'
GROUP BY oi.product_id
HAVING COUNT(DISTINCT c.customer_id) = (
 SELECT COUNT(*) FROM customers WHERE region = 'WEST'
);
```

**Answer / Explanation**

The HAVING comparison requires the product to appear for every WEST customer.

---

### 79. Find departments where at least one employee earns over 150000.

**Difficulty:** `Medium`  
**Concept:** `JOIN filter`

**SQL**

```sql
SELECT DISTINCT d.department_id, d.department_name
FROM departments d
JOIN employees e ON e.department_id = d.department_id
WHERE e.salary > 150000;
```

**Answer / Explanation**

DISTINCT returns each qualifying department once.

---

### 80. Find departments where all employees earn at least 50000.

**Difficulty:** `Hard`  
**Concept:** `HAVING MIN`

**SQL**

```sql
SELECT d.department_id, d.department_name
FROM departments d
JOIN employees e ON e.department_id = d.department_id
GROUP BY d.department_id, d.department_name
HAVING MIN(e.salary) >= 50000;
```

**Answer / Explanation**

If the minimum salary meets the threshold, every employee in that group does.

---

### 81. Find employees who share a manager with employee 101.

**Difficulty:** `Medium`  
**Concept:** `Self join`

**SQL**

```sql
SELECT e.employee_id, e.employee_name
FROM employees e
JOIN employees target ON target.employee_id = 101
WHERE e.manager_id = target.manager_id
 AND e.employee_id <> target.employee_id;
```

**Answer / Explanation**

Joining the target employee provides the manager ID used to find peers.

---

### 82. Find orders that contain at least one item from category 'Electronics'.

**Difficulty:** `Easy`  
**Concept:** `Multi-table join`

**SQL**

```sql
SELECT DISTINCT o.order_id
FROM orders o
JOIN order_items oi ON oi.order_id = o.order_id
JOIN products p ON p.product_id = oi.product_id
WHERE p.category_name = 'Electronics';
```

**Answer / Explanation**

The chain from orders to products allows category filtering.

---

### 83. Find customers whose latest order was placed in the current year.

**Difficulty:** `Hard`  
**Concept:** `JOIN + HAVING`

**SQL**

```sql
SELECT c.customer_id, c.customer_name
FROM customers c
JOIN orders o ON o.customer_id = c.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING MAX(o.order_date) >= DATE_TRUNC('year', CURRENT_DATE);
```

**Answer / Explanation**

MAX(order_date) represents the customer's latest order.

---

### 84. Find employee pairs who work in the same department.

**Difficulty:** `Hard`  
**Concept:** `Self join`

**SQL**

```sql
SELECT e1.employee_name, e2.employee_name, e1.department_id
FROM employees e1
JOIN employees e2


 ON e1.department_id = e2.department_id
 AND e1.employee_id < e2.employee_id;
```

**Answer / Explanation**

The ID ordering prevents self-pairs and duplicate mirrored pairs.

---

### 85. Find orders with no matching customer.

**Difficulty:** `Medium`  
**Concept:** `Data-quality anti-join`

**SQL**

```sql
SELECT o.order_id, o.customer_id
FROM orders o
LEFT JOIN customers c ON c.customer_id = o.customer_id
WHERE c.customer_id IS NULL;
```

**Answer / Explanation**

The query detects orphaned foreign-key references.

---

### 86. Find employees whose department has no assigned location.

**Difficulty:** `Medium`  
**Concept:** `Multi-table LEFT JOIN`

**SQL**

```sql
SELECT e.employee_id, e.employee_name
FROM employees e
JOIN departments d ON d.department_id = e.department_id
LEFT JOIN locations l ON l.location_id = d.location_id
WHERE l.location_id IS NULL;
```

**Answer / Explanation**

The final NULL check identifies departments lacking a location.

---

### 87. Find the highest-value order for each customer.

**Difficulty:** `Hard`  
**Concept:** `Correlated subquery`

**SQL**

```sql
SELECT o.customer_id, o.order_id, o.total_amount
FROM orders o
WHERE o.total_amount = (
 SELECT MAX(o2.total_amount)
 FROM orders o2
 WHERE o2.customer_id = o.customer_id
);
```

**Answer / Explanation**

The correlated MAX compares each order against its customer's maximum.

---

### 88. Find customers who bought a product but never bought category 9.

**Difficulty:** `Hard`  
**Concept:** `NOT EXISTS`

**SQL**

```sql
SELECT DISTINCT o.customer_id
FROM orders o
JOIN order_items oi ON oi.order_id = o.order_id
WHERE NOT EXISTS (
 SELECT 1


 FROM orders o2
 JOIN order_items oi2 ON oi2.order_id = o2.order_id
 JOIN products p2 ON p2.product_id = oi2.product_id
 WHERE o2.customer_id = o.customer_id
 AND p2.category_id = 9
);
```

**Answer / Explanation**

NOT EXISTS removes customers for whom a category-9 purchase can be found.

---

### 89. Find the number of orders per customer while retaining customers with zero orders.

**Difficulty:** `Medium`  
**Concept:** `LEFT JOIN aggregation`

**SQL**

```sql
SELECT c.customer_id, c.customer_name,
 COUNT(o.order_id) AS order_count
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.customer_id
GROUP BY c.customer_id, c.customer_name;
```

**Answer / Explanation**

COUNT of the joined key gives zero for customers with no matching order.

---

## Aggregation & GROUP BY

### 90. Find average salary by department.

**Difficulty:** `Easy`  
**Concept:** `GROUP BY`

**SQL**

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id;
```

**Answer / Explanation**

GROUP BY creates one aggregate group per department.

---

### 91. Find departments with average salary above 90000.

**Difficulty:** `Medium`  
**Concept:** `HAVING`

**SQL**

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 90000;
```

**Answer / Explanation**

HAVING filters groups after aggregation.

---

### 92. Find departments with more than 20 employees.

**Difficulty:** `Easy`  
**Concept:** `HAVING`

**SQL**

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees


GROUP BY department_id
HAVING COUNT(*) > 20;
```

**Answer / Explanation**

COUNT creates the group metric and HAVING filters on it.

---

### 93. Find the department with the lowest average salary.

**Difficulty:** `Medium`  
**Concept:** `ORDER BY + aggregate`

**SQL**

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
ORDER BY avg_salary
FETCH FIRST 1 ROW ONLY;
```

**Answer / Explanation**

Sorting group averages ascending and taking one row finds the minimum.

---

### 94. Find the department with the highest payroll.

**Difficulty:** `Medium`  
**Concept:** `SUM`

**SQL**

```sql
SELECT department_id, SUM(salary) AS payroll
FROM employees
GROUP BY department_id
ORDER BY payroll DESC
FETCH FIRST 1 ROW ONLY;
```

**Answer / Explanation**

SUM measures payroll per department.

---

### 95. Find the total sales by month.

**Difficulty:** `Medium`  
**Concept:** `Date aggregation`

**SQL**

```sql
SELECT DATE_TRUNC('month', order_date) AS month,
 SUM(total_amount) AS sales
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY month;
```

**Answer / Explanation**

Orders are grouped by calendar month before summing revenue.

---

### 96. Find the number of orders by status.

**Difficulty:** `Easy`  
**Concept:** `COUNT`

**SQL**

```sql
SELECT status, COUNT(*) AS order_count
FROM orders
GROUP BY status;
```

**Answer / Explanation**

Each status becomes an aggregate group.

---

### 97. Find the percentage of orders that are completed.

**Difficulty:** `Medium`  
**Concept:** `Conditional aggregation`

**SQL**

```sql
SELECT 100.0 * SUM(CASE WHEN status = 'COMPLETED' THEN 1 ELSE 0 END) / COUNT(*) AS completed_pct
FROM orders;
```

**Answer / Explanation**

CASE counts completed rows while COUNT(*) provides the denominator.

---

### 98. Find customers with total purchases over 10000.

**Difficulty:** `Easy`  
**Concept:** `HAVING`

**SQL**

```sql
SELECT customer_id, SUM(total_amount) AS total_spend
FROM orders
GROUP BY customer_id
HAVING SUM(total_amount) > 10000;
```

**Answer / Explanation**

The HAVING clause filters customers after total spend is calculated.

---

### 99. Find the average order value per customer.

**Difficulty:** `Easy`  
**Concept:** `AVG`

**SQL**

```sql
SELECT customer_id, AVG(total_amount) AS avg_order_value
FROM orders
GROUP BY customer_id;
```

**Answer / Explanation**

AVG summarizes order values for each customer.

---

### 100. Find the largest single order per month.

**Difficulty:** `Medium`  
**Concept:** `MAX`

**SQL**

```sql
SELECT DATE_TRUNC('month', order_date) AS month,
 MAX(total_amount) AS largest_order
FROM orders
GROUP BY DATE_TRUNC('month', order_date);
```

**Answer / Explanation**

MAX identifies the largest order within each month.

---

### 101. Find the minimum order amount for each region.

**Difficulty:** `Medium`  
**Concept:** `JOIN + MIN`

**SQL**

```sql
SELECT c.region, MIN(o.total_amount) AS min_order
FROM customers c
JOIN orders o ON o.customer_id = c.customer_id


GROUP BY c.region;
```

**Answer / Explanation**

Orders are associated with customer regions before calculating the minimum.

---

### 102. Count distinct customers per month.

**Difficulty:** `Medium`  
**Concept:** `COUNT DISTINCT`

**SQL**

```sql
SELECT DATE_TRUNC('month', order_date) AS month,
 COUNT(DISTINCT customer_id) AS customers
FROM orders
GROUP BY DATE_TRUNC('month', order_date);
```

**Answer / Explanation**

COUNT(DISTINCT) prevents repeat orders from inflating customer counts.

---

### 103. Find products with total quantity sold above 1000.

**Difficulty:** `Easy`  
**Concept:** `SUM + HAVING`

**SQL**

```sql
SELECT product_id, SUM(quantity) AS units_sold
FROM order_items
GROUP BY product_id
HAVING SUM(quantity) > 1000;
```

**Answer / Explanation**

The aggregate quantity is filtered using HAVING.

---

### 104. Calculate revenue per product.

**Difficulty:** `Medium`  
**Concept:** `Derived aggregation`

**SQL**

```sql
SELECT product_id,
 SUM(quantity * unit_price) AS revenue
FROM order_items
GROUP BY product_id;
```

**Answer / Explanation**

Revenue is calculated at line level and summed by product.

---

### 105. Find categories with more than 50 products.

**Difficulty:** `Easy`  
**Concept:** `COUNT + HAVING`

**SQL**

```sql
SELECT category_id, COUNT(*) AS product_count
FROM products
GROUP BY category_id
HAVING COUNT(*) > 50;
```

**Answer / Explanation**

The group count identifies categories with a large catalog.

---

### 106. Find the most common customer region.

**Difficulty:** `Easy`  
**Concept:** `Top group`

**SQL**

```sql
SELECT region, COUNT(*) AS customer_count
FROM customers
GROUP BY region
ORDER BY customer_count DESC
FETCH FIRST 1 ROW ONLY;
```

**Answer / Explanation**

Sorting grouped counts descending returns the most common region.

---

### 107. Find customers with exactly three orders.

**Difficulty:** `Easy`  
**Concept:** `Exact count`

**SQL**

```sql
SELECT customer_id, COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) = 3;
```

**Answer / Explanation**

HAVING can enforce an exact aggregate count.

---

### 108. Find employees whose department payroll exceeds 1 million.

**Difficulty:** `Medium`  
**Concept:** `Payroll`

**SQL**

```sql
SELECT department_id, SUM(salary) AS payroll
FROM employees
GROUP BY department_id
HAVING SUM(salary) > 1000000;
```

**Answer / Explanation**

Department payroll is the sum of all salaries in the group.

---

### 109. Find the ratio of active to inactive customers.

**Difficulty:** `Hard`  
**Concept:** `Conditional aggregation`

**SQL**

```sql
SELECT
 SUM(CASE WHEN status = 'ACTIVE' THEN 1 ELSE 0 END) * 1.0 /
 NULLIF(SUM(CASE WHEN status = 'INACTIVE' THEN 1 ELSE 0 END), 0) AS active_inactive_ratio
FROM customers;
```

**Answer / Explanation**

Conditional SUMs create both counts and NULLIF protects against division by zero.

---

### 110. Find the median salary.

**Difficulty:** `Hard`  
**Concept:** `Median`

**SQL**

```sql
SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) AS median_salary


FROM employees;
```

**Answer / Explanation**

PERCENTILE_CONT calculates a continuous percentile; 0.5 is the median.

---

### 111. Find the 90th percentile of order values.

**Difficulty:** `Hard`  
**Concept:** `Percentile`

**SQL**

```sql
SELECT PERCENTILE_CONT(0.9) WITHIN GROUP (ORDER BY total_amount) AS p90_order_value
FROM orders;
```

**Answer / Explanation**

The 0.9 percentile estimates the value below which 90% of orders fall.

---

### 112. Find departments with both low and high earners.

**Difficulty:** `Medium`  
**Concept:** `MIN/MAX`

**SQL**

```sql
SELECT department_id
FROM employees
GROUP BY department_id
HAVING MIN(salary) < 50000
 AND MAX(salary) > 150000;
```

**Answer / Explanation**

Both predicates must be true within the same department.

---

### 113. Find the average number of orders per customer.

**Difficulty:** `Medium`  
**Concept:** `Aggregate of aggregate`

**SQL**

```sql
SELECT AVG(order_count) AS avg_orders_per_customer
FROM (
 SELECT customer_id, COUNT(*) AS order_count
 FROM orders
 GROUP BY customer_id
) x;
```

**Answer / Explanation**

The inner query calculates orders per customer; the outer query averages those counts.

---

### 114. Find daily sales and the number of unique buyers.

**Difficulty:** `Medium`  
**Concept:** `Multiple aggregates`

**SQL**

```sql
SELECT CAST(order_date AS DATE) AS order_day,
 SUM(total_amount) AS sales,
 COUNT(DISTINCT customer_id) AS buyers
FROM orders
GROUP BY CAST(order_date AS DATE)
ORDER BY order_day;
```

**Answer / Explanation**

Daily revenue and distinct buyers are calculated in one grouped query.

---

### 115. Find the month with the highest revenue.

**Difficulty:** `Medium`  
**Concept:** `Top aggregate`

**SQL**

```sql
SELECT DATE_TRUNC('month', order_date) AS month,
 SUM(total_amount) AS revenue
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY revenue DESC
FETCH FIRST 1 ROW ONLY;
```

**Answer / Explanation**

The highest monthly revenue is the first row after descending sort.

---

### 116. Find departments where the salary spread exceeds 100000.

**Difficulty:** `Medium`  
**Concept:** `Spread`

**SQL**

```sql
SELECT department_id,
 MAX(salary) - MIN(salary) AS salary_spread
FROM employees
GROUP BY department_id
HAVING MAX(salary) - MIN(salary) > 100000;
```

**Answer / Explanation**

Subtracting the minimum from the maximum measures salary spread.

---

### 117. Find products whose average selling price is above list price.

**Difficulty:** `Hard`  
**Concept:** `Aggregate comparison`

**SQL**

```sql
SELECT oi.product_id,
 AVG(oi.unit_price) AS avg_selling_price,
 MAX(p.list_price) AS list_price
FROM order_items oi
JOIN products p ON p.product_id = oi.product_id
GROUP BY oi.product_id
HAVING AVG(oi.unit_price) > MAX(p.list_price);
```

**Answer / Explanation**

The HAVING clause compares the observed average selling price with the catalog price.

---

## Subqueries

### 118. Find employees earning above the company average.

**Difficulty:** `Medium`  
**Concept:** `Scalar subquery`

**SQL**

```sql
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

**Answer / Explanation**

The subquery calculates one company-wide average used by the outer query.

---

### 119. Find employees earning above their department average.

**Difficulty:** `Hard`  
**Concept:** `Correlated subquery`

**SQL**

```sql
SELECT e.employee_id, e.employee_name, e.salary
FROM employees e
WHERE e.salary > (
 SELECT AVG(e2.salary)
 FROM employees e2
 WHERE e2.department_id = e.department_id
);
```

**Answer / Explanation**

The inner average is recalculated for the current employee's department.

---

### 120. Find customers whose order count exceeds the average customer order count.

**Difficulty:** `Hard`  
**Concept:** `Nested aggregate`

**SQL**

```sql
SELECT customer_id, COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > (
 SELECT AVG(order_count)
 FROM (
 SELECT customer_id, COUNT(*) AS order_count
 FROM orders
 GROUP BY customer_id
 ) x
);
```

**Answer / Explanation**

The inner derived table produces one order count per customer before averaging.

---

### 121. Find the third-highest distinct salary.

**Difficulty:** `Hard`  
**Concept:** `Nested scalar subqueries`

**SQL**

```sql
SELECT MAX(salary) AS third_highest
FROM employees
WHERE salary < (
 SELECT MAX(salary)
 FROM employees
 WHERE salary < (SELECT MAX(salary) FROM employees)
);
```

**Answer / Explanation**

Successive MAX operations move downward through distinct salary levels.

---

### 122. Find employees who share a salary with someone else.

**Difficulty:** `Medium`  
**Concept:** `IN subquery`

**SQL**

```sql
SELECT e.employee_id, e.employee_name, e.salary
FROM employees e
WHERE e.salary IN (
 SELECT salary
 FROM employees
 GROUP BY salary
 HAVING COUNT(*) > 1
);
```

**Answer / Explanation**

The inner query identifies duplicate salary values.

---

### 123. Find customers who have at least one order over 5000.

**Difficulty:** `Medium`  
**Concept:** `EXISTS`

**SQL**

```sql
SELECT c.customer_id, c.customer_name
FROM customers c
WHERE EXISTS (
 SELECT 1
 FROM orders o
 WHERE o.customer_id = c.customer_id
 AND o.total_amount > 5000
);
```

**Answer / Explanation**

EXISTS stops searching once a qualifying order is found.

---

### 124. Find customers who have never placed an order.

**Difficulty:** `Medium`  
**Concept:** `NOT EXISTS`

**SQL**

```sql
SELECT c.customer_id, c.customer_name
FROM customers c
WHERE NOT EXISTS (
 SELECT 1 FROM orders o
 WHERE o.customer_id = c.customer_id
);
```

**Answer / Explanation**

NOT EXISTS keeps customers for whom no order row can be found.

---

### 125. Find products priced above every product in category 3.

**Difficulty:** `Hard`  
**Concept:** `ALL`

**SQL**

```sql
SELECT product_id, product_name, list_price
FROM products
WHERE list_price > ALL (
 SELECT list_price FROM products WHERE category_id = 3
);
```

**Answer / Explanation**

The price must exceed every value returned by the subquery.

---

### 126. Find products cheaper than at least one product in category 3.

**Difficulty:** `Hard`  
**Concept:** `ANY`

**SQL**

```sql
SELECT product_id, product_name, list_price
FROM products
WHERE list_price < ANY (
 SELECT list_price FROM products WHERE category_id = 3
);
```

**Answer / Explanation**

ANY requires the comparison to succeed against at least one subquery value.

---

### 127. Find the department with the highest average salary.

**Difficulty:** `Hard`  
**Concept:** `Aggregate subquery`

**SQL**

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) = (
 SELECT MAX(avg_salary)
 FROM (
 SELECT department_id, AVG(salary) AS avg_salary
 FROM employees
 GROUP BY department_id
 ) x
);
```

**Answer / Explanation**

The inner grouped query computes department averages, then the outer subquery finds their 
maximum.

---

### 128. Find the most recent order for each customer.

**Difficulty:** `Hard`  
**Concept:** `Correlated subquery`

**SQL**

```sql
SELECT o.*
FROM orders o
WHERE o.order_date = (
 SELECT MAX(o2.order_date)
 FROM orders o2
 WHERE o2.customer_id = o.customer_id
);
```

**Answer / Explanation**

Each order is compared with the maximum date for its customer.

---

### 129. Find customers whose total spend is above the overall average customer spend.

**Difficulty:** `Hard`  
**Concept:** `Derived-table subquery`

**SQL**

```sql
SELECT customer_id, SUM(total_amount) AS total_spend
FROM orders


GROUP BY customer_id
HAVING SUM(total_amount) > (
 SELECT AVG(total_spend)
 FROM (
 SELECT customer_id, SUM(total_amount) AS total_spend
 FROM orders
 GROUP BY customer_id
 ) x
);
```

**Answer / Explanation**

Customer totals are calculated first; the outer aggregate computes their average.

---

### 130. Find employees whose salary is equal to the maximum salary in their department.

**Difficulty:** `Medium`  
**Concept:** `Correlated MAX`

**SQL**

```sql
SELECT e.employee_id, e.employee_name, e.salary
FROM employees e
WHERE e.salary = (
 SELECT MAX(e2.salary)
 FROM employees e2

 WHERE e2.department_id = e.department_id
);
```

**Answer / Explanation**

The correlated maximum identifies departmental top earners.

---

### 131. Find customers with an order but no order above 10000.

**Difficulty:** `Hard`  
**Concept:** `EXISTS + NOT EXISTS`

**SQL**

```sql
SELECT DISTINCT o.customer_id
FROM orders o
WHERE EXISTS (
 SELECT 1 FROM orders x WHERE x.customer_id = o.customer_id
)
AND NOT EXISTS (
 SELECT 1 FROM orders y
 WHERE y.customer_id = o.customer_id
 AND y.total_amount > 10000
);
```

**Answer / Explanation**

Both conditions enforce at least one order and no order above the threshold.

---

### 132. Find products whose price is greater than their category average.

**Difficulty:** `Medium`  
**Concept:** `Correlated aggregate`

**SQL**

```sql
SELECT p.product_id, p.product_name, p.list_price
FROM products p
WHERE p.list_price > (
 SELECT AVG(p2.list_price)
 FROM products p2


 WHERE p2.category_id = p.category_id
);
```

**Answer / Explanation**

The category-specific average is calculated for each product.

---

### 133. Find the customer with the highest total spend.

**Difficulty:** `Easy`  
**Concept:** `Top aggregate`

**SQL**

```sql
SELECT customer_id, SUM(total_amount) AS total_spend
FROM orders
GROUP BY customer_id
ORDER BY total_spend DESC
FETCH FIRST 1 ROW ONLY;
```

**Answer / Explanation**

Grouping by customer and sorting total spend identifies the top spender.

---

### 134. Find orders that exceed the customer's average order value.

**Difficulty:** `Hard`  
**Concept:** `Correlated AVG`

**SQL**

```sql
SELECT o.order_id, o.customer_id, o.total_amount
FROM orders o
WHERE o.total_amount > (
 SELECT AVG(o2.total_amount)
 FROM orders o2
 WHERE o2.customer_id = o.customer_id
);
```

**Answer / Explanation**

Each order is compared with the average for its own customer.

---

### 135. Find employees whose manager exists and earns less.

**Difficulty:** `Medium`  
**Concept:** `EXISTS`

**SQL**

```sql
SELECT e.employee_id, e.employee_name
FROM employees e
WHERE EXISTS (
 SELECT 1 FROM employees m
 WHERE m.employee_id = e.manager_id
 AND m.salary < e.salary
);
```

**Answer / Explanation**

EXISTS validates both the manager relationship and salary condition.

---

### 136. Find categories whose product count exceeds the average category product count.

**Difficulty:** `Hard`  
**Concept:** `Nested aggregate`

**SQL**

```sql
SELECT category_id, COUNT(*) AS product_count
FROM products


GROUP BY category_id
HAVING COUNT(*) > (
 SELECT AVG(product_count)
 FROM (
 SELECT category_id, COUNT(*) AS product_count
 FROM products
 GROUP BY category_id
 ) x
);
```

**Answer / Explanation**

The derived table calculates category counts before their average is computed.

---

### 137. Find duplicate emails using a subquery.

**Difficulty:** `Medium`  
**Concept:** `Duplicate detection`

**SQL**

```sql
SELECT *
FROM customers
WHERE email IN (
 SELECT email
 FROM customers
 GROUP BY email
 HAVING COUNT(*) > 1
);
```

**Answer / Explanation**

The subquery finds duplicated email values, which the outer query returns.

---

### 138. Find employees not assigned to any existing department.

**Difficulty:** `Medium`  
**Concept:** `NOT IN`

**SQL**

```sql
SELECT e.*
FROM employees e
WHERE e.department_id NOT IN (
 SELECT d.department_id FROM departments d
);
```

**Answer / Explanation**

The outer query returns department IDs absent from the department master.

---

### 139. Find customers who purchased every product in category 7.

**Difficulty:** `Hard`  
**Concept:** `Relational division`

**SQL**

```sql
SELECT o.customer_id
FROM orders o
JOIN order_items oi ON oi.order_id = o.order_id
WHERE oi.product_id IN (
 SELECT product_id FROM products WHERE category_id = 7
)
GROUP BY o.customer_id
HAVING COUNT(DISTINCT oi.product_id) = (
 SELECT COUNT(*) FROM products WHERE category_id = 7


);
```

**Answer / Explanation**

The customer must have purchased as many distinct category-7 products as exist.

---

### 140. Find the oldest employee in each department using a subquery.

**Difficulty:** `Medium`  
**Concept:** `Correlated MIN`

**SQL**

```sql
SELECT e.*
FROM employees e
WHERE e.hire_date = (
 SELECT MIN(e2.hire_date)
 FROM employees e2
 WHERE e2.department_id = e.department_id
);
```

**Answer / Explanation**

The minimum hire date per department identifies the earliest employee.

---

### 141. Find orders from customers whose account was created before 2020.

**Difficulty:** `Easy`  
**Concept:** `IN subquery`

**SQL**

```sql
SELECT o.order_id, o.customer_id
FROM orders o
WHERE o.customer_id IN (
 SELECT customer_id
 FROM customers
 WHERE created_at < TIMESTAMP '2020-01-01 00:00:00'
);
```

**Answer / Explanation**

The subquery produces eligible customer IDs used by the outer order query.

---

## CTEs & Recursive SQL

### 142. Use a CTE to calculate department payroll.

**Difficulty:** `Medium`  
**Concept:** `CTE`

**SQL**

```sql
WITH dept_payroll AS (
 SELECT department_id, SUM(salary) AS payroll
 FROM employees
 GROUP BY department_id
)
SELECT * FROM dept_payroll;
```

**Answer / Explanation**

The CTE names the intermediate aggregate and makes the main query easier to read.

---

### 143. Use a CTE to find high-value customers.

**Difficulty:** `Medium`  
**Concept:** `CTE`

**SQL**

```sql
WITH customer_spend AS (
 SELECT customer_id, SUM(total_amount) AS spend
 FROM orders
 GROUP BY customer_id
)
SELECT *
FROM customer_spend
WHERE spend > 10000;
```

**Answer / Explanation**

The first query produces reusable customer-level totals.

---

### 144. Use multiple CTEs to calculate order revenue by category.

**Difficulty:** `Hard`  
**Concept:** `Multiple CTEs`

**SQL**

```sql
WITH line_revenue AS (
 SELECT product_id, SUM(quantity * unit_price) AS revenue
 FROM order_items
 GROUP BY product_id
),
category_revenue AS (
 SELECT p.category_id, SUM(l.revenue) AS revenue
 FROM products p
 JOIN line_revenue l ON l.product_id = p.product_id
 GROUP BY p.category_id
)
SELECT * FROM category_revenue;
```

**Answer / Explanation**

Chaining CTEs breaks a multi-stage aggregation into understandable steps.

---

### 145. Use a CTE to remove duplicate customers, keeping the smallest ID.

**Difficulty:** `Hard`  
**Concept:** `CTE + ROW_NUMBER`

**SQL**

```sql
WITH ranked AS (
 SELECT customer_id, email,
 ROW_NUMBER() OVER (PARTITION BY email ORDER BY customer_id) AS rn
 FROM customers
)
SELECT customer_id, email
FROM ranked
WHERE rn = 1;
```

**Answer / Explanation**

The CTE ranks duplicates so only the preferred row is selected.

---

### 146. Use a recursive CTE to generate numbers 1 through 10.

**Difficulty:** `Hard`  
**Concept:** `Recursive CTE`

**SQL**

```sql
WITH RECURSIVE nums(n) AS (
 SELECT 1


 UNION ALL
 SELECT n + 1 FROM nums WHERE n < 10
)
SELECT n FROM nums;
```

**Answer / Explanation**

The recursive member keeps adding one until the stop condition is reached.

---

### 147. Build an employee management hierarchy.

**Difficulty:** `Hard`  
**Concept:** `Recursive hierarchy`

**SQL**

```sql
WITH RECURSIVE org AS (
 SELECT employee_id, employee_name, manager_id, 0 AS level
 FROM employees
 WHERE manager_id IS NULL
 UNION ALL
 SELECT e.employee_id, e.employee_name, e.manager_id, o.level + 1
 FROM employees e
 JOIN org o ON e.manager_id = o.employee_id
)
SELECT * FROM org ORDER BY level, employee_id;
```

**Answer / Explanation**

The anchor starts at top-level managers and the recursive member walks downward.

---

### 148. Generate a calendar for one month.

**Difficulty:** `Hard`  
**Concept:** `Calendar CTE`

**SQL**

```sql
WITH RECURSIVE days(d) AS (
 SELECT DATE '2026-01-01'
 UNION ALL
 SELECT d + INTERVAL '1 day'
 FROM days
 WHERE d < DATE '2026-01-31'
)
SELECT d FROM days;
```

**Answer / Explanation**

A recursive CTE is useful for generating a contiguous sequence of dates.

---

### 149. Use a CTE to find each customer's first order.

**Difficulty:** `Easy`  
**Concept:** `CTE + MIN`

**SQL**

```sql
WITH first_orders AS (
 SELECT customer_id, MIN(order_date) AS first_order_date
 FROM orders
 GROUP BY customer_id
)
SELECT * FROM first_orders;
```

**Answer / Explanation**

The CTE creates a reusable customer-level first-order dataset.

---

### 150. Use a CTE to calculate monthly revenue and then rank months.

**Difficulty:** `Hard`  
**Concept:** `CTE + window`

**SQL**

```sql
WITH monthly AS (
 SELECT DATE_TRUNC('month', order_date) AS month,
 SUM(total_amount) AS revenue
 FROM orders
 GROUP BY DATE_TRUNC('month', order_date)
)
SELECT month, revenue,
 RANK() OVER (ORDER BY revenue DESC) AS revenue_rank
FROM monthly;
```

**Answer / Explanation**

The CTE creates monthly grain before the ranking window is applied.

---

### 151. Use a CTE to identify inactive customers.

**Difficulty:** `Hard`  
**Concept:** `CTE + anti-activity`

**SQL**

```sql
WITH last_order AS (
 SELECT customer_id, MAX(order_date) AS last_order_date
 FROM orders
 GROUP BY customer_id
)
SELECT c.customer_id, c.customer_name
FROM customers c
LEFT JOIN last_order l ON l.customer_id = c.customer_id
WHERE l.last_order_date < CURRENT_DATE - INTERVAL '180' DAY
 OR l.last_order_date IS NULL;
```

**Answer / Explanation**

The CTE reduces orders to one last-activity row per customer.

---

### 152. Use a CTE to calculate product margin.

**Difficulty:** `Medium`  
**Concept:** `Derived metrics`

**SQL**

```sql
WITH product_sales AS (
 SELECT product_id,
 SUM(quantity * unit_price) AS revenue,
 SUM(quantity * cost_price) AS cost
 FROM order_items
 GROUP BY product_id
)
SELECT product_id, revenue - cost AS margin
FROM product_sales;
```

**Answer / Explanation**

Revenue and cost are aggregated before margin is calculated.

---

### 153. Use a CTE to find departments with no high earners.

**Difficulty:** `Medium`  
**Concept:** `CTE + anti-join`

**SQL**

```sql
WITH high_earners AS (
 SELECT DISTINCT department_id
 FROM employees
 WHERE salary > 150000
)
SELECT d.*
FROM departments d
LEFT JOIN high_earners h ON h.department_id = d.department_id
WHERE h.department_id IS NULL;
```

**Answer / Explanation**

The CTE creates a compact set of qualifying departments.

---

### 154. Use a CTE to compare current month sales with previous month sales.

**Difficulty:** `Hard`  
**Concept:** `CTE + LAG`

**SQL**

```sql
WITH monthly AS (
 SELECT DATE_TRUNC('month', order_date) AS month,
 SUM(total_amount) AS sales
 FROM orders
 GROUP BY DATE_TRUNC('month', order_date)
)
SELECT month, sales,
 LAG(sales) OVER (ORDER BY month) AS previous_sales
FROM monthly;
```

**Answer / Explanation**

Monthly aggregation is completed first so LAG operates at month grain.

---

### 155. Use a CTE to find customers with three consecutive monthly purchases.

**Difficulty:** `Hard`  
**Concept:** `Gaps and islands`

**SQL**

```sql
WITH months AS (
 SELECT DISTINCT customer_id,
 DATE_TRUNC('month', order_date) AS month
 FROM orders
),
groups AS (
 SELECT customer_id, month,
 month - (ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY month) * INTERVAL '1 month') AS grp
 FROM months
)
SELECT customer_id
FROM groups
GROUP BY customer_id, grp
HAVING COUNT(*) >= 3;
```

**Answer / Explanation**

Subtracting a row-number interval turns consecutive months into the same island key.

---

### 156. Use a CTE to find the top product by revenue.

**Difficulty:** `Easy`  
**Concept:** `CTE + top-N`

**SQL**

```sql
WITH revenue AS (
 SELECT product_id, SUM(quantity * unit_price) AS revenue
 FROM order_items
 GROUP BY product_id
)
SELECT * FROM revenue
ORDER BY revenue DESC
FETCH FIRST 1 ROW ONLY;
```

**Answer / Explanation**

The CTE isolates revenue calculation from the final ranking step.

---

### 157. Use a recursive CTE to calculate factorial values from 1 to 5.

**Difficulty:** `Hard`  
**Concept:** `Recursive calculation`

**SQL**

```sql
WITH RECURSIVE f(n, value) AS (
 SELECT 1, 1
 UNION ALL
 SELECT n + 1, value * (n + 1)
 FROM f WHERE n < 5
)
SELECT * FROM f;
```

**Answer / Explanation**

Each recursive step multiplies the previous value by the next integer.

---

### 158. Use a CTE to find orders whose total differs from line-item total.

**Difficulty:** `Hard`  
**Concept:** `Data reconciliation`

**SQL**

```sql
WITH line_totals AS (
 SELECT order_id, SUM(quantity * unit_price) AS line_total
 FROM order_items
 GROUP BY order_id
)
SELECT o.order_id, o.total_amount, l.line_total
FROM orders o
JOIN line_totals l ON l.order_id = o.order_id
WHERE o.total_amount <> l.line_total;
```

**Answer / Explanation**

The CTE independently recomputes line totals so stored totals can be validated.

---

### 159. Use a CTE to calculate daily active customers.

**Difficulty:** `Medium`  
**Concept:** `CTE + distinct grain`

**SQL**

```sql
WITH daily AS (
 SELECT CAST(order_date AS DATE) AS day,
 customer_id
 FROM orders


 GROUP BY CAST(order_date AS DATE), customer_id
)
SELECT day, COUNT(*) AS active_customers
FROM daily
GROUP BY day;
```

**Answer / Explanation**

The first CTE removes duplicate same-day orders per customer.

---

### 160. Use a CTE to identify the first purchase category per customer.

**Difficulty:** `Hard`  
**Concept:** `CTE + first event`

**SQL**

```sql
WITH first_order AS (
 SELECT customer_id, MIN(order_date) AS first_date
 FROM orders
 GROUP BY customer_id
)
SELECT o.customer_id, p.category_id
FROM orders o
JOIN first_order f ON f.customer_id = o.customer_id AND f.first_date = o.order_date
JOIN order_items oi ON oi.order_id = o.order_id
JOIN products p ON p.product_id = oi.product_id;
```

**Answer / Explanation**

The CTE identifies each customer's first order date before joining to its products.

---

## Window Functions

### 161. Rank employees by salary within each department.

**Difficulty:** `Medium`  
**Concept:** `RANK`

**SQL**

```sql
SELECT employee_name, department_id, salary,
 RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS salary_rank
FROM employees;
```

**Answer / Explanation**

RANK assigns the same position to ties and leaves gaps after ties.

---

### 162. Assign a unique salary position within each department.

**Difficulty:** `Medium`  
**Concept:** `ROW_NUMBER`

**SQL**

```sql
SELECT employee_name, department_id, salary,
 ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC, employee_id) AS rn
FROM employees;
```

**Answer / Explanation**

ROW_NUMBER always produces a unique sequence when the ORDER BY is deterministic.

---

### 163. Rank employees without gaps after ties.

**Difficulty:** `Medium`  
**Concept:** `DENSE_RANK`

**SQL**

```sql
SELECT employee_name, salary,
 DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM employees;
```

**Answer / Explanation**

DENSE_RANK gives equal salaries the same rank without skipped numbers.

---

### 164. Find the top three earners per department.

**Difficulty:** `Hard`  
**Concept:** `Top-N per group`

**SQL**

```sql
SELECT *
FROM (
 SELECT e.*,
 ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rn
 FROM employees e
) x
WHERE rn <= 3;
```

**Answer / Explanation**

ROW_NUMBER partitions employees by department and the outer query keeps three rows.

---

### 165. Calculate a running total of order sales.

**Difficulty:** `Medium`  
**Concept:** `Running total`

**SQL**

```sql
SELECT order_date, order_id, total_amount,
 SUM(total_amount) OVER (ORDER BY order_date, order_id) AS running_sales
FROM orders;
```

**Answer / Explanation**

The window SUM accumulates values in chronological order.

---

### 166. Calculate a running total per customer.

**Difficulty:** `Medium`  
**Concept:** `Partitioned running total`

**SQL**

```sql
SELECT customer_id, order_date, total_amount,
 SUM(total_amount) OVER (
 PARTITION BY customer_id ORDER BY order_date, order_id
 ) AS customer_running_total
FROM orders;
```

**Answer / Explanation**

PARTITION BY resets the running total for each customer.

---

### 167. Find the previous order amount for each customer.

**Difficulty:** `Medium`  
**Concept:** `LAG`

**SQL**

```sql
SELECT customer_id, order_id, order_date, total_amount,
 LAG(total_amount) OVER (
 PARTITION BY customer_id ORDER BY order_date, order_id


 ) AS previous_amount
FROM orders;
```

**Answer / Explanation**

LAG reads a value from the previous row within each customer.

---

### 168. Find the next order date for each customer.

**Difficulty:** `Medium`  
**Concept:** `LEAD`

**SQL**

```sql
SELECT customer_id, order_date,
 LEAD(order_date) OVER (
 PARTITION BY customer_id ORDER BY order_date
 ) AS next_order_date
FROM orders;
```

**Answer / Explanation**

LEAD looks ahead to the following row in each customer partition.

---

### 169. Calculate days between customer orders.

**Difficulty:** `Hard`  
**Concept:** `LAG + date arithmetic`

**SQL**

```sql
SELECT customer_id, order_date,
 order_date - LAG(order_date) OVER (
 PARTITION BY customer_id ORDER BY order_date
 ) AS days_since_previous
FROM orders;
```

**Answer / Explanation**

Subtracting the previous order date measures the customer purchase gap.

---

### 170. Calculate each product's percentage of total revenue.

**Difficulty:** `Hard`  
**Concept:** `Window over aggregate`

**SQL**

```sql
SELECT product_id,
 SUM(quantity * unit_price) AS revenue,
 100.0 * SUM(quantity * unit_price) /
 SUM(SUM(quantity * unit_price)) OVER () AS revenue_pct
FROM order_items
GROUP BY product_id;
```

**Answer / Explanation**

The outer window sum creates the grand total while the grouped SUM provides product revenue.

---

### 171. Find salary difference from department average.

**Difficulty:** `Medium`  
**Concept:** `Window AVG`

**SQL**

```sql
SELECT employee_name, department_id, salary,
 salary - AVG(salary) OVER (PARTITION BY department_id) AS diff_from_dept_avg
FROM employees;
```

**Answer / Explanation**

The departmental average is available on every employee row without collapsing rows.

---

### 172. Find the highest salary in each department beside every employee.

**Difficulty:** `Medium`  
**Concept:** `Window MAX`

**SQL**

```sql
SELECT employee_name, department_id, salary,
 MAX(salary) OVER (PARTITION BY department_id) AS dept_max_salary
FROM employees;
```

**Answer / Explanation**

Window MAX retains employee detail while exposing the group maximum.

---

### 173. Find each employee's salary percentile within the company.

**Difficulty:** `Hard`  
**Concept:** `PERCENT_RANK`

**SQL**

```sql
SELECT employee_name, salary,
 PERCENT_RANK() OVER (ORDER BY salary) AS salary_percentile
FROM employees;
```

**Answer / Explanation**

PERCENT_RANK expresses a row's relative position from 0 to 1.

---

### 174. Divide employees into four salary quartiles.

**Difficulty:** `Medium`  
**Concept:** `NTILE`

**SQL**

```sql
SELECT employee_name, salary,
 NTILE(4) OVER (ORDER BY salary DESC) AS quartile
FROM employees;
```

**Answer / Explanation**

NTILE distributes ordered rows as evenly as possible across four buckets.

---

### 175. Find the first order for each customer using ROW_NUMBER.

**Difficulty:** `Medium`  
**Concept:** `ROW_NUMBER`

**SQL**

```sql
SELECT *
FROM (
 SELECT o.*,
 ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date, order_id) AS rn
 FROM orders o
) x
WHERE rn = 1;
```

**Answer / Explanation**

The first row per customer represents the first chronological order.

---

### 176. Find the last order for each customer using ROW_NUMBER.

**Difficulty:** `Medium`  
**Concept:** `Latest row`

**SQL**

```sql
SELECT *
FROM (
 SELECT o.*,
 ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC, order_id DESC) AS rn
 FROM orders o
) x
WHERE rn = 1;
```

**Answer / Explanation**

Descending order makes the most recent order row number one.

---

### 177. Calculate a three-order moving average.

**Difficulty:** `Hard`  
**Concept:** `Moving average`

**SQL**

```sql
SELECT order_date, order_id, total_amount,
 AVG(total_amount) OVER (
 ORDER BY order_date, order_id
 ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
 ) AS moving_avg
FROM orders;
```

**Answer / Explanation**

The frame includes the current row and the two immediately preceding orders.

---

### 178. Calculate cumulative quantity sold per product.

**Difficulty:** `Medium`  
**Concept:** `Cumulative window`

**SQL**

```sql
SELECT product_id, order_id, quantity,
 SUM(quantity) OVER (
 PARTITION BY product_id ORDER BY order_id
 ) AS cumulative_qty
FROM order_items;
```

**Answer / Explanation**

The cumulative SUM resets for each product.

---

### 179. Find the difference between current and previous month's revenue.

**Difficulty:** `Hard`  
**Concept:** `LAG`

**SQL**

```sql
WITH monthly AS (
 SELECT DATE_TRUNC('month', order_date) AS month,
 SUM(total_amount) AS revenue
 FROM orders
 GROUP BY DATE_TRUNC('month', order_date)
)
SELECT month, revenue,
 revenue - LAG(revenue) OVER (ORDER BY month) AS change
FROM monthly;
```

**Answer / Explanation**

LAG supplies the prior month's revenue for direct comparison.

---

### 180. Calculate month-over-month growth percentage.

**Difficulty:** `Hard`  
**Concept:** `MoM growth`

**SQL**

```sql
WITH monthly AS (
 SELECT DATE_TRUNC('month', order_date) AS month,
 SUM(total_amount) AS revenue
 FROM orders
 GROUP BY DATE_TRUNC('month', order_date)
)
SELECT month, revenue,
 100.0 * (revenue - LAG(revenue) OVER (ORDER BY month)) /
 NULLIF(LAG(revenue) OVER (ORDER BY month), 0) AS growth_pct
FROM monthly;
```

**Answer / Explanation**

The formula compares current revenue with the prior month and protects division by zero.

---

### 181. Find the first and last order date for every customer.

**Difficulty:** `Hard`  
**Concept:** `FIRST_VALUE/LAST_VALUE`

**SQL**

```sql
SELECT DISTINCT customer_id,
 FIRST_VALUE(order_date) OVER (PARTITION BY customer_id ORDER BY order_date) AS first_order,
 LAST_VALUE(order_date) OVER (
 PARTITION BY customer_id ORDER BY order_date
 ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
 ) AS last_order
FROM orders;
```

**Answer / Explanation**

The explicit frame is important for LAST_VALUE so it can see the complete partition.

---

### 182. Find employees whose salary is in the top 10%.

**Difficulty:** `Hard`  
**Concept:** `CUME_DIST`

**SQL**

```sql
SELECT employee_id, employee_name, salary
FROM (
 SELECT e.*,
 CUME_DIST() OVER (ORDER BY salary DESC) AS cd
 FROM employees e
) x
WHERE cd <= 0.10;
```

**Answer / Explanation**

CUME_DIST gives the cumulative distribution used to identify the top ten percent.

---

### 183. Find the median salary per department.

**Difficulty:** `Hard`  
**Concept:** `Window percentile`

**SQL**

```sql
SELECT DISTINCT department_id,
 PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary)


 OVER (PARTITION BY department_id) AS median_salary
FROM employees;
```

**Answer / Explanation**

The percentile window computes a median independently for each department.

---

### 184. Detect salary increases between adjacent history records.

**Difficulty:** `Medium`  
**Concept:** `LAG`

**SQL**

```sql
SELECT employee_id, effective_date, salary,
 LAG(salary) OVER (PARTITION BY employee_id ORDER BY effective_date) AS previous_salary
FROM employee_salary_history;
```

**Answer / Explanation**

Comparing current salary with the previous history row identifies changes.

---

### 185. Identify consecutive duplicate statuses.

**Difficulty:** `Medium`  
**Concept:** `LAG`

**SQL**

```sql
SELECT customer_id, status_date, status,
 LAG(status) OVER (PARTITION BY customer_id ORDER BY status_date) AS previous_status
FROM customer_status_history;
```

**Answer / Explanation**

A row is a consecutive duplicate when its status equals the previous status.

---

### 186. Create a group number whenever an order status changes.

**Difficulty:** `Hard`  
**Concept:** `Gaps and islands`

**SQL**

```sql
WITH x AS (
 SELECT order_id, status, order_date,
 CASE WHEN status = LAG(status) OVER (ORDER BY order_date, order_id)
 THEN 0 ELSE 1 END AS new_group
 FROM order_status_history
), y AS (
 SELECT x.*,
 SUM(new_group) OVER (ORDER BY order_date, order_id) AS group_id
 FROM x
)
SELECT * FROM y;
```

**Answer / Explanation**

A cumulative sum of change flags creates an identifier for each status island.

---

### 187. Find the two highest-paid employees in each department including ties.

**Difficulty:** `Hard`  
**Concept:** `DENSE_RANK`

**SQL**

```sql
SELECT employee_id, employee_name, department_id, salary
FROM (
 SELECT e.*,
 DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rnk


 FROM employees e
) x
WHERE rnk <= 2;
```

**Answer / Explanation**

DENSE_RANK preserves all employees tied within the top two salary levels.

---

### 188. Find each order's share of the customer's total spend.

**Difficulty:** `Hard`  
**Concept:** `Window SUM`

**SQL**

```sql
SELECT customer_id, order_id, total_amount,
 100.0 * total_amount /
 SUM(total_amount) OVER (PARTITION BY customer_id) AS pct_of_customer_spend
FROM orders;
```

**Answer / Explanation**

The partition total provides the denominator for each customer's percentage.

---

### 189. Compare each employee's salary to the previous employee in salary order.

**Difficulty:** `Medium`  
**Concept:** `LAG`

**SQL**

```sql
SELECT employee_name, salary,
 salary - LAG(salary) OVER (ORDER BY salary) AS salary_gap
FROM employees;
```

**Answer / Explanation**

LAG provides the previous salary in the sorted sequence.

---

### 190. Find the largest gap between customer orders.

**Difficulty:** `Hard`  
**Concept:** `Window + aggregate`

**SQL**

```sql
WITH gaps AS (
 SELECT customer_id, order_date,
 order_date - LAG(order_date) OVER (PARTITION BY customer_id ORDER BY order_date) AS gap
 FROM orders
)
SELECT customer_id, MAX(gap) AS largest_gap
FROM gaps
GROUP BY customer_id;
```

**Answer / Explanation**

The window calculates gaps first; MAX then finds each customer's largest gap.

---

### 191. Return the cumulative percentage of revenue by product.

**Difficulty:** `Hard`  
**Concept:** `Cumulative percentage`

**SQL**

```sql
WITH p AS (
 SELECT product_id, SUM(quantity * unit_price) AS revenue
 FROM order_items GROUP BY product_id
)
SELECT product_id, revenue,


 100.0 * SUM(revenue) OVER (ORDER BY revenue DESC) /
 SUM(revenue) OVER () AS cumulative_pct
FROM p;
```

**Answer / Explanation**

Two window sums provide the running numerator and overall denominator.

---

### 192. Find the median order amount per customer.

**Difficulty:** `Hard`  
**Concept:** `Median window`

**SQL**

```sql
SELECT DISTINCT customer_id,
 PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY total_amount)
 OVER (PARTITION BY customer_id) AS median_order_value
FROM orders;
```

**Answer / Explanation**

The percentile function computes a customer-specific median while retaining one row per customer.

---

## Dates, Strings & NULL Handling

### 193. Find orders placed on a weekend.

**Difficulty:** `Medium`  
**Concept:** `Date parts`

**SQL**

```sql
SELECT order_id, order_date
FROM orders
WHERE EXTRACT(ISODOW FROM order_date) IN (6, 7);
```

**Answer / Explanation**

ISO day-of-week values 6 and 7 represent Saturday and Sunday.

---

### 194. Find employees with more than five years of service.

**Difficulty:** `Easy`  
**Concept:** `Date arithmetic`

**SQL**

```sql
SELECT employee_id, employee_name
FROM employees
WHERE hire_date < CURRENT_DATE - INTERVAL '5 years';
```

**Answer / Explanation**

Subtracting five years from today creates the service cutoff.

---

### 195. Calculate employee tenure in years.

**Difficulty:** `Medium`  
**Concept:** `AGE`

**SQL**

```sql
SELECT employee_name,
 EXTRACT(YEAR FROM AGE(CURRENT_DATE, hire_date)) AS tenure_years
FROM employees;
```

**Answer / Explanation**

AGE computes elapsed time and EXTRACT returns the year component.

---

### 196. Find orders from the previous calendar month.

**Difficulty:** `Medium`  
**Concept:** `Calendar range`

**SQL**

```sql
SELECT *
FROM orders
WHERE order_date >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '1 month'
 AND order_date < DATE_TRUNC('month', CURRENT_DATE);
```

**Answer / Explanation**

A half-open range isolates the complete previous calendar month.

---

### 197. Find customers whose birthday is today.

**Difficulty:** `Medium`  
**Concept:** `Date parts`

**SQL**

```sql
SELECT customer_id, customer_name
FROM customers
WHERE EXTRACT(MONTH FROM birth_date) = EXTRACT(MONTH FROM CURRENT_DATE)
 AND EXTRACT(DAY FROM birth_date) = EXTRACT(DAY FROM CURRENT_DATE);
```

**Answer / Explanation**

Comparing month and day ignores the birth year.

---

### 198. Find orders older than 90 days but not older than one year.

**Difficulty:** `Easy`  
**Concept:** `Date range`

**SQL**

```sql
SELECT order_id, order_date
FROM orders
WHERE order_date < CURRENT_DATE - INTERVAL '90 days'
 AND order_date >= CURRENT_DATE - INTERVAL '1 year';
```

**Answer / Explanation**

Both boundaries define the requested rolling interval.

---

### 199. Extract the year and month from order dates.

**Difficulty:** `Easy`  
**Concept:** `EXTRACT`

**SQL**

```sql
SELECT order_id,
 EXTRACT(YEAR FROM order_date) AS order_year,
 EXTRACT(MONTH FROM order_date) AS order_month
FROM orders;
```

**Answer / Explanation**

EXTRACT retrieves individual date components.

---

### 200. Format an order date as YYYY-MM.

**Difficulty:** `Easy`  
**Concept:** `Formatting`

**SQL**

```sql
SELECT TO_CHAR(order_date, 'YYYY-MM') AS year_month
FROM orders;
```

**Answer / Explanation**

TO_CHAR converts a date into a formatted text representation in PostgreSQL-style SQL.

---

### 201. Find strings containing digits.

**Difficulty:** `Hard`  
**Concept:** `Regular expression`

**SQL**

```sql
SELECT customer_code
FROM customers
WHERE customer_code ~ '[0-9]';
```

**Answer / Explanation**

The regular expression matches a string containing at least one digit.

---

### 202. Find emails with a valid-looking @ symbol.

**Difficulty:** `Easy`  
**Concept:** `String validation`

**SQL**

```sql
SELECT customer_id, email
FROM customers
WHERE email LIKE '%@%';
```

**Answer / Explanation**

This is a basic structural check, not full email validation.

---

### 203. Split an email into username and domain.

**Difficulty:** `Medium`  
**Concept:** `String parsing`

**SQL**

```sql
SELECT email,
 SPLIT_PART(email, '@', 1) AS username,
 SPLIT_PART(email, '@', 2) AS domain
FROM customers;
```

**Answer / Explanation**

SPLIT_PART separates the email at the @ delimiter.

---

### 204. Find duplicate emails ignoring case.

**Difficulty:** `Medium`  
**Concept:** `Normalization`

**SQL**

```sql
SELECT LOWER(email) AS normalized_email, COUNT(*) AS cnt
FROM customers
GROUP BY LOWER(email)
HAVING COUNT(*) > 1;
```

**Answer / Explanation**

Lowercasing before grouping makes the duplicate test case-insensitive.

---

### 205. Convert empty strings to NULL.

**Difficulty:** `Medium`  
**Concept:** `NULLIF`

**SQL**

```sql
SELECT NULLIF(TRIM(phone), '') AS phone


FROM customers;
```

**Answer / Explanation**

TRIM removes spaces and NULLIF converts an empty result into NULL.

---

### 206. Display 'Unknown' for missing department names.

**Difficulty:** `Easy`  
**Concept:** `COALESCE`

**SQL**

```sql
SELECT COALESCE(department_name, 'Unknown') AS department_name
FROM departments;
```

**Answer / Explanation**

COALESCE provides a fallback value when the source is NULL.

---

### 207. Count missing phone numbers.

**Difficulty:** `Medium`  
**Concept:** `NULL counting`

**SQL**

```sql
SELECT COUNT(*) - COUNT(phone) AS missing_phone_count
FROM customers;
```

**Answer / Explanation**

COUNT(*) counts all rows while COUNT(phone) excludes NULL phones.

---

### 208. Find rows where two nullable columns are different.

**Difficulty:** `Hard`  
**Concept:** `NULL-safe comparison`

**SQL**

```sql
SELECT *
FROM customer_profile
WHERE phone IS DISTINCT FROM mobile_phone;
```

**Answer / Explanation**

IS DISTINCT FROM treats NULL as a comparable value instead of producing UNKNOWN.

---

### 209. Find the first non-empty address field.

**Difficulty:** `Medium`  
**Concept:** `COALESCE + NULLIF`

**SQL**

```sql
SELECT customer_id,
 COALESCE(NULLIF(TRIM(address1), ''),
 NULLIF(TRIM(address2), ''),
 'N/A') AS address
FROM customers;
```

**Answer / Explanation**

Each candidate is normalized before COALESCE chooses the first usable value.

---

### 210. Remove leading zeros from customer codes.

**Difficulty:** `Medium`  
**Concept:** `String cleanup`

**SQL**

```sql
SELECT customer_code,
 LTRIM(customer_code, '0') AS normalized_code


FROM customers;
```

**Answer / Explanation**

LTRIM removes specified leading characters.

---

### 211. Find names with repeated spaces.

**Difficulty:** `Easy`  
**Concept:** `Data quality`

**SQL**

```sql
SELECT customer_id, customer_name
FROM customers
WHERE customer_name LIKE '% %';
```

**Answer / Explanation**

Two consecutive spaces indicate a simple formatting anomaly.

---

### 212. Find the number of days between order and shipment.

**Difficulty:** `Easy`  
**Concept:** `Date difference`

**SQL**

```sql
SELECT order_id,
 shipped_date - order_date AS shipping_days
FROM orders;
```

**Answer / Explanation**

Subtracting dates returns the elapsed number of days in PostgreSQL-style SQL.

---

### 213. Find orders shipped later than promised.

**Difficulty:** `Easy`  
**Concept:** `Date comparison`

**SQL**

```sql
SELECT order_id, shipped_date, promised_date
FROM orders
WHERE shipped_date > promised_date;
```

**Answer / Explanation**

A shipment after its promised date is late.

---

### 214. Find customers whose account age is between 1 and 3 years.

**Difficulty:** `Medium`  
**Concept:** `Timestamp range`

**SQL**

```sql
SELECT customer_id, customer_name
FROM customers
WHERE created_at >= CURRENT_TIMESTAMP - INTERVAL '3 years'
 AND created_at < CURRENT_TIMESTAMP - INTERVAL '1 year';
```

**Answer / Explanation**

The range expresses account age between one and three years.

---

### 215. Extract the domain from a URL.

**Difficulty:** `Medium`  
**Concept:** `String parsing`

**SQL**

```sql
SELECT url,


 SPLIT_PART(SPLIT_PART(url, '://', 2), '/', 1) AS domain
FROM customer_websites;
```

**Answer / Explanation**

The expression removes the protocol and then takes the host portion.

---

### 216. Find orders created exactly on the hour.

**Difficulty:** `Medium`  
**Concept:** `Timestamp parts`

**SQL**

```sql
SELECT order_id, created_at
FROM orders
WHERE EXTRACT(MINUTE FROM created_at) = 0
 AND EXTRACT(SECOND FROM created_at) = 0;
```

**Answer / Explanation**

Minute and second equal to zero indicate an exact hour boundary.

---

### 217. Find the last day of the order month.

**Difficulty:** `Hard`  
**Concept:** `Month end`

**SQL**

```sql
SELECT order_id,
 (DATE_TRUNC('month', order_date) + INTERVAL '1 month - 1 day')::date AS month_end
FROM orders;
```

**Answer / Explanation**

Truncating to the month and adding one month minus one day yields the month end.

---

## Advanced Data Modification

### 218. Insert a new customer row.

**Difficulty:** `Easy`  
**Concept:** `INSERT`

**SQL**

```sql
INSERT INTO customers (customer_id, customer_name, email)
VALUES (1001, 'Asha Rao', 'asha@example.com');
```

**Answer / Explanation**

INSERT creates a new row using the listed columns.

---

### 219. Insert multiple products in one statement.

**Difficulty:** `Easy`  
**Concept:** `Multi-row INSERT`

**SQL**

```sql
INSERT INTO products (product_id, product_name, list_price)
VALUES
 (101, 'Keyboard', 40),
 (102, 'Mouse', 20),
 (103, 'Monitor', 250);
```

**Answer / Explanation**

A single INSERT can add multiple value rows.

---

### 220. Update salaries for one department.

**Difficulty:** `Easy`  
**Concept:** `UPDATE`

**SQL**

```sql
UPDATE employees
SET salary = salary * 1.05
WHERE department_id = 20;
```

**Answer / Explanation**

The expression increases every matching salary by five percent.

---

### 221. Delete cancelled orders.

**Difficulty:** `Easy`  
**Concept:** `DELETE`

**SQL**

```sql
DELETE FROM orders
WHERE status = 'CANCELLED';
```

**Answer / Explanation**

DELETE removes rows satisfying the condition.

---

### 222. Delete orphaned order items.

**Difficulty:** `Hard`  
**Concept:** `DELETE + NOT EXISTS`

**SQL**

```sql
DELETE FROM order_items oi
WHERE NOT EXISTS (
 SELECT 1 FROM orders o WHERE o.order_id = oi.order_id
);
```

**Answer / Explanation**

The anti-existence test identifies order items with no parent order.

---

### 223. Update NULL commissions to zero.

**Difficulty:** `Easy`  
**Concept:** `UPDATE + NULL`

**SQL**

```sql
UPDATE employees
SET commission = 0
WHERE commission IS NULL;
```

**Answer / Explanation**

Only missing commission values are changed.

---

### 224. Increase salaries for employees below department average.

**Difficulty:** `Hard`  
**Concept:** `Correlated UPDATE`

**SQL**

```sql
UPDATE employees e
SET salary = salary * 1.10
WHERE salary < (
 SELECT AVG(e2.salary)
 FROM employees e2
 WHERE e2.department_id = e.department_id


);
```

**Answer / Explanation**

The correlated subquery compares each employee with the average of their department.

---

### 225. Copy archived orders into a history table.

**Difficulty:** `Medium`  
**Concept:** `INSERT SELECT`

**SQL**

```sql
INSERT INTO orders_history
SELECT *
FROM orders
WHERE order_date < CURRENT_DATE - INTERVAL '2 years';
```

**Answer / Explanation**

INSERT SELECT copies qualifying rows without needing a separate value list.

---

### 226. Upsert a customer by email.

**Difficulty:** `Hard`  
**Concept:** `UPSERT`

**SQL**

```sql
INSERT INTO customers (customer_id, email, customer_name)
VALUES (1001, 'asha@example.com', 'Asha Rao')
ON CONFLICT (email)
DO UPDATE SET customer_name = EXCLUDED.customer_name;
```

**Answer / Explanation**

ON CONFLICT updates the existing row when the unique email already exists.

---

### 227. Merge incoming customer data into a target table.

**Difficulty:** `Hard`  
**Concept:** `MERGE`

**SQL**

```sql
MERGE INTO customers c
USING customer_stage s
ON c.customer_id = s.customer_id
WHEN MATCHED THEN
 UPDATE SET customer_name = s.customer_name, email = s.email
WHEN NOT MATCHED THEN
 INSERT (customer_id, customer_name, email)
 VALUES (s.customer_id, s.customer_name, s.email);
```

**Answer / Explanation**

MERGE handles matched updates and unmatched inserts in one statement.

---

### 228. Increase prices by 8% for one category.

**Difficulty:** `Easy`  
**Concept:** `UPDATE`

**SQL**

```sql
UPDATE products
SET list_price = list_price * 1.08
WHERE category_id = 5;
```

**Answer / Explanation**

The category predicate limits the price change to the intended products.

---

### 229. Delete duplicate customers while keeping the smallest ID.

**Difficulty:** `Hard`  
**Concept:** `Deduplication`

**SQL**

```sql
DELETE FROM customers c
WHERE c.customer_id IN (
 SELECT customer_id
 FROM (
 SELECT customer_id,
 ROW_NUMBER() OVER (PARTITION BY email ORDER BY customer_id) AS rn
 FROM customers
 ) x
 WHERE rn > 1
);
```

**Answer / Explanation**

ROW_NUMBER identifies duplicate rows while preserving the first ID.

---

### 230. Move inactive customers to an archive table.

**Difficulty:** `Hard`  
**Concept:** `DELETE RETURNING`

**SQL**

```sql
WITH moved AS (
 DELETE FROM customers
 WHERE status = 'INACTIVE'
 RETURNING *
)
INSERT INTO customers_archive
SELECT * FROM moved;
```

**Answer / Explanation**

The data-changing CTE moves rows atomically in PostgreSQL-style SQL.

---

### 231. Create a snapshot of current employee salaries.

**Difficulty:** `Medium`  
**Concept:** `CTAS`

**SQL**

```sql
CREATE TABLE employee_salary_snapshot AS
SELECT employee_id, salary, CURRENT_DATE AS snapshot_date
FROM employees;
```

**Answer / Explanation**

CREATE TABLE AS materializes a query result as a new table.

---

### 232. Add a default status to a table.

**Difficulty:** `Easy`  
**Concept:** `ALTER TABLE`

**SQL**

```sql
ALTER TABLE customers
ALTER COLUMN status SET DEFAULT 'ACTIVE';
```

**Answer / Explanation**

A default is applied when an INSERT omits the status column.

---

### 233. Add a NOT NULL constraint after cleaning data.

**Difficulty:** `Medium`  
**Concept:** `Data cleanup + DDL`

**SQL**

```sql
UPDATE customers
SET email = 'unknown@example.com'
WHERE email IS NULL;
ALTER TABLE customers
ALTER COLUMN email SET NOT NULL;
```

**Answer / Explanation**

Existing NULLs must be addressed before adding a NOT NULL constraint.

---

### 234. Create a unique constraint on customer email.

**Difficulty:** `Easy`  
**Concept:** `Constraint`

**SQL**

```sql
ALTER TABLE customers
ADD CONSTRAINT uq_customers_email UNIQUE (email);
```

**Answer / Explanation**

A unique constraint prevents duplicate email values.

---

### 235. Create a foreign key from orders to customers.

**Difficulty:** `Easy`  
**Concept:** `Foreign key`

**SQL**

```sql
ALTER TABLE orders
ADD CONSTRAINT fk_orders_customer
FOREIGN KEY (customer_id) REFERENCES customers(customer_id);
```

**Answer / Explanation**

The foreign key enforces referential integrity.

---

### 236. Use a transaction for a money transfer.

**Difficulty:** `Medium`  
**Concept:** `Transaction`

**SQL**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 500 WHERE account_id = 2;
COMMIT;
```

**Answer / Explanation**

The two updates succeed or can be rolled back as one unit of work.

---

## Views, Procedures, Functions & Transactions

### 237. Create a view for active customers.

**Difficulty:** `Easy`  
**Concept:** `VIEW`

**SQL**

```sql
CREATE VIEW active_customers AS
SELECT customer_id, customer_name, email
FROM customers
WHERE status = 'ACTIVE';
```

**Answer / Explanation**

A view stores the query definition so consumers can reuse it like a table.

---

### 238. Create a view for department salary summaries.

**Difficulty:** `Medium`  
**Concept:** `VIEW + aggregation`

**SQL**

```sql
CREATE VIEW department_salary_summary AS
SELECT department_id,
 COUNT(*) AS employee_count,
 AVG(salary) AS avg_salary,
 SUM(salary) AS payroll
FROM employees
GROUP BY department_id;
```

**Answer / Explanation**

The view exposes a reusable department-level summary.

---

### 239. Create an index for customer email lookups.

**Difficulty:** `Easy`  
**Concept:** `INDEX`

**SQL**

```sql
CREATE INDEX idx_customers_email
ON customers(email);
```

**Answer / Explanation**

An index can accelerate equality and lookup predicates on email.

---

### 240. Create a composite index for customer/date queries.

**Difficulty:** `Medium`  
**Concept:** `Composite index`

**SQL**

```sql
CREATE INDEX idx_orders_customer_date
ON orders(customer_id, order_date);
```

**Answer / Explanation**

The index supports queries filtering by customer and then ordering/filtering by date.

---

### 241. Start a transaction and roll back a test update.

**Difficulty:** `Easy`  
**Concept:** `ROLLBACK`

**SQL**

```sql
BEGIN;
UPDATE employees SET salary = salary * 2 WHERE employee_id = 101;
ROLLBACK;
```

**Answer / Explanation**

ROLLBACK discards all changes made since the transaction began.

---

### 242. Create a savepoint inside a transaction.

**Difficulty:** `Medium`  
**Concept:** `SAVEPOINT`

**SQL**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
SAVEPOINT after_debit;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
ROLLBACK TO SAVEPOINT after_debit;
COMMIT;
```

**Answer / Explanation**

A savepoint allows partial rollback without discarding the whole transaction.

---

### 243. Demonstrate transaction isolation conceptually.

**Difficulty:** `Hard`  
**Concept:** `Isolation level`

**SQL**

```sql
BEGIN;
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SELECT * FROM accounts WHERE account_id = 1;
COMMIT;
```

**Answer / Explanation**

The transaction requests a consistent snapshot for reads within the transaction.

---

### 244. Create a function that returns an employee's annual salary.

**Difficulty:** `Medium`  
**Concept:** `Function`

**SQL**

```sql
CREATE FUNCTION annual_salary(monthly_salary NUMERIC)
RETURNS NUMERIC
LANGUAGE SQL
AS $$
 SELECT monthly_salary * 12;
$$;
```

**Answer / Explanation**

A SQL function encapsulates reusable calculation logic.

---

### 245. Create a function that classifies salary.

**Difficulty:** `Medium`  
**Concept:** `Function + CASE`

**SQL**

```sql
CREATE FUNCTION salary_band(p_salary NUMERIC)
RETURNS TEXT
LANGUAGE SQL

AS $$
 SELECT CASE WHEN p_salary >= 100000 THEN 'HIGH'
 WHEN p_salary >= 60000 THEN 'MEDIUM'
 ELSE 'LOW' END;
$$;
```

**Answer / Explanation**

The function centralizes a reusable salary classification rule.

---

### 246. Create a stored procedure that updates a department's salaries.

**Difficulty:** `Hard`  
**Concept:** `Procedure`

**SQL**

```sql
CREATE PROCEDURE raise_department(p_department_id INT, p_pct NUMERIC)
LANGUAGE SQL
AS $$
 UPDATE employees
 SET salary = salary * (1 + p_pct / 100)
 WHERE department_id = p_department_id;
$$;
```

**Answer / Explanation**

A procedure packages a parameterized data modification operation.

---

### 247. Grant read access to a reporting role.

**Difficulty:** `Medium`  
**Concept:** `GRANT`

**SQL**

```sql
GRANT SELECT ON employees, departments, orders TO reporting_role;
```

**Answer / Explanation**

GRANT assigns object privileges to the reporting role.

---

### 248. Revoke direct update access from a user.

**Difficulty:** `Medium`  
**Concept:** `REVOKE`

**SQL**

```sql
REVOKE UPDATE ON employees FROM analyst_user;
```

**Answer / Explanation**

REVOKE removes the specified object privilege.

---

### 249. Create an index only if it does not already exist.

**Difficulty:** `Easy`  
**Concept:** `Idempotent DDL`

**SQL**

```sql
CREATE INDEX IF NOT EXISTS idx_orders_status
ON orders(status);
```

**Answer / Explanation**

IF NOT EXISTS makes repeated deployment scripts safer.

---

### 250. Create a materialized monthly sales summary.

**Difficulty:** `Hard`  
**Concept:** `Materialized view`

**SQL**

```sql
CREATE MATERIALIZED VIEW monthly_sales AS
SELECT DATE_TRUNC('month', order_date) AS month,
 SUM(total_amount) AS sales
FROM orders


GROUP BY DATE_TRUNC('month', order_date);
```

**Answer / Explanation**

A materialized view stores query results and can speed repeated reporting queries.

---

### 251. Refresh a materialized view.

**Difficulty:** `Medium`  
**Concept:** `Refresh`

**SQL**

```sql
REFRESH MATERIALIZED VIEW monthly_sales;
```

**Answer / Explanation**

Refreshing rebuilds the stored result from the underlying data.

---

### 252. Create a trigger to record salary changes.

**Difficulty:** `Hard`  
**Concept:** `Trigger`

**SQL**

```sql
CREATE TRIGGER audit_salary_change
AFTER UPDATE OF salary ON employees
FOR EACH ROW
EXECUTE FUNCTION log_salary_change();
```

**Answer / Explanation**

The trigger invokes audit logic whenever salary is updated.

---

### 253. Create an audit table for data changes.

**Difficulty:** `Medium`  
**Concept:** `Audit design`

**SQL**

```sql
CREATE TABLE audit_log (
 audit_id BIGINT GENERATED ALWAYS AS IDENTITY,
 table_name TEXT NOT NULL,
 action_name TEXT NOT NULL,
 changed_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

**Answer / Explanation**

The table provides a durable structure for recording change events.

---

### 254. Use a transaction with an explicit isolation level and error rollback.

**Difficulty:** `Hard`  
**Concept:** `Serializable transaction`

**SQL**

```sql
BEGIN;
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;
```

**Answer / Explanation**

SERIALIZABLE provides the strongest standard isolation and may require retry handling.

---

### 255. Create a read-only view that hides sensitive salary data.

**Difficulty:** `Medium`  
**Concept:** `Security view`

**SQL**

```sql
CREATE VIEW employee_directory AS
SELECT employee_id, employee_name, department_id, hire_date
FROM employees;
```

**Answer / Explanation**

The view exposes useful directory data without exposing salary.

---

### 256. Create an index that supports a frequent active-customer query.

**Difficulty:** `Hard`  
**Concept:** `Partial index`

**SQL**

```sql
CREATE INDEX idx_customers_active_region
ON customers(region)
WHERE status = 'ACTIVE';
```

**Answer / Explanation**

A partial index stores only active customers, reducing index size for that workload.

---

## Performance & Optimization

### 257. Find why a query is scanning the full orders table.

**Difficulty:** `Medium`  
**Concept:** `EXPLAIN`

**SQL**

```sql
EXPLAIN
SELECT * FROM orders
WHERE customer_id = 1001;
```

**Answer / Explanation**

EXPLAIN reveals the optimizer's chosen access path, including scans and indexes.

---

### 258. Get runtime details for a query plan.

**Difficulty:** `Hard`  
**Concept:** `EXPLAIN ANALYZE`

**SQL**

```sql
EXPLAIN ANALYZE
SELECT *
FROM orders
WHERE customer_id = 1001;
```

**Answer / Explanation**

EXPLAIN ANALYZE executes the query and reports actual timing and row counts.

---

### 259. Identify a missing index for customer/date filtering.

**Difficulty:** `Medium`  
**Concept:** `Index design`

**SQL**

```sql
CREATE INDEX idx_orders_customer_date
ON orders(customer_id, order_date);
```

**Answer / Explanation**

A composite index can support predicates that begin with customer_id and then use order_date.

---

### 260. Avoid SELECT * in a reporting query.

**Difficulty:** `Easy`  
**Concept:** `Projection`

**SQL**

```sql
SELECT order_id, customer_id, order_date, total_amount
FROM orders
WHERE order_date >= CURRENT_DATE - INTERVAL '30 days';
```

**Answer / Explanation**

Selecting only needed columns can reduce I/O and network transfer.

---

### 261. Replace an unnecessary DISTINCT after a one-to-one join.

**Difficulty:** `Medium`  
**Concept:** `Query simplification`

**SQL**

```sql
SELECT o.order_id, c.customer_name
FROM orders o
JOIN customers c ON c.customer_id = o.customer_id;
```

**Answer / Explanation**

If the relationship is one-to-one for the query grain, DISTINCT adds unnecessary work.

---

### 262. Use EXISTS instead of a duplicate-producing join for an existence check.

**Difficulty:** `Medium`  
**Concept:** `EXISTS`

**SQL**

```sql
SELECT c.customer_id, c.customer_name
FROM customers c
WHERE EXISTS (
 SELECT 1 FROM orders o
 WHERE o.customer_id = c.customer_id
);
```

**Answer / Explanation**

EXISTS expresses the requirement directly and avoids multiplying customer rows.

---

### 263. Filter rows before aggregation.

**Difficulty:** `Easy`  
**Concept:** `Predicate pushdown`

**SQL**

```sql
SELECT department_id, SUM(salary) AS payroll
FROM employees
WHERE status = 'ACTIVE'
GROUP BY department_id;
```

**Answer / Explanation**

Filtering before grouping reduces the rows that aggregation must process.

---

### 264. Use a covering index for a common lookup.

**Difficulty:** `Hard`  
**Concept:** `Covering index`

**SQL**

```sql
CREATE INDEX idx_orders_customer_cover
ON orders(customer_id, order_date, total_amount);
```

**Answer / Explanation**

Including frequently selected columns may allow an index-only access path in some engines.

---

### 265. Detect a non-sargable predicate.

**Difficulty:** `Medium`  
**Concept:** `Sargability`

**SQL**

```sql
SELECT *
FROM customers
WHERE LOWER(email) = 'a@b.com';
```

**Answer / Explanation**

Applying a function to the indexed column can prevent a normal index seek unless an expression 
index exists.

---

### 266. Rewrite a date function predicate as a range.

**Difficulty:** `Medium`  
**Concept:** `Sargable date filter`

**SQL**

```sql
SELECT *
FROM orders
WHERE order_date >= DATE '2026-01-01'
 AND order_date < DATE '2026-02-01';
```

**Answer / Explanation**

A range on the raw column is generally more index-friendly than applying a function to it.

---

### 267. Find duplicate rows efficiently.

**Difficulty:** `Easy`  
**Concept:** `Duplicate detection`

**SQL**

```sql
SELECT email, COUNT(*)
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;
```

**Answer / Explanation**

Grouping on the duplicate key is a straightforward way to identify repeated values.

---

### 268. Avoid correlated subqueries when a grouped join is clearer.

**Difficulty:** `Hard`  
**Concept:** `Derived join`

**SQL**

```sql
SELECT e.employee_id, e.employee_name, d.avg_salary
FROM employees e
JOIN (
 SELECT department_id, AVG(salary) AS avg_salary
 FROM employees
 GROUP BY department_id


) d ON d.department_id = e.department_id;
```

**Answer / Explanation**

Precomputing group values once can be easier for the optimizer than repeated correlated work.

---

### 269. Limit rows early when exploring a large table.

**Difficulty:** `Easy`  
**Concept:** `Top-N`

**SQL**

```sql
SELECT order_id, order_date, total_amount
FROM orders
ORDER BY order_date DESC
FETCH FIRST 100 ROWS ONLY;
```

**Answer / Explanation**

Returning a small result is useful for interactive investigation and testing.

---

### 270. Use a partial index for active records.

**Difficulty:** `Hard`  
**Concept:** `Partial index`

**SQL**

```sql
CREATE INDEX idx_orders_open
ON orders(customer_id)
WHERE status = 'OPEN';
```

**Answer / Explanation**

Only rows relevant to the workload are indexed.

---

### 271. Find table statistics that can affect query planning.

**Difficulty:** `Medium`  
**Concept:** `Statistics`

**SQL**

```sql
ANALYZE orders;
```

**Answer / Explanation**

ANALYZE refreshes optimizer statistics in PostgreSQL-style systems.

---

### 272. Reduce a large join by pre-aggregating detail.

**Difficulty:** `Hard`  
**Concept:** `Pre-aggregation`

**SQL**

```sql
SELECT o.customer_id, SUM(x.revenue) AS revenue
FROM orders o
JOIN (
 SELECT order_id, SUM(quantity * unit_price) AS revenue
 FROM order_items
 GROUP BY order_id
) x ON x.order_id = o.order_id
GROUP BY o.customer_id;
```

**Answer / Explanation**

Aggregating detail before a higher-level join can reduce row multiplication.

---

### 273. Detect a Cartesian product.

**Difficulty:** `Easy`  
**Concept:** `CROSS JOIN`

**SQL**

```sql
SELECT COUNT(*)
FROM employees e
CROSS JOIN departments d;
```

**Answer / Explanation**

A cross join produces every employee/department combination, often accidentally when a join 
condition is missing.

---

### 274. Explain why COUNT(column) can differ from COUNT(*).

**Difficulty:** `Easy`  
**Concept:** `COUNT semantics`

**SQL**

```sql
SELECT COUNT(*) AS rows,
 COUNT(commission) AS non_null_commissions
FROM employees;
```

**Answer / Explanation**

COUNT(*) counts rows; COUNT(column) ignores NULLs.

---

### 275. Create a foreign-key index for join performance.

**Difficulty:** `Medium`  
**Concept:** `Join index`

**SQL**

```sql
CREATE INDEX idx_orders_customer_id
ON orders(customer_id);
```

**Answer / Explanation**

Indexing a frequently joined foreign key can speed lookups and parent-child joins.

---

### 276. Use keyset pagination instead of deep OFFSET pagination.

**Difficulty:** `Hard`  
**Concept:** `Keyset pagination`

**SQL**

```sql
SELECT order_id, order_date, total_amount
FROM orders
WHERE order_id > 500000
ORDER BY order_id
FETCH FIRST 50 ROWS ONLY;
```

**Answer / Explanation**

Keyset pagination avoids scanning and discarding large numbers of earlier rows.

---

## Real-World Interview Scenarios

### 277. Find customers who have not purchased in the last 180 days.

**Difficulty:** `Medium`  
**Concept:** `Customer inactivity`

**SQL**

```sql
SELECT c.customer_id, c.customer_name
FROM customers c


LEFT JOIN orders o ON o.customer_id = c.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING MAX(o.order_date) < CURRENT_DATE - INTERVAL '180 days'
 OR MAX(o.order_date) IS NULL;
```

**Answer / Explanation**

MAX(order_date) gives last activity; NULL identifies customers with no orders.

---

### 278. Find the top-selling product in each category.

**Difficulty:** `Hard`  
**Concept:** `Top per group`

**SQL**

```sql
WITH sales AS (
 SELECT p.category_id, p.product_id,
 SUM(oi.quantity) AS units
 FROM products p
 JOIN order_items oi ON oi.product_id = p.product_id
 GROUP BY p.category_id, p.product_id
), ranked AS (
 SELECT *, RANK() OVER (PARTITION BY category_id ORDER BY units DESC) AS rnk
 FROM sales
)
SELECT * FROM ranked WHERE rnk = 1;
```

**Answer / Explanation**

Ranked sales within each category identify the category leaders including ties.

---

### 279. Find employees who changed departments more than once.

**Difficulty:** `Medium`  
**Concept:** `History analysis`

**SQL**

```sql
SELECT employee_id
FROM employee_department_history
GROUP BY employee_id
HAVING COUNT(*) > 2;
```

**Answer / Explanation**

If each history row represents a change event, more than two rows indicates multiple changes.

---

### 280. Find the longest employee tenure in each department.

**Difficulty:** `Medium`  
**Concept:** `First-row per group`

**SQL**

```sql
SELECT employee_id, employee_name, department_id, hire_date
FROM (
 SELECT e.*,
 ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY hire_date) AS rn
 FROM employees e
) x
WHERE rn = 1;
```

**Answer / Explanation**

The earliest hire date represents the longest tenure.

---

### 281. Find customers whose spend increased month over month for three months.

**Difficulty:** `Hard`  
**Concept:** `Trend analysis`

**SQL**

```sql
WITH monthly AS (
 SELECT customer_id, DATE_TRUNC('month', order_date) AS month,
 SUM(total_amount) AS spend
 FROM orders
 GROUP BY customer_id, DATE_TRUNC('month', order_date)
), x AS (
 SELECT *,
 LAG(spend, 1) OVER (PARTITION BY customer_id ORDER BY month) AS p1,
 LAG(spend, 2) OVER (PARTITION BY customer_id ORDER BY month) AS p2
 FROM monthly
)
SELECT DISTINCT customer_id
FROM x
WHERE spend > p1 AND p1 > p2;
```

**Answer / Explanation**

LAG compares consecutive monthly spend values to identify increasing sequences.

---

### 282. Find orders where the stored total does not equal item total.

**Difficulty:** `Medium`  
**Concept:** `Reconciliation`

**SQL**

```sql
SELECT o.order_id, o.total_amount,
 SUM(oi.quantity * oi.unit_price) AS calculated_total
FROM orders o
JOIN order_items oi ON oi.order_id = o.order_id
GROUP BY o.order_id, o.total_amount
HAVING o.total_amount <> SUM(oi.quantity * oi.unit_price);
```

**Answer / Explanation**

The query recalculates the total from detail rows and reports mismatches.

---

### 283. Find duplicate payments for the same customer and amount on the same day.

**Difficulty:** `Easy`  
**Concept:** `Duplicate detection`

**SQL**

```sql
SELECT customer_id, payment_date, amount, COUNT(*) AS cnt
FROM payments
GROUP BY customer_id, payment_date, amount
HAVING COUNT(*) > 1;
```

**Answer / Explanation**

Grouping by the business key reveals repeated payments.

---

### 284. Find the second purchase date for every customer.

**Difficulty:** `Hard`  
**Concept:** `Nth event`

**SQL**

```sql
SELECT customer_id, order_date
FROM (
 SELECT customer_id, order_date,
 DENSE_RANK() OVER (PARTITION BY customer_id ORDER BY order_date) AS rnk


 FROM orders
) x
WHERE rnk = 2;
```

**Answer / Explanation**

DENSE_RANK identifies the second distinct purchase date.

---

### 285. Find customers with purchases in every quarter of a year.

**Difficulty:** `Medium`  
**Concept:** `Coverage analysis`

**SQL**

```sql
SELECT customer_id
FROM orders
WHERE EXTRACT(YEAR FROM order_date) = 2026
GROUP BY customer_id
HAVING COUNT(DISTINCT EXTRACT(QUARTER FROM order_date)) = 4;
```

**Answer / Explanation**

Four distinct quarters means the customer purchased during every quarter.

---

### 286. Find employees whose salary is above their manager's salary.

**Difficulty:** `Medium`  
**Concept:** `Manager comparison`

**SQL**

```sql
SELECT e.employee_id, e.employee_name, e.salary,
 m.employee_name AS manager, m.salary AS manager_salary
FROM employees e
JOIN employees m ON m.employee_id = e.manager_id
WHERE e.salary > m.salary;
```

**Answer / Explanation**

A self join supplies the manager salary for comparison.

---

### 287. Find the first and last purchase plus lifetime spend for each customer.

**Difficulty:** `Easy`  
**Concept:** `Customer 360`

**SQL**

```sql
SELECT customer_id,
 MIN(order_date) AS first_purchase,
 MAX(order_date) AS last_purchase,
 SUM(total_amount) AS lifetime_spend
FROM orders
GROUP BY customer_id;
```

**Answer / Explanation**

One grouped query produces three common customer-lifetime metrics.

---

### 288. Find products with declining sales for two consecutive months.

**Difficulty:** `Hard`  
**Concept:** `Trend detection`

**SQL**

```sql
WITH monthly AS (
 SELECT product_id, DATE_TRUNC('month', order_date) AS month,
 SUM(quantity) AS units
 FROM order_items oi


 JOIN orders o ON o.order_id = oi.order_id
 GROUP BY product_id, DATE_TRUNC('month', order_date)
), x AS (
 SELECT *,
 LAG(units, 1) OVER (PARTITION BY product_id ORDER BY month) AS p1,
 LAG(units, 2) OVER (PARTITION BY product_id ORDER BY month) AS p2
 FROM monthly
)
SELECT DISTINCT product_id
FROM x
WHERE units < p1 AND p1 < p2;
```

**Answer / Explanation**

Two LAG values allow three consecutive monthly observations to be compared.

---

### 289. Find employees eligible for a bonus: above target sales and no prior bonus.

**Difficulty:** `Medium`  
**Concept:** `Business rule`

**SQL**

```sql
SELECT e.employee_id, e.employee_name
FROM employees e
JOIN employee_sales s ON s.employee_id = e.employee_id
LEFT JOIN bonuses b ON b.employee_id = e.employee_id
WHERE s.sales_target_pct >= 100
 AND b.employee_id IS NULL;
```

**Answer / Explanation**

The join combines performance and bonus history while the NULL test enforces no prior bonus.

---

### 290. Find the busiest sales day.

**Difficulty:** `Easy`  
**Concept:** `Top day`

**SQL**

```sql
SELECT CAST(order_date AS DATE) AS order_day,
 COUNT(*) AS orders
FROM orders
GROUP BY CAST(order_date AS DATE)
ORDER BY orders DESC
FETCH FIRST 1 ROW ONLY;
```

**Answer / Explanation**

Daily order counts reveal the busiest calendar day.

---

### 291. Find customers whose average order value is above the company average order value.

**Difficulty:** `Hard`  
**Concept:** `Business comparison`

**SQL**

```sql
WITH customer_avg AS (
 SELECT customer_id, AVG(total_amount) AS avg_order
 FROM orders
 GROUP BY customer_id
)
SELECT customer_id, avg_order
FROM customer_avg


WHERE avg_order > (SELECT AVG(total_amount) FROM orders);
```

**Answer / Explanation**

Customer-level averages are compared against the overall order-level average.

---

## SQL Fundamentals

### 292. Find employees whose salary is NULL or zero.

**Difficulty:** `Easy`  
**Concept:** `NULL + OR`

**SQL**

```sql
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary IS NULL OR salary = 0;
```

**Answer / Explanation**

The predicate captures both missing salary values and explicit zero values.

---

## Aggregation & GROUP BY

### 293. Find departments where the highest salary is at least three times the lowest salary.

**Difficulty:** `Medium`  
**Concept:** `Aggregate comparison`

**SQL**

```sql
SELECT department_id,
 MAX(salary) AS max_salary,
 MIN(salary) AS min_salary
FROM employees
GROUP BY department_id
HAVING MAX(salary) >= 3 * MIN(salary);
```

**Answer / Explanation**

Comparing MAX and MIN within each group identifies departments with a wide salary range.

---

## Subqueries

### 294. Find customers whose first order was larger than 1000.

**Difficulty:** `Hard`  
**Concept:** `Correlated subquery`

**SQL**

```sql
SELECT o.customer_id, o.order_id, o.total_amount
FROM orders o
WHERE o.order_date = (
 SELECT MIN(o2.order_date)
 FROM orders o2
 WHERE o2.customer_id = o.customer_id
)
AND o.total_amount > 1000;
```

**Answer / Explanation**

The correlated MIN locates each customer's first order before applying the value threshold.

---

## CTEs & Recursive SQL

### 295. Use a CTE to calculate each customer's lifetime value and order count.

**Difficulty:** `Easy`  
**Concept:** `CTE + aggregation`

**SQL**

```sql
WITH customer_metrics AS (
 SELECT customer_id,
 COUNT(*) AS order_count,
 SUM(total_amount) AS lifetime_value
 FROM orders
 GROUP BY customer_id
)
SELECT * FROM customer_metrics;
```

**Answer / Explanation**

The CTE creates a reusable customer-level metrics dataset.

---

## Window Functions

### 296. Find the percentage difference between an employee's salary and the department maximum.

**Difficulty:** `Hard`  
**Concept:** `Window MAX`

**SQL**

```sql
SELECT employee_name, department_id, salary,
 100.0 * (salary - MAX(salary) OVER (PARTITION BY department_id)) /
 NULLIF(MAX(salary) OVER (PARTITION BY department_id), 0) AS pct_from_max
FROM employees;
```

**Answer / Explanation**

The departmental maximum is available on each row, allowing a percentage comparison without 
collapsing the result.

---

## Dates, Strings & NULL Handling

### 297. Find orders placed in the current quarter.

**Difficulty:** `Medium`  
**Concept:** `Quarter filtering`

**SQL**

```sql
SELECT order_id, order_date, total_amount
FROM orders
WHERE order_date >= DATE_TRUNC('quarter', CURRENT_DATE)
 AND order_date < DATE_TRUNC('quarter', CURRENT_DATE) + INTERVAL '3 months';
```

**Answer / Explanation**

The half-open range isolates the current calendar quarter.

---

## Advanced Data Modification

### 298. Archive and delete orders older than five years safely.

**Difficulty:** `Hard`  
**Concept:** `Transactional archive`

**SQL**

```sql
BEGIN;
INSERT INTO orders_archive
SELECT * FROM orders
WHERE order_date < CURRENT_DATE - INTERVAL '5 years';
DELETE FROM orders
WHERE order_date < CURRENT_DATE - INTERVAL '5 years';
COMMIT;
```

**Answer / Explanation**

The transaction keeps the archive insert and source deletion atomic.

---

## Views, Procedures, Functions & Transactions

### 299. Create a view that exposes only orders from the last 30 days.

**Difficulty:** `Easy`  
**Concept:** `VIEW`

**SQL**

```sql
CREATE VIEW recent_orders AS
SELECT order_id, customer_id, order_date, total_amount
FROM orders
WHERE order_date >= CURRENT_DATE - INTERVAL '30 days';
```

**Answer / Explanation**

The view centralizes a commonly used recent-order filter.

---

## Performance & Optimization

### 300. Create an index for recent orders by customer.

**Difficulty:** `Medium`  
**Concept:** `Composite index`

**SQL**

```sql
CREATE INDEX idx_orders_customer_recent
ON orders(customer_id, order_date DESC);
```

**Answer / Explanation**

Putting customer_id first supports customer filtering while the date ordering helps recent-order 
retrieval.

---

## Interview Tips

- Explain the approach before writing the query.
- Mention assumptions and NULL behavior where relevant.
- Be ready to adapt PostgreSQL syntax to MySQL, SQL Server, or Oracle.
- For production SQL, discuss indexes, execution plans, data volume, locking, and transaction safety when relevant.
