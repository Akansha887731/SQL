# SQL: Tables

# CREATE TABLE

### `CREATE TABLE`

- **Syntax:** `CREATE TABLE table_name (column1 datatype, column2 datatype, ...);`
- **Purpose:** To define how data is stored, ensuring accuracy and consistency.
- **Components:**
    - **Table Name:** The name you assign to the table.
    - **Column Name:** The name of each data field.
    - **Data Type:** Defines the type of data a column can hold (e.g., `INT` for numbers, `VARCHAR` for text).
    - **Constraints:** Rules that enforce data integrity, such as:
        - `PRIMARY KEY`: Ensures each record is unique.
        - `CHECK`: Validates data to ensure it meets a specific condition (e.g., `Age >= 0`).
        - `NOT NULL`: Ensures a column must contain a value.

### Examples

- **Creating a new table:**
    
    ```sql
    CREATE TABLE Customer (
        CustomerID INT PRIMARY KEY,
        CustomerName VARCHAR(50)
    );
    ```
    
- **Creating a table from an existing one:**
    - The `CREATE TABLE AS SELECT` command is used to duplicate the structure and data from another table.
    - **Syntax:** `CREATE TABLE new_table AS SELECT columns FROM existing_table;`
    - **Example:** `CREATE TABLE SubTable AS SELECT CustomerID FROM Customer;`

### Important Tips

- **Avoid Errors:** Use `CREATE TABLE IF NOT EXISTS table_name (...);` to prevent an error if the table already exists.
- **Performance:** Always define appropriate data types and sizes to optimize storage and performance.
- **Verify:** Use the `DESC table_name;` command to view a table's structure after creation.
- **Modify:** Use the `ALTER TABLE` command to change a table's structure after it has been created.

# DROP TABLE

### Key Concepts

- **Command:** `DROP TABLE`
- **Irreversible Action:** This command permanently deletes the table and all its data. **It cannot be undone**, so use it with extreme caution.
- **Syntax:**
    - `DROP TABLE table_name;`
- **Best Practice:** Use `DROP TABLE IF EXISTS table_name;` to prevent an error if the table you are trying to delete doesn't exist. This is a common practice in scripts.
- **Associated Objects:** When a table is dropped, all of its components are also removed, including:
    - All data stored in the table.
    - Indexes.
    - Constraints (`PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL`, etc.).
    - Triggers and permissions.
- **Temporary Tables:** The `DROP TEMPORARY TABLE` command is used to specifically delete temporary tables, which are session-specific.
- **Verification:** You can confirm a table's deletion by using `SHOW TABLES;` (in MySQL) or by querying the database's information schema.

# ALTER TABLE

The `ALTER TABLE` command is a crucial DDL (Data Definition Language) statement used to modify the structure of an existing table without losing its data.

### Key Use Cases and Syntax

- **Adding a Column:** Use `ADD` to add a new column and its data type.
    - `ALTER TABLE table_name ADD column_name datatype;`
    - Initially, the new column will be `NULL` for all existing rows.
- **Modifying a Column:** Use `MODIFY COLUMN` (or `ALTER COLUMN` in some systems) to change a column's data type or size.
    - `ALTER TABLE table_name MODIFY COLUMN column_name new_datatype;`
    - **Note:** If you reduce a column's size, data that exceeds the new limit may be truncated or cause an error.
- **Renaming a Column:** Use `RENAME COLUMN` to change a column's name.
    - `ALTER TABLE table_name RENAME COLUMN old_name TO new_name;`
    - **Note:** In MySQL, you might need to use `CHANGE COLUMN` and re-specify the data type.
- **Renaming a Table:** Use `RENAME TO` to change a table's name.
    - `ALTER TABLE table_name RENAME TO new_table_name;`
- **Dropping a Column:** Use `DROP COLUMN` to permanently remove a column and all its data.
    - `ALTER TABLE table_name DROP COLUMN column_name;`
    - **Warning:** This action is irreversible.

### Important Considerations

- **Schema Evolution:** `ALTER TABLE` is the command for **schema evolution**, allowing you to adapt your database structure to changing business requirements.
- **Production Impact:** Be cautious when using this command on a production table. `ALTER` operations can be slow and may lock the table, temporarily blocking reads and writes from your applications and data pipelines. It's best to perform these operations during off-peak hours.
- **Dependencies:** When you drop or rename a column, you must update any dependent views, stored procedures, and application code to avoid breaking them.

# TRUNCATE TABLE

The `TRUNCATE TABLE` Command is a fast way to remove all rows from a table while keeping the table's structure intact. It's an essential tool for a data engineer to quickly clear data, especially in staging tables for data loading.

---

### Key Concepts

- **Command:** `TRUNCATE TABLE table_name;`
- **Purpose:** To quickly remove all data from a table, leaving its definition (columns, data types, constraints) as is.
- **Performance:** `TRUNCATE` is significantly faster than `DELETE FROM table_name;` for a full table cleanup. It works by deallocating the table's data pages instead of deleting rows one by one.
- **Identity Reset:** It typically resets the identity (auto-increment) counter back to its starting value.

