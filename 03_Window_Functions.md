# 📊 Pattern 3: Window Functions (Questions 19–28)

Window Functions perform calculations across a set of rows without collapsing them into a single row. They are among the most frequently asked SQL interview topics for Data Engineers, Data Analysts, and BI Developers.

## Topics Covered

- **ROW_NUMBER()** – Assigns a unique sequential number to each row within a partition.
- **RANK()** – Assigns a rank to each row, with gaps in ranking when there are ties.
- **DENSE_RANK()** – Assigns a rank to each row without gaps, even when there are ties.
- **LEAD()** – Retrieves the value from the next row without using a self-join.
- **LAG()** – Retrieves the value from the previous row without using a self-join.
- **FIRST_VALUE()** – Returns the first value in the ordered window (partition).
- **LAST_VALUE()** – Returns the last value in the ordered window (partition); often requires an explicit window frame to return the expected last value.
- **Running Total** – Calculates a cumulative total by continuously adding values from the beginning up to the current row.
- **Moving Average** – Calculates the average of a fixed number of consecutive rows (or a time window) around the current row.
- **NTILE()** – Divides rows into a specified number of approximately equal-sized groups (buckets) and assigns a bucket number to each row.

---

# Q19. Assign a Unique Row Number to Employees Based on Salary

### Difficulty

🟢 Beginner

### Concept

Assign a unique sequential number to each employee based on salary.

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS RowNum
FROM employees;
```

### Explanation

- ROW_NUMBER() always generates unique numbers.
- No duplicate ranking.

### Expected Output

|Employee|Salary|Row Number|
|---------|------|----------|
|John|90000|1|
|Alice|90000|2|
|David|85000|3|

### Interview Tip

ROW_NUMBER() never produces duplicate ranks.

---

# Q20. Rank Employees Based on Salary

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) AS SalaryRank
FROM employees;
```

### Explanation

Employees with the same salary receive the same rank.

Ranks may skip numbers.

Example:

```
1
2
2
4
```

### Interview Tip

Use RANK() when ties should create gaps.

---

# Q21. Dense Rank Employees by Salary

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS DenseRank
FROM employees;
```

### Explanation

Unlike RANK(),

DENSE_RANK()

does not skip numbers.

Example

```
1
2
2
3
```

### Interview Tip

One of Microsoft's favorite interview questions.

---

# Q22. Find Previous Month Sales Using LAG()

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    month_num,
    sales,
    LAG(sales) OVER
    (
        ORDER BY month_num
    ) AS PreviousMonthSales
FROM monthly_sales;
```

### Explanation

LAG() returns data from the previous row.

### Expected Output

|Month|Sales|Previous|
|------|------|---------|
|1|2000|NULL|
|2|2500|2000|
|3|1800|2500|

### Performance Tip

No self join required.

---

# Q23. Find Next Month Sales

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    month_num,
    sales,
    LEAD(sales) OVER
    (
        ORDER BY month_num
    ) AS NextMonthSales
FROM monthly_sales;
```

### Explanation

LEAD() looks ahead.

Useful for forecasting.

---

# Q24. Running Total of Sales

### Difficulty

🟟 Intermediate

### SQL Server Solution

```sql
SELECT
    order_date,
    amount,
    SUM(amount) OVER
    (
        ORDER BY order_date
    ) AS RunningTotal
FROM orders;
```

### Explanation

SUM() OVER()

calculates cumulative totals.

Example

```
100
250
420
650
```

---

# Q25. Moving Average of Last Three Orders

### Difficulty

🔴 Advanced

### SQL Server Solution

```sql
SELECT
    order_date,
    amount,
    AVG(amount) OVER
    (
        ORDER BY order_date
        ROWS BETWEEN 2 PRECEDING
        AND CURRENT ROW
    ) AS MovingAverage
FROM orders;
```

### Explanation

Calculates rolling average over

Current Row

+

Previous Two Rows

---

# Q26. First Order Amount for Every Customer

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    customer_id,
    order_date,
    amount,
    FIRST_VALUE(amount)
    OVER
    (
        PARTITION BY customer_id
        ORDER BY order_date
    ) AS FirstOrderAmount
FROM orders;
```

### Explanation

Returns the first order amount of every customer.

---

# Q27. Last Order Amount for Every Customer

### Difficulty

🔴 Advanced

### SQL Server Solution

```sql
SELECT
    customer_id,
    order_date,
    amount,
    LAST_VALUE(amount)
    OVER
    (
        PARTITION BY customer_id
        ORDER BY order_date
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS LastOrderAmount
FROM orders;
```

### Explanation

Without

```
ROWS BETWEEN UNBOUNDED PRECEDING
AND UNBOUNDED FOLLOWING
```

LAST_VALUE()

often gives incorrect results.

This is a very common SQL interview trick.

---

# Q28. Divide Employees into Four Salary Groups

### Difficulty

🔴 Advanced

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    salary,
    NTILE(4) OVER
    (
        ORDER BY salary DESC
    ) AS SalaryGroup
FROM employees;
```

### Explanation

NTILE()

splits data into equal-sized buckets.

Example

```
Group 1

Highest salaries

Group 2

High salaries

Group 3

Medium salaries

Group 4

Lowest salaries
```

Useful for segmentation.

---

# 🎯 Window Function Cheat Sheet

| Function | Purpose |
|----------|---------|
| ROW_NUMBER() | Unique numbering |
| RANK() | Ranking with gaps |
| DENSE_RANK() | Ranking without gaps |
| LEAD() | Next row |
| LAG() | Previous row |
| FIRST_VALUE() | First value |
| LAST_VALUE() | Last value |
| NTILE() | Divide rows into groups |
| SUM() OVER() | Running Total |
| AVG() OVER() | Moving Average |

---

# ⭐ Most Asked Window Function Questions

✅ Second Highest Salary

✅ Top N Employees

✅ Running Total

✅ Previous Month Sales

✅ Next Month Sales

✅ Ranking

✅ Dense Ranking

✅ Customer First Order

✅ Customer Last Order

✅ Quartile Analysis

---

## Interview Tips

### ROW_NUMBER()

Always unique.

### RANK()

Duplicates allowed.

Rank skipped.

```
1

2

2

4
```

### DENSE_RANK()

Duplicates allowed.

No skipped rank.

```
1

2

2

3
```

---

## Next Chapter

➡ **04_Subqueries_and_CTEs.md (Questions 29–36)**

Topics Covered

- Scalar Subqueries
- Correlated Subqueries
- EXISTS
- NOT EXISTS
- IN
- Recursive CTE
- Common Table Expressions
- Nested Queries
