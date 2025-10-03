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
    
<img width="621" height="141" alt="image" src="https://github.com/user-attachments/assets/52316059-4611-4bbf-ad9d-bf57dabe42c2" />

    
- **Attribute**: A property that describes an entity. For example, `StudentID`, `Name`, or `Age` for a `Student` entity. Attributes are represented by an **oval**.
    - **Key Attribute**: An attribute that uniquely identifies an entity instance (e.g., `StudentID`). It's underlined in an ERD.
    - **Composite Attribute**: An attribute composed of several other attributes (e.g., `Address` is made up of `Street`, `City`, `State`, etc.).
    - **Multivalued Attribute**: An attribute that can hold multiple values for a single entity (e.g., a `Student` can have multiple `Phone_Numbers`). Represented by a **double oval**.
    - **Derived Attribute**: An attribute that can be calculated or derived from other attributes (e.g., `Age` from `Date of Birth`). Represented by a **dashed oval**.

<img width="1022" height="693" alt="image" src="https://github.com/user-attachments/assets/466189f3-465f-40ea-9c07-1b12f75540af" />

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
    
    <img width="862" height="313" alt="image" src="https://github.com/user-attachments/assets/696f3c93-9e28-48f7-a308-5320464d9d51" />

    
    - **One-to-Many (1:N)**: One entity can be associated with multiple entities in another set.
    
    ![image.png](attachment:5ca9a709-c2e8-434e-aeb9-27c9cdcc2052:image.png)
    
    - **Many-to-One (N:1)**: Multiple entities in one set can be associated with a single entity in another set.
    
   <img width="862" height="313" alt="image" src="https://github.com/user-attachments/assets/87162b82-34c4-472c-acd5-38961bd80b91" />


    
    - **Many-to-Many (M:N)**: Multiple entities can be associated with multiple entities.
    
   <img width="862" height="313" alt="image" src="https://github.com/user-attachments/assets/1dd5128f-0faa-4ca6-8e14-89c31a7ec886" />

    
- **Participation Constraint**: Specifies whether an entity must participate in a relationship.
    - **Total Participation**: Every entity instance in a set must participate in the relationship. Shown with a **double line** connecting the entity to the relationship.
    - **Partial Participation**: An entity instance may or may not participate in the relationship. Shown with a single line.
    
    The diagram depicts the ‘Enrolled in’ relationship set with Student Entity set having total participation and Course Entity set having partial participation.
    
    <img width="982" height="192" alt="image" src="https://github.com/user-attachments/assets/7ec82dde-b8a8-4917-8a0d-456ef9e08587" />
)
    

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
<img width="1093" height="522" alt="image" src="https://github.com/user-attachments/assets/05a75bb9-f7a4-4979-90e0-94aca1a784cc" />

