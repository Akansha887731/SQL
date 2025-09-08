# How to delete duplicate rows

### 1. Identifying Duplicates

To find duplicate rows, use the **`GROUP BY`** clause with the **`COUNT(*)`** aggregate function. You group the rows by the columns that define a unique record, and then use the **`HAVING`** clause to filter for groups where the count is greater than 1.

- **Syntax:**
    
    ```sql
    SELECT col1, col2, COUNT(*)
    FROM your_table
    GROUP BY col1, col2
    HAVING COUNT(*) > 1;
    ```
    

---

### 2. Deleting Duplicates

Once identified, you can remove duplicate rows while keeping one unique copy. Here are four effective methods.

### **Method 1: Using `MIN()` with a `DELETE` Subquery**

This method is suitable when your table has a primary key or an auto-incrementing serial number (`SN` in the example). The query keeps the row with the lowest primary key value for each group of duplicates and deletes all other rows.

- **Syntax:**
    
    ```sql
    DELETE FROM table_name
    WHERE SN NOT IN (
        SELECT MIN(SN)
        FROM table_name
        GROUP BY col1, col2
    );
    ```
    

### **Method 2: Using `ROW_NUMBER()` with a CTE**

This is the most common and powerful method. The `ROW_NUMBER()` window function assigns a unique rank to each row within a partition (a group of duplicates). You then use a Common Table Expression (CTE) to easily identify and delete the rows with a rank greater than 1, as they are the duplicates.

- **Syntax:**
    
    ```sql
    WITH CTE AS (
        SELECT *,
               ROW_NUMBER() OVER(PARTITION BY col1, col2 ORDER BY SN) AS RowNum
        FROM your_table
    )
    DELETE FROM CTE
    WHERE RowNum > 1;
    ```
    
    This method is highly flexible as you can choose which row to keep by changing the `ORDER BY` clause. For instance, `ORDER BY SN DESC` would keep the most recently added row.
    

### **Method 3: Using a Temporary Table**

This approach involves creating a new, temporary table with only the unique rows, then clearing and repopulating the original table.

1. **Insert unique rows into a temporary table:**`SELECT DISTINCT * INTO #TempTable FROM your_table;`
2. **Truncate the original table:**`TRUNCATE TABLE your_table;`
3. **Insert data back from the temporary table:**`INSERT INTO your_table SELECT * FROM #TempTable;`
4. **Drop the temporary table:**`DROP TABLE #TempTable;`

### **Method 4: Using `DISTINCT` with `INSERT INTO`**

This method is similar to the temporary table approach but can be done in a single query block, though it might be less efficient for very large tables.

1. **Delete all rows from the original table:**`DELETE FROM table_name;`
2. **Re-insert only the unique rows:**`INSERT INTO table_name (col1, col2) SELECT DISTINCT col1, col2 FROM table_name;`

---

### 3. Preventing Duplicates

The best practice is to prevent duplicates from occurring in the first place. You can do this by:

- **Creating a Primary Key** on a single column (like `SN`).
- **Creating a `UNIQUE` Constraint** on one or more columns that should have unique values (e.g., `(EMPNAME, CONTACTNO)`). This will throw an error if an identical record is inserted.
- **Implementing data validation** at the application level before data is sent to the database.
