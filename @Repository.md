[[@Service]] and `@Repository` are special cases of [[@Component]]. They are technically the same, but we use them for the different purposes.

_@Repository_’s job is to catch persistence-specific exceptions and re-throw them as one of Spring’s unified unchecked exceptions.

For this, Spring provides `PersistenceExceptionTranslationPostProcessor`, which we are required to add in our application context (already included if we’re using [[Spring Boot]]):

```xml
<bean class=
  "org.springframework.dao.annotation.PersistenceExceptionTranslationPostProcessor"/>
```

This bean post processor adds an advisor to any bean that’s annotated with _@Repository._