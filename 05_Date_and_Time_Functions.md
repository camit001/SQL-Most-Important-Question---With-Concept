# 📅 Pattern 5: Date & Time Functions (Questions 37–43)

Date functions are frequently used in reporting, analytics, ETL pipelines, and interview questions. SQL Server provides powerful built-in date functions for filtering, grouping, and manipulating dates.

## Topics Covered

- **GETDATE()** – Returns the current system date and time.
- **DATEADD()** – Adds or subtracts a specified time interval (day, month, year, etc.) to a date.
- **DATEDIFF()** – Returns the difference between two dates in a specified unit (day, month, year, etc.).
- **YEAR()** – Extracts the year part from a date.
- **MONTH()** – Extracts the month part from a date.
- **DAY()** – Extracts the day part from a date.
- **EOMONTH()** – Returns the last day of the month for a given date.
- **DATEFROMPARTS()** – Creates a date from the specified year, month, and day values.


---

# Q37. Retrieve Employees Who Joined in 2023

### Difficulty

🟢 Beginner

### Concept

Extract the year from a date.

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    joining_date
FROM employees
WHERE YEAR(joining_date) = 2023;
```

### Explanation

YEAR() extracts the year portion from a date.

### Interview Tip

Avoid applying YEAR() on indexed columns in production filters.

Better approach:

```sql
WHERE joining_date >= '2023-01-01'
AND joining_date < '2024-01-01'
```

---

# Q38. Find Orders Placed During the Last 30 Days

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    order_id,
    customer_id,
    order_date,
    amount
FROM orders
WHERE order_date >= DATEADD(DAY,-30,GETDATE());
```

### Explanation

DATEADD() subtracts 30 days from today's date.

GETDATE() returns the current system date and time.

---

# Q39. Calculate Employee Experience in Years

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    joining_date,
    DATEDIFF(YEAR, joining_date, GETDATE()) AS ExperienceYears
FROM employees;
```

### Explanation

DATEDIFF()

returns the difference between two dates.

---

# Q40. Find Total Sales by Year and Month

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    YEAR(order_date) AS SalesYear,
    MONTH(order_date) AS SalesMonth,
    SUM(amount) AS TotalSales
FROM orders
GROUP BY
    YEAR(order_date),
    MONTH(order_date)
ORDER BY
    SalesYear,
    SalesMonth;
```

### Explanation

YEAR()

and

MONTH()

are commonly used in reporting dashboards.

---

# Q41. Find the Last Day of Every Order Month

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    order_id,
    order_date,
    EOMONTH(order_date) AS MonthEnd
FROM orders;
```

### Explanation

EOMONTH()

returns the last day of the month.

Example

```
2025-02-12

↓

2025-02-28
```

---

# Q42. Create a Date Using Year, Month, and Day

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    DATEFROMPARTS(2025,7,14) AS CreatedDate;
```

### Explanation

DATEFROMPARTS()

constructs a valid DATE value from separate year, month, and day values.

---

# Q43. Find Orders Placed Today

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    order_id,
    customer_id,
    order_date,
    amount
FROM orders
WHERE CAST(order_date AS DATE) = CAST(GETDATE() AS DATE);
```

### Alternative (Index-Friendly)

```sql
WHERE order_date >= CAST(GETDATE() AS DATE)
AND order_date < DATEADD(DAY,1,CAST(GETDATE() AS DATE));
```

### Explanation

The second query is preferred in production because it allows SQL Server to use indexes efficiently.

---

# 📅 Date Function Cheat Sheet

| Function | Purpose |
|----------|---------|
| GETDATE() | Current Date & Time |
| DATEADD() | Add/Subtract Dates |
| DATEDIFF() | Difference Between Dates |
| YEAR() | Extract Year |
| MONTH() | Extract Month |
| DAY() | Extract Day |
| EOMONTH() | Last Day of Month |
| DATEFROMPARTS() | Create Date |

---

# ⭐ Most Asked Interview Questions

✅ Employees Joined This Year

✅ Orders in Last 30 Days

✅ Experience Calculation

✅ Monthly Sales Report

✅ Last Day of Month

✅ Current Date Filtering

✅ Date Difference

---

# 💡 Interview Tips

### GETDATE()

Returns the current system date and time.

### DATEADD()

Used to add or subtract years, months, days, hours, or minutes.

### DATEDIFF()

Returns the difference between two dates in the specified unit.

### Performance Tip

Avoid wrapping indexed date columns in functions inside the WHERE clause.

Prefer range-based filtering.

---

## Next Chapter

➡ **[CASE Statements](06_CASE_Statements.md) (Questions 44–50)**

Topics Covered

- CASE WHEN
- Conditional Aggregation
- Classification
- Custom Sorting
- Pivot-style Logic