### `TRUNCATE` vs. `DELETE`

| Feature | `TRUNCATE TABLE` | `DELETE FROM` |
| --- | --- | --- |
| **Operation** | Removes all rows | Removes rows based on a `WHERE` clause or all rows if no condition is specified |
| **Transaction Logging** | Minimal logging | Fully logged (slower) |
| **Rollback** | Generally cannot be rolled back | Can be rolled back if within a transaction |
| **`WHERE` Clause** | Not supported | Supported |
| **Performance** | Much faster for large tables | Slower, especially for large datasets |
| **Triggers** | Does not fire triggers | Fires triggers |
| **Foreign Key Constraints** | Cannot truncate a table referenced by a foreign key (without disabling the constraint) | Can delete rows in a table referenced by a foreign key |

### `TRUNCATE` vs. `DROP`

| Feature | `TRUNCATE TABLE` | `DROP TABLE` |
| --- | --- | --- |
| **Table Structure** | Retained | Deleted (table, data, and structure are all removed) |
| **Purpose** | To empty a table | To completely remove a table from the database |
| **Recovery** | Data is lost; cannot be recovered without a backup | The entire table is lost; cannot be recovered without a backup |
| **Triggers** | Triggers are not fired. | Not applicable, as the table no longer exists. |
| **Foreign Key Constraints** | Cannot truncate a table if it is referenced by a foreign key. | Cannot drop a table if other tables reference it unless the foreign key constraint is removed first. |
| **Permissions Required** | Requires `ALTER` permission on the table. | Requires `DROP` permission on the table. |
| **Speed** | Generally faster than `DELETE` since it deallocates data pages. | Fast operation since it removes both data and structure. |

# SQL TABLE CLONING/COPYING

**Cloning** or **copying a table** in SQL means creating a duplicate of an existing table. This can include just the table's structure or both the structure and its data. It's a key task for backups, testing, and development.

---

### 1. Simple Cloning

- **Concept:** This method copies the table's structure and all its data.
- **Key Behavior:** **Does NOT preserve constraints**, such as `PRIMARY KEY`, `UNIQUE`, or `AUTO_INCREMENT`.
- **Drawback:** It can lead to data integrity issues because constraints are lost.
- **Syntax:** `CREATE TABLE new_table AS SELECT * FROM original_table;`

---

### 2. Shallow Cloning

- **Concept:** This method copies only the table's structure, but **no data**.
- **Key Behavior:** **Preserves** all constraints, including `PRIMARY KEY`, `UNIQUE`, `NOT NULL`, and `AUTO_INCREMENT`.
- **Use Case:** Ideal for creating an empty table with the same structure and rules as the original, ready for new data.
- **Syntax:** `CREATE TABLE new_table LIKE original_table;`

---

### 3. Deep Cloning

- **Concept:** This method is the most comprehensive. It copies both the table's structure and its data, while **preserving all constraints**.
- **Key Behavior:** The new table is an exact duplicate of the original in every way.
- **Syntax:** This is a two-step process:
    1. First, perform a shallow clone to copy the structure and constraints.
    `CREATE TABLE new_table LIKE original_table;`
    2. Second, insert all the data from the original table into the new table.
    `INSERT INTO new_table SELECT * FROM original_table;`

# Temporary tables in SQL

A **temporary table** is a special table that stores data for a short period, ideal for holding intermediate results of complex queries or transformations without affecting permanent tables.

---

### Local Temporary Table (`#`)

- **Syntax:** `CREATE TABLE #table_name (...)`
- **Scope:** The table is local to the session that created it and is only accessible by that session.
- **Lifetime:** It's automatically dropped when the session or the stored procedure that created it ends.
- **Example:**
    
    ```sql
    CREATE TABLE #EmpDetails (id INT, name VARCHAR(25));
    INSERT INTO #EmpDetails VALUES (1, 'Lalit'), (2, 'Atharva');
    SELECT * FROM #EmpDetails;
    ```
    
    This table will be deleted once the user's connection to the database is closed.
    

---

### Global Temporary Table (`##`)

- **Syntax:** `CREATE TABLE ##table_name (...)`
- **Scope:** The table is global and is visible to all connections and sessions on the database.
- **Lifetime:** It's automatically dropped when the last session referencing the table is closed.
- **Example:**
    
    ```sql
    CREATE TABLE ##EmpDetails (id INT, name VARCHAR(25));
    ```
    
    Any user session can access this table until all sessions that have used it are closed.
    

### Common Usage

Temporary tables are ideal for tasks such as:

- **Storing intermediate results** of complex calculations.
- **Improving query performance** by breaking down a large query into smaller, more manageable steps.
- **Performing data manipulation and transformations** without affecting the permanent tables.
