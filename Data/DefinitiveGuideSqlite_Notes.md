The Definitive Guide to Sqlite
======

Chapter 2
------
### Alter table

    alter table table { rename to name | add column column_def }

### Relational Operations

#### Fundamental operations
- Restriction
    - SQL where
- Projection
    - heading
    - SQL select
- Cartesian product
- Union
- Rename

#### Additional Operations
- Intersection
- Natural Join
- Assign

#### Extended operations
- Generalized projection
- Left outer join
- Right outer join
- Full outer join

#### Select and the operational pipeline

    select [distinct] heading
    from tables
    where predicate
    group by columns
    having predicate
    order by columns
    limit count,offset;

###### In order that they are applied

    FROM tables
    WHERE predicate
    GROUP BY column
    HAVING predicate
    heading
    DISTINCT
    ORDER BY columns
    LIMIT int, int; /* The command notation is short for LIMIT int, OFFSET int;*/

### SQLite operators
- ||
    - string concatenation
- <>
    - not equal to
- =
    - equal to
- AND
- OR
- IS
- NOT
- LIKE
    - for matching strings against patterns
    - % matches any sequence of zero or more characters
    - _ matches any single character
- GLOB

#### List of aggregate functions
- upper
- length
- abs
- count
- sum
- avg
- min
- max

#### Joins
- Inner Join
    - default
    - (AND)
- Cross Join
    - product
- Outer Join
    - LEFT
        - INNER UNION LEFT with NULL values
    - RIGHT
    - FULL
        - INNER UNION LEFT/RIGHT

##### syntax

    select * from foods, food_types where foods.id=food_types.food_id;
    select heading from left_table join_type right_table on join_condition;

### Case statements

    select name || case type_id
            when 7 then ' is a drink'
            when 8 then ' is a fruit'
            when 9 then ' is junkfood'
            when 13 then ' is seafood'
            else null
        end description
    from foods
    where description is not null
    order by name
    limit 10;

### NULL
> Most relational databases support the concept of “unknown” or “unknowable”
> through a special placeholder called null, which is a placeholder for missing
> information and is not a value per se. Rather, null is the absence of a value:
> null is not nothing, null is not something, null is not true, null is not
> false, null is not zero, null is not an empty string. Simply put, null is
> resolutely what it is: null.



Chapter 4
------

### Inserting multiple row

    insert into foods2 select * from foods;

    create table foods3 as select * from foods;

### Constraints
#### Column
- not null
- unique
- primary key
- foreign key
- check
- collate

### Unique
\_rowid\_ accesses the unique id for each row

### Default

    default ... i.e. current_timestamp

### Not null

### check constraints

    check predicate

### Foreign Key Constraints

    create table table_name
    ( column_definition references foreign_table (column_name)
    on {delete|update} integrity_action
    [not] deferrable [initially {deferred|immediate}. ] ....);

sdfs

#### Integrity Actions
- restrict
- set null
- set default
- cascade
- no action

#### Collations
They specify how the values are stored in the database
##### options
- binary default
- nocase
- reverse
- RTRIM

### Views

create view <em>name</em> as <em>select_stmt</em>;

drop view <em>name</em>

### Indexes

    create index [unique] <em>index_name</em> on <em>table_name</em> (<em>columns</em>)

    drop index <em>index_name</em>;

    create index foods_name_idx on foods (name collate nocase );

