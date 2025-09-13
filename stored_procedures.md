# SQL: Stored Procedures

# Stored Procedures

- SQL Stored Procedures are precompiled SQL statements stored in a database to perform a specific task.
- They are a powerful feature that enhances performance, security, and code reusability.

# **Key Concepts**

- **What it is:** A collection of SQL statements bundled together to perform a specific task.
- **Syntax:**
    
    ```sql
    CREATE PROCEDURE procedure_name
    (parameter1 data_type, ...)
    AS
    BEGIN
    -- SQL statements
    END
    ```
    

# **Types of Stored Procedures**

- **System:** Predefined procedures for administrative tasks (e.g., `sp_help`, `sp_rename` ).
- **User-Defined (UDPs):** Custom procedures created by a user for specific operations.
- **Extended:** Allow the execution of external functions (e.g., C/C++).
- **CLR:** Written in .NET languages (e.g., C#) for advanced functionality.

# **Why Use Them?**

- **Performance:** They are precompiled, leading to faster execution.
- **Security:** Users can execute procedures without having direct access to the underlying tables.
- **Code Reusability:** They simplify maintenance and reduce redundant code.
- **Reduced Network Traffic:** They bundle multiple statements into a single call.

# **Example: GetCustomersByCountry**

- **Purpose:** This example demonstrates how to retrieve customer information based on a country parameter.
- **Query:**
    
    ```sql
    CREATE PROCEDURE GetCustomersByCountry
    @Country VARCHAR(50)
    AS
    BEGIN
    SELECT CustomerName, ContactName FROM Customers WHERE Country = @Country;
    END;
    -- Execution
    EXEC GetCustomersByCountry @Country = 'Sri lanka';
    ```
    

# **Best Practices**

- **Keep it Simple:** Use modular procedures.
- **Error Handling:** Use `TRY...CATCH` blocks.
- **Limit Cursors:** Prefer set-based operations over cursors.
- **Avoid Hardcoding:** Use parameters for flexibility.
