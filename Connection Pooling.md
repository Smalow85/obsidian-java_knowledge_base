**At the most basic level,** **a connection pool is a database connection cache implementation** that can be configured to suit specific requirements.

The sequence of steps involved in a typical database connection life cycle:

1. Opening a connection to the database using the database driver
2. Opening a [TCP socket](https://en.wikipedia.org/wiki/Network_socket) for reading/writing data
3. Reading / writing data over the socket
4. Closing the connection
5. Closing the socket

It becomes evident that **database connections are fairly expensive operations**, and as such, should be reduced to a minimum in every possible use case (in edge cases, just avoided).

Here’s where connection pooling implementations come into play.

By just simply implementing a database connection container, which allows us to reuse a number of existing connections, we can effectively save the cost of performing a huge number of expensive database trips. This boosts the overall performance of our database-driven applications.
### Connection Pooling Frameworks

1. Apache Commons DBCP
2. HikariCP

```java
public class HikariCPDataSource {
    
    private static HikariConfig config = new HikariConfig();
    private static HikariDataSource ds;
    
    static {
        config.setJdbcUrl("jdbc:h2:mem:test");
        config.setUsername("user");
        config.setPassword("password");
        config.addDataSourceProperty("cachePrepStmts", "true");
        config.addDataSourceProperty("prepStmtCacheSize", "250");
        config.addDataSourceProperty("prepStmtCacheSqlLimit", "2048");
        ds = new HikariDataSource(config);
    }
    
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }
    
    private HikariCPDataSource(){}
}
```

Here’s how to get a pooled connection with the _HikariCPDataSource_ class:

```java
Connection con = HikariCPDataSource.getConnection();
```

