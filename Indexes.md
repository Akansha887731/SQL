# SQL: Indexes

- SQL indexes are **data structures** that improve the speed of data retrieval operations, making queries faster.
- They work like a book's index, allowing the database to jump directly to the relevant data without scanning the entire table.

---

# **1. Creating an Index**

You can create an index on a single column or multiple columns. Primary keys and unique constraints automatically create indexes.

- **Single-Column Index:**
    - **Purpose:** Speeds up searches and sorts on a single column.
    - **Syntax:** `CREATE INDEX index_name ON table_name (column_name);`
    - **Example:** `CREATE INDEX idx_product_id ON Sales (product_id);`
- **Multi-Column (Composite) Index:**
    - **Purpose:** Speeds up queries that filter or sort on a combination of two or more columns. The order of the columns in the index matters.
    - **Syntax:** `CREATE INDEX index_name ON table_name (column1, column2, ...);`
    - **Example:** `CREATE INDEX idx_product_quantity ON Sales (product_id, quantity);`
- **Unique Index:**
    - **Purpose:** Ensures all values in a column or set of columns are unique. This is a crucial tool for data integrity, preventing duplicate entries.
    - **Syntax:** `CREATE UNIQUE INDEX index_name ON table_name (column_name);`
    - **Example:** `CREATE UNIQUE INDEX idx_unique_customer_id ON Sales (customer_id);`
    - **Note:** If you try to insert a duplicate value after creating this index, the database will return an error.

---

# **2. Managing Indexes**

- **Viewing an Index:** The command to view indexes varies by database system. It's a key way to audit your indexing strategy.
    - **MySQL:** `SHOW INDEXES FROM table_name;`
    - **SQL Server:** `SELECT * FROM sys.indexes WHERE object_id = OBJECT_ID('table_name');`
    - **PostgreSQL:** `SELECT * FROM pg_indexes WHERE tablename = 'table_name';`
- **Removing an Index:** To delete an index and free up storage and reduce write overhead, use the `DROP INDEX` statement.
    - **Syntax:** `DROP INDEX index_name;` (for Oracle, DB2) or `ALTER TABLE table_name DROP INDEX index_name;` (for MySQL).
    - **Important:** You cannot drop an index that was created by a primary key or unique constraint without first dropping the constraint itself.
- **Altering an Index:** In some cases, you may want to alter an index to optimize its performance.
    - **Syntax:** `ALTER INDEX IndexName ON TableName REBUILD;`
    - **Purpose:** This rebuilds the index's internal structure to improve efficiency, which can be useful after many updates or deletions to the underlying data.

---

# **When to Use Indexes**

Indexes are great, but they aren't a silver bullet. Use them strategically.

- **`WHERE` Clauses:** Index columns that are frequently used to filter data.
- **`JOIN` Conditions:** Index the columns used to join tables together.
- **`ORDER BY` and `GROUP BY`:** Index columns used for sorting or grouping.
- **High Cardinality:** Indexes work best on columns with a high number of unique values (e.g., a `user_id` column is a good candidate, but a `gender` column is not). (**Index selectivity:** which is the ratio of unique values to the total number of rows. This is a key factor in determining if an index will be effective.)

# **When to Avoid Indexes**

- **Write-Heavy Tables:** Indexes add overhead to `INSERT`, `UPDATE`, and `DELETE` operations because the database must also update the index structure.
    - Explanation: Over time, as data is inserted, updated, and deleted, an index can become **fragmented**. This means its physical order is no longer logical, which can slow down queries. Rebuilding an index reorganizes its physical structure to be more efficient, especially in heavily modified tables.
- **Small Tables:** The performance benefit of an index on a small table is negligible and doesn't justify the storage and maintenance costs.
