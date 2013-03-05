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

### Are there cases where storing data in a flat file are better than storing data in an RDBMS?
embedded
others?

What is a data model
-----
framework describes how you can work with/ think about the data

What is an er model
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

Given a relational schema: 

    CREATE TABLE snacks(id integer not null 
    primary key, sku TEXT, name TEXT, calories INTEGER)
    
answer the following.

What is the average number of calories contained in a snack? What is the min?Max?
----

    select avg(calories), min(calories), max(calories) from snacks;

What are the names of all snacks, in reverse alphabetical order?
----

    select name from snacks ORDER BY name DESC;

How many total calories are in one particular campus vending machine?
----

    select sum(calories) from snacks where id = ? group by id;

How many calories are contained in snacks with a name containing the word “Yum”?
------

    select calories from snacks where name like "%yum%";

Add a new snack to the database, without associating it with a vending machine.
-----

    insert into snacks( id, sku, name, calories) values( 1, "1", "Reese Pieces",
    300);

Associate a snack having the id of 2 with a vending machine having the id of 3.
----

If the snack having an id of 7 is no longer available in the machine having the id of 3, what DML statement would you execute?
------

    begin;
    insert into snacks(id, name, calloriers) values (7, "Oreos", 200);

    end;

Remove a snack with id 42 from the database. (Assume no FK constraints exist.)
------

    delete from snacks where id = 42;

Modify the names of all snacks in the database having the specific name “Yucky Muck”  such that they have a new name, “Yummy Yum.”
-----

    update snacks set name = "Yummy Yum" where name = "Yucky Muck";

Modify the calories of all snacks associated with vending machine 3, setting the snack calories to 2000.
------

    
