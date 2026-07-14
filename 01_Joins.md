# 📊 Pattern 1: JOINs (Questions 1–10)

JOINs are one of the most frequently asked SQL interview topics. They allow you to retrieve data from multiple tables based on related columns.

## Types of JOINs Covered

- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL OUTER JOIN
- CROSS JOIN
- SELF JOIN

---

# Q1. Find All Customers and Their Orders

### Difficulty
🟢 Beginner

### Concept

Retrieve customers who have placed orders.

### Tables Used

- customers
- orders

### SQL Server Solution

```sql
SELECT
    c.customer_id,
    c.name,
    o.order_id,
    o.amount,
    o.order_date
FROM customers AS c
INNER JOIN orders AS o
    ON c.customer_id = o.customer_id;
```

### Explanation

- INNER JOIN returns only matching rows.
- Customers without orders are excluded.

### Expected Output

|Customer|Order|Amount|
|---------|------|------|
|Alice|1|1299.99|
|Alice|3|49.99|
|Bob|2|29.99|

### Interview Tip

> INNER JOIN returns only matching records from both tables.

---

# Q2. Find Customers Who Never Placed an Order

### Difficulty

🟢 Beginner

### Concept

Find unmatched rows using LEFT JOIN.

### SQL Server Solution

```sql
SELECT
    c.customer_id,
    c.name
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

### Explanation

LEFT JOIN keeps every customer.

Rows having NULL order_id indicate customers who never ordered.

### Expected Output

|Customer ID|Name|
|------------|----|
|11|Karen Taylor|
|12|Liam Anderson|

### Alternative Solution

```sql
SELECT customer_id,name
FROM customers
WHERE customer_id NOT IN
(
SELECT customer_id
FROM orders
);
```

### Interview Tip

This is one of the Top 10 SQL interview questions.

---

# Q3. Show Customer Name, Product Name and Order Amount

### Difficulty

🟡 Intermediate

### Tables

- customers
- orders
- products

### SQL Server Solution

```sql
SELECT
    c.name AS CustomerName,
    p.product_name,
    o.amount
FROM customers c
INNER JOIN orders o
    ON c.customer_id = o.customer_id
INNER JOIN products p
    ON o.product_id = p.product_id;
```

### Explanation

This joins three tables using two INNER JOIN operations.

### Expected Output

|Customer|Product|Amount|
|---------|--------|------|
|Alice|Laptop Pro|1299.99|

### Interview Tip

Whenever more than two tables are required, join them one by one.

---

# Q4. Find Products Never Ordered

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    p.product_id,
    p.product_name
FROM products p
LEFT JOIN orders o
    ON p.product_id = o.product_id
WHERE o.product_id IS NULL;
```

### Explanation

Products without matching orders will have NULL values.

### Expected Output

|Product|
|--------|
|Desk Lamp|
|Notebook|

### Alternative Solution

```sql
SELECT product_id,product_name
FROM products
WHERE product_id NOT IN
(
SELECT product_id
FROM orders
);
```

### Performance Tip

For large tables,

LEFT JOIN ... IS NULL

usually performs better than

NOT IN

when NULL values exist.

---

# Q5. Total Revenue by Customer Category

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    c.category,
    SUM(o.amount) AS TotalRevenue
FROM customers c
INNER JOIN orders o
    ON c.customer_id=o.customer_id
GROUP BY c.category
ORDER BY TotalRevenue DESC;
```

### Explanation

- INNER JOIN combines customer and order data.
- SUM() calculates total revenue.
- GROUP BY creates one row per category.

### Expected Output

|Category|Revenue|
|----------|-------|
|Premium|xxxx|
|Standard|xxxx|
|Budget|xxxx|

### Interview Tip

Whenever you use an aggregate function with non-aggregate columns, GROUP BY is required.

---

## Summary

Topics Covered

- INNER JOIN
- LEFT JOIN
- Multiple JOINs
- Finding Missing Records
- Aggregation with JOIN

# Q6. Find Employee–Manager Relationships (Self JOIN)

### Difficulty

🟡 Intermediate

### Concept

A **Self JOIN** joins a table with itself. It is commonly used to represent hierarchical data like employee-manager relationships.

### Table Used

- employees

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

- `e` represents employees.
- `m` represents managers.
- Employees without a manager (CEO) will display NULL.

### Expected Output

|Employee|Manager|
|---------|--------|
|John|Robert|
|Alice|Robert|
|Robert|NULL|

### Interview Tip

Self JOIN is frequently asked in interviews for hierarchical data like:

- Employee → Manager
- Category → Parent Category
- Friend → Friend

---

# Q7. Find Matching Transactions Based on Amount and Date

### Difficulty

🔴 Advanced

### Concept

Join the same table to identify transactions having the same amount and transaction date.

### SQL Server Solution

```sql
SELECT
    t1.transaction_id AS Transaction1,
    t2.transaction_id AS Transaction2,
    t1.amount,
    t1.transaction_date
