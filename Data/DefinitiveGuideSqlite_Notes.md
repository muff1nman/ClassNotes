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



    

