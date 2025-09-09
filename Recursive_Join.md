# Recursive Join

### **What is a Recursive Join?**

A **recursive join** is a technique used in SQL to query **hierarchical data**, which is data that has a parent-child or nested relationship within the same table. It is implemented using a **recursive Common Table Expression (CTE)** and allows you to repeatedly join a table to itself to traverse a chain of related records.

---

### **How It Works: The Two-Part Query**

A recursive CTE consists of two main parts, combined with a `UNION ALL` operator.

1. **Anchor Member:** This is the non-recursive part. It serves as the starting point for the query, selecting the "root" or top-level row(s) of your hierarchy.
2. **Recursive Member:** This part joins the original table with the CTE itself. It takes the rows from the previous step and finds the next level of related data. This process repeats until no new rows are found, effectively traversing the entire hierarchy.

---

### **Syntax**

The syntax for a recursive join is a `WITH` clause that includes the `RECURSIVE` keyword.

```sql
WITH RECURSIVE cte_name AS (
    -- Anchor Member (the starting point)
    SELECT columns FROM table WHERE condition
    
    UNION ALL
    
    -- Recursive Member (the part that finds the next level)
    SELECT t.columns
    FROM table t
    JOIN cte_name AS cte
    ON t.column = cte.column
)
SELECT * FROM cte_name;
```

---

### **Example: Employee-Manager Hierarchy**

A classic example is an employee-manager reporting structure, where each employee record has a `manager_id` that links to another employee's `employee_id`.

`employees`  table

| **employee_id** | **employee_name** | **manager_id** | **age** |
| --- | --- | --- | --- |
| 1 | Ankit | NULL | 32 |
| 2 | Ayush | 1 | 31 |
| 3 | Piyush | 1 | 42 |
| 4 | Ramesh | 2 | 31 |
| 5 | Rohan | 3 | 29 |
| 6 | Harry | 3 | 28 |
| 7 | Rohit | 4 | 32 |
| 8 | Gogi | 4 | 32 |
| 9 | Tapu | 5 | 33 |
| 10 | Sonu | 5 | 40 |

**Goal:** Retrieve a list of all employees and their managers, starting from a specific employee and going up the chain.

- **Query:**
    
    ```sql
    WITH RECURSIVE employee_hierarchy AS (
        -- Anchor: Select the root employee (e.g., employee_id = 1)
        SELECT employee_id, employee_name, manager_id
        FROM employees
        WHERE employee_id = 1
    
        UNION ALL
    
        -- Recursive part: Join the employees table with the CTE to find managers
        SELECT e.employee_id, e.employee_name, e.manager_id
        FROM employees e
        INNER JOIN employee_hierarchy eh ON e.manager_id = eh.employee_id
    )
    SELECT * FROM employee_hierarchy;
    ```
    
- **Step-by-step Execution:**
    1. **Anchor:** The query first finds Ankit (`employee_id = 1`) and puts his record into the `employee_hierarchy` CTE.
    2. **First Recursion:** It then looks for all employees where `manager_id` equals `1` (Ankit's `employee_id`). These are Ayush and Piyush. It adds their records to the CTE.
    3. **Second Recursion:** The query now looks for all employees where `manager_id` equals `2` or `3` (Ayush and Piyush's `employee_id`). This finds Ramesh and Rohan. These are added to the CTE.
    4. **Third Recursion:** It continues this process, finding the employees who report to Ramesh and Rohan, and so on.
    5. **Termination:** The process stops when the `INNER JOIN` in the recursive part returns no new rows, meaning the entire reporting hierarchy has been traversed.
- **How it Works:** The anchor member starts with the employee `1`. The recursive member then finds all employees who have employee `1` as their manager, adds them to the result set, and repeat the process for the newly found employees until the entire reporting chain is listed.

---

### **Key Applications**

Recursive joins are an essential tool for:

- **Hierarchical Data:** Representing and querying organizational charts, family trees, and parent-child relationships.
- **Graph Traversal:** Analyzing networks, such as social connections or transportation routes.
- **Bill of Materials:** Exploding a product assembly into all its constituent parts and sub-parts.
