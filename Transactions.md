A `transaction` in SQL is a unit of work that is performed against a database. Transactions are units or sequences of work accomplished in a logical order, whether in a manual fashion by a user or automatically by some sort of a database program.

Transactions are used to ensure data integrity and to handle database errors while processing. SQL transactions are controlled by the following commands:

- `BEGIN TRANSACTION`
- `COMMIT`
- `ROLLBACK`
## BEGIN TRANSACTION

This command is used to start a new transaction.

```sql
BEGIN TRANSACTION; 
```
## COMMIT

The `COMMIT` command is the transactional command used to save changes invoked by a transaction to the database.

```sql
COMMIT;
```

When you commit the transaction, the changes are permanently saved in the database.
## ROLLBACK

The `ROLLBACK` command is the transactional command used to undo transactions that have not already been saved to the database.

```sql
ROLLBACK;
```

When you roll back a transaction, all changes made since the last commit in the database are undone, and the database is rolled back to the state it was in at the last commit.
## Transaction Example

```sql
BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 100 WHERE id = 1;
UPDATE Accounts SET Balance = Balance + 100 WHERE id = 2;

IF @@ERROR = 0
   COMMIT;
ELSE
   ROLLBACK;
```

During its lifecycle, a database transaction goes through multiple states. These states are called **transaction states** and are typically one of the following:

1. **Active states:** It is the first state during the execution of a transaction. A transaction is _active_ as long as its instructions (read or write operations) are performed.
2. **Partially committed:** A change has been executed in this state, but the database has not yet committed the change on disk. In this state, data is stored in the memory buffer, and the buffer is not yet written to disk.
3. **Committed:** In this state, all the transaction updates are permanently stored in the database. Therefore, it is not possible to rollback the transaction after this point.
4. **Failed:** If a transaction fails or has been aborted in the active state or partially committed state, it enters into a _failed_ state.
5. **Terminated state:** This is the last and final transaction state after a committed or aborted state. This marks the end of the database transaction life cycle.

![[Pasted image 20240512225038.png]]

In [[Relational databases]] transactions must be atomic, consistent, isolated and durable. These properties are commonly abbreviated as [[ACID]]. ACID properties ensure that a database transaction is processed reliably. In this section, let’s learn a bit more about what these properties mean for the application.