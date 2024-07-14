## What Is an Actuator?

The actuator mainly exposes operational information about the running [[Spring Boot]] application — health, metrics, info, dump, env, etc. It uses HTTP endpoints or JMX beans to enable us to interact with it. Once this dependency is on the classpath, several endpoints are available for us out of the box. As with most Spring modules, we can easily configure or extend it in many ways.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Only two available by default endpoints are _/health_ and _/info_. If we want to enable all of them, we could set `management.endpoints.web.exposure.include=*`

## Predefined Endpoints

Let’s have a look at some available endpoints, most of which were available in 1.x already.

Also, **some endpoints have been added, some removed, and some have been restructured**:

- _/auditevents_ lists security audit-related events such as user login/logout. Also, we can filter by principal or type among other fields.
- _/beans_ returns all available beans in our _BeanFactory_. Unlike _/auditevents_, it doesn’t support filtering.
- _/conditions_, formerly known as /_autoconfig_, builds a report of conditions around autoconfiguration.
- _/configprops_ allows us to fetch all _@ConfigurationProperties_ beans.
- _/env_ returns the current environment properties. Additionally, we can retrieve single properties.
- _/flyway_ provides details about our Flyway database migrations.
- _/health_ summarizes the health status of our application.
- _/heapdump_ builds and returns a heap dump from the JVM used by our application.
- _/info_ returns general information. It might be custom data, build information or details about the latest commit.
- _/liquibase_ behaves like _/flyway_ but for Liquibase.
- _/logfile_ returns ordinary application logs.
- _/loggers_ enables us to query and modify the logging level of our application.
- _/metrics_ details metrics of our application. This might include generic metrics as well as custom ones.
- _/prometheus_ returns metrics like the previous one, but formatted to work with a Prometheus server.
- _/scheduledtasks_ provides details about every scheduled task within our application.
- _/sessions_ lists HTTP sessions, given we are using Spring Session.
- _/shutdown_ performs a graceful shutdown of the application.
- _/threaddump_ dumps the thread information of the underlying JVM.

Spring Boot adds a discovery endpoint that returns links to all available actuator endpoints. This will facilitate discovering actuator endpoints and their corresponding URLs.

By default, this discovery endpoint is accessible through the `/actuator` endpoint.



