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
    SELECT heading
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
- <code>savepoint</code>
    - instead of rolling back the entire transaction we can roll back to a
      particular save point
    - I.e. 
        
        savepoint justincase;
        --- ... do stuff
        rollback [transaction] to justincase;
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
- pending
    - see below
- shared
    - read
    - no session can write will there are shared locks
- reserved
    - the first phase of writing to a database
    - does not prevent shared locks
    - can start writing but changes are stored in a memory cache
    - can only be one reserved lock at a time
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

    attach [database] filename as database_name;    detach [database] database_name;

    select * from db2.foo;

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

Chapter 5
---------

### Executing Prepared Queries
#### Preparation
The parser, tokenizer and code generator compile the sql statement in VDBE byte
code.

    sqlite3_prepare_v2()

#### Execution
Step through the byte code with the <code>sqlite3_step()</code> command
For each call, it returns SQLITE_ROW or SQLITE_DONE.

#### Finalization
Release resources

    sqlite3_finalize()

### Flow
See figure 5-2

    db = open('foods.db');
    stmt = db.prepare( 'select * from episodes' );A

    while stmt.step() == SQLITE_ROW
        print stmt.colum('name');
    end

    stmt.finalaize()

### Parameters

    insert into foods(id, name) values (?, ?);
    insert into epixodes(id, name) values (:id, :name);

    stmt.biind('id', '1' );
    stmt.step()
    stmt.reset() -- dellocates statments resources but leaves VDBE byte code

### Operational Control
Allows you to monitor, control or generally limit what can happen in a database

#### Hook functions
- <code>sqlite3_commit_hook</code>
    - monitors transaction commits on a connection
- <code>sqlite3_rollback_hook()</code>
    - monitors rollbacks
- <code>sqlite3_update_hook()</code>
    - monitors changes to rows from insert, update, and delete
- <code>wal_hook()</code>
- <code>sqlite3_set_authorizer()</code>


    def commit_hook(cnx)
        log('Attemped commit on connection %x', cnx )
        return -1 # causes rollback
    end
    db = open('foods.db')
    db.set_commit_hook(rollback_hook, cnx)
    db.exec("begin; delete from episodes; rollback")
    db.close()

### The Extension API

#### User Defined Functions

    void hello_newman( sqlite3_context* ctx, int nargs, sqlite3_value** values)
    {
        /* Create Newman's reply */
        const char* msg = "Hello Jerry";

        /* Set the return value. */
        sqlite3_result_text( ctx, msg, strlen(msg), SQLITE_STATIC);
    }

    sqlite3_create_function( db, "hello_newman", 0, hello_newman);

    /* 
     * 1. db: database connection
     * 2. the name of the function as it will appear in sql
     * 3. How many args
     * 4. function pointer for the actual function
     */

#### User Defined Aggregates
1. Register the aggregate
2. implement a step function to be called for each record in the result set
3. implement a finalize function to be called after record processing - allows
   you to compute the final return value and do any necessary cleanup


        connection=apsw.Connection("foods.db")

        def step( context, *args):
            context['value'] += args[0]

        def finalize( context ):
            return context['value']

        def pysum():
            return ({'value' : 0}, step, finalize)

        connection.createaggregatefunction("pysum", pysum)

        c = connection.cursor()
        print c.execute("select pysum(id) from foods").next()[0]

#### Create User Defined Collations

    sqlite3_create_collation()

11 Sqlite Internals and New Features
------

### Manifest Typing, Storage Classes and Affinity

#### Manifest Typing
Could mean that variables types must be explicitly declared in the code. Or that
variables don't have types at all and only values have types.

Former is MT1
Latter is MT2

Sqlite is both MT1 and MT2 and neither.  haha

- MT1
    - columns can have types
    - column type can have influence on the value type
- MT2
    - columns don't have to have types
    - a value has some influence on the type

So we will call sqlite MT3

Sqlite does not include strict type checking

#### Type Affinity
- By default, a column's default affinity is numeric. That is, if a column is
  not integer, text, or none, then it is automatically assigned numeric
  affinity.

- If a column's declared type contains the string 'int' (in uppercase or
  lowercase), then the column is assigned integer affinity.

- If a column's declared type contains any of the strings 'char', 'clob', or
  'text' (in uppercase or lowercase), then that column is assigned text
  affinity. Notice that 'varchar' contains the string 'char' and thus will
  confer text affinity.

- If a column's declared type contains the string 'blob' (in uppercase or
  lowercase), or if it has no declared type, then it is assigned none affinity.

Note Pay attention to defaults. For instance, floatingpoint has affinity
integer. If you don’t declare a column’s type, then its affinity will be none,
in which case all values will be stored using their given storage class (or
inferred from their representation). If you are not sure what you want to put in
a column or want to leave it open to change, this is the best affinity to use.
However, be careful of the scenario where you declare a type that does not match
any of the rules for none, text, or integer. Although you might intuitively
think the default should be none, it is actually numeric. For example, if you
declare a column of type JUJYFRUIT, it will not have affinity none just because
SQLite doesn’t recognize it. Rather, it will have affinity numeric.
(Interestingly, the scenario also happens when you declare a column’s type to be
numeric for the same reason.) Rather than using an unrecognized type that ends
up as numeric, you may prefer to leave the column’s declared type out
altogether, which will ensure it has affinity none.

- A numeric column may contain all five storage classes. A numeric column has a
  bias toward numeric storage classes (integer and real). When a text value is
  inserted into a numeric column, it will attempt to convert it to an integer
  storage class. If this fails, it will attempt to convert it to a real storage
  class. Failing that, it stores the value using the text storage class.

- An integer column tries to be as much like a numeric column as it can. An
  integer column will store a real value as real. However, if a real value has
  no fractional component, then it will be stored using an integer storage
  class. integer column will try to store a text value as real if possible. If
  that fails, they try to store it as integer. Failing that, text values are
  stored as TEXT.

- A text column will convert all integer or real values to text.

- A none column does not attempt to convert any values. All values are stored
  using their given storage class.

- No column will ever try to convert null or blob values—regardless of affinity.
  null and blob values are always stored as is in every column.


#### Storage Classes and Type Conversions
you can manually convert with

    cast(3.14 as text)

### Write Ahead Logging (WAL)
Instead of writing the original to the rollback cache, the origial data is
untouched in the database file

    pragma journal_mode=WAL;
    sqlite3_wal_checkpoint();
    sqlite3_wal_autocheckpoint()
    OR pragma wal_autocheckpoint=
    sqlite3_wall_hook() -- callback for when a commit to the WAL happens

##### Advantages
- reader dont block writers and vice versa
- faster
- more predictable I/O

##### Disadvantages
- cannot use a networked filesystem because of shared memory
- two additional semi persistent files
- very large or long duration transactions introduce additional overhead
