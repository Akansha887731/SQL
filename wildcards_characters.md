# SQL: Wildcard Characters

SQL wildcard characters are special symbols used with the **`LIKE`** and **`NOT LIKE`** operators to perform pattern-based searches on string data. They allow you to find partial matches rather than just exact ones.

---

# 📌 **Types of Wildcard Characters**

| **Wildcard** | **Description** | **Example Use Case** |
| --- | --- | --- |
| **`%`** | Matches **zero or more** characters. | **`'A%'`** matches any string starting with 'A' (e.g., Apple, A, Ant). |
| **`_`** | Matches **exactly one** character. | **`'H_t'`** matches 'Hot', 'Hat', 'Hut', but not 'Heat'. |
| **`[charlist]`** | Matches any single character within the specified **set or range**. | **`'[A-C]%'`** matches any string starting with 'A', 'B', or 'C'. |
| **`[^charlist]`** | Matches any single character **NOT** in the specified set or range. | **`'[^A-C]%'`** matches any string that does not start with 'A', 'B', or 'C'. |

# 💡 **Practical Examples**

Let's use a `Customer` table with `CustomerName` and `Phone` columns to illustrate.

- **Using `%`**:
    - Find customers whose name **starts with 'A'**: `SELECT * FROM Customer WHERE CustomerName LIKE 'A%';`
    - Find customers whose name **ends with 'A'**: `SELECT * FROM Customer WHERE CustomerName LIKE '%A';`
    - Find customers whose name **contains 'A'**: `SELECT * FROM Customer WHERE CustomerName LIKE '%A%';`
- **Using `_`**:
    - Find customers with a name that is **exactly five letters long**: `SELECT * FROM Customer WHERE CustomerName LIKE '_____';`
    - Find customers with a phone number that is **10 digits long and starts with '9'**: `SELECT * FROM Customer WHERE Phone LIKE '9_________';`
- **Combining `%` and `_`**:
    - Find phone numbers that **start with '8', have any two characters, then '5'**: `SELECT * FROM Customer WHERE Phone LIKE '8__5%';` This is useful for matching patterns in structured data, such as phone numbers or product codes.
- **A note on `[]` and `^`**: While the tutorial mentions `REGEXP` (`SELECT * FROM Customer WHERE LastName REGEXP '[A-C]';`), the `[]` A wildcard is a standard SQL feature in some database systems, such as MySQL and SQL Server. However, in others, you might need to use `REGEXP` or `RLIKE`. The `NOT LIKE` Operator is the standard SQL way to use the "not" logic. For example: `WHERE LastName NOT LIKE '%[y-z]%';`
