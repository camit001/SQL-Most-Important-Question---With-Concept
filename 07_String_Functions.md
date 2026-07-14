# 🔤 Pattern 7: String Functions (Questions 51–56)

String functions are commonly used for data cleaning, formatting, validation, and ETL transformations. They are frequently asked in SQL Server interviews.

## Topics Covered

- **UPPER()** – Converts all characters in a string to uppercase.
- **LOWER()** – Converts all characters in a string to lowercase.
- **LEN()** – Returns the number of characters in a string (excluding trailing spaces).
- **LEFT()** – Returns a specified number of characters from the beginning (left side) of a string.
- **RIGHT()** – Returns a specified number of characters from the end (right side) of a string.
- **SUBSTRING()** – Extracts a specified portion of a string starting from a given position.
- **CHARINDEX()** – Returns the starting position of a specified substring within a string.
- **CONCAT()** – Combines two or more strings into a single string.
- **REPLACE()** – Replaces all occurrences of a specified substring with another substring.
- **TRIM()** – Removes leading and trailing spaces (or specified characters in SQL Server 2022+) from a string.

---

# Q51. Convert Customer Names to Uppercase

### Difficulty

🟢 Beginner

### Concept

Convert all customer names to uppercase.

### SQL Server Solution

```sql
SELECT
    customer_id,
    name,
    UPPER(name) AS UpperCaseName
FROM customers;
```

### Explanation

UPPER() converts all alphabetic characters to uppercase.

### Expected Output

|Original|Uppercase|
|---------|----------|
|Alice|ALICE|
|John|JOHN|

### Interview Tip

Frequently used while standardizing imported data.

---

# Q52. Convert Customer Names to Lowercase

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    customer_id,
    name,
    LOWER(name) AS LowerCaseName
FROM customers;
```

### Explanation

LOWER() converts every character to lowercase.

### Expected Output

|Original|Lowercase|
|---------|----------|
|ALICE|alice|
|JOHN|john|

---

# Q53. Find the Length of Customer Names

### Difficulty

🟢 Beginner

### SQL Server Solution

```sql
SELECT
    customer_id,
    name,
    LEN(name) AS NameLength
FROM customers;
```

### Explanation

LEN() returns the number of characters excluding trailing spaces.

### Expected Output

|Name|Length|
|----|------|
|Alice|5|
|Christopher|11|

### Interview Tip

LEN() ignores trailing spaces.

---

# Q54. Extract the Email Domain

### Difficulty

🟡 Intermediate

### Concept

Extract everything after '@' from an email address.

### SQL Server Solution

```sql
SELECT
    email,
    SUBSTRING
    (
        email,
        CHARINDEX('@', email) + 1,
        LEN(email)
    ) AS EmailDomain
FROM customers;
```

### Explanation

- CHARINDEX() finds the position of '@'
- SUBSTRING() extracts everything after it

### Expected Output

|Email|Domain|
|------|------|
|john@gmail.com|gmail.com|
|alice@yahoo.com|yahoo.com|

---

# Q55. Replace '.com' with '.org'

### Difficulty

🟡 Intermediate

### SQL Server Solution

```sql
SELECT
    email,
    REPLACE(email,'.com','.org') AS UpdatedEmail
FROM customers;
```

### Explanation

REPLACE()

searches for a string and replaces it with another string.

### Expected Output

|Original|Updated|
|---------|-------|
|abc@gmail.com|abc@gmail.org|

---

# Q56. Display Customer Initials

### Difficulty

🔴 Advanced

### Concept

Generate initials from the customer's first and last name.

### SQL Server Solution

```sql
SELECT
    name,
    CONCAT
    (
        LEFT(name,1),
        '.',
        LEFT
        (
            SUBSTRING
            (
                name,
                CHARINDEX(' ',name)+1,
                LEN(name)
            ),
            1
        )
    ) AS Initials
FROM customers
WHERE CHARINDEX(' ',name) > 0;
```

### Explanation

Example

```
John Smith

↓

J.S
```

The WHERE clause excludes single-word names.

### Alternative Solution

```sql
SELECT
    name,
    LEFT(name,1) + '.' +
    LEFT
    (
        SUBSTRING(name,
        CHARINDEX(' ',name)+1,
        LEN(name)),
        1
    ) AS Initials
FROM customers
WHERE CHARINDEX(' ',name) > 0;
```

---

# 🎯 String Function Cheat Sheet

| Function | Purpose |
|----------|---------|
| UPPER() | Convert to Uppercase |
| LOWER() | Convert to Lowercase |
| LEN() | Count Characters |
| LEFT() | Extract Left Characters |
| RIGHT() | Extract Right Characters |
| SUBSTRING() | Extract Part of String |
| CHARINDEX() | Find Position |
| REPLACE() | Replace Text |
| CONCAT() | Join Strings |
| TRIM() | Remove Leading & Trailing Spaces |

---

# ⭐ Most Asked Interview Questions

✅ Convert Names to Uppercase

✅ Convert Names to Lowercase

✅ Count Characters

✅ Extract Email Domain

✅ Replace Characters

✅ Generate Initials

✅ Remove Extra Spaces

---

# 💡 Interview Tips

### CHARINDEX()

Returns the starting position of a substring.

```sql
SELECT CHARINDEX('@','john@gmail.com');
```

Output

```
5
```

---

### SUBSTRING()

```sql
SUBSTRING(column,start,length)
```

Example

```sql
SELECT SUBSTRING('Microsoft',1,5);
```

Output

```
Micro
```

---

### CONCAT()

```sql
SELECT CONCAT(first_name,' ',last_name)
```

Automatically handles NULL values.

---

### TRIM()

```sql
SELECT TRIM('    SQL Server    ');
```

Output

```
SQL Server
```

---

## Next Chapter

➡ **[DISTINCT & Duplicates](08_DISTINCT_and_Duplicates.md) (Questions 57–61)**

Topics Covered

- DISTINCT
- Duplicate Records
- COUNT(DISTINCT)
- Duplicate Detection
- Removing Duplicates
```
