Fundamentals of Database Systems
=====

Chapter 3
=

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

- relationship type 
    - R among n entity types E1,.., En defines a set of associations - or a
      <em>relationship set</em> among entities from these entity types. 

- degree
    - of a relationship type is the number of participating entity types.

- cardinality ratio
    - specifies the maximum number of relationship instances that an entity can
      participate in.  (1:1), (1:N), (M:N), (N:1)

- participation constraint
    - specifies whether the existence of an entity depends on its being related
      to another entity via the relationship type.  This constraint specifies
      the minimum number of relationship instances that each entity can
      participate in and is also called the minimum cardinality constraint.
      Two types:
        - total participation OR existence dependency
            - every entity in the total set of employee entities must be related
              to a department entity via WORKS_FOR
        - partial participation
            - some or part of the set of employee entities are related to some
              department entity via MANAGES, but not necessarily all.

- weak entity types
    - entity types that do not have key attributes of their own
    - it always has a total participation constraint with respect to its
      identifying relationship because a weak entity cannot be identified
      without an owner identity.
    - normally has a partial key

- regular entity types OR strong entity types
    - entity types that do have key attributes

- identifying relationship
    - the relationship type that relates a weak entity type to its owner

- partial key
    - the set of attributes that can uniquely identify weak entities that are
      related to the same owner entity


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

### 3.4 Relationship Types, Relationship Sets, Roles, and Structural Constraints
Relationships are displayed with diamond shaped boxes.

Binary relationships ( or relationships with two entities ) are the most common

There are two main types of relationship constraints:
    - cardinality ratio
    - participation

The two of these combined are called <em>structural constraints</em> of a
relationship type.

### 3.5 Weak Entity Types
// Nothing here, just definitions.

### 3.6 Refining the ER Design for the COMPANY Database
// Nothing

### 3.7 ER Diagrams, Naming Conventions, and Design Issues

####<em>Alternative</em> ER notation for structural constraints on relationships
> This notation involves associating a pair of integer numbers (min, max) with
> each participation of an entity type E in a relationship type R, where 
> 
>     0 <= min<= max and max = 1
> 
> The numbers mean that for each entity e in E, e must
> participate in at least min and at most max relationship instances in R at any
> point in time. In this method, min = 0 implies partial participation, whereas
> min > 0 implies total participation.

Hint: I prefer the alternative notation

    cout << "Here is code" << endl;
    <html><i>test</i>

> Usually, one uses either the cardinality ratio/single-line/double-line
> notation or the (min, max) notation. The (min, max) notation is more precise,
> and we can use it to specify structural constraints for relationship types of
> any degree. However, it is not sufficient for specifying some key constraints
> on higher-degree relation-ships, as discussed in Section 3.9

As discussed in class

### 3.8 Example of Other Notation: UML Class Diagrams
// skipped for now
