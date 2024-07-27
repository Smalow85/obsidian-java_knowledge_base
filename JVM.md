JVM (Java Virtual Machine) is an abstract machine. It is a specification that provides runtime environment in which [[Java]] bytecode can be executed.
### What is JVM

1. **A specification** where working of Java Virtual Machine is specified. But implementation provider is independent to choose the algorithm. Its implementation has been provided by Oracle and other companies.
2. **An implementation** Its implementation is known as JRE (Java Runtime Environment).
3. **Runtime Instance** Whenever you write java command on the command prompt to run the java class, an instance of JVM is created.

Being a virtual machine means the JVM is an abstraction of an underlying, actual machine—such as the server that your program is running on. Regardless of what operating system or hardware is actually present, the JVM creates a predictable environment for programs to run within. Unlike a true virtual machine, however, the JVM doesn't create a virtual operating system. It would be more accurate to describe the JVM as a _managed runtime environment_, or a _process virtual machine_.
### Java class loader

The **Java class loader** is the part of the JVM that loads classes into memory and makes them available for execution. Class loaders use techniques like lazy-loading and caching to make class loading as efficient as it can be.

There are three built-in classloaders in Java.

1. **Bootstrap ClassLoader**: This is the first classloader which is the super class of Extension classloader. It loads the _rt.jar_ file which contains all class files of Java Standard Edition like java.lang package classes, java.net package classes, java.util package classes, java.io package classes, java.sql package classes etc.
2. **Extension ClassLoader**: This is the child classloader of Bootstrap and parent classloader of System classloader. It loades the jar files located inside _$JAVA_HOME/jre/lib/ext_ directory.
3. **System/Application ClassLoader**: This is the child classloader of Extension classloader. It loads the classfiles from classpath. By default, classpath is set to current directory. You can change the classpath using "-cp" or "-classpath" switch. It is also known as Application classloader.

### Execution engine

The _execution engine_ is the JVM component that handles this function. The execution engine is essential to the running JVM. In fact, for all practical purposes, it _is_ the JVM instance.

The JVM execution engine stands between the running program—with its demands for file, network, and memory resources—and the operating system, which supplies those resources.

1. **A virtual processor**
2. **Interpreter:** Read bytecode stream then execute the instructions.
3. **Just-In-Time(JIT) compiler:** It is used to improve the performance. JIT compiles parts of the byte code that have similar functionality at the same time, and hence reduces the amount of time needed for compilation. Here, the term "compiler" refers to a translator from the instruction set of a Java virtual machine (JVM) to the instruction set of a specific CPU.
# Internal Structure of JVM

![[Pasted image 20240613230810.png]]

There are three main mechanisms inside the JVM as shown in the above diagram.

- ClassLoader
- Memory Area
- Execution Engine

**ClassLoader** is responsible for loading class file into the memory area. The classloader is an abstract class, generates the data which constitutes a definition for the class using a class binary name which is constituted of the package name, and a class name. The classLoading mechanism consists of three main steps as follows.
- Loading
- Linking
- Initialization
## Loading

Whenever JVM loads a file, it will load and read,

- Fully qualified class name
- Variable information (instance variables)
- Immediate parent information
- Whether class or interface or enum

When the class is loaded to the JVM, it creates an object of class type and is put into the heap area. This class type object will be created only the very first time the class is loaded to the JVM.

## Linking

This is the process of linking the data in the class file into the memory area. It begins with **verification** to ensure this class file and the compiler.

1. Make sure this compiler is valid
2. The class file has the correct formating
3. The class file has the correct structure

When considering security threads, there is a possibility that some programs like malware can change or add some content to the class file. In this case, it will be identified by the byte code verifier and throw an exception called “verify exception”.

When the verification process is done, the next step is **preparation**. In this stage, all the variables are initialized with the default value. As an example, it will assign 0 for int variables, null for all objects, false for all boolean variables, etc.

Java allows you to use any domain-specific words in your java program. As an example, you can create a class by giving any name (except java keywords). But the JVM cannot understand these words. Therefore, JVM replaces these words from a memory address. That process is called **resolution**. At the end of these three steps, the java file is loaded to the memory area.
## Initialization

This is the final stage of class loading. In this stage, the actual values are assigned to all static and instance variables. There is a rule that every class must be initialized before doing any active use. There are six active uses as follows.

1. Use `new` keyword.
2. Invoke static methods.
3. Assign values for static fields.
4. Initialize class.
5. Use `getInstance()` in Reflection API.
6. Instantiation of sub-class.

