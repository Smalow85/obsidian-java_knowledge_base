
`pg_stat_activity` is a crucial system view in [[PostgreSQL]] that provides real-time information on current database connections and queries being executed.

The `pg_stat_activity` view contains several important fields, which include:

- `datid`: The OID of the database the backend is connected to.
- `datname`: The name of the database the backend is connected to.
- `pid`: The process ID of the backend.
- `usesysid`: The OID of the user who initiated the backend.
- `usename`: The name of the user who initiated the backend.
- `application_name`: The name of the application that is connected to the backend.
- `client_addr`: The IP address of the client connected to the backend.
- `client_port`: The port number of the client connected to the backend.
- `backend_start`: The timestamp when the backend was started.
- `xact_start`: The start time of the current transaction.
- `query_start`: The start time of the current query.
- `state_change`: The timestamp of the last state change.
- `state`: The current state of the backend (active/idle/idle in transaction).
- `query`: The most recent/currently running query of the backend.

## Common Uses

`pg_stat_activity` is commonly used for several monitoring and diagnostic purposes, such as:

- **Monitoring active queries:** To get a list of currently running queries, you can use the following query:
    
    ```sql
    SELECT pid, query, state, query_start
    FROM pg_stat_activity
    WHERE state = 'active';
    ```
    
- **Identifying idle transactions:** To detect idle transactions, which can cause performance issues, use this query:
    
    ```sql
    SELECT pid, query, state, xact_start
    FROM pg_stat_activity
    WHERE state = 'idle in transaction';
    ```
    
- **Terminating long-running queries:** To terminate specific long-running queries or backends, you can use the `pg_terminate_backend()` function. For example, to terminate a backend with the process ID `12345`:
    
    ```sql
    SELECT pg_terminate_backend(12345);
    ```
