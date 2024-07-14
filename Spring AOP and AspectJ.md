Simply put, [[Spring AOP]] and AspectJ have different goals.

Spring AOP aims to provide a simple AOP implementation across Spring IoC to solve the most common problems that programmers face. **It is not intended as a complete AOP solution** – it can only be applied to beans that are managed by a Spring container.

On the other hand, **AspectJ is the original AOP technology which aims to provide complete AOP solution.** It is more robust but also significantly more complicated than Spring AOP. It’s also worth noting that AspectJ can be applied across all domain objects.

### Weaving

AspectJ makes use of three different types of weaving:

1. **Compile-time weaving**: The AspectJ compiler takes as input both the source code of our aspect and our application and produces a woven class files as output
2. **Post-compile weaving**: This is also known as binary weaving. It is used to weave existing class files and JAR files with our aspects
3. **Load-time weaving**: This is exactly like the former binary weaving, with a difference that weaving is postponed until a class loader loads the class files to the JVM

Spring AOP makes use of runtime weaving.

With runtime weaving, the aspects are woven during the execution of the application using proxies of the targeted object – using either JDK dynamic proxy or CGLIB proxy (which are discussed in next point):

![[Pasted image 20240714231008.png]]

### Internal Structure and Application

Spring AOP is a **proxy-based AOP framework**. This means that to implement aspects to the target objects, it’ll create proxies of that object. This is achieved using either of two ways:

1. JDK dynamic proxy – the preferred way for Spring AOP. Whenever the targeted object implements even one interface, then JDK dynamic proxy will be used
2. CGLIB proxy – if the target object doesn’t implement an interface, then CGLIB proxy can be used

**AspectJ, on the other hand, doesn’t do anything at runtime as the classes are compiled directly with aspects.**

### Joinpoints

Spring AOP is based on proxy patterns. Because of this, it needs to subclass the targeted Java class and apply cross-cutting concerns accordingly.

But it comes with a limitation. **We cannot apply cross-cutting concerns (or aspects) across classes that are “final” because they cannot be overridden and thus it would result in a runtime exception.**

The same applies for static and final methods. Spring aspects cannot be applied to them because they cannot be overridden. Hence Spring AOP because of these limitations, only supports method execution join points.

However, **AspectJ weaves the cross-cutting concerns directly into the actual code before runtime.** Unlike Spring AOP, it doesn’t require to subclass the targetted object and thus supports many others joinpoints as well. Following is the summary of supported joinpoints:

|Joinpoint|Spring AOP Supported|AspectJ Supported|
|---|---|---|
|Method Call|No|Yes|
|Method Execution|Yes|Yes|
|Constructor Call|No|Yes|
|Constructor Execution|No|Yes|
|Static initializer execution|No|Yes|
|Object initialization|No|Yes|
|Field reference|No|Yes|
|Field assignment|No|Yes|
|Handler execution|No|Yes|
|Advice execution|No|Yes|
