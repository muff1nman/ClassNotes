Fundamentals of Database Systems
=====
Chapter 3
=====
#### Definitions
- database application
    - a particular database and the associated programs that implement the
      database queries and updates

- Entity-Relationship Model (ER)
    - a popular high-level conceptual data model

- ER diagrams
    - the diagrammatic notation associated with the ER model

- Universal Modeling Language (UML)
    - an alternative object modeling language
    - goes beyond data to specify the design of software modules and their
      interactions with diagrams

- class diagrams
    - operations on objects are specified, in addition to specifying the database
      schema structure

- functional requirements (FR)
    - ?

- conceptual schema
    - a concise description of the data requirements of the users and includes
      detailed descriptions of the entity types, relationships, and constraints

- logical design OR data model mapping
    - its result is a database schema in the implementation data model of the
      DBMS.

- physical design
    - the internal storage structures, indexes, access paths, and file
      organizations for the database files are specified. 

- entity
    - a <em>thing</em> in the real world with an independent existence
        - physical existence ( person, tree )
        - conceptual existence ( company, job )

- attributes
    - the particular properties that describe an entity

- entity type
    - defines a collection or set of entities that have the same attributes.
      They are described by a name and attributes.  
    - It describes the schema or intension for a set of entities that share the
      same structure

- extension
    - The collection of entities of a particular entity type is grouped into an
      entity set which is also called the extension of the entity type.

- key OR uniqueness constraint
    - a key attribute is an attribute of an entity whose value is distinct for
      each individual entity in the entity set.

- value set OR domain
    - specifies the set of values that may be assigned to that attribute for
      each individual entity
xx

### 3.1 Using High-level Conceptual Data Models for Database Design
// insert Figure 3.1

The conceptual schema does not include implementation details
### 3.2 An Example Database Application

// Nothing to see here
### 3.3 Entity Types, Entity Sets, Attributes, and Keys

There are several types of attributes:
- simple versus composite
- single valued versus multivalued
- stored versus derived

#### Simple versus Composite Attributes

Simple are also called atomic attributes

Simple attributes are ones that cannot be further divided. Composite attributes
form a hierarchy. E.g. street_address can be subdivided into number, street, and
apt number

#### Single-Valued versus Multivalued Attributes

Single-valued attributes can have only a single value for any particular entity.
E.g. age

Multi-valued attributes can have a set of values for any particular attribute.
There many be upper and lower constraints on the number of attributes.  They are
specified with braces and comma separated values

#### Stored versus Derived Attributes
E.g. age can be derived from date of birth. The age attribute is the <em>derived
attribute</em> and is said to be <em>derivable from</em> the date of birth
attribute.  The date of birth attribute is then the <em>stored attribute</em>

#### Null Values
- unknown value
    - the value exists but is not recorded
    - it is not known whether the attribute value exists.

#### Complex Attributes
Specified by grouping attributes with a set of parenthesis and separating
attributes with a comma.

#### Key Attributes of an Entity Type
key attribute names are underlined in the ER diagram

#### Value Sets (Domains) of Attributes
// Nothing too interesting here

### 3.4 Relationship Types, Relationship Sets, Roles, and Structural Constrains

