# SQL: Clustered Vs Non-Clustered

# Clustered Index

A **clustered index** determines the physical storage order of the data rows in a table. Because the data itself is sorted based on the index key, a table can only have one clustered index. A common example is when a **primary key** is defined on a column; SQL Server automatically creates a clustered index on it.

- **Key Concept:** The index reorders the actual data rows in the table.
- **Limitation:** A table can have only one clustered index.
- **Structure:** The leaf nodes of the index contain the actual data rows.
- **Use Case:** Ideal for columns used in **range-based queries** or sorting, like `ORDER BY` or `GROUP BY` clauses.

To understand it better: 

https://www.youtube.com/watch?v=UpJ9ICmzaAM

---

# Non-Clustered Index

A **non-clustered index** creates a separate data structure that contains the indexed column(s) and pointers to the actual data rows in the table. The physical order of the table's data remains unchanged. This separation allows you to create **multiple** non-clustered indexes on a single table.

- **Key Concept:** The index is a separate structure from the data. It contains index values and pointers (row locators) to the data.
- **Flexibility:** A table can have many non-clustered indexes.
- **Structure:** The leaf nodes of the index contain the indexed values and pointers to the data rows.
- **Use Case:** Best for columns frequently used in **specific lookups** or `JOIN` conditions.

To understand it better: 

https://www.youtube.com/watch?v=Ua08uVgsk4k&pp=0gcJCckJAYcqIYzv

---

# Comparison of Clustered and Non-Clustered Indexes

| Feature | Clustered Index | Non-Clustered Index |
| --- | --- | --- |
| **Physical Order** | Defines the physical order of data. | Doesn't affect the physical order of data. |
| **Count per Table** | Only one per table. | Can have multiple per table. |
| **Data Storage** | Leaf nodes are the actual data rows. | Leaf nodes are pointers to data rows. |
| **Speed** | Faster for range queries. | Faster for specific lookups. |
| **Memory/Disk** | Requires less memory. | Requires more memory due to a separate structure. |
| **Creation** | Automatically created with a primary key. | Manually created on other columns. |

