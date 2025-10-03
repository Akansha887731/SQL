# SQL: Regular Expressions

- SQL uses regular expressions, often called **regex**, to perform advanced string matching and manipulation. Regex patterns are sequences of characters that define a search pattern, providing a more flexible and powerful alternative to basic string functions.
- The three primary regex functions in SQL are **`REGEXP_LIKE`**, **`REGEXP_REPLACE`**, and **`REGEXP_SUBSTR`**.

---

# **Regex Functions in SQL**

1. **`REGEXP_LIKE`**: This function checks if a string matches a given regex pattern. It returns `TRUE` or `FALSE`.
    - **Syntax**: `REGEXP_LIKE(column_name, 'pattern')`
    - **Example**: To find all product names that start with 'A'.SQL
        
        ```sql
        SELECT product_name 
        FROM products 
        WHERE REGEXP_LIKE(product_name, '^A');
        ```
        
2. **`REGEXP_REPLACE`**: This function searches for a pattern and replaces all its occurrences with a specified string.
    - **Syntax**: `REGEXP_REPLACE(string, 'pattern', 'replacement')`
    - **Example**: To remove all non-numeric characters from a phone number.SQL
        
        ```sql
        SELECT REGEXP_REPLACE(phone_number, '[^0-9]', '') AS cleaned_number 
        FROM contacts;
        ```
        
3. **`REGEXP_SUBSTR`**: This function extracts a substring that matches a specific pattern.
    - **Syntax**: `REGEXP_SUBSTR(string, 'pattern', [start_position], [occurrence], [match_parameter])`
    - **Example**: To extract the domain name from an email address.SQL
        
        ```sql
        SELECT REGEXP_SUBSTR(email, '@[^.]+') AS domain 
        FROM users;
        ```
        

---

# **Common Regex Patterns**

| **Pattern** | **Description** | **Example** |
| --- | --- | --- |
| `.` | Matches any single character. | `h.t` matches `hat`, `hit`, `hot` |
| `^` | Matches the start of a string. | `^A` matches `Apple` |
| `$` | Matches the end of a string. | `ing$` matches `sing` |
| `*` | Matches zero or more of the preceding character. | `ab*` matches `a`, `ab`, `abb` |
| `+` | Matches one or more of the preceding character. | `ab+` matches `ab`, `abb`, `abbb` |
| `?` | Matches zero or one of the preceding character. | `colou?r` matches `color`, `colour` |
| `[]` | Matches any character inside the brackets. | `[aeiou]` matches any vowel |
| `[^]` | Matches any character not inside the brackets. | `[^aeiou]` matches any non-vowel |
| `\d` | Matches any digit (0-9). | `\d+` matches `123`, `4567` |
| `\s` | Matches any whitespace character. | `a\sb` matches `a b` |

---

# **Real-World Use Cases**

- **Data Validation**: Enforce specific data formats (e.g., email addresses, phone numbers).
- **Data Cleaning**: Remove unwanted characters or whitespace.
- **Data Extraction**: Pull specific information (e.g., URLs, subdomains) from unstructured text.
