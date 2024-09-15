Using the [[Inversion of Control]] principle, **Spring collects bean instances from our application and uses them at the appropriate time.** We can show bean dependencies to Spring without handling the setup and instantiation of those objects.

_@Component_ is an annotation that allows Spring to detect our custom beans automatically.

In other words, without having to write any explicit code, Spring will:

- Scan our application for classes annotated with _@Component_
- Instantiate them and inject any specified dependencies into them
- Inject them wherever needed

Spring has provided a few specialized stereotype annotations: [[@Controller]], [[@Service]] and [[@Repository]]. They all provide the same function as _@Component_.

They all act the same because they are all composed annotations with _@Component_ as a meta-annotation for each of them. They are like _@Component_ aliases with specialized uses and meaning outside Spring auto-detection or dependency injection.
Before we rely entirely on _@Component_, we must understand that it’s only a plain annotation. The annotation serves the purpose of differentiating beans from other objects, such as domain objects.

However, Spring uses the [[@ComponentScan]] annotation to gather them into its [[ApplicationContext]].
## _@Component_ vs _@Bean_

[[@Bean]] is also an annotation that Spring uses to gather beans at runtime, but it’s not used at the class level. Instead, we annotate methods with _@Bean_ so that Spring can store the method’s result as a Spring bean.

- _@Component_ is a class-level annotation, but _@Bean_ is at the method level, so _@Component_ is only an option when a class’s source code is editable. _@Bean_ can always be used, but it’s more verbose.
- _@Component_ is compatible with Spring’s auto-detection, but _@Bean_ requires manual class instantiation.
- Using _@Bean,_ decouples the instantiation of the bean from its class definition. This is why we can use it to make third-party classes into Spring beans. It also means we can introduce logic to decide which of several possible instance options for a bean to use.



