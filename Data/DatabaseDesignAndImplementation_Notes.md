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

3 The Relational Model (OLD BOOK)
=====
#### Definitions

## Background
1969 IBM E.F. Codd 
1979 - Relational model 

### Three Components
> 1. The structural components
>     - The structural component: This component defines how information is
>       structured, or represented. Specifically, all information is represented
>       as relations, which are composed of tuples, which in turn are composed of
>       attribute and value components. The relation is the sole data structure
>       used to represent all information in the database.  Relations as defined
>       in the relational model are derived from relations in set theory, a branch
>       of mathematics, and share many of their properties. They are formally
>       defined in Codd’s 1970 paper.
>     - think of relation as table, tuples as columns
> 
> 2. The integrity component
>     - The integrity component: This component defines methods that enforce
>       relationships within and between relations (or tables) in the structural
>       component.  These methods are called constraints, and are expressed in the
>       form of rules. There are three principle types of integrity: domain
>       integrity, governing values in columns; entity integrity, governing rows
>       in tables; and referential integrity, governing how tables relate to one
>       another. Integrity has no analog in set theory, but rather is unique to
>       relational theory. It was initially addressed in Codd’s 1970 paper and
>       greatly expanded upon in the 1980s by Codd and others.
> 
> 3. The manipulative component
>     - The manipulative component: This aspect defines the methods with which to
>       operate on or manipulate information. Like relations, these operations
>       also have their roots in mathematics. They are formalized in relational
>       algebra and relational calculus as originally presented in Codd’s 1972
>       paper “Relational Completeness of Data Base Sublanguages.”

### SQL and the Relational Model
lots of information that the author cannot all touch on, yada yada

## The Structural Component
The first of Codd's rules. 

### The Information Principle (Information Rule)
> All information in a relational database is represented explicitly at the
> logical level and in exactly one way - by values in tables. 

OR in Date's words
> The entire information content of the database is represented in one and only
> one way, namely as explicit values in column positions in rows in tables.

##### Logical Level
- The view presented in the logical level consists of tables, made up of rows,
  which in turn are made up of values.

- The view is completely independent of the database system - the technology
  (software or hardware) that enables it.

##### Physical Level (Physical Representation)
- The technology of the system

### The Sanctity of the Logical Level
Codd:

> 8. <em>Physical Data Independence</em> The logical view can in no way be impaired by the
>    underlying software or hardware.
> 
> 9. <em>Logical Data Independence</em> Application programs and terminal
>    activities remain logically unimpaired when information-preserving changes of
>    any kind that theoretically permit un-impairment are made to the base tables.
> 
> 
> 11. <em>Distribution Independence</em> Even if the database is spread across
>     various locations, it cannot impact the logical view of data.
> 
> 12. <em>Nonsubversion Rule</em> The database software may not provide any
>     facility which can subvert the integrity constraints of the logical view.

table = relation (variables)
rows = tuples
columns = values
types = domains

*Tables are not the same thing as relations*

### Tuples
A set of values each of which has an associated attribute.  An attribute defines
the values name and domain.  The combination of an attribute and a value is
called a component.

Figure 3-2

### Relations
Is a set of one or more tuples that share the same heading.

Think of an array of C structs.

##### Degree and Cardinality
Degree => width
Cardinality => height

##### Mathematical Relations
Cross product or Cartesian product
    - a combination of every value in every domain with every value in every
      other domain.

The cross product of the columns is the total set of all tuples for a relation

##### Relational Relations
Order in a relational tuple does not matter because all tuple attributes have
names to identify them where this is not the case in mathematics.

Codd:

> Moreover, the (relational) approach has a close tie to first-order predicate
> logic—a logic on which most of mathematics is based, hence a logic which can
> be expected to have strength, endurance, and many applications.

### Tables: Relation Variables

