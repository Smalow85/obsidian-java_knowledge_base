The **Write Ahead Log** is a technique where any modification to the data is first recorded in the log before being written into the main data storage. WAL ensures that any write operation is atomic, i.e., it either completes successfully or not at all. Atomicity is one of the key properties in [[ACID]] transactions _(Atomicity, Consistency, Isolation, and Durability)._
## How WAL Works

- **Write operation:** When a change is made to the data, PostgreSQL writes the changes to the WAL buffer instead of immediately modifying the disk pages.
- **Flush operation:** Once the transaction is committed, the WAL buffer contents are flushed to the on-disk WAL file.
- **Checkpoint:** The background writer process writes the ‘dirty’ pages from the shared buffer to the main data files at specific intervals called ‘checkpoints.’ It ensures that the actual data files are updated to match the state recorded in the WAL logs.

## Benefits of WAL

- **Recovery:** WAL ensures that the database can recover from a system crash or power failure by replaying the changes recorded in the WAL files.
- **Concurrency:** WAL improves concurrency and performance by allowing multiple transactions to proceed simultaneously without conflicting with each other.
- **Archive and Replication:** WAL files can be archived and used for point-in-time recovery, or it can be streamed to a standby server for a real-time backup or read-only queries.