FROM transactions t1
INNER JOIN transactions t2
ON t1.amount = t2.amount
AND t1.transaction_date = t2.transaction_date
AND t1.transaction_id < t2.transaction_id;
```

### Explanation

The condition

```sql
t1.transaction_id < t2.transaction_id
```

prevents duplicate pairs.

Without it,

```
1 - 2
2 - 1
```

would both appear.

### Expected Output

|Transaction1|Transaction2|Amount|
|------------|------------|------|
|10|18|1500|
|22|25|750|

### Interview Tip

Whenever matching records from the same table are required,

Self JOIN is usually the solution.

---

# Q8. Compare Two Systems Using FULL OUTER JOIN

### Difficulty

🔴 Advanced

### Concept

Reconcile records coming from two different systems.

### Tables Used

- system1
- system2

### SQL Server Solution

```sql
SELECT
    ISNULL(s1.id, s2.id) AS ID,
    s1.amount AS System1Amount,
    s2.amount AS System2Amount
FROM system1 s1
FULL OUTER JOIN system2 s2
ON s1.id = s2.id
WHERE
    ISNULL(s1.amount,0) <> ISNULL(s2.amount,0)
    OR s1.id IS NULL
    OR s2.id IS NULL;
```

### Explanation

FULL OUTER JOIN returns

- Matching rows
- Left-only rows
- Right-only rows

Perfect for reconciliation reports.

### Expected Output

|ID|System1|System2|
|---|--------|--------|
|5|500|NULL|
|8|NULL|650|
|10|750|700|

### Performance Tip

FULL OUTER JOIN is generally slower than INNER JOIN because SQL Server must scan both tables.

---

# Q9. Generate Every Product-Color Combination

### Difficulty

🟢 Beginner

### Concept

Generate all possible combinations.

### Tables Used

- products
- colors

### SQL Server Solution

```sql
SELECT
    p.product_name,
    c.color
FROM products p
CROSS JOIN colors c
ORDER BY
    p.product_name,
    c.color;
```

### Explanation

CROSS JOIN creates a Cartesian Product.

If

Products = 10

Colors = 5

Result =

```
10 × 5 = 50 rows
```

### Expected Output

|Product|Color|
|--------|------|
|Laptop|Black|
|Laptop|Silver|
|Laptop|Blue|

### Interview Tip

Typical interview questions include:

- Generate combinations
- Calendar generation
- Matrix generation

---

# Q10. Customer Lifetime Value (CLV)

### Difficulty

🔴 Advanced

### Concept

Calculate

- Number of Orders
- Total Revenue
- Average Order Value

for each customer.

### SQL Server Solution

```sql
SELECT
    c.customer_id,
    c.name,
    COUNT(o.order_id) AS TotalOrders,
    ISNULL(SUM(o.amount),0) AS LifetimeValue,
    ISNULL(AVG(o.amount),0) AS AverageOrderValue
FROM customers c
LEFT JOIN orders o
ON c.customer_id = o.customer_id
GROUP BY
    c.customer_id,
    c.name
ORDER BY LifetimeValue DESC;
```

### Explanation

Using LEFT JOIN ensures customers with no orders are also included.

Aggregate functions calculate:

- COUNT()
- SUM()
- AVG()

### Expected Output

|Customer|Orders|Revenue|Average|
|---------|------|--------|--------|
|Alice|8|5600|700|
|Bob|5|2600|520|

### Performance Tip

For production databases:

- Create an index on `orders.customer_id`.
- Avoid `SELECT *`.
- Aggregate only the required columns.

---

# 🎯 JOIN Interview Cheat Sheet

| JOIN | Returns |
|------|----------|
| INNER JOIN | Matching rows only |
| LEFT JOIN | All left rows + matching right rows |
| RIGHT JOIN | All right rows + matching left rows |
| FULL OUTER JOIN | All rows from both tables |
| CROSS JOIN | Every possible combination |
| SELF JOIN | Table joined with itself |

---

# 💡 Most Asked JOIN Interview Questions

✅ Find customers without orders

✅ Find products never sold

✅ Employee–Manager hierarchy

✅ Customer Lifetime Value

✅ Revenue by category

✅ FULL OUTER JOIN reconciliation

✅ Generate all combinations

✅ Three-table JOIN

---

## Next Chapter

➡ **02_Group_By_and_Aggregations.md**

Questions **11–18**

Topics covered:

- GROUP BY
- HAVING
- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()
- Nested Aggregations
- Conditional Aggregations