There are 4 ways to initialize a class in Java.

1. New Keyword: Goes through the normal initialization process.
2. Reflection API: `getInstance()` method and goes through the normal initialization process.
3. Clone Method: Gets the information from the source object.
4. IO.ObjectInputStream: Gets data from non-transient variables passed in the parameter.

Now you got a clear idea about the inside mechanism of the ClassLoader. There are three types of ClassLoaders as shown below.

- **Bootstrap ClassLoader** — Load classes from JRE/lib/rt.jar
- **Extension ClassLoader** — Load classes from JRE/lib/ext
- **Application/System ClassLoader** — Load classes from classpath, -cp, Manifest

# Memory Area

This is the place data will store until the program is executed. It consists of 5 major components as follows.

![[Pasted image 20240615133420.png]]

##### Method Area

This is a shared resource (only 1 method area per JVM). Method area stores class-level information such as

- ClassLoader reference
- Run time constant pool
- Constructor data — Per constructor: parameter types (in order)
- Method data — Per method: name, return type, parameter types (in order), modifiers, attributes
- Field data — Per field: name, type, modifiers, attributes

##### Heap Area

All the objects and their corresponding instance variables are stored in the heap area. As an example, if your program consists of a class called “Employee” and you are declaring an instance as follows,

```java
Employee employee = new Employee();
```

When the class is loaded, there is an instance of `employee` is created and it will be loaded into the Heap Area.

##### Stack Area

There are separate stack areas for each thread. The stack is responsible for hold the methods (method local variables, etc) and whenever we invoke a method, a new frame creates in the stack. These frames use **Last-In-First-Out** structure. They will be destroyed when the method execution is completed.
##### Program Counter (PC) Register

PC Register keeps a record of the current instruction executing at any moment. That is like a pointer to the current instruction in a sequence of instructions in a program. Once the instruction is executed, the PC register is updated with the next instruction. If the currently executing method is ‘native’, then the value of the program counter register will be undefined.

##### Native Method Area

A native method is a Java method that is implemented in another programming language, such as C or C++. This memory area is responsible to hold the information about these native methods.
# Execution Engine (additional info)

At the end of the loading and storing mechanisms, the final stage of JVM is executing the class file. Execution Engine has three main components.

1. Interpreter
2. JIT Compiler
3. Garbage Collector

## Interpreter

When the program is started to execute, the interpreter reads byte code line by line. This process will use some sort of dictionary that implies this kind of byte code should be converted into this kind of machine instructions. The main advantage of this process is the interpreter is very quick to load and fast execution.

But whenever it executes the same code blocks (methods, loops, etc) again and again, the interpreter executes repeatedly. It means the interpreter cannot be optimized the run time by reducing the same code repeated execution.
## JIT Compiler

Just In Time Compiler (JIT Compiler) is introduced to overcome the main disadvantage of the interpreter. That means the JIT compiler can remember code blocks that execute again and again. As an example, there is a class called “Employee” and `getEmployeeID()`method is declared in this class. If a program uses `getEmployeeID()` method 1000 times, each time it is executed by the interpreter. But the JIT compiler can identify those repeated code segments and they will be stored as native codes in the cache. Since it is stored, next time JIT compiler uses the native code which stored in the cache.
## Garbage Collector (GC)

As I already explained, all the objects are stored in the heap area before JVM starts the execution. . Since this area is limited, it is required to manage this area efficiently by removing the objects that are no longer in use. The process of removing unused objects from heap memory is known as [[Garbage collection]] and this is a part of memory management in Java. There are two ways the objects are considered as no longer in use.

1. When the reference is pointed to a null value.

```java
Employee employee = new Employee();  
employee = null;
```

Here the reference `employee` was pointing to the object of class ‘Employee’ and then a null is assigned to the reference. Since that moment, there is no reference pointing to the Employee object. Then this object is considered as a no longer reachable object.

2. When the reference is pointed to another object

```java
Employee employee1 = new Employee(); //object 1  
Employee employee2 = new Employee(); //object 2  
employee1 = employee2;
```

Since the reference `employee1`is pointed to `employee2`, object 1 is considered as longer reachable.

Actually, the GC is a daemon thread that runs in the background. This process has two steps. First, the GC identifies the unused objects in memory (Mark), and then it removes the objects identified during the first step (Sweep).

Even though the garbage collection happens automatically, the programmer can trigger by calling `System.gc()` method.