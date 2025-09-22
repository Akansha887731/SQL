# SQL: Window Functions

- Window functions in SQL perform calculations across a set of table rows related to the current row without collapsing the result into a single output row.
- They are defined by the **`OVER`** clause, which specifies the "window" or set of rows for the calculation. This clause can include **`PARTITION BY`** to divide rows into groups and **`ORDER BY`** to specify the order of rows within each group.
- The main types of window functions are **Aggregate Window Functions** and **Ranking Window Functions**.

---

# **Aggregate Window Functions**

These functions calculate aggregate values (like `SUM`, `AVG`, `COUNT`, `MAX`, `MIN`) over a defined window while still returning individual rows. This allows you to compare each row's value to the aggregate for its group.

- **Example: Calculating Average Salary per Department**
    
    ```sql
    SELECT Name, Age, Department, Salary, 
           AVG(Salary) OVER( PARTITION BY Department) AS Avg_Salary
     FROM employee
    ```
    
    | Name | Age | Department | Salary | Avg_Salary |
    | --- | --- | --- | --- | --- |
    | Ramesh | 20 | Finance | 50,000 | 40,000 |
    | Suresh | 22 | Finance | 50,000 | 40,000 |
    | Ram | 28 | Finance | 20,000 | 40,000 |
    | Deep | 25 | Sales | 30,000 | 25,000 |
    | Pradeep | 22 | Sales | 20,000 | 25,000 |

---

# **Ranking Window Functions**

These functions assign a rank to each row based on its value within a window.

- **`RANK()`:** Assigns a rank, with ties receiving the same rank, and the **next rank number is skipped**.
    
    ```sql
    SELECT Name, Department, Salary,
           RANK() OVER(PARTITION BY Department ORDER BY Salary DESC) AS emp_rank
    FROM employee;
    ```
    
    | **Name** | **Department** | **Salary** | **emp_rank** |
    | --- | --- | --- | --- |
    | Ramesh | Finance | 50,000 | 1 |
    | Suresh | Finance | 50,000 | 1 |
    | Ram | Finance | 20,000 | 3 |
    | Deep | Sales | 30,000 | 1 |
    | Pradeep | Sales | 20,000 | 2 |
- **`DENSE_RANK()`:** Assigns a rank, with ties receiving the same rank, **but no rank numbers are skipped.**
    
    ```sql
    SELECT Name, Department, Salary,
           DENSE_RANK() OVER(PARTITION BY Department ORDER BY Salary DESC) AS emp_dense_rank
    FROM employee;
    ```
    
    | **Name** | **Department** | **Salary** | **emp_dense_rank** |
    | --- | --- | --- | --- |
    | Ramesh | Finance | 50,000 | 1 |
    | Suresh | Finance | 50,000 | 1 |
    | Ram | Finance | 20,000 | 2 |
    | Deep | Sales | 30,000 | 1 |
    | Pradeep | Sales | 20,000 | 2 |
- **`ROW_NUMBER()`:** Assigns a unique, **sequential number to each row within a partition**. It does not account for ties.
    
    ```sql
    SELECT Name, Department, Salary,
           ROW_NUMBER() OVER(PARTITION BY Department ORDER BY Salary DESC) AS emp_row_no
    FROM employee;
    ```
    
    | **Name** | **Department** | **Salary** | **emp_row_no** |
    | --- | --- | --- | --- |
    | Ramesh | Finance | 50,000 | 1 |
    | Suresh | Finance | 50,000 | 2 |
    | Ram | Finance | 20,000 | 3 |
    | Deep | Sales | 30,000 | 1 |
    | Pradeep | Sales | 20,000 | 2 |
- **`PERCENT_RANK()`:** Returns the relative rank of a row as a percentage from 0 to 1. The formula is `(RANK - 1) / (Total Rows in Partition - 1)`.
    
    ```sql
    SELECT Name, Department, Salary,
           PERCENT_RANK() OVER(PARTITION BY Department ORDER BY Salary DESC) AS emp_percent_rank
    FROM employee;
    ```
    
    | **Name** | **Department** | **Salary** | **emp_percent_rank** |
    | --- | --- | --- | --- |
    | Ramesh | Finance | 50,000 | 0.00 |
    | Suresh | Finance | 50,000 | 0.00 |
    | Ram | Finance | 20,000 | 1.00 |
    | Deep | Sales | 30,000 | 0.00 |
    | Pradeep | Sales | 20,000 | 1.00 |

---

# **Common Pitfalls and Best Practices**

- **Performance:** Window functions can be computationally expensive on large datasets. Always ensure your queries are optimized.
- **`PARTITION BY` and `ORDER BY`:** Make sure these clauses correctly align with your desired calculation.
- Omitting **`PARTITION BY`** treats the entire dataset as a single window.
