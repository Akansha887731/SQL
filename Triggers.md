# SQL: Triggers

- A trigger is a special type of **stored procedure** that automatically executes when a specific event (like an `INSERT`, `UPDATE`, or `DELETE`) occurs on a table.
- Triggers are used for automating tasks, enforcing data integrity, and creating audit trails.

# **Key Concepts and Syntax**

A trigger is associated with a specific table and event. The basic syntax is:

```sql
CREATE TRIGGER trigger_name
[BEFORE | AFTER]
{INSERT | UPDATE | DELETE}
ON table_name
FOR EACH ROW
BEGIN
  -- SQL statements to execute
END;
```

- **`BEFORE` vs. `AFTER`**:
    - `BEFORE` triggers run before the event and are great for data validation.
    - `AFTER` triggers run after the event and are perfect for logging or updating other tables.
- **`:new` and `:old`**:
    - These pseudo-records are available in row-level triggers.
    - `:new` refers to the new data in an `INSERT` or `UPDATE`, while
    - `:old` refers to the old data in an `UPDATE` or `DELETE`.

# **Example: A Trigger for Data Validation**

Let's say you have a `student_grades` table and want to ensure that no grade can be less than 0 or greater than 100 before the data is inserted. You can use a `BEFORE INSERT` trigger to check this.

```sql
- This trigger will prevent a row from being inserted if the grade is out of range.
CREATE TRIGGER validate_grade
BEFORE INSERT ON student_grades
FOR EACH ROW
BEGIN IF :new.grade < 0 OR :new.grade > 100 THEN RAISE_APPLICATION_ERROR(-20001, 'Invalid grade value. Grade must be between 0 and 100.'); END IF;
END;
```

---

# **Example: A Trigger for an Audit Trail**

An audit trail is a record of changes made to data. You can use an `AFTER UPDATE` trigger to log every time a student's grade is changed.

```sql
- Create an audit table to store the changes
CREATE TABLE grade_audit ( student_id NUMBER, old_grade NUMBER, new_grade NUMBER, change_date TIMESTAMP
);
-- This trigger will automatically log changes to the `grade_audit` table.
CREATE TRIGGER audit_grade_changes
AFTER UPDATE ON student_grades
FOR EACH ROW
BEGIN INSERT INTO grade_audit (student_id, old_grade, new_grade, change_date) 
			VALUES (:old.student_id, :old.grade, :new.grade, SYSTIMESTAMP);
END;
```
