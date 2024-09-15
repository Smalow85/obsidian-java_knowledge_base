**@Configuration** is one of [[Spring Annotations]] that indicates that a class declares one or more @Bean methods and may be processed by the Spring container to generate bean definitions and service requests for those beans at runtime.

```java
@Configuration
public class AppConfig {

	@Bean
	public MyService myService() {
		return new MyServiceImpl();
	}
}
```

To use the `@Configuration` annotation in a Spring Boot application, 1. In your main application class, annotate it with [[@EnableAutoConfiguration]] or [[@SpringBootApplication]] to enable Spring Boot’s auto-configuration features.

