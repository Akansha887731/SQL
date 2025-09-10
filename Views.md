# SQL: Views

- `CREATE VIEW` is a SQL statement used to create a **virtual table** based on the result of a `SELECT` query.
- A view doesn't store data itself; instead, it saves the query that defines it.
- Every time you query the view, the database runs the underlying query to present the latest data. This simplifies complex queries, improves security, and provides a layer of abstraction.

---

### **1. `CREATE VIEW` Syntax & Examples**

A view is created with the `CREATE VIEW` statement followed by the defining `SELECT` query.

- **Syntax:**
    
    ```sql
    CREATE VIEW view_name AS
    SELECT column1, column2, ...
    FROM table_name
    WHERE condition;
    ```
    
- **Simple Example:**
To create a view of products priced over $100:
    
    ```sql
    CREATE VIEW expensive_products AS
    SELECT product_id, product_name, price
    FROM products
    WHERE price > 100;
    ```
    
    This view now acts like a table, but it only contains the specified columns and filtered rows.
    
- **Joined View Example:**
You can create views from multiple tables using joins. This is a common use case for simplifying complex reports.SQL
    
    ```sql
    CREATE VIEW employee_department_info AS
    SELECT e.employee_id, e.first_name, d.department_name
    FROM employees e
    JOIN departments d ON e.department_id = d.department_id;
    ```
    
    This view lets you query employee names and their department names without needing to write the join every time.
    

---

### **2. `UPDATE`, `RENAME`, and `DROP` Views**

### **Updating a View**

Views can be updated in a few ways, but the rules can be complex depending on the view's definition.

- **Modifying View Data:** You can use the `UPDATE` statement on a view to change the data in the underlying base tables. However, this only works for simple views (not those with joins, aggregations, or subqueries).SQL
    
    ```sql
    UPDATE my_view
    SET column = 'new_value'
    WHERE condition;
    ```
    
- **Modifying View Definition:** To change a view's structure (e.g., add or remove a column, or change the `SELECT` query), use `CREATE OR REPLACE VIEW`. This command will create the view if it doesn't exist or replace its definition if it does.SQL
    
    ```sql
    CREATE OR REPLACE VIEW my_view AS
    SELECT new_column FROM other_table;
    ```
    

### **Renaming a View**

The method for renaming a view varies by database system.

- **SQL Server:** Use the `sp_rename` system procedure.SQL
    
    `EXEC sp_rename 'old_view_name', 'new_view_name';`
    
- **MySQL:** Use the `RENAME TABLE` command.SQL
    
    `RENAME TABLE old_view_name TO new_view_name;`
    
- **PostgreSQL:** Use the `ALTER VIEW` command.SQL
    
    `ALTER VIEW old_view_name RENAME TO new_view_name;`
    

### **Dropping a View**

To permanently delete a view, use the `DROP VIEW` command. This removes the view's definition but **does not affect the underlying data** in the source tables.

- **Syntax:**
    
    `DROP VIEW view_name;`
    
- **Example:**
    
    `DROP VIEW expensive_products;` 
    

# Summary

| **Operation** | **Purpose** | **Command(s)** |
| --- | --- | --- |
| **Create** | Make a new virtual table | `CREATE VIEW` |
| **Update** | Modify underlying data | `UPDATE` |
| **Replace** | Change the view's query | `CREATE OR REPLACE VIEW` |
| **Rename** | Change the view's name | `sp_rename` (SQL Server), `RENAME TABLE` (MySQL), `ALTER VIEW` (PostgreSQL) |
| **Drop** | Delete the view's definition | `DROP VIEW` |

---

# **Why Use a View?**

- **Simplification:** Views abstract complex queries (like those with multiple joins or subqueries) into a simple, single virtual table.
- **Security:** You can restrict user access to a view that only shows specific columns or rows, protecting sensitive data in the underlying tables.
- **Abstraction:** Views can hide the complexity of the database schema, making it easier for users and applications to interact with the data without needing to know the table relationships.
