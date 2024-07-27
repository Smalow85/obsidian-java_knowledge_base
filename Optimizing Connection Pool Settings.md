#### Pool Size Settings

1. Initial and Minimum Pool Size -> Size of the pool when created, and its minimum allowable size
2. Maximum Pool Size -> Upper limit of size of the pool.
3. Pool Resize Quantity -> Number of connections to be removed when the idle timeout expires. Connections that have idled for longer than the timeout are candidates for removal. When the pool size reaches the initial and minimum pool size, removal of connections stops.
#### Timeout Settings

- **Max Wait Time**: Amount of time the caller (the code requesting a connection) will wait before getting a connection timeout. The default is 60 seconds. A value of zero forces caller to wait indefinitely.
    
    To improve performance set Max Wait Time to zero (0). This essentially blocks the caller thread until a connection becomes available. Also, this allows the server to alleviate the task of tracking the elapsed wait time for each request and increases performance.
    
- **Idle Timeout**: Maximum time in seconds that a connection can remain idle in the pool. After this time, the pool can close this connection. This property does not control connection timeouts on the database server.
    
    Keep this timeout shorter than the database server timeout (if such timeouts are configured on the database), to prevent accumulation of unusable connection in Application Server.
    
    For best performance, set Idle Timeout to zero (0) seconds, so that idle connections will not be removed. This ensures that there is normally no penalty in creating new connections and disables the idle monitor thread. However, there is a risk that the database server will reset a connection that is unused for too long.

#### Isolation Level Settings

Two settings control the connection pool’s transaction isolation level on the database server:

- **Transaction Isolation Level**: specifies the transaction isolation level of the pooled database connections. If this parameter is unspecified, the pool uses the default isolation level provided by the JDBC Driver.
    
- **Isolation Level Guaranteed**: Guarantees that every connection obtained from the pool has the isolation specified by the Transaction Isolation Level parameter. Applicable only when the Transaction Isolation Level is specified. The default value is true.
    
    This setting can have some performance impact on some JDBC drivers. Set to false when certain that the application does not change the isolation level before returning the connection.
    

Avoid specifying Transaction Isolation Level. If that is not possible, consider setting Isolation Level Guaranteed to false and make sure applications do not programmatically alter the connections’ isolation level.

If you must specify one of [[isolation levels]], specify the best-performing level possible. The isolation levels listed from best performance to worst are:

1. `READ_UNCOMMITTED`
    
2. `READ_COMMITTED`
    
3. `REPEATABLE_READ`
    
4. `SERIALIZABLE`

