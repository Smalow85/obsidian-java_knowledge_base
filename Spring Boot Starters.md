**Spring Boot** provides a number of **starters** that allow us to add jars in the classpath. Spring Boot built-in **starters** make development easier and rapid. **Spring Boot Starters** are the **dependency descriptors**.

In the Spring Boot Framework, all the starters follow a similar naming pattern: spring-boot-starter-, and after - goes name, that denotes a particular type of application.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
## Spring Boot Starter Parent

The spring-boot-starter-parent is a project starter. It provides default configurations for our applications. It is used internally by all dependencies. All Spring Boot projects use spring-boot-starter-parent as a parent in `pom.xml` file.

```xml
<parent>  
	<groupId>org.springframework.boot</groupId>  
	<artifactId>spring-boot-starter-parent</artifactId>  
	<version>1.4.0.RELEASE</version>  
</parent>
```

## Spring Initializr

**Spring Initializr** is a Web-based tool that provides simple web UI to generate the [[Spring Boot]] project structure or we can say it builds the skeleton of the Spring-based application.

The Spring Initializr UI has the following options,

- Project: Using this one can create [Maven or Gradle](https://www.geeksforgeeks.org/difference-between-gradle-and-maven/) project i.e; Maven or Gradle can be used as a build tool. The default option is Maven Project. Maven project is used in the entire tutorial. 
- Language: Spring Initializr provide [[Java]], [[Kotlin]] and [[Groovy]] as a programming language for the project. Java is the default option.
- Spring Boot Version: Using this one can select the Spring Boot version for their project. Spring Boot latest Version is 3.1.5. The SNAPSHOT versions are under development and are not stable.
- Project Dependencies: Dependencies are artifacts that we can add to the project.
- Project Metadata: It is the information about the project. 

Information in the metadata does include below key points:

- ****Group ID:**** It is the ID of the project group.
- ****Artifact****: It is the name of the Application.
- ****Name****: Application name.
- ****Description****: About the project.
- ****Package name****: It is the combination of Group and Artifact Id.
- ****Packaging****: Using this Jar or War packaging can be selected
- ****Generate:**** When generate option is clicked, the project is downloaded in zip format. The zip file can be unzipped and the project can be loaded into an IDE.
- ****Explore:**** This allows to see and makes changes in the generated project.