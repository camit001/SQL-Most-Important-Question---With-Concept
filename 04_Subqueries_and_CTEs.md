# 📊 Pattern 4: Subqueries & Common Table Expressions (CTEs) (Questions 29–36)

Subqueries and CTEs are among the most important SQL concepts. They are widely used to simplify complex queries and improve readability.

## Topics Covered

- **Scalar Subquery** – A subquery that returns a single value and can be used anywhere a single value is expected (e.g., `SELECT`, `WHERE`).
- **Correlated Subquery** – A subquery that depends on the outer query and executes once for each row processed by the outer query.
- **EXISTS** – Returns `TRUE` if the subquery returns at least one row; commonly used to check for the existence of related records.
- **NOT EXISTS** – Returns `TRUE` if the subquery returns no rows; commonly used to find unmatched records.
- **IN** – Checks whether a value exists in a list of values or the result of a subquery.
- **Common Table Expression (CTE)** – A temporary named result set defined using `WITH` that can simplify complex SQL queries.
- **Recursive CTE** – A CTE that references itself to process hierarchical or recursive data, such as employee-manager relationships.
- **Multiple CTEs** – Two or more CTEs defined in a single `WITH` clause, separated by commas, allowing modular and readable query construction.

---

# Q29. Find Employees Earning More Than the Average Salary

### Difficulty

🟢 Beginner

### Concept

Use a subquery to compare each employee's salary with the average salary.

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    salary
FROM employees
WHERE salary >
(
    SELECT AVG(salary)
    FROM employees
);
```

### Explanation

The inner query calculates the average salary.

The outer query returns employees earning above that average.

### Interview Tip

A scalar subquery returns a single value.

---

# Q30. Find Customers Who Have Placed Orders

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    customer_id,
    name
FROM customers
WHERE customer_id IN
(
    SELECT customer_id
    FROM orders
);
```

### Explanation

The subquery returns all customer IDs present in the Orders table.

IN checks whether each customer exists in that list.

---

# Q31. Find Customers Who Never Placed an Order

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    customer_id,
    name
FROM customers
WHERE customer_id NOT IN
(
    SELECT customer_id
    FROM orders
);
```

### Alternative (Recommended)

```sql
SELECT
    c.customer_id,
    c.name
FROM customers c
WHERE NOT EXISTS
(
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

### Interview Tip

Prefer **NOT EXISTS** over **NOT IN** when NULL values may exist.

---

# Q32. Find the Highest Paid Employee in Each Department

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    department,
    salary
FROM employees e
WHERE salary =
(
    SELECT MAX(salary)
    FROM employees
    WHERE department = e.department
);
```

### Explanation

The subquery executes for each department.

This is called a **Correlated Subquery**.

---

# Q33. Find Customers Whose Total Purchase Exceeds 1000

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    customer_id,
    SUM(amount) AS TotalPurchase
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > 1000;
```

### Explanation

HAVING filters groups after aggregation.

---

# Q34. Display the Top 3 Highest Paid Employees Using a CTE

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
WITH EmployeeRank AS
(
    SELECT
        employee_id,
        name,
        salary,
        ROW_NUMBER() OVER
        (
            ORDER BY salary DESC
        ) AS RN
    FROM employees
)
SELECT *
FROM EmployeeRank
WHERE RN <= 3;
```

### Explanation

CTEs improve readability compared to nested subqueries.

---

# Q35. Calculate Running Total Using a CTE

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
WITH SalesCTE AS
(
    SELECT
        order_date,
        amount
    FROM orders
)
SELECT
    order_date,
    amount,
    SUM(amount) OVER
    (
        ORDER BY order_date
    ) AS RunningTotal
FROM SalesCTE;
```

### Explanation

The CTE acts like a temporary result set.

---

# Q36. Generate Numbers from 1 to 10 Using a Recursive CTE

### Difficulty

🔴 Advanced

### SQL Server Solution

```sql
WITH Numbers AS
(
    SELECT 1 AS Num

    UNION ALL

    SELECT Num + 1
    FROM Numbers
    WHERE Num < 10
)
SELECT *
FROM Numbers
OPTION (MAXRECURSION 10);
```

### Explanation

A recursive CTE has:

- Anchor query
- Recursive query
- Termination condition

Without a termination condition, recursion continues until SQL Server reaches the maximum recursion limit.

---

# 🎯 Subquery & CTE Cheat Sheet

| Feature | Description |
|----------|-------------|
| Scalar Subquery | Returns one value |
| Multi-row Subquery | Returns multiple rows |
| Correlated Subquery | Depends on outer query |
| EXISTS | Checks existence |
| NOT EXISTS | Checks non-existence |
| CTE | Temporary named result set |
| Recursive CTE | Self-referencing CTE |

---

# ⭐ Most Asked Interview Questions

✅ Employees earning above average salary

✅ Customers with no orders

✅ Highest salary in each department

✅ Top N employees

✅ Recursive CTE

✅ Running Total using CTE

✅ EXISTS vs IN

✅ Correlated Subquery

---

# 💡 Interview Tips

### EXISTS vs IN

| EXISTS | IN |
|---------|----|
| Faster for large datasets | Better for small datasets |
| Stops after first match | Checks all values |
| Handles NULL safely | Can behave unexpectedly with NULL |

### Correlated Subquery

A correlated subquery executes once for every row processed by the outer query.

### Recursive CTE

A recursive CTE consists of:

- Anchor Member
- Recursive Member
- Termination Condition

---

## Next Chapter

➡ **05_Date_and_Time_Functions.md (Questions 37–43)**

Topics Covered

- GETDATE()
- DATEADD()
- DATEDIFF()
- YEAR()
- MONTH()
- DAY()
- EOMONTH()
- DATEFROMPARTS()
- Date Filtering
