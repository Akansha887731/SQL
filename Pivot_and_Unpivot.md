# **SQL: Pivot and Unpivot**

- Used to transform data by changing rows to columns and vice versa.
- They're particularly useful for data reporting and analysis, where different data layouts may be needed for different tasks.

---

# **PIVOT** 📈

**`PIVOT`** rotates data from rows to columns. It aggregates data and presents it in a cross-tabular or matrix format, where unique values from one column become the new column headers.

- **Syntax**:
    
    ```sql
    PIVOT (
      <aggregate_function>(<value_column>)
      FOR <pivot_column> IN (<column_list>)
    ) AS <alias>
    ```
    
- **Example**: To see the total prices for different course categories as columns.SQL
    
    ```sql
    - Sample Table
    CourseName | CourseCategory | Price
    C | PROGRAMMING | 5000
    PYTHON | PROGRAMMING | 8000
    INTERVIEW | INTERVIEW | 5000
    
    -- PIVOT Query
    SELECT *
    FROM geeksforgeeks
    PIVOT ( SUM(Price) FOR CourseCategory IN (PROGRAMMING, INTERVIEW)
    ) AS PivotTable;
    
    -- Result
    CourseName | PROGRAMMING | INTERVIEW
    C | 5000 | NULL
    PYTHON | 8000 | NULL
    INTERVIEW | NULL | 5000
    ```
    
    The `PROGRAMMING` and `INTERVIEW` values from the `CourseCategory` column are turned into new columns. The `Price` values are then aggregated using `SUM()` and placed under the correct new column.
    

---

# **UNPIVOT** 📉

**`UNPIVOT`** is the reverse of `PIVOT`. It rotates data from columns back to rows. It's used to normalize data by converting multiple related columns into a single column with a corresponding value column.

- **Syntax**:

```sql
UNPIVOT (
  <value_column>
  FOR <pivot_column> IN (<column_list>)
) AS <alias>
```

- **Example**: Reversing the pivoted table back to its original row-based format.SQL
    
    ```sql
    - Pivoted Table
    CourseName | PROGRAMMING | INTERVIEW
    C | 5000 | NULL
    PYTHON | 8000 | NULL
    INTERVIEW | NULL | 5000
    
    -- UNPIVOT Query
    SELECT *
    FROM PivotedTable
    UNPIVOT ( Price FOR CourseCategory IN (PROGRAMMING, INTERVIEW)
    ) AS UnpivotTable;
    
    -- Result
    CourseName | CourseCategory | Price
    C | PROGRAMMING | 5000
    PYTHON | PROGRAMMING | 8000
    INTERVIEW | INTERVIEW | 5000
    ```
    
    The `PROGRAMMING` and `INTERVIEW` columns are converted back into rows under a new `CourseCategory` column, and their corresponding values are placed in a new `Price` column.
