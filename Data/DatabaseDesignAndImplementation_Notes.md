Database Design and Implementation

1 Introduction:  Why a Database System?
===

2 Data Definition
==
#### Definitions
- Relational Database
    - The data in a relational database is organized into tables.  Each table
      contains zero or more records and one or more fields.

- type
    - each field has a specified type. Some examples are: numbers, strings,
      date/time 

- Constraint
    - Restricts the allowable records in a table.  The database system ensures
      that the constraints are satisfied by refusing to execute any update
      request that would violate them.

      Some types are:
        - Integrity
            - Specifies business rules for an organization. I.e. a professor
              teaches at most two sections per year
        - Null Value
        - Key
            - No two records can have the same values for the key
            - A primary key can never be null
        - Referential Integrity
            - requires each non-null foreign key value to be the key value of
              some record

- Null Value
    - a placeholder for a missing or unknown value.  

- Key
    - A minimal set of fields that can be used to identify a record.  

- foreign key
    - a set of fields from one table that corresponds to the primary key of
      another table. 

- SQL 'create table' command
    - specifies the name of the table, the name and type of the fields and any
      constraints
      In addition:
        - An integrity constraint on individual records is specified by means of
          the <span class='italics' >check</span> keyword, followed by the constraint expression.
        - A null value constraint

3 Data Design
==
#### Definitions
- Class Diagram
    - describes the entities that the database should hold and how they should
      relate to each other.  It has three constructs:
        - class: a type of entity
        - relationship: a meaningful correspondence between the entities of two
          or more classes
        - attribute: denotes a value that describes an entity.

- Cardinality
    - annotations indicate, for a given entity, how many other entities can be
      related to it. I.e. 1 - *

- Multi-way relationship
    - involves three or more classes

- Reification
    - the process of turning a relationship into a class

- Redundant
    - if removing a relationship does not change the information content of the
      class diagram.

- functional dependency (FD)
    - A1A2..AN --> B asserts that two records having the same values for the Ai
      fields must also have the same value for the field B.

- key-based FD
    - the fields on the LHS form a key
    - a table that has a non-key-based FD contains redundancy

- Boyce-Codd Normal Form (BCNF)
    - a relational schema is in this form if all full FD's are key-based

- Third Normal Form (3NF)
    - a relational schema is in this form if the RHS field for all non-key-based
      FD's belongs to some key

Note a foreign key corresponds to a weak-strong relationship

### 3.3.2 Transforming a Relational Schema
1. Create a class for each table in the schema
2. If table T1 contains a foreign key of table T2, then create a relationship
   between classes T1 and T2.  The annotation next to T2 will be 1.  The
   annotation next to T1 will be 0..1 if the foreign key is also a key;
   otherwise, the annotation will be *
3. Add attributes to each class.  The attributes should include all fields of
   its table, except for the foreign keys and any artificial key fields. 

### 3.3.3 Transforming a Class Diagram
1. Create a table for each class, whose fields are the attributes of that class.
2. Choose a primary key for each table.  If there is no natural key, then add a
   field to the table to serve as an artificial key
3. For each weak-strong relationship, add a foreign key field to its weak-side
   table to correspond to the key of the strong-side table.

## 3.2 The Design Process
1. Create a requirements specification
2. Create a preliminary class diagram from the nouns and verbs of the
   specification
3. Check for inadequate relationships in the diagram
4. Remove redundant relationships from the diagram.
5. Revise weak-weak and strong-strong relationships
6. Identify the attributes for each class.


