# Database Normalization

# **Introduction to Database Normalization**

**Normalization** is the process of organizing data in a relational database to reduce data redundancy and improve data integrity. The goal is to design a schema that avoids anomalies and ensures data is stored in a logical, efficient manner. This is achieved by splitting a large table into smaller, related tables.

# **Why Normalization is Essential: The Problem of Anomalies**

Normalization is crucial because unnormalized tables are prone to data anomalies, which can compromise the database's integrity. These anomalies include:

- **Insertion Anomaly:** Occurs when you cannot insert a new record without also adding redundant data for an existing entity.
    - **Example:** In an unnormalized `Student_Course` table, you can't add a new student's details until they enroll in a course, as `CourseID` might be part of the primary key.
- **Deletion Anomaly:** Occurs when deleting a record unintentionally deletes other related, non-redundant information.
    - **Example:** Deleting the last student from a course might also delete the course details and instructor information from the table.
- **Update Anomaly:** Occurs when updating a piece of information requires changing multiple records, leading to inconsistencies if a change is missed.
    - **Example:** If an instructor's room number is stored in multiple records, updating it requires changing every instance. If one is missed, the data becomes inconsistent.

# **Normal Forms in Detail**

Normal Forms are a set of rules that progressively eliminate data redundancy and anomalies. Each normal form builds upon the previous one.

### **1. First Normal Form (1NF)**

A relation is in **1NF** if all attribute values are **atomic** (indivisible) and there are no repeating groups of columns. Each column should contain a single value.

| StudentID | StudentName | Phone_Numbers |
| --- | --- | --- |
| 101 | Alice | 123-456, 789-012 |
| 102 | Bob | 345-678 |

**How to fix it:** Split the multi-valued attribute into separate rows or create a new table.

| StudentID | StudentName | Phone_Number |
| --- | --- | --- |
| 101 | Alice | 123-456 |
| 101 | Alice | 789-012 |
| 102 | Bob | 345-678 |

---

### **2. Second Normal Form (2NF)**

A relation is in **2NF** if it is in 1NF and every non-key attribute is **functionally dependent** on the **entire** primary key. This is only a concern for tables with a composite primary key (a key made of two or more attributes).

| StudentID | CourseID | StudentName | CourseName | Instructor |
| --- | --- | --- | --- | --- |
| S1 | C1 | Alice | History | Smith |
| S1 | C2 | Alice | Math | Jones |
| S2 | C1 | Bob | History | Smith |

Here, `StudentName` depends only on `StudentID`, not on the full composite key. Similarly, `CourseName` and `Instructor` depend only on `CourseID`. This is a partial dependency.

**How to fix it:** Decompose the table into separate tables to eliminate partial dependencies.

- **Students Table:** `(StudentID, StudentName)`
- **Courses Table:** `(CourseID, CourseName, Instructor)`
- **Enrollment Table:** `(StudentID, CourseID)`

---

### **3. Third Normal Form (3NF)**

A relation is in **3NF** if it is in 2NF and there are no **transitive dependencies**. A transitive dependency occurs when a non-key attribute is dependent on another non-key attribute.

| CourseID | CourseName | Instructor | InstructorOffice |
| --- | --- | --- | --- |
| C1 | History | Smith | Room 101 |
| C2 | Math | Jones | Room 202 |

Here, `InstructorOffice` is dependent on `Instructor`, and `Instructor` is dependent on `CourseID`. This is a transitive dependency: `CourseID` → `Instructor` → `InstructorOffice`.

**How to fix it:** Decompose the table to remove the transitive dependency.

- **Courses Table:** `(CourseID, CourseName, Instructor)`
- **Instructors Table:** `(InstructorID, InstructorName, InstructorOffice)` (Note: we would need to add an `InstructorID` to make this a unique key).

---

### **4. Boyce-Codd Normal Form (BCNF)**

BCNF is a stricter version of 3NF. A relation is in **BCNF** if for every functional dependency `X → Y`, `X` is a **superkey**.

**Example of a BCNF Violation:**
Consider a table `(StudentID, Subject, Instructor)` where a student can have multiple instructors for a subject, but each instructor teaches only one subject.

- `StudentID, Subject` → `Instructor` (This is a dependency)
- `Instructor` → `Subject` (This is also a dependency)

The second dependency violates BCNF because `Instructor` is not a superkey.

**How to fix it:** Decompose the table into a `Student_Instructor` table and an `Instructor_Subject` table.
