# SQL: Joins

SQL joins are used to combine rows from two or more tables based on a related column between them. Joins are necessary because in a normalized database, data is spread across multiple, smaller tables to reduce redundancy.

---

### **Inner Join**

An **INNER JOIN** selects all rows from both tables where the join condition is met. It returns only the matching rows from both tables, discarding any non-matching data. This is the most common type of join and is often simply referred to as `JOIN`.

- **Venn Diagram:** The intersection of two circles.
- **Syntax:** `SELECT ... FROM table1 INNER JOIN table2 ON table1.column = table2.column;`
- **Example:** To find all students who are enrolled in a course, you would `INNER JOIN` a `Student` table and a `StudentCourse` table on their common `ROLL_NO` column.

---

### **Outer Joins**

Outer joins are used to retrieve all rows from one or both tables, even if there isn't a match in the other. There are three types:

### **Left Join**

A **LEFT JOIN** (or `LEFT OUTER JOIN`) returns all rows from the left table and the matching rows from the right table. If a row in the left table has no match in the right table, the columns from the right table will contain `NULL` values.

- **Venn Diagram:** The left circle and the intersection.
- **Syntax:** `SELECT ... FROM table1 LEFT JOIN table2 ON table1.column = table2.column;`
- **Example:** To list every employee and their department, including employees who aren't assigned to any department, you would `LEFT JOIN` the `Employees` table with the `Departments` table. The employees without a department would have `NULL` values for the department columns.

### **Right Join**

A **RIGHT JOIN** (or `RIGHT OUTER JOIN`) is the opposite of a left join. It returns all rows from the right table and the matching rows from the left table. Rows from the right table without a match in the left table will have `NULL` values for the left table's columns.

- **Venn Diagram:** The right circle and the intersection.
- **Syntax:** `SELECT ... FROM table1 RIGHT JOIN table2 ON table1.column = table2.column;`
- **Example:** To list all departments and their employees, including departments that currently have no employees, you would `RIGHT JOIN` the `Employees` table with the `Departments` table. Departments without any employees would have `NULL` values for the employee columns.

### **Full Join**

A **FULL JOIN** (or `FULL OUTER JOIN`) returns all rows from both the left and right tables. Rows without a match in the other table will have `NULL` values for the missing columns. It's essentially a combination of a `LEFT JOIN` and a `RIGHT JOIN`.

- **Venn Diagram:** Both circles combined.
- **Syntax:** `SELECT ... FROM table1 FULL JOIN table2 ON table1.column = table2.column;`
- **Example:** To see all employees and all departments, regardless of whether they have a match, you would `FULL JOIN` the two tables. This is useful for identifying gaps in data.

---

### **Specialized Joins**

### **Natural Join**

A **NATURAL JOIN** is a type of `INNER JOIN` that automatically joins two tables based on all columns that have the same name and data type. It's a quick shortcut but can be risky, as a new column with a matching name could unintentionally change the join logic.

- **Syntax:** `SELECT ... FROM table1 NATURAL JOIN table2;`

### **Cross Join**

A **CROSS JOIN** creates a **Cartesian product** of two tables. This means every row from the first table is combined with every row from the second table. The result can be very large and is often used for creating all possible combinations, such as generating a list of all products for all customers.

- **Syntax:** `SELECT ... FROM table1 CROSS JOIN table2;`

### **Self Join**

A SELF-JOIN is not a new type of join, but rather a technique where a table is joined to itself. This is done by using **aliases** to treat the same table as if it were two separate tables. It's useful for finding relationships within a single table, such as an employee and their manager, or for comparing records within the same table.

- **Syntax:** `SELECT ... FROM table AS alias1 JOIN table AS alias2 ON alias1.column = alias2.related_column;`
- **Example:** To find which employees are managed by whom, you would self-join an `Employees` table by matching the `manager_id` from one alias to the `employee_id` of the other.
