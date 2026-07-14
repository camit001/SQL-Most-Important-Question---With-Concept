# 📊 Pattern 9: HAVING vs WHERE (Questions 62–66)

`WHERE` and `HAVING` are two of the most commonly confused SQL clauses. Understanding when to use each is a favorite interview topic.

## Topics Covered

- **WHERE** – Filters individual rows **before** grouping and aggregation are performed.
- **HAVING** – Filters grouped data **after** the `GROUP BY` operation based on aggregate conditions.
- **Aggregate Filtering** – Filters groups using aggregate functions such as `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()`, typically in the `HAVING` clause.
- **Multiple Conditions** – Combines multiple conditions using logical operators such as `AND`, `OR`, and `NOT` in `WHERE` or `HAVING` clauses.
- **Conditional Aggregation** – Uses aggregate functions with `CASE` expressions to calculate values based on specific conditions (e.g., `SUM(CASE WHEN status = 'Approved' THEN amount ELSE 0 END)`).
- **Best Practices** – Use `WHERE` to filter rows before aggregation, `HAVING` only for aggregate conditions, avoid unnecessary calculations, and write clear, optimized conditions for better query performance.

---

# Q62. Find Employees with Salary Greater Than 60,000

### Difficulty

🟢 Beginner

### Concept

Use **WHERE** to filter rows before aggregation.

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    department,
    salary
FROM employees
WHERE salary > 60000;
```

### Explanation

WHERE filters individual rows before any GROUP BY operation.

### Expected Output

|Employee|Department|Salary|
|---------|----------|------|
|John|IT|85000|
|Alice|Finance|72000|

### Interview Tip

Use **WHERE** when filtering normal rows.

---

# Q63. Find Total Sales for Orders Greater Than ₹500

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    customer_id,
    SUM(amount) AS TotalSales
FROM orders
WHERE amount > 500
GROUP BY customer_id;
```

### Explanation

Rows with amount less than or equal to 500 are filtered before aggregation.

---

# Q64. Find Customers Whose Total Purchase Exceeds ₹500

### Difficulty

🟡 Intermediate

### Concept

Use **HAVING** to filter aggregated results.

### SQL Server Solution

```sql
SELECT
    customer_id,
    SUM(amount) AS TotalPurchase
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > 500;
```

### Explanation

HAVING filters groups after GROUP BY.

### Expected Output

|Customer|Total Purchase|
|---------|--------------|
|101|2450|
|103|1280|

### Interview Tip

Aggregate functions like SUM(), COUNT(), AVG() cannot be used inside WHERE.

---

# Q65. Find Departments Having More Than 2 Employees

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    department,
    COUNT(*) AS EmployeeCount
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

### Explanation

HAVING filters departments after counting employees.

### Expected Output

|Department|Employees|
|-----------|---------|
|IT|5|
|Finance|4|

---

# Q66. Find Departments with Average Salary Greater Than ₹70,000

### Difficulty

🔴 Advanced

### SQL Server Solution

```sql
SELECT
    department,
    AVG(salary) AS AverageSalary
FROM employees
WHERE salary > 30000
GROUP BY department
HAVING AVG(salary) > 70000
ORDER BY AverageSalary DESC;
```

### Explanation

This query demonstrates using both WHERE and HAVING together.

Execution order:

1. WHERE filters rows.
2. GROUP BY creates groups.
3. AVG() calculates average salary.
4. HAVING filters the groups.
5. ORDER BY sorts the result.

### Expected Output

|Department|Average Salary|
|-----------|--------------|
|IT|92000|
|Finance|78500|

---

# 🎯 WHERE vs HAVING Cheat Sheet

| WHERE | HAVING |
|--------|---------|
| Filters rows | Filters groups |
| Executes before GROUP BY | Executes after GROUP BY |
| Cannot use aggregate functions | Can use aggregate functions |
| Faster | Slightly slower |

---

# ⭐ Most Asked Interview Questions

✅ WHERE vs HAVING

✅ Can HAVING work without GROUP BY?

✅ Can aggregate functions be used in WHERE?

✅ Query execution order

✅ WHERE + HAVING together

---

# 💡 Interview Tips

### SQL Query Execution Order

```text
FROM
    ↓
WHERE
    ↓
GROUP BY
    ↓
HAVING
    ↓
SELECT
    ↓
ORDER BY
```

---

### Example: WHERE

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

Filters individual employee records.

---

### Example: HAVING

```sql
SELECT
    department,
    COUNT(*) AS TotalEmployees
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

Filters grouped results.

---

### WHERE + HAVING Together

```sql
SELECT
    department,
    AVG(salary) AS AvgSalary
FROM employees
WHERE salary > 30000
GROUP BY department
HAVING AVG(salary) > 70000;
```

This is one of the most common SQL interview questions.

---

## Next Chapter

➡ **10_Self_Joins.md (Questions 67–75)**

Topics Covered

- Self JOIN
- Employee–Manager Hierarchy
- Consecutive Records
- Duplicate Detection
- Version Comparison
- Hierarchical Queries
- Advanced Self JOIN Scenarios
