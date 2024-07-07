[[PostgreSQL]] does not use IN-PLACE update mechanism, so as per the way DELETE and UPDATE command is designed:
- *Whenever DELETE operations are performed, it marks the existing tuple as DEAD instead of physically removing those tuples.*
- *Similarly, whenever UPDATE operation is performed, it marks the corresponding existing tuple as DEAD and inserts a new tuple (i.e. UPDATE operation = DELETE + INSERT).*

**VACUUM** is the maintenance process which takes care of dealing with DEAD tuple along with a few more activities useful for optimizing VACUUM operation.

The VACUUM primary job is to ==reclaim storage space occupied by DEAD tuples==. Reclaimed storage space is not given back to the operating system rather they are just defragmented within the same page, so they are just available to be re-used by future data insertion within the same table. While VACUUM operation going on a particular table, concurrently other READ/WRITE operation can be done on the same table as exclusive lock is not taken on the particular table. In-case a table name is not specified, VACUUM will be performed on all tables of the database. The VACUUM operation performs below a series of operation within a ShareUpdateExclusive lock:

- Scan all pages of all tables (or specified table) of the database to get all dead tuples.
- Freeze old tuples if required.
- Remove the index tuple pointing to the respective DEAD tuples.
- Remove the DEAD tuples of a page corresponding to a specific table and reallocate the live tuples in the page.
- Update Free space Map (FSM) and Visibility Map (VM).
- Truncate the last page if possible (if there were DEAD tuples which got freed).
- Update all corresponding system tables.

## Full VACUUM

Even though VACUUM removes all DEAD tuples and defragment the page for future use, it does not help in reducing the overall storage of the table as space is actually not released to the operating system. 
Full VACUUM solves this problem by actually freeing space and returning it back to the operating system. But this comes at a cost. Unlike VACUUM, FULL VACUUM does not allow parallel operation as it takes an exclusive lock on the relation getting FULL VACUUMed. Below are the steps:

- Takes exclusive lock on the relation.
- Create a parallel empty storage file.
- Copy all live tuples from current storage to newly allocated storage.
- Then free up the original storage.
- Free up the lock.

Full VACUUM should be avoided unless it is known that the majority of storage space is because of dead tuples. PostgreSQL extension pg_freespacemap can be used to get a fair hint about free space.