> A relation, though it contains values, is itself just a value, just like an
> integer or a string. The value of a relation is given by the particular set of
> tuples it contains. Likewise, the value of each tuple in turn is given by the
> specific values within it. Thus, the value of a relation is determined by the
> sum of its parts. Figure 3-5 illustrates this. It depicts three relations,
> represented by R1, R2, and R3, taken over I×F. Each represents a different
> value, or relation.  R1, R2, and R3 are not relations but rather relation
> variables. They represent relations.  Codd referred to them as named
> relations. Date (2003) calls them relvars. SQL calls them tables.  They are
> also known as base tables. The bottom line is that they are simply variables
> whose values are relations. I will simply refer to them here as tables, as
> that is what you will be dealing with in SQL.

Figure 3-5

A relation is a value, a table is a variable to which relations are assigned.

> Tables, like variables, have both a name and a value.  Their name is just a
> symbol. Their value is a relation. They are no different than variables in
> algebra, such as x and y in the equation of a line. Tables share all the
> properties of relations (heading, degree, cardinality, etc.) just as integer
> variables share the properties of integers.

SQL update on a table produces a new relation!!

#### Views: Virtual Tables
*Views are relational expressions that yield relations*

Codd:
> A view is a virtual relation (table) defined by means of an expression or
> sequence of commands. Although not directly supported by actual data, a view
> appears to a user as if were an additional base table kept up-to-date and in
> the state of integrity with other base tables. Views are useful for permitting
> application programs and users at terminals to interact with constant view
> structures, even when the base tables themselves are under- going structural
> changes at the logical level...

##### Logical Data Independence

Codd:
> 9. Logical Data Independence. Application programs and terminal activities
>    remain logically unimpaired when information-preserving changes of any kind
>    that theoretically permit un-impairment are made to the base tables.


Am example is when a relation is decomposed into two relations for the sake of
normalization.

##### Update-able Views

> 6. View Updating Rule. All views that are theoretically update-able are also
>    update-able by the system.

i.e. changes to a view should be able to be mapped to the logical layer.  Should
because it is pretty difficult to design

### The System Catalog

> 4. Dynamic On-Line Catalog Based on the Relational Model. The data base
>    description is represented at the logical level in the same way as ordinary
>    data, so that authorized users can apply the same relational language to
>    its interrogation as they apply to the regular data.

Information about the information a database holds.

## The Integrity Component
*Holy shit* I am only *this* far?!

### Primary Keys

> 2. Guaranteed Access Rule. Each and every datum (atomic value) in a relational
>    data base is guaranteed to be logically accessible by resorting to a
>    combination of table name, primary key value and column name.

The primary key is the set of attributes in a relation that uniquely identifies
each tuple within it.

> 1.The value (or combined values) of that attribute (or attributes) is unique
> for every tuple in the relation.  

> 2.If the key is composed of more than one attribute, all of the attributes
> that define the key must be necessary to ensure uniqueness. That is, every
> attribute in the key is sufficient to ensure uniqueness, but also necessary as
> well—if one were absent, then the uniqueness condition would not hold.

If both 1 and 2 are met then the resulting attribute or group of attributes is a
key ( or candidate key).

If condition 1 is met but not condition 2, then the attribute (or group ) is
called a superkey. ( a key that could stand to lose some weight)

### Foreign Keys

foreign key relationship
- a keys identification property allows a tuple in one relation to identify (or
  reference) a specific tuple in another relation by way of a common key value.

Figure 3-6

### Constraints
They enforce database rules and relationships and preserve order in general.
four general classes of integrity:

