Each [[Spring Boot]] application includes an embedded server. Embedded server is embedded as a part of deployable application. The advantage of embedded server is, we do not require pre-installed server in the environment. With Spring Boot, default embedded server is [[Tomcat]]. Spring Boot also supports another two embedded servers:

- Jetty Server
- Undertow Server

Without embedded server, you would need to:

- Install Java
- Install Web Server
- Deploy application artififact to web server

How can we make it simpler?

An interesting approach would be to make the server a part of your application. In that case, there are just two steps: install the right version of Java, and run the application.

```xml
<properties>
	<servlet-api.version>3.1.0</servlet-api.version>
</properties>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
