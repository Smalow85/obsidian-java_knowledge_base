Logback is one of the most widely used logging frameworks in the Java Community. It’s a replacement for its predecessor, [[Log4j]].
## Logback Architecture

The Logback architecture is comprised of three classes: _Logger_, _Appender_, and _Layout_.

A _Logger_ is a context for log messages. This is the class that applications interact with to create log messages.

_Appenders_ place log messages in their final destinations. A _Logger_ can have more than one _Appender_. We generally think of _Appenders_ as being attached to text files, but Logback is much more potent than that.

```xml
<configuration> 
	<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender"> 
		<encoder>
			<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern> </encoder> 
		</appender> 

	<root level="debug"> 
		<appender-ref ref="STDOUT" /> 
	</root> 
</configuration>
```

This configuration and code give us a few hints as to how this works:

1. We have an _appender_ named _STDOUT_ that references class name _ConsoleAppender._
2. There is a pattern that describes the format of our log message.

A configuration file can be placed in the classpath and named either _logback.xml_ or _logback-test.xml._

Here’s how Logback will attempt to find configuration data:

1. Search for files named _logback-test.xml_ or _logback.xml_ in the classpath, in that order
2. If the library doesn’t find those files, it will attempt to use Java’s _[ServiceLoader](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ServiceLoader.html)_ to locate an implementor of the _com.qos.logback.classic.spi.Configurator._
3. Configure itself to log output directly to the console
## Loggers

```java
private static final Logger logger = LoggerFactory.getLogger(Example.class);
```

Then we use it:

```java
logger.info("Example log from {}", Example.class.getSimpleName());
```

This is our logging context. When we created it, we passed _LoggerFactory_ our class. This gives the _Logger_ a name (there is also an overload that accepts a _String)._ 

Logging contexts exist in a hierarchy that closely resembles the Java object hierarchy:

1. A _logger_ is an ancestor when its name, followed by a dot, prefixes a descendant _logger_‘s name
2. A _logger_ is a parent when there are no ancestors between it and a child
## Appenders

_Loggers_ pass _LoggingEvents_ to _Appenders._ _Appenders_ do the actual work of logging. We usually think of logging as something that goes to a file or the console, but Logback is capable of much more. _Logback-core_ provides several useful _appenders_.
### FileAppender

_FileAppender_ appends messages to a file. It supports a broad range of configuration parameters. Let’s add a file _appender_ to our basic configuration:

```xml
<configuration debug="true">
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <!-- encoders are assigned the type
             ch.qos.logback.classic.encoder.PatternLayoutEncoder by default -->
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>tests.log</file>
        <append>true</append>
        <encoder>
            <pattern>%-4relative [%thread] %-5level %logger{35} - %msg%n</pattern>
        </encoder>
    </appender>
    
    <logger name="com.baeldung.logback" level="INFO" /> 
    <logger name="com.baeldung.logback.tests" level="WARN"> 
        <appender-ref ref="FILE" /> 
    </logger> 

    <root level="debug">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
```

The _FileAppender_ is configured with a file name via _file_. The _append_ tag instructs the _Appender_ to append to an existing file rather than truncating it. If we run the test several times, we see that the logging output is appended to the same file.

## Layouts

Layouts_ format log messages. We’ve used _PatternLayout_ in all of our examples so far:

```xml
<encoder>
    <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
</encoder>
```



