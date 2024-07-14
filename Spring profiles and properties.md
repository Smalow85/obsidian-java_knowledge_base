## Configuration Must Be Environment Specific

Let’s start our Proof of Concept – by defining the environments we want to target:
- Dev
- Staging
- Production

Next – let’s create 3 properties files – one for each of these environments:

- `persistence-dev.properties`
- `persistence-staging.properties`
- `persistence-production.properties`

In a typical Maven application, these can reside in _src/main/resources_, but the wherever they are, they will need to be **available on the classpath** when the [[Spring Framework]]application is deployed.

Include the correct file based on the environment:

```java
@PropertySource({ "classpath:persistence-${envTarget:dev}.properties" })
```

This approach allows for the flexibility of having multiple _*.properties_ files for **specific, focused purposes**.

Deployable war **will contain all properties files** – for persistence, the three variants of _persistence-*.properties_. Since the files are actually named differently, there is no fear of accidentally including the wrong one. We will set the _envTarget_ variable and thus select the instance we want from the multiple existing variants.

The _envTarget_ variable can be set in the OS/environment or as a parameter to the JVM command line:

```bash
-DenvTarget=dev
```
