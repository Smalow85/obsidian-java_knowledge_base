Log4J is a popular, open-source logging framework written in [[Java]]. Various Java-based applications widely use Log4j. Moreover, it’s thread-safe, fast, and provides a named _Logger_ hierarchy. Log4j is distributed under the open-source Apache Software License.

```xml
<dependency> 
	<groupId>org.apache.logging.log4j</groupId>
	<artifactId>log4j-api</artifactId> 
	<version>1.2.17</version> 
</dependency>
```

## Log4j Components

There are three main components of Log4j – _logger_, _appender_, and _layout_– that could be used together to print the customized log statements at the desired destinations. Let’s look at them in brief.

### 1. Logger

The _Logger_ object is responsible for representing the logging information. It’s the first mandatory layer in Log4j architecture.
Generally, we create one _Logger_ instance per application class to log important events belonging to that class. Also, we generally create this instance at the beginning of the class using a static factory method that accepts the class name as a parameter:

```java
private static final Logger logger = Logger.getLogger(JavaClass.class.getName());
```

The priority order of the _Logger_ methods is: _TRACE_ < _DEBUG_ < _INFO_ < _WARN_ < _ERROR_ < _FATAL_. Therefore, these methods print the log messages depending on the _logger_ level set in the _log4j.properties_ file. This means if we set the _logger_ level as _INFO_, then all _INFO_, _WARN_, _ERROR_, and _FATAL_ events will be logged.

### 2. Appender

_Appender_ denotes the destination of log output. We can print the log out to multiple preferred destinations using Log4j like console, files, remote socket server, database, etc.

Appenders work according to the **appender additivity rule**. This rule states that *the output of a log statement of any Logger will go to all of its appender and its ancestors*  - the appenders that are higher in the hierarchy.

### 3. Layout

We use layouts for customizing the format of the log statements. We can do this by associating a _layout_ with the already defined _appender_. Thus, a combination of _layout_ and appenders helps us send the formatted log statements to the desired destinations.

## log4j.properties

The default name of the log4j properties configuration file is _log4j.properties_. The _Logger_ looks for this file name in the **CLASSPATH**. However, if we need to use a different configuration file name, we can set it using the system property _log4j.configuration_.

```properties
# The root logger with appender name 
log4j.rootLogger = DEBUG, NAME
  
# Assign NAME a valid appender  
log4j.appender.NAME = org.apache.log4j.FileAppender

# Define the layout for NAME
log4j.appender.NAME.layout=org.apache.log4j.PatternLayout  
log4j.appender.NAME.layout.conversionPattern=%m%n  
```

Here, _NAME_ is the name of the _Appender_. As discussed earlier, we can attach multiple appenders to a _Logger_ to direct logs to different destinations.

```java
import org.apache.log4j.Logger;

public class Log4jExample {

    private static Logger logger = Logger.getLogger(Log4jExample.class);

    public static void main(String[] args) throws InterruptedException {
        for(int i = 1; i <= 2000; i++) {
            logger.info("This is the " + i + " time I say 'Hello World'.");
            Thread.sleep(100);
        }
    }
}
```

