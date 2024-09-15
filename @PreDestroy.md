A method annotated with `@PreDestroy` runs only once, just before Spring removes our bean from the application context.

Same as with [[@PostConstruct]], the methods annotated with `@PreDestroy` can have any access level, but can’t be static.

```java
@Component
public class UserRepository {

    private DbConnection dbConnection;
    @PreDestroy
    public void preDestroy() {
        dbConnection.close();
    }
}
```

The purpose of this method should be to release resources or perform other cleanup tasks, such as closing a database connection, before the bean gets destroyed.