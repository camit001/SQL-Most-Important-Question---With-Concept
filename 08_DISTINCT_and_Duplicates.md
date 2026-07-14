# 📌 Pattern 8: DISTINCT & Duplicates (Questions 57–61)

Duplicate records are one of the most common real-world data quality problems. SQL Server provides several techniques to identify, count, and remove duplicate data.

## Topics Covered

- **DISTINCT** – Returns only unique rows by removing duplicate values from the result set.
- **COUNT(DISTINCT)** – Returns the count of unique (non-duplicate) values in a column.
- **Duplicate Detection** – Identifies duplicate records by grouping data and finding values with a count greater than one.
- **ROW_NUMBER()** – Assigns a unique sequential number to each row, commonly used to identify duplicate records.
- **DELETE Duplicates** – Removes duplicate rows while retaining one record, typically using a CTE with `ROW_NUMBER()`.
- **Duplicate Analysis** – Analyzes duplicate records to determine their count, frequency, and occurrence patterns before taking corrective action.

---

# Q57. Display Unique Department Names

### Difficulty

🟢 Beginner

### Concept

Retrieve unique department names from the Employees table.

### SQL Server Solution

```sql
SELECT DISTINCT department
FROM employees
ORDER BY department;
```

### Explanation

DISTINCT removes duplicate values.

### Expected Output

|Department|
|----------|
|Finance|
|HR|
|IT|
|Marketing|

### Interview Tip

DISTINCT removes duplicate rows, not duplicate columns.

---

# Q58. Count Total Unique Departments

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
COUNT(DISTINCT department) AS TotalDepartments
FROM employees;
```

### Explanation

COUNT(DISTINCT)

counts only unique values.

### Expected Output

|TotalDepartments|
|----------------|
|4|

---

# Q59. Find Duplicate Customer Records

### Difficulty

🟡 Intermediate

### Concept

Identify customers appearing more than once.

### SQL Server Solution

```sql
SELECT
name,
COUNT(*) AS DuplicateCount
FROM customers
GROUP BY name
HAVING COUNT(*) > 1;
```

### Explanation

GROUP BY groups identical names.

HAVING filters groups with duplicates.

### Expected Output

|Customer|Count|
|---------|-----|
|John Smith|2|
|Alice Brown|3|

### Interview Tip

One of the most frequently asked SQL interview questions.

---

# Q60. Retrieve Complete Duplicate Records

### Difficulty

🟡 Intermediate

### Concept

Display the complete duplicate rows instead of just the duplicate names.

### SQL Server Solution

```sql
WITH DuplicateCustomers AS
(
    SELECT
        customer_id,
        name,
        email,
        ROW_NUMBER() OVER
        (
            PARTITION BY name, email
            ORDER BY customer_id
        ) AS RN
    FROM customers
)
SELECT *
FROM DuplicateCustomers
WHERE RN > 1;
```

### Explanation

ROW_NUMBER()

assigns sequential numbers within each duplicate group.

Rows having

```
RN > 1
```

are duplicate records.

### Expected Output

|Customer ID|Name|Email|
|-----------|----|-----|
|15|John Smith|john@gmail.com|

---

# Q61. Delete Duplicate Records While Keeping One Copy

### Difficulty

🔴 Advanced

### Concept

Remove duplicate rows but keep the first occurrence.

### SQL Server Solution

```sql
WITH DuplicateRows AS
(
    SELECT
        customer_id,
        ROW_NUMBER() OVER
        (
            PARTITION BY
                name,
                email
            ORDER BY customer_id
        ) AS RN
    FROM customers
)
DELETE
FROM DuplicateRows
WHERE RN > 1;
```

### Explanation

ROW_NUMBER()

assigns

```
1
```

to the first occurrence.

All rows greater than

```
1
```

are deleted.

### ⚠️ Production Tip

Always verify duplicates before deleting.

```sql
SELECT *
FROM DuplicateRows
WHERE RN > 1;
```

---

# 🎯 DISTINCT & Duplicate Cheat Sheet

| Function | Purpose |
|----------|---------|
| DISTINCT | Remove duplicate rows |
| COUNT(DISTINCT) | Count unique values |
| GROUP BY | Group duplicates |
| HAVING | Filter duplicate groups |
| ROW_NUMBER() | Identify duplicate records |
| DELETE + CTE | Remove duplicates |

---

# ⭐ Most Asked Interview Questions

✅ Find Duplicate Records

✅ Remove Duplicate Records

✅ Count Distinct Values

✅ Find Duplicate Emails

✅ Find Duplicate Customer Names

✅ Keep Latest Record

---

# 💡 Interview Tips

### DISTINCT

Returns only unique rows.

```sql
SELECT DISTINCT department
FROM employees;
```

---

### COUNT(DISTINCT)

```sql
SELECT COUNT(DISTINCT department)
FROM employees;
```

---

### Duplicate Detection Pattern

```sql
SELECT
column_name,
COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > 1;
```

---

### Best Method to Remove Duplicates

```sql
ROW_NUMBER()
+
CTE
+
DELETE
```

This is the preferred approach in SQL Server interviews and production environments.

---

## Next Chapter

➡ **09_HAVING_vs_WHERE.md (Questions 62–66)**

Topics Covered

- WHERE
- HAVING
- Aggregate Filtering
- Multiple Conditions
- Conditional Aggregation
```
