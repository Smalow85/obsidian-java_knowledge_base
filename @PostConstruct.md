Spring calls the methods annotated with `@PostConstruct` only once, just after the initialization of bean properties. Keep in mind that these methods will run even if there’s nothing to initialize.

The method annotated with `@PostConstruct` can have any access level, but it can’t be static.

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