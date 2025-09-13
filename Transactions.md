# SQL: Transactions

- SQL transactions are a series of SQL operations that are treated as a single, indivisible unit of work.
- They ensure that all operations within the unit either succeed completely or fail completely, maintaining data integrity.
- The reliability of transactions is based on the **ACID** properties:
    - **Atomicity:** All operations in a transaction are a single unit; if one fails, the entire transaction is rolled back.
    - **Consistency:** A transaction brings the database from one valid state to another, upholding all integrity rules.
    - **Isolation:** Concurrent transactions don't interfere with each other.
    - **Durability:** Once a transaction is committed, its changes are permanent.

---

# Transaction Control Commands

These commands are used to manage the flow and outcome of transactions.

- **BEGIN TRANSACTION**: Marks the start of a new transaction. All subsequent statements are part of this transaction until a `COMMIT` or `ROLLBACK` command is issued.
    - Example :  `BEGIN TRANSACTION TransferFunds;`
- **COMMIT**: Saves all changes made during the current transaction to the database, making them permanent.
    - Example :
    
    ```sql
    DELETE FROM Student WHERE AGE = 20;
    COMMIT;
    ```
    
- **ROLLBACK**: Undoes all changes made during the current transaction, reverting the database to its state before `BEGIN TRANSACTION`.
    - Example :
    
    ```sql
    DELETE FROM Student WHERE AGE = 20;
    ROLLBACK;
    ```
    
- **SAVEPOINT**: Creates a checkpoint within a transaction. You can use this to roll back to a specific point without undoing the entire transaction.
    - Syntax : `SAVEPOINT SAVEPOINT_NAME;`
    - Example :
        
        ```sql
        SAVEPOINT SP1;
        //Savepoint created.
        DELETE FROM Student WHERE AGE = 20;
        //deleted
        SAVEPOINT SP2;
        //Savepoint created.
        ```
        
- **ROLLBACK TO SAVEPOINT**: Reverts the transaction to the specified `SAVEPOINT`, undoing all changes made since that point.
    - Syntax:  `ROLLBACK TO SAVEPOINT SAVEPOINT_NAME;`
    - Example :
    
    ```sql
    ROLLBACK TO SP1;
    //Rollback completed
    ```
    
- **RELEASE SAVEPOINT**: Removes a named `SAVEPOINT` from the transaction.
    - Syntax : `RELEASE SAVEPOINT SAVEPOINT_NAME;`

---

# Example: Bank Transfer

This example demonstrates how transactions ensure data integrity in a bank transfer.

```sql
BEGIN TRANSACTION;

-- Deduct money from Account A
UPDATE Accounts
SET Balance = Balance - 150
WHERE AccountID = 'A';

-- Add money to Account B
UPDATE Accounts
SET Balance = Balance + 150
WHERE AccountID = 'B';

-- If both operations succeed, commit the changes
COMMIT;

-- If an error occurs, undo both operations
ROLLBACK;
```

This sequence guarantees that money is either successfully deducted from one account and added to the other, or neither change occurs. This prevents an inconsistent state where money is lost.

---

# Types of Transactions

- **Read Transactions:** Involve only `SELECT` queries and don't modify data.
- **Write Transactions:** Modify data using `INSERT`, `UPDATE`, or `DELETE`.
- **Distributed Transactions:** Span across multiple databases.
- **Implicit Transactions:** Automatically started by the database for specific operations.
- **Explicit Transactions:** Manually controlled by a user using `BEGIN TRANSACTION` and `COMMIT` or `ROLLBACK`.

---

# Best Practices

- **Monitor Locks:** Watch for locking conflicts to avoid performance issues.
- **Limit Transaction Scope:** Keep transactions as small as possible to minimize the number of rows affected.
- **Use Batch Processing:** Break large data operations into smaller batches to prevent system overload.