### Triggers

    create [temp|temporary] trigger <em>name</em>
    [before|after] 
    [insert|delete|update|update of <em>columns</em> on <em>table</em>
    action

#### Action

    begin
        insert into log values('updated foods: new name=' || newname);
    end;

Errors can also be raised:

    raise(resolution, error_message);

### Transactions
- <strong>atomic</strong>
    - all the commands execute together successfully or none at all

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
- savepoint
    - instead of rolling back the entire transaction we can roll back to a
      particular save point
    - i.e. 
        
        savepoint justincase;
        --- ... do stuff
        rollback [transaction] to justincase;
- release
    - 

<strong>auto-commit mode</strong> when SQLite automatically enclosed each
command in its own transaction. 

#### Conflict Resolution
Instead of undoing the commands, we can set up a alternate action to be done
when a transaction fails

<strong>five resolutions</strong>
- replace
    - When a unique constraint violation is encountered, SQLite removes the row
      (or rows) that caused the violation and replaces it (them) with the new
      row from the insert or update. The SQL operation continues without error.
      If a NOT NULL constraint violation occurs, the NULL value is replaced by
      the default value for that column. If the column has no default value,
      then SQLite applies the abort policy. It is important to note that when
      this conflict resolution strategy deletes rows in order to satisfy a
      constraint, it does not invoke delete triggers on those rows.  This
      behavior, however, is subject to change in a future release.
- ignore
    - leave the conflicting row unchanged and continue
- fail
    - fail at this point but keep changes up to this point
- abort *default*
    - restores all changes the command made and terminates it
    - most expensive
- rollback
    - perform a <code>rollback</code> or in other words, abort the current
      command and the entire transaction leaving the database unchanged

<strong>examples</strong>

    insert or resolution into table (column_list) values (value_list);
    update or resolution table set(value_list) where predicate;

*Note* ordering in SQLite is not implicit so it usually makes more sense to use
ignore than fail.

    create temp table cast(name text unique on conflict rollback);

So in the previous create table statement, the entire transaction will rollback
instead of the default abort.

Also, conflict resolution specified at the DML level overrides what is defined
in the DDL level. (resolution specified for insert operation overrides tables
resolution)

### Database Locks
<strong>lock states</strong>
- unlocked
    - no lock required
- shared
    - read
    - no session can write will there are shared locks
- reserved
    - the first phase of writing to a database
    - does not prevent shared locks
    - can start writing but changes are stored in a memory cache
- pending
    - when a session wants to start committing its changes it first must get
      this lock before moving onto the exclusive lock
    - no new shared locks can be obtained
    - waiting for existing shared locks to finish.
- exclusive
    - no shared locks
    - changes written to database

### Deadlock

#### Transaction Types
<strong>types</strong>
- deferred *default*
    - does not acquire any locks until it has to
    - begin starts as unlocked.
- immediate
    - begin starts to obtain a reserved lock
- exclusive
    - begin starts an exclusive lock

Using immediate and exclusive prevents deadlock for writes

    begin deferred transaction;

### Database Administration

#### Attaching Databases

    attach [database] filename as database_name;
    detach [database] database_name;

#### Cleaning Databases
<strong>reindex</strong> is used to rebuild indexes. 

    reindex collation_name;
    reindex table_name|index_name;

<strong>vacumm</strong> cleans out any unused space in the database by
rebuilding the database file. There is also an auto_vacuum pragma

#### Database Configuration

##### The Connection Cache Size
the cache size pragmas control how many database pages a session can hold in
memory

    pragma cache_size;
    pragma cache_size=10000;

you can permanently set the cache size for all sessions using the
default_cache_size pragma.

#### Database Information
pragma <em>keyword</em> 
- database_list
    - lists information about all attached databases
- index_info
    - lists information about the columns within an index
    - takes an index name as argument
- index_list
    - lists information about the indexes in a table (takes a table name as an
      arg)
- table_info
    - lists information about all the columns in a table
    - takes a table name as an argument

#### Synchronous Writes
<strong>settings</strong>
- full
    - database pauses at critical moments to ensure that the data has been
      written to disk
- normal
    - less often than full mode
    - small possiblity that a database failure could happen if power is cut
- off
    - no pauses.

#### Temporary Storage
temp_store determines whether SQLite uses memory or disk for temporary storage
- default (compile in default)
- file (OS file)
- memory (RAM)
temp_store_directory determines which directory is used to place the file if
that option is chosen

#### Page Size, Encoding, and Autovacuum
auto_vacuum occurs when a delete command happens

#### Debugging
integrity_check returns ok unless there are missing records, missing pages,
malformed records or corrupted indexes

#### The System Catalog
- <code>type</code>
    - the type of the object
- <code>name</code>
    - duh
- <code>rootpage</code>
    - the first B-tree page of the object in the database file

sqlite_master table holds this information

    select type, name, rootpage from sqlite_master;

column <code>sql</code> holds the DML used to create the object.

#### Viewing Query Plans
<code>explain query plan</code> will show what SQLite will do

5 Sqlite Design and Concepts
-----

### Transactions
see Figure 5-3

#### Read Transactions

    begin
    select * from episodes;
    select * from episodes;
    commit

For the above:

    UNLOCKED -> PENDING -> SHARED -> UNLOCKED

For the above without begin / commit

    UNLOCKED -> PENDING -> SHARED -> UNLOCKED -> PENDING -> SHARED -> UNLOCKED

*NOTE* a writer could sneak in between the latter's selects and modify the
database

#### Write Transactions
Before a transaction, the database copies the original database pages to the
rollback journal so that the database can rollback in case something goes wrong

<strong>page types</strong>
- unmodified pages
    - at most read by the database
- modified pages
    - database pages that contain modified records
    - *stored in the page cache*
- journal pages 
    - original versions of the modified pages
    - stored in the rollback journal

Once the journal is ensured to be written to disk, the modified pages are copied
into the database file. If the transaction is complete, the pager cleans up the
journal, clears the page cache and proceeds to UNLOCKED

Every autocommit modify goes through the following process flow

    UNLOCKED -> PENDING -> SHARED -> RESERVED -> PENDING -> EXCLUSIVE -> UNLOCKED

##### Sizing the Page Cache
Use <code>sqlite3_analyzer</code> to see how many pages a transaction might
affect

##### Shard Cache Mode

    pragma read_uncommitted;

allows a single writer and multiple readers in EXCLUSIVE mode by sharing the
cached.
