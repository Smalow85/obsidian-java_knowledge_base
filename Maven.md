Building a software project typically consists of such tasks as downloading dependencies, putting additional jars on a classpath, compiling source code into binary code, running tests, packaging compiled code into deployable artifacts such as JAR, WAR, and ZIP files, and deploying these artifacts to an application server or repository.

[Apache Maven](https://maven.apache.org/)  is a [[Build automation tool]], that automates these tasks, minimizing the risk of humans making errors while building the software manually and separating the work of compiling and packaging our code from that of code construction.

The key features of Maven are:

- **simple project setup that follows best practices:** Maven tries to avoid as much configuration as possible, by supplying project templates (named _archetypes_)
- **dependency management:** it includes automatic updating, downloading and validating the compatibility, as well as reporting the dependency closures (known also as transitive dependencies)
- **isolation between project dependencies and plugins:** with Maven, project dependencies are retrieved from the _dependency repositories_ while any plugin’s dependencies are retrieved from the _plugin repositories,_ resulting in fewer conflicts when plugins start to download additional dependencies
- **central repository system:** project dependencies can be loaded from the local file system or public repositories, such as [Maven Central](https://search.maven.org/)

## Project Object Model

The configuration of a Maven project is done via a _Project Object Model (POM)_, represented by a _pom.xml_ file. The _POM_ describes the project, manages dependencies, and configures plugins for building the software.

```xml
<project> 
	<modelVersion>4.0.0</modelVersion> 
	<groupId>com.baeldung</groupId> 
	<artifactId>baeldung</artifactId> 
	<packaging>jar</packaging> 
	<version>1.0-SNAPSHOT</version> 
	<name>com.baeldung</name> 
	<url>http://maven.apache.org</url> 
	<dependencies> 
		<dependency> 
			<groupId>org.junit.jupiter</groupId> 
			<artifactId>junit-jupiter-api</artifactId> 
			<version>5.8.2</version> 
			<scope>test</scope> 
		</dependency> 
	</dependencies> 
	<build> 
		<plugins> 
			<plugin> //... </plugin> 
		</plugins> 
	</build> 
</project>
```

### Project Identifiers

Maven uses a set of identifiers, also called coordinates, to uniquely identify a project and specify how the project artifact should be packaged:

- _groupId_ – a unique base name of the company or group that created the project
- _artifactId_ – a unique name of the project
- _version_ – a version of the project
- _packaging_ – a packaging method (e.g. _WAR_/_JAR_/_ZIP_)

The first three of these (_groupId:artifactId:version_) combine to form the unique identifier and are the mechanism by which you specify which versions of external libraries (e.g. JARs) your project will use.
### Dependencies

These external libraries that a project uses are called dependencies. The dependency management feature in Maven ensures the automatic download of those libraries from a central repository, so you don’t have to store them locally.

This is a key feature of Maven and provides the following benefits:

- **uses less storage by significantly reducing the number of downloads off remote repositories**
- **makes checking out a project quicker**
- **provides an effective platform for exchanging binary artifacts within your organization and beyond without the need for building artifacts from source every time**
```XML
<dependency> 
	<groupId>org.springframework</groupId> 
	<artifactId>spring-core</artifactId> 
	<version>5.3.16</version> 
</dependency>
```
### Repositories

A repository in Maven is used to hold build artifacts and dependencies of varying types. The default local repository is located in the _.m2/repository_ folder under the home directory of the user.

If an artifact or a plugin is available in the local repository, Maven uses it. Otherwise, it is downloaded from a central repository and stored in the local repository. The default central repository is [Maven Central](https://mvnrepository.com/search?q=centra).

Some libraries, such as the JBoss server, are not available at the central repository but are available at an alternate repository. For those libraries, you need to provide the URL to the alternate repository inside the _pom.xml_ file:

```xml
<repositories>
    <repository>
        <id>JBoss repository</id>
        <url>http://repository.jboss.org/nexus/content/groups/public/</url>
    </repository>
</repositories>
```

==Please note that you can use multiple repositories in your projects.==

### Properties

Custom properties can help to make your _pom.xml_ file easier to read and maintain. In the classic use case, you would use custom properties to define versions for your project’s dependencies.

Maven properties are value-placeholders and are accessible anywhere within a *pom.xml* by using the notation **${name}**, where _name_ is the property.

```xml
<properties>
    <spring.version>5.3.16</spring.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```

Properties are also often used to define build path variables:

```xml
<properties>
<project.build.folder>${project.build.directory}/tmp/</project.build.folder>
</properties>

<plugin>
    //...
    <outputDirectory>${project.resources.build.folder}</outputDirectory>
    //...
</plugin>
```
### Build

The _build_ section is also a very important section of the Maven _POM._ It provides information about the default Maven _goal_, the directory for the compiled project, and the final name of the application.

```xml
<build>
    <defaultGoal>install</defaultGoal>
    <directory>${basedir}/target</directory>
    <finalName>${artifactId}-${version}</finalName>
    <filters>
      <filter>filters/filter1.properties</filter>
    </filters>
    //...
</build>
```

### Profiles

Another important feature of Maven is its support for _profiles._ A _profile_ is basically a set of configuration values. By using _profiles_, you can customize the build for different environments such as Production/Test/Development:

```xml
<profiles>
    <profile>
        <id>production</id>
        <build>
            <plugins>
                <plugin>
                //...
                </plugin>
            </plugins>
        </build>
    </profile>
    <profile>
        <id>development</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <build>
            <plugins>
                <plugin>
                //...
                </plugin>
            </plugins>
        </build>
     </profile>
 </profiles>
```

As you can see in the example above, the default profile is set to _development_. If you want to run the _production_ _profile_, you can use the following Maven command:

```bash
mvn clean install -Pproduction
```
## Maven Build Lifecycles

Every Maven build follows a specified _lifecycle_. You can execute several build _lifecycle_ _goals_, including the ones to _compile_ the project’s code, create a _package,_ and _install_ the archive file in the local Maven dependency repository.
### 1. Lifecycle Phases

The following list shows the most important Maven _lifecycle_ phases:

- _validate_ – checks the correctness of the project
- _compile_ – compiles the provided source code into binary artifacts
- _test_ – executes unit tests
- _package_ – packages compiled code into an archive file
- _integration-test_ – executes additional tests, which require the packaging
- _verify_ – checks if the package is valid
- _install_ – installs the package file into the local Maven repository
- _deploy_ – deploys the package file to a remote server or repository

### 2. Plugins and Goals

A Maven _plugin_ is a collection of one or more _goals_. Goals are executed in phases, which helps to determine the order in which the _goals_ are executed.

To go through any one of the above phases, we just have to call one command:

```bash
mvn <phase>
```

For example, _mvn clean install_ will remove the previously created jar/war/zip files and compiled classes (_clean)_ and execute all the phases necessary to install the new archive (_install)_.

**A Maven plugin is a group of goals**; however, these goals aren’t necessarily all bound to the same phase.

For example, here’s a simple configuration of the Maven Failsafe plugin, which is responsible for running integration tests:

```xml
<build>
    <plugins>
        <plugin>
            <artifactId>maven-failsafe-plugin</artifactId>
            <version>${maven.failsafe.version}</version>
            <executions>
                <execution>
                    <goals>
                        <goal>integration-test</goal>
                        <goal>verify</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

As we can see, the Failsafe plugin has two main goals configured here:

- _integration-test_: run integration tests
- _verify_: verify all integration tests passed


