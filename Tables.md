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
