# 🔄 Pattern 10: Self JOINs & Advanced JOIN Scenarios (Questions 67–75)

Self JOIN is used when a table needs to be joined with itself. It is commonly used for hierarchical data, duplicate detection, consecutive records, and version comparison.

## Topics Covered

- **Employee → Manager Hierarchy** – Retrieves and analyzes reporting relationships between employees and managers using self-joins or recursive CTEs.
- **Duplicate Detection** – Identifies duplicate records using `GROUP BY`, `HAVING`, or window functions such as `ROW_NUMBER()`.
- **Consecutive Records** – Finds continuous sequences of records (gaps and islands) using window functions like `ROW_NUMBER()`, `LAG()`, or `LEAD()`.
- **Previous & Next Records** – Compares the current row with the previous or next row using `LAG()` and `LEAD()` without self-joins.
- **Version Comparison** – Compares different versions of the same record to identify changes, additions, or updates over time.
- **Hierarchical Queries** – Retrieves parent-child or multi-level relationships (e.g., organizational charts or category trees) using recursive CTEs or self-joins.

---

# Q67. Display Employee and Their Manager

### Difficulty

🟢 Beginner

### Concept

Display each employee along with their manager.

### SQL Server Solution

```sql
SELECT
    e.employee_id,
    e.name AS EmployeeName,
    m.name AS ManagerName
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id
ORDER BY e.employee_id;
```

### Explanation

The employees table is joined with itself.

One alias represents employees.

The other alias represents managers.

### Expected Output

|Employee|Manager|
|---------|--------|
|John|Robert|
|Alice|Robert|
|Robert|NULL|

### Interview Tip

This is one of the most common Self JOIN interview questions.

---

# Q68. Find Employees Reporting to the Same Manager

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    e1.name AS Employee1,
    e2.name AS Employee2,
    e1.manager_id
FROM employees e1
INNER JOIN employees e2
ON e1.manager_id = e2.manager_id
AND e1.employee_id < e2.employee_id
ORDER BY e1.manager_id;
```

### Explanation

Employees sharing the same manager are paired together.

The condition

```sql
e1.employee_id < e2.employee_id
```

avoids duplicate combinations.

### Expected Output

|Employee 1|Employee 2|Manager|
|-----------|----------|-------|
|John|Alice|101|

---

# Q69. Find Consecutive Stock Prices

### Difficulty

🔴 Advanced

### Concept

Compare each day's stock price with the previous day.

### SQL Server Solution

```sql
SELECT
    s1.trade_date,
    s1.price AS CurrentPrice,
    s2.price AS PreviousPrice
FROM stock_prices s1
INNER JOIN stock_prices s2
ON s2.trade_date = DATEADD(DAY,-1,s1.trade_date)
ORDER BY s1.trade_date;
```

### Explanation

The current day's record is joined with the previous day's record.

### Alternative

Use

```sql
LAG(price)
```

which is simpler and faster.

---

# Q70. Find All Employees Working Under Employee ID = 2

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    manager_id
FROM employees
WHERE manager_id = 2;
```

### Explanation

Returns employees whose manager is Employee ID 2.

### Expected Output

|Employee|Manager ID|
|---------|----------|
|David|2|
|Sophia|2|

---

# Q71. Find Duplicate Email Addresses

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    email,
    COUNT(*) AS TotalDuplicates
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;
```

### Explanation

Groups email addresses and returns only duplicates.

---

# Q72. Compare Product Versions

### Difficulty

🔴 Advanced

### Concept

Compare old and new product versions.

### SQL Server Solution

```sql
SELECT
    p1.product_id,
    p1.version AS OldVersion,
    p2.version AS NewVersion
FROM product_versions p1
INNER JOIN product_versions p2
ON p1.product_id = p2.product_id
AND p1.version < p2.version
ORDER BY
    p1.product_id,
    p1.version;
```

### Explanation

Returns all version upgrades for each product.

### Expected Output

|Product|Old|New|
|--------|---|---|
|101|1.0|2.0|
|101|2.0|3.0|

---

# Q73. Find Customers Sharing the Same City

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    c1.name AS Customer1,
    c2.name AS Customer2,
    c1.city
FROM customers c1
INNER JOIN customers c2
ON c1.city = c2.city
AND c1.customer_id < c2.customer_id
ORDER BY c1.city;
```

### Explanation

Displays customer pairs living in the same city.

---

# Q74. Find Employees Earning More Than Their Manager

### Difficulty

🔴 Advanced

### SQL Server Solution

```sql
SELECT
    e.name AS EmployeeName,
    e.salary AS EmployeeSalary,
    m.name AS ManagerName,
    m.salary AS ManagerSalary
FROM employees e
INNER JOIN employees m
ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

### Explanation

A classic SQL interview question.

Compares employee salary with manager salary.

### Expected Output

|Employee|Employee Salary|Manager|Manager Salary|
|---------|---------------|-------|--------------|
|John|95000|Robert|90000|

---

# Q75. Find All Employees Reporting to Employee ID = 9

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    manager_id
FROM employees
WHERE manager_id = 9;
```

### Explanation

Returns all employees directly reporting to Employee ID 9.

---

# 🎯 Self JOIN Cheat Sheet

| Pattern | Use Case |
|---------|----------|
| Employee → Manager | Organizational hierarchy |
| Customer Pairs | Same city |
| Product Versions | Compare versions |
| Consecutive Records | Previous/Next values |
| Salary Comparison | Employee vs Manager |
| Duplicate Matching | Identify duplicates |

---

# ⭐ Most Asked Interview Questions

✅ Employee–Manager Hierarchy

✅ Employees Reporting to Same Manager

✅ Employee Salary Greater Than Manager

✅ Product Version Comparison

✅ Previous Day Sales

✅ Consecutive Dates

✅ Duplicate Email Detection

---

# 💡 Interview Tips

### When to Use a Self JOIN

Use a Self JOIN when:

- A table references itself (hierarchical data).
- You need to compare rows within the same table.
- You need previous/next row logic (although window functions are often preferred).
- You are identifying relationships or duplicates.

---

## 📚 SQL Interview Revision Checklist

Before attending an interview, make sure you are comfortable with:

### Joins
- ✅ INNER JOIN
- ✅ LEFT JOIN
- ✅ RIGHT JOIN
- ✅ FULL OUTER JOIN
- ✅ CROSS JOIN
- ✅ SELF JOIN

### Aggregations
- ✅ GROUP BY
- ✅ HAVING
- ✅ COUNT()
- ✅ SUM()
- ✅ AVG()
- ✅ MIN()
- ✅ MAX()

### Window Functions
- ✅ ROW_NUMBER()
- ✅ RANK()
- ✅ DENSE_RANK()
- ✅ LEAD()
- ✅ LAG()
- ✅ FIRST_VALUE()
- ✅ LAST_VALUE()
- ✅ NTILE()

### Advanced SQL
- ✅ CTE
- ✅ Recursive CTE
- ✅ Correlated Subquery
- ✅ EXISTS
- ✅ NOT EXISTS
- ✅ CASE
- ✅ String Functions
- ✅ Date Functions

---

# 🎉 Congratulations!

You have completed all **75 SQL Interview Questions** with:

- ✅ SQL Server (SSMS) compatible solutions
- ✅ Concepts and explanations
- ✅ Interview tips
- ✅ Performance best practices
- ✅ Beginner → Advanced progression

Happy Learning and Best of Luck for Your SQL Interviews! 🚀
