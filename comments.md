# SQL: Comments

# **Types of SQL Comments**

1. **Single-line Comments**:
    - **Syntax**: Begin with `-`.
    - **Example**:
        
        ```sql
        - This query fetches all customer records
        SELECT * FROM customers;
        ```
        
2. **Multi-line Comments**:
    - **Syntax**: Enclosed between `/*` and `/`.
    - **Example**:
        
        ```sql
        /*
        This query retrieves orders placed
        in the year 2022 to analyze yearly sales trends.
        */
        SELECT *
        FROM orders
        WHERE YEAR(order_date) = 2022;
        ```
        
3. **In-line Comments**:
    - **Syntax**: Enclosed between `/*` and `/`.
    
    ```sql
    SELECT customer_name, /* customer's full name */
           order_date /* date the order was placed */
    FROM orders;
    ```
    

# **Best Practices**

- **Explain "Why," Not "What"**: Don't just re-state the obvious (e.g., `SELECT * FROM customers;` -- This selects customers). Instead, explain the reason behind complex logic or a business rule.
- **Be Concise**: Keep comments short and to the point.
- **Use for Debugging**: Comments are an easy way to "comment out" problematic or non-essential parts of a query during the testing and debugging process.
