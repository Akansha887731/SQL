# SQL Clauses and Aliases

### `WHERE` Clause

- **Purpose:** Filters rows based on specific conditions before any grouping or aggregation.
- **Syntax:** `SELECT ... FROM table WHERE condition;`
- **Operators:** Use with comparison (`=, >, <, <>`), logical (`AND`, `OR`, `NOT`), range (`BETWEEN`), pattern matching (`LIKE`), or set membership (`IN`) operators.
- **Key Concept:** The `WHERE` clause works on **individual rows**.

---

### `GROUP BY` Clause

- **Purpose:** Groups rows with the same values into summary rows. It's almost always used with **aggregate functions** like `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()`.
- **Syntax:** `SELECT column, aggregate_function FROM table GROUP BY column;`
- **Execution Order:** The `WHERE` clause runs before `GROUP BY`. You cannot use an aggregate function in a `WHERE` clause.

---

### `HAVING` Clause

- **Purpose:** Filters the results of `GROUP BY` after aggregation has occurred. It's the equivalent of `WHERE` for grouped data.
- **Syntax:** `SELECT ... FROM table GROUP BY ... HAVING condition;`
- **Key Concept:** `HAVING` works on **grouped rows** or aggregated results, not individual rows.

---

### `ORDER BY` Clause

- **Purpose:** Sorts the final result set in either ascending (`ASC`) or descending (`DESC`) order. `ASC` is the default.
- **Syntax:** `SELECT ... FROM table ORDER BY column1 [ASC/DESC], column2 [ASC/DESC];`
- **Best Practice:** Always use a column name instead of its position number to make your code more readable and maintainable.

---

### `LIMIT`, `TOP`, and `FETCH FIRST` Clauses

These clauses restrict the number of rows returned by a query. The one you use depends on your database system.

- **`LIMIT` Clause:**
    - **Supported by:** MySQL, PostgreSQL, SQLite.
    - **Purpose:** Restricts the number of rows. Can use an optional `OFFSET` to skip rows.
    - **Syntax:** `SELECT ... FROM table LIMIT row_count [OFFSET offset_value];`
    - **Use Cases:** Ideal for pagination and fetching top-N records.
- **`TOP` Clause:**
    - **Supported by:** SQL Server, MS Access.
    - **Purpose:** Retrieves a specified number or percentage of rows.
    - **Syntax:** `SELECT TOP number [PERCENT] ... FROM table;`
- **`FETCH FIRST` Clause:**
    - **Supported by:** Oracle, DB2, PostgreSQL, SQL Server (`OFFSET-FETCH`).
    - **Purpose:** Fetches a specified number of rows. Part of the standard SQL syntax.
    - **Syntax:** `SELECT ... FROM table FETCH FIRST n ROWS ONLY;`

---

### `DISTINCT` Clause

- **Purpose:** Removes duplicate rows from a result set.
- **Syntax:** `SELECT DISTINCT column1, column2 FROM table;`
- **Key Concept:** When used on multiple columns, it returns unique **combinations** of values across all selected columns.

---

### `WITH` Clause (Common Table Expressions - CTEs)

- **Purpose:** Simplifies complex queries by defining a temporary, reusable result set.
- **Syntax:**
    
    ```sql
    WITH temp_table_name AS (
        SELECT ...
    )
    SELECT ... FROM temp_table_name;
    ```
    
- **Key Benefits:**
    - **Readability:** Breaks down complex logic into manageable steps.
    - **Reusability:** Prevents you from writing the same subquery multiple times.
    - **Performance:** The database engine can optimize the CTE, calculating the intermediate result only once.
- **Note:** CTEs are temporary and only exist for the duration of the query.

---

### SQL Aliases

Aliases are temporary names given to columns or tables within a query to make it more readable and efficient.

- **Column Aliases:** Renames a column in the output.
    - **Syntax:** `SELECT column_name AS alias_name FROM table_name;`
- **Table Aliases:** Provides a temporary, shorthand name for a table. Essential for multi-table queries and self-joins.
    - **Syntax:** `SELECT ... FROM table_name AS alias_name ...;`
- **Summary:** Aliases improve **readability**, **clarity**, and **simplicity** and help **avoid naming conflicts**.
