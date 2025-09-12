# SQL: Subquery

Subqueries in SQL are queries nested inside another query, used for complex filtering, aggregation, and data manipulation. They can appear in the **WHERE**, **FROM**, or **HAVING** clauses of a SQL statement. The main purpose of a subquery is to use the result of an inner query to influence the outer query.

---

# 📝 Key Concepts of SQL Subqueries

## 📌 **Types of Subqueries**

- **Single-Row Subquery:** Returns exactly one row. Used with single-value comparison operators like `=`, `>`, `<`.
    - **Example:** `SELECT * FROM Employees WHERE Salary = (SELECT MAX(Salary) FROM Employees);`
- **Multi-Row Subquery:** Returns multiple rows. Used with operators that can handle multiple values, such as **IN**, **ANY**, or **ALL**.
    - **Example:** `SELECT * FROM Employees WHERE DepartmentID IN (SELECT DepartmentID FROM Departments WHERE Location = 'New York');`
- **Correlated Subquery:** A subquery that depends on the outer query. It's executed once for each row of the outer query, referencing columns from it. Often slower but powerful for row-by-row comparisons.
    - **Example:** `SELECT e.Name, e.Salary FROM Employees e WHERE e.Salary > (SELECT AVG(Salary) FROM Employees WHERE DepartmentID = e.DepartmentID);`
- **Nested Subquery:** A general term for a subquery, but specifically refers to non-correlated subqueries (also called independent subqueries). These run once and pass their result to the outer query.
    - **Example:** `SELECT S_NAME FROM STUDENT WHERE S_ID IN (SELECT S_ID FROM STUDENT_COURSE WHERE C_ID IN (SELECT C_ID FROM COURSE WHERE C_NAME IN ('DSA', 'DBMS')));`

# 📌 **Common Operators with Subqueries**

- **IN / NOT IN**: Checks if a value is **in** or **not in** the list of values returned by the subquery.
- **EXISTS / NOT EXISTS**: Checks for the existence of any rows in the subquery result. It is very efficient as it stops processing as soon as it finds the first match. This is often a good choice for correlated subqueries.
- **ANY**: Returns `TRUE` if the comparison is true for **at least one** of the values returned by the subquery.
- **ALL**: Returns `TRUE` if the comparison is true for **all** of the values returned by the subquery.

---

# 💡 Practical Applications

Subqueries are versatile and can be used in different parts of a SQL query:

- **WHERE Clause**: Filters rows based on a condition determined by the subquery. This is the most common use case.
    - **Example**: `DELETE FROM Student WHERE ROLL_NO IN (SELECT ROLL_NO FROM New_Student WHERE SECTION = 'A');`
- **FROM Clause**: The subquery acts as a temporary, or **derived**, table. This is useful for performing aggregations or other operations before joining with other tables.
    - **Example**: `SELECT NAME FROM (SELECT NAME, LOCATION FROM Student WHERE LOCATION = 'Delhi') AS DelhiStudents;`
- **Data Manipulation**: Subqueries can be used with `INSERT`, `UPDATE`, and `DELETE` statements to dynamically select the data to be modified.
    - **Example**: `UPDATE employees SET salary = (SELECT AVG(salary) FROM employees WHERE department_id = 101) WHERE department_id = 101;`

# ⚙️ Performance Considerations

- **Correlated vs. Non-Correlated**: Non-correlated subqueries (independent) are generally more efficient because they execute only once. Correlated subqueries can be slow on large datasets because they run for every single row of the outer query.
- **Subquery vs. JOIN**: In many cases, a `JOIN` can achieve the same result as a subquery, often with better performance. It is recommended to prefer `JOIN`s for joining tables, especially in the `FROM` and `WHERE` clauses.
