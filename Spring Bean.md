## 1. Bean lifecycle

The lifecycle of any object means when & how it is born, how it behaves throughout its life, and when & how it dies.

![[Pasted image 20240710230512.png]]

Bean life cycle is managed by the spring container. When we run the program then, first of all, the spring container gets started. After that, the container creates the instance of a bean as per the request, and then dependencies are injected. And finally, the bean is destroyed when the spring container is closed.

Sometimes we want to initialize resources in the bean classes, for example creating database connections or validating third party services at the time of initialization before any client request. [[Spring Framework]] provide different ways through which we can provide post-initialization and pre-destroy methods in a spring bean life cycle.

1. By implementing **InitializingBean** and **DisposableBean** interfaces - Both these interfaces declare a single method where we can initialize/close resources in the bean. For post-initialization, we can implement `InitializingBean` interface and provide implementation of `afterPropertiesSet()` method. For pre-destroy, we can implement `DisposableBean` interface and provide implementation of `destroy()` method. These methods are the callback methods and similar to servlet listener implementations. This approach is simple to use but it’s not recommended because it will create tight coupling with the Spring framework in our bean implementations.
2. Providing `init()` and `destroy()` attribute values for the bean in the spring bean configuration file. This is the recommended approach because of no direct dependency to spring framework and we can create our own methods.

#### @PostConstruct

Spring calls the methods annotated with _@PostConstruct_ only once, just after the initialization of bean properties. 

```java
@Component
public class DbInit {

    @Autowired
    private UserRepository userRepository;

    @PostConstruct
    private void postConstruct() {
        User admin = new User("admin", "admin password");
        User normalUser = new User("user", "user password");
        userRepository.save(admin, normalUser);
    }
}
```

#### @PreDestroy

A method annotated with _@PreDestroy_ runs only once, just before Spring removes our bean from the application context. 

Same as with _@PostConstruct_, the methods annotated with _@PreDestroy_ can have any access level, but can’t be static.

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
