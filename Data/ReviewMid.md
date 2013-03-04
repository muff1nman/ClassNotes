Definitions
Problem and Context
History -> RDBMS -> Relational

Data models, schemas

ER model, erd
|
relational
implementation
SQLITE
SQL

DDL
DML

Schema design and Normalization
ACID
transactions
programming and APIs

Definitions
-----------
- whats a data model
- whats a schema

History
-------
- who invented relational model
- what were the two key features that set it apart

Data Models, Schemas
-------

Given ERD and extract meaning
ERD cheat sheet

short Paragraph of domain -> ERD
 - entity
 - relationship
 - degree of participation
 - cardinality

screen spec, sketch out the tables

count
aggregate functions

dont need or having, or group by

ACID

syntax of transactions
- begin, commit, rollback

programming
------
one question
 - open
 - prepare
 - execute
 - iterate over result set
 - close

Questions
--------
### What is data?
A known meaningful fact that is recorded.

### What is a database?
A collection of related data

### What is a DBMS
A collection of software tools, adhering to a data model, that is used to CRUD

### Who 
e.f. codd

### What are the two distinguishing aspects of the invention of the relational
### database?
1. Physical data independence: remove the structure from the physical stored
   banks
2. To create a query language based on relational algebra

### Describe the benefits and drawbacks of flat file vs RDBMS.
- drawbacks to flat file
    - searching
    - modifying
    - no logical/physical data independence
    - no locking/transactions/hardly any rdbms features

- benefits
    - small footprint
    - simplistic
    - fast

- drawbacks of RDBMS
    - overhead
        - space
        - software
        - computation
    - maintenance

- benefits
    - abstraction
    - concurrency
    - scales well
    - integrity

### 
embedded

data model
-----
framework describes how you can work with/ think about the data

er model
-------
model that represents the world as entities and relationships

er model vs erd
-----
cannot draw er model (framework)
erd is just a picture

schema
------
structure of the database along with its constraints and rules
- meta data on a database

containt
-----
rules

sql
----
structured query language

ddl
----
data definition language

dml
----
data manipulation language


