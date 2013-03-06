Definitions
Problem and Context
History -> RDBMS -> Relational

Data models, schemas

ER model, erd
relational
implementation
SQLITE
SQL

DDL
DML

Schema design and Normalization


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
sequential access / appending data
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

What is a schema?
------
structure of the database along with its constraints and rules
- meta data on a database

containt
-----
rules
They enforce database rules and relationships and preserve order in general.

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
    CREATE TABLE machines(id integer not null primary key);
    CREATE TABLE join(machine_id, snack_id);
    
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

    insert into join(machine_id, snack_id) values( 3,2);

If the snack having an id of 7 is no longer available in the machine having the id of 3, what DML statement would you execute?
------

    delete from join where machine_id = 3 and snack_id = 7;

Remove a snack with id 42 from the database. (Assume no FK constraints exist.)
------

    delete from snacks where id = 42;
    delete from join where snack_id = 42;

Modify the names of all snacks in the database having the specific name “Yucky Muck”  such that they have a new name, “Yummy Yum.”
-----

    update snacks set name = "Yummy Yum" where name = "Yucky Muck";

Modify the calories of all snacks associated with vending machine 3, setting the snack calories to 2000.
------

    update snacks set calories = 2000 where id in (select snacks.id as id from
    snacks left outer join ( select * from join_table  left outer join machines
    on join_table.machine_id = machines.id));

Other Stuff
=======
ACID
----
- Atomicity
    - database needs to perform operations that are atomic
    - all or nothing, can't be broken down
    - Entirely succeeds or fails
- Consistency
    - every transaction leaves the database in a valid state
- Isolation
    - independent
    - all transactions, especially during concurrency, must execute in isolation
- Durability
    - Once a transaction is done, the results are permanent

Explain the syntax of transactions including <code>begin, commit, rollback</code>
-----
<strong>Three commands/parts for a transaction</strong>
- begin
    - starts a transaction. 
    - every operation after a begin can potentially be undone
- commit
    - commits the work performed by all operations since the beginning of the
      transaction
- rollback
    - undoes all work since the begin command

<strong>additional commands</strong>
- <code>savepoint</code>
    - instead of rolling back the entire transaction we can roll back to a
      particular save point
    - I.e. 
        
        begin;
        savepoint justincase;
        --- ... do stuff
        release [savepoint] justincase;        
        rollback [transaction] to justincase;
        commit;

- release

> The <code>RELEASE</code> command is like a <code>COMMIT</code> for a
> <code>SAVEPOINT</code>. The <code>RELEASE</code> command causes all
> <code>savepoints</code> back to and including the most recent
> <code>savepoint</code> with a matching name to be removed from the transaction
> stack. The <code>RELEASE</code> of an inner transaction does not cause any
> changes to be written to the database file; it merely removes
> <code>savepoints</code> from the transaction stack such that it is no longer
> possible to ROLLBACK TO those <code>savepoints</code>. If a
> <code>RELEASE</code> command <code>releases</code> the outermost
> <code>savepoint</code>, so that the transaction stack becomes empty, then
> <code>RELEASE</code> is the same as <code>COMMIT</code>. The
> <code>COMMIT</code> command may be used to <code>release</code> all
> <code>savepoints</code> and <code>commit</code> the transaction even if the
> transaction was originally started by a <code>SAVEPOINT</code> command instead
> of a BEGIN command.

## Index

    create index MAJOR_IDX on STUDENT( MajorId );




