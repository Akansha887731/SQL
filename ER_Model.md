# SQL: ER Model

- The **Entity-Relationship (ER) Model** is a high-level conceptual data model that helps in designing databases. It provides a graphical representation of the logical structure of a database, including entities, their attributes, and the relationships between them.
- An **Entity-Relationship Diagram (ERD)** is the visual blueprint of this model, used to design and debug relational databases.

<img width="1000" height="393" alt="image" src="https://github.com/user-attachments/assets/e2450b90-e5b9-497f-ad2d-e00fd90ae683" />


---

# **ER Model Components**

- **Entity**: A real-world object or concept about which data is stored, such as a `Student` or a `Car`. Entities are represented by a **rectangle** in an ERD.
    - **Strong Entity**: An entity that has a primary key and can be uniquely identified on its own.
    - **Weak Entity**: An entity that cannot be uniquely identified without a strong entity. It's dependent on its "identifying entity" and is represented by a **double rectangle**.
    - Example :
    
    ![image.png](attachment:f6d04656-38c2-409b-8822-51cf3b460459:image.png)
    
- **Attribute**: A property that describes an entity. For example, `StudentID`, `Name`, or `Age` for a `Student` entity. Attributes are represented by an **oval**.
    - **Key Attribute**: An attribute that uniquely identifies an entity instance (e.g., `StudentID`). It's underlined in an ERD.
    - **Composite Attribute**: An attribute composed of several other attributes (e.g., `Address` is made up of `Street`, `City`, `State`, etc.).
    - **Multivalued Attribute**: An attribute that can hold multiple values for a single entity (e.g., a `Student` can have multiple `Phone_Numbers`). Represented by a **double oval**.
    - **Derived Attribute**: An attribute that can be calculated or derived from other attributes (e.g., `Age` from `Date of Birth`). Represented by a **dashed oval**.

![image.png](attachment:20c1ec62-ce2e-4927-a8f6-a0f5f68267e6:image.png)

- **Relationship**: An association between two or more entities. Relationships are represented by a **diamond**.
    - **Degree of a Relationship**: The number of entity sets participating in a relationship.
        - **Unary**: Involves one entity set (e.g., a `Person` is married to a `Person`).
        - **Binary**: Involves two entity sets (e.g., a `Student` enrolls in a `Course`).
        - **Ternary**: Involves three entity sets.

---

# **Cardinality and Participation Constraints**

These concepts define the numerical constraints of a relationship.

- **Cardinality**: The maximum number of times an entity can participate in a relationship.
    - **One-to-One (1:1)**: Each entity in one set is linked to at most one entity in the other set.
    
    ![image.png](attachment:6aff853f-2b17-4396-93eb-b0430b33844c:image.png)
    
    - **One-to-Many (1:N)**: One entity can be associated with multiple entities in another set.
    
    ![image.png](attachment:5ca9a709-c2e8-434e-aeb9-27c9cdcc2052:image.png)
    
    - **Many-to-One (N:1)**: Multiple entities in one set can be associated with a single entity in another set.
    
    ![image.png](attachment:d1e82c42-37e3-4bb8-a7ba-e076e36dc131:image.png)
    
    - **Many-to-Many (M:N)**: Multiple entities can be associated with multiple entities.
    
    ![image.png](attachment:73fad663-54ad-43b3-8390-d2285a2e3459:image.png)
    
- **Participation Constraint**: Specifies whether an entity must participate in a relationship.
    - **Total Participation**: Every entity instance in a set must participate in the relationship. Shown with a **double line** connecting the entity to the relationship.
    - **Partial Participation**: An entity instance may or may not participate in the relationship. Shown with a single line.
    
    The diagram depicts the ‘Enrolled in’ relationship set with Student Entity set having total participation and Course Entity set having partial participation.
    
    ![image.png](attachment:de9e0df2-0904-465a-83e2-60e882a4cef0:image.png)
    

---

# **ER Model to Relational Model Mapping**

Converting an ERD to a relational model (tables) is a critical step in database design. Here's a summary of how different relationships map to tables:

| **Relationship Type** | **Table Mapping** | **Key Takeaway** |
| --- | --- | --- |
| **1:1 with Total Participation** | Can be represented as a single table. | Merge entities into one table with a foreign key from the partial participant. |
| **1:1 with Partial Participation** | Requires two separate tables. | Cannot be merged as some rows might have `NULL` values. |
| **1:N** | Requires two tables. | The foreign key goes in the table on the "many" side of the relationship. |
| **M:N** | Requires three tables. | Create a separate "junction" table for the relationship with a composite key of the primary keys from both entities. |
| **Weak Entity** | Requires two tables. | The weak entity's primary key is a combination of its partial key and the primary key of its identifying (strong) entity. |

# Example

The below diagram is our final entity relationship diagram for bank with all entities, their attributes and the relationship between them with the PRIMARY KEY and Cardinality ratio.
