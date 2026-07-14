# 🔀 Pattern 6: CASE Statements (Questions 44–50)

The **CASE** expression is SQL's equivalent of an IF-ELSE statement. It allows you to implement conditional logic directly in your SQL queries.

## Topics Covered

- **CASE WHEN** – Evaluates conditions and returns a value based on the first matching condition, similar to an `IF...ELSE` statement.
- **Multiple Conditions** – Uses multiple `WHEN` clauses or logical operators (`AND`, `OR`) to evaluate several conditions in a single `CASE` expression.
- **Conditional Aggregation** – Uses aggregate functions with `CASE` expressions to calculate values based on specific conditions (e.g., `SUM(CASE WHEN status = 'Approved' THEN amount ELSE 0 END)`).
- **Custom Sorting** – Uses `CASE` in the `ORDER BY` clause to sort data in a user-defined order instead of the default alphabetical or numeric order.
- **Data Classification** – Uses `CASE` to categorize data into groups based on specified conditions (e.g., High, Medium, Low).
- **Dynamic Labels** – Uses `CASE` to assign descriptive labels to values dynamically based on conditions.
- **Pivot-style Logic** – Uses `CASE` with aggregate functions to transform row values into separate columns, simulating a pivot table.

---

# Q44. Categorize Employees Based on Salary

### Difficulty

🟢 Beginner

### Concept

Classify employees into different salary bands.

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    salary,
    CASE
        WHEN salary >= 100000 THEN 'High Salary'
        WHEN salary >= 60000 THEN 'Medium Salary'
        ELSE 'Low Salary'
    END AS SalaryCategory
FROM employees;
```

### Explanation

CASE evaluates conditions from top to bottom.

The first matching condition is returned.

### Expected Output

|Employee|Salary|Category|
|---------|------|---------|
|John|120000|High Salary|
|Alice|75000|Medium Salary|
|David|45000|Low Salary|

### Interview Tip

CASE is SQL's IF-ELSE statement.

---

# Q45. Show Customer Purchase Status

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    customer_id,
    name,
    CASE
        WHEN customer_id IN
        (
            SELECT customer_id
            FROM orders
        )
        THEN 'Purchased'
        ELSE 'Not Purchased'
    END AS PurchaseStatus
FROM customers;
```

### Explanation

Checks whether a customer has placed an order.

---

# Q46. Count High-Value and Low-Value Orders

### Difficulty

🟡 Intermediate

### Concept

Conditional Aggregation.

### SQL Server Solution

```sql
SELECT
    COUNT(CASE WHEN amount >= 1000 THEN 1 END) AS HighValueOrders,
    COUNT(CASE WHEN amount < 1000 THEN 1 END) AS LowValueOrders
FROM orders;
```

### Alternative

```sql
SELECT
    SUM(CASE WHEN amount >= 1000 THEN 1 ELSE 0 END) AS HighValueOrders,
    SUM(CASE WHEN amount < 1000 THEN 1 ELSE 0 END) AS LowValueOrders
FROM orders;
```

### Explanation

Conditional aggregation is extremely common in dashboards.

---

# Q47. Calculate Bonus Based on Salary

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    salary,
    CASE
        WHEN salary >= 100000 THEN salary * 0.20
        WHEN salary >= 70000 THEN salary * 0.15
        ELSE salary * 0.10
    END AS Bonus
FROM employees;
```

### Explanation

Different bonus percentages are assigned according to salary slabs.

---

# Q48. Assign Letter Grades to Students

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    student_id,
    student_name,
    marks,
    CASE
        WHEN marks >= 90 THEN 'A'
        WHEN marks >= 80 THEN 'B'
        WHEN marks >= 70 THEN 'C'
        WHEN marks >= 60 THEN 'D'
        ELSE 'F'
    END AS Grade
FROM students;
```

### Explanation

CASE can replace long IF-ELSE logic.

---

# Q49. Sort Employees by Department Priority

### Difficulty

🔴 Advanced

### Concept

Custom Sorting using CASE.

### SQL Server Solution

```sql
SELECT
    employee_id,
    name,
    department
FROM employees
ORDER BY
CASE
    WHEN department='IT' THEN 1
    WHEN department='Finance' THEN 2
    WHEN department='HR' THEN 3
    ELSE 4
END;
```

### Explanation

CASE inside ORDER BY provides custom sorting.

### Interview Tip

Frequently asked in reporting interviews.

---

# Q50. Generate Sales Performance Report

### Difficulty

🔴 Advanced

### SQL Server Solution

```sql
SELECT
    employee_id,
    employee_name,
    total_sales,
    CASE
        WHEN total_sales >= 100000 THEN 'Excellent'
        WHEN total_sales >= 50000 THEN 'Good'
        WHEN total_sales >= 25000 THEN 'Average'
        ELSE 'Needs Improvement'
    END AS Performance
FROM sales;
```

### Explanation

CASE converts numeric values into meaningful business labels.

---

# 🎯 CASE Statement Cheat Sheet

| CASE Expression | Purpose |
|-----------------|---------|
| CASE WHEN | Conditional Logic |
| Conditional Aggregation | Dashboard Metrics |
| CASE + ORDER BY | Custom Sorting |
| CASE + GROUP BY | Dynamic Grouping |
| CASE + SUM() | Pivot-style Reports |

---

# ⭐ Most Asked Interview Questions

✅ Employee Salary Classification

✅ Customer Purchase Status

✅ Conditional Aggregation

✅ Dynamic Bonus Calculation

✅ Grade Calculation

✅ Business Performance Dashboard

✅ Custom Sorting

---

# 💡 Interview Tips

### CASE Evaluation Order

CASE stops evaluating as soon as the first matching condition is found.

Example

```sql
CASE
    WHEN salary > 100000 THEN 'High'
    WHEN salary > 70000 THEN 'Medium'
    ELSE 'Low'
END
```

### CASE in ORDER BY

Useful for:

- Custom Department Order
- Status Priority
- Business-defined Sorting

### CASE in Aggregation

Example

```sql
SUM(CASE WHEN amount > 1000 THEN amount ELSE 0 END)
```

Very common in Power BI, SSRS, Tableau, and reporting queries.

---

## Next Chapter

➡ **[String Functions](07_String_Functions.md) (Questions 51–56)**

Topics Covered

- UPPER()
- LOWER()
- LEN()
- SUBSTRING()
- CHARINDEX()
- REPLACE()
- CONCAT()
- LEFT()
- RIGHT()
- TRIM()
