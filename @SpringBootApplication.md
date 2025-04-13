```java
@SpringBootApplication  
public class SpringBootDemoApplication {  
	public static void main(String[] args) {  
		SpringApplication.run(SpringBootDemoApplication.class, args);  
	}  
}
```

`@SpringBootApplication` is combination of several annotations like

- [[@EnableAutoConfiguration]]
- @SpringBootConfiguration
- [[@ComponentScan]]
