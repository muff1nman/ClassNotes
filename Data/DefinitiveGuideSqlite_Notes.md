The Definitive Guide to Sqlite
======

Chapter 4
------

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
- type
    - the type of the object
- name
    - duh
- rootpage
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
