In [[PostgreSQL]], the **ANALYZE** command collects the statistics about a database, table, or table’s columns for the query planner. Afterward, the query planner utilizes that data to yield efficient/appropriate execution plans for the Postgres queries.

`ANALYZE` requires only a read lock on the target table, so it can run in parallel with other activity on the table.

There is no `ANALYZE` statement in the SQL standard.


