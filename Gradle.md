**Gradle** is an open source [[Build automation tool]] that is based on the concept of Apache [[Maven]] and [[Apache Ant]].

Gradle describes everything on the basis of **projects** and **tasks**. Every **Gradle build** contains one or more projects, and these projects contain some tasks.
## Gradle Build
The **Gradle build** is a process of creating a Gradle project. When we run a gradle command, it will look for a file called **build.gradle** in the current directory. This file is also called **the Gradle build script**. The build configuration, tasks, and plugins are described in this file. The build script describes a project and its tasks.

![[Pasted image 20240618204026.png]]

It is the default structure of a Gradle project. Gradle will generate the following things for us:

1. The **gradle** file is build script for configuring the current project.
2. An **executable JAR** file is used as a Gradle wrapper.
3. **Configuration properties** for Gradle Wrapper.
4. The **gradlew** is a Gradle wrapper script for UNIX based OS.
5. The **bat** is the Gradle Wrapper script for Windows.
6. **The settings script** for configuring the Gradle build.

The build.gradle file is build script of a Gradle project. All the tasks and plugins are defined in this file.

When we run a gradle command, it looks for a file called build.gradle in the current directory. Although we have called it a build script, strictly, it is a build configuration script. The build script defines a project and its tasks.

The default **build.gradle** file looks like as follows:

![Gradle Build](https://static.javatpoint.com/tutorial/gradle/images/gradle-build9.png)

The **build.gradle** file contains three default sections. They are as follows:

- **plugins:** In this section, we can apply the java-library plugin to add support for java library.
- **Repositories:** In this section, we can declare internal and external repository for resolving dependencies. We can declare the different types of repository supported by Gradle like Maven, Ant, and Ivy.
- **Dependencies:** In this section, we can declare dependencies that are necessary for a particular subject.
## Gradle Dependencies

Gradle build script describes a process of building projects. Most of the projects are not self-contained. They need some files to compile and test the source files. For example, to use Hibernate, we must include some Hibernate JARs in the classpath. Gradle uses some unique script to manage the dependencies, which needs to be downloaded.

Dependency configuration is a set of dependencies and artifacts. Following are three main tasks of configuration:

- Declaring dependencies
- Resolving dependencies
- Exposing artifacts for consumptions
### Declaring Dependency

Dependency is an essential part of any project. We must declare a dependency to use it. Dependency configuration is a process of defining a set of dependencies. This feature is used to declare external dependencies, which we want to download from the web.

```groovy
apply plugin: 'java.'   
repositories {  
	mavenCentral()  
}  
dependencies {  
	compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'  
	testCompile group: 'junit', name: 'junit', version: '4.+'  
}
```

Any dependency can be used on different phases of the project. These phases can be:

- **Compile:** At compile time, we will use the dependencies that are required to compile the production source of the project.
- **Runtime:** These dependencies are used at runtime by production classes. By default, it also contains the compile-time dependencies.
- **Test Compile:** These dependencies are required to compile the test source of the project. It also contains the compiled production classes and the compile-time dependencies.
### Dependency Management

A software project is a collection of various functionality. It rarely works in isolation. In most cases, a project depends on the reusability of libraries. Also, a project can be divided into separate components to form a modularized system. Dependency management is a process for declaring, resolving, and using dependencies required by the project in an automated fashion. The figure below demonstrates the structure of a Gradle project.

![[Pasted image 20240618204846.png]]

Gradle provides built-in support for dependency management. In Gradle, dependency management is made up of two things. They are as follows:

- Gradle must know the requirements of the project to build or to run a project. These files are said as the dependencies of the project.
- Gradle needs to build and upload data that is produced by a project. These files are the declaration of the project.

In Gradle, mostly projects are not independent. Projects need files that were built by other projects for compilation or to test and more. For example, if we want to use the Hibernate framework in a project, we would need the **hibernate jar file** in the classpath at compile time. These files are said as the dependency of the project. In Gradle, we can specify the dependencies of a project, and Gradle focuses on finding these dependencies and make it available in the project. We can download these dependencies from remote Maven or Ivy repository, or we can build our dependency in a project and include it. This process is known as **dependency resolution**.

Dependency resolution provides an advantage over Ant. Using Ant, we can specify absolute or relative paths of jars to load. Comparatively, in Gradle, we have to declare the name of the dependencies to define the dependency. Also, Ant reflects the similar behavior when we add Apache Ivy, so Gradle is better in this case.

The dependency of a project itself behaves as a dependency; for example, in hibernate-core, we require many libraries that must be available on the classpath. So when the Gradle test runs a project, it searches the dependencies and makes it available. These dependencies are known as **transitive dependencies**.

## Gradle Tasks

In Gradle, Task is a single unit of work that a build performs.**These tasks can be compiling classes, creating a JAR, Generating Javadoc,** and **publishing some archives** to a repository and more. It can be considered as a single atomic piece of work for a build process.

There are two types of tasks in Gradle. They are as following:
- Default task
- Custom task
Default tasks are predefined tasks of Gradle. We can define it in our projects. Gradle let us define one or more default tasks that are executed if no other tasks are specified.

In Gradle, a project is a collection of one or more tasks.

A simple task can be defined as:

```groovy
task hello {
    doLast {
        println 'Baeldung'
    }
}
```

## Gradle Wrapper

The Gradle wrapper allows us to run a build with a specified version and settings without the Gradle installation. This wrapper can be considered as a batch script on Windows and shell script for other OS.

- Gradle wrapper standardizes a project on a specified Gradle version, and it leads to more reliable and robust builds.
- The Gradle wrapper provides the same Gradle version to different users, and the execution environment is as simple as changing the Wrapper definition.

## Multi-Project Builds

We can execute the _gradle init_ command in the root folder to create a skeleton for both _settings.gradle_ and _build.gradle_ file.

```groovy
allprojects {
    repositories {
        mavenCentral() 
    }
}

subprojects {
    version = '1.0'
}
```

The setting file needs to include the root project name and subproject name:

```groovy
rootProject.name = 'multi-project-builds'
include 'greeting-library','greeter'
```

Now we need to have a couple of subproject folders named _greeting-library_ and _greeter_ to have a demo of a multi-project build. Each subproject needs to have an individual build script to configure its individual dependencies and other necessary configurations.