> - Domain integrity: Domain integrity is the relationship between attribute
> values and their associated domains. In the relational model, domain integrity
> is instituted through domain constraints. A domain constraint requires that each
> attribute value in a tuple exist within its associated domain. For example, if
> the type_id attribute in the foods table is declared as an integer, then the
> corresponding values of type_id in all tuples in the foods table must be integer
> values—not floating-point numbers or strings. Domain integrity is also referred
> to as attribute integrity—it pertains to the attributes of a relation. Domain
> integrity is not limited to just checking that a given value resides in a given
> domain.  It includes additional constraints as well, such as CHECK constraints.
> CHECK constraints (which are covered in Chapter 4) can define arbitrarily
> complex rules on what constitutes a permissible value for a given attribute.
> 
> - Entity integrity: This form of integrity is mandated by the Guaranteed Access
> Rule: each tuple in a relation must be uniquely identifiable. Whereas domain
> integrity is concerned with a relation’s attribute values, entity integrity is
> concerned with its tuples. The term "entity" here is a rather loose term for
> table. It originates from database modeling (i.e., entity-relationship
> diagrams). In this particular context, an entity simply refers to anything in
> the real world that must be represented in a database.
> 
> - Referential integrity: This form of integrity pertains to relationships between
> tables, specifically the preservation of foreign key relationships. Whereas
> entity integrity pertains to tuples in a relation, referential integrity
> pertains to tuples between relations.
> 
> - User-defined integrity: User-defined integrity encompasses any form of
> integrity not defined in the other forms. Many relational databases offer
> various facilities that go beyond the normal constraint mechanisms. One such
> example is triggers, which are covered in Chapter 4.

Codd:

> 10. Integrity Independence. Integrity constraints specific to a particular
>     relational data base must be definable in the relational data sub-language
>     and storable in the catalog, not in the application programs.

### Null values

Codd:
> 3. Systematic Treatment of Null Values. Null values (distinct from the empty
>    character string or a string of blank characters and distinct from zero or
>    any other number) are supported in fully relational DBMS for representing
>    missing information and inapplicable information in a systematic way,
>    independent of data type.


> In this case, the null value doesn't necessarily mean “unknown” but rather
> “not applicable.” Thus, a value may be null because it is either missing (a
> value exists but was not input) or uncertain (it is not known whether a value
> exists at all), or it is simply not applicable for the tuple (or employee) in
> question.


> Date, Codd’s colleague and well-known authority on the relational model, is
> opposed to them:
> > ...we should make it very clear that in our opinion (and in that of many
> > other writers too, we hasten to add), nulls and 3VL are a serious mistake
> > and have no place in a clean formal system like the relational model.

## Normalization
The concern of organization of attributes within relations so as to minimize the
duplication of data.

### Functional Dependencies
When the values of one column(s) correspond to the values in another column.

Referencing Table 3-2
Season is functionally dependent on year and vice versa.


### Normal Forms

#### First Normal Form (1NF)
All attributes in a relation use domains that are made up of atomic values.

#### Second Normal Form (2NF)
Requires the relation to be in 1NF and that all non-key attributes be
functionally dependent upon *all* attributes of the primary key. 

#### Third Normal Form (3NF)
Roots out what is called transitive dependencies.

Transitive dependency
- a chain of tow or more functional dependencies spanning two or more attribute
  groups (two or more correlations).

See Table 3-5 for example.

## The Manipulative Component
relational algebra - procedural language
relational calculus - declarative language

### Relational Query Language

Any query language that can express all of the fundamental operations set forth
in relational algebra and/or calculus is said to be *relationally complete*.

> 5. Comprehensive Data Sublanguage Rule. A relational system may support
>    several languages and various modes of terminal use (for example, the
>    fill-in-the-blanks mode). However, there must be at least one language
>    whose statements are expressible, per some well-defined syntax, as
>    character strings and that is comprehensive in supporting all the following
>    items:

> Data Definition, View Definition, Data Manipulation (Interactive and by
> program), Integrity Constraints, and Authorization, Transaction boundaries
> (begin, commit, and rollback).


> 7. High Level Insert, Update, and Delete. The system must support set at a
>    time insert, update, and delete operators.

#### The Advent of SQL

DDL (Data Definition Language)
- The part of the language that is dedicated to working with the structural
  aspect of the model, specifically to creating, altering, and destroying
  tables.  Also contains the integrity aspect (specifying keys and constraints)

DML (Data Manipulation Language)
- The operational aspect that includes both the relational calculus and algebra.


> 0. Rule Zero. For any system that is advertised as, or claimed to be, a
>    relational data base management system, that system must be able to manage
>    data bases entirely through its relational capabilities.


