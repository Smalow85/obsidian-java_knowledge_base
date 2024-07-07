Java is one of the most popular programming languages worldwide. It was created by James Gosling and Patrick Naughton, employees of Sun Microsystems, with support from Bill Joy, co-founder of Sun Microsystems.

Sun officially presented the Java language at SunWorld on May 23, 1995. Then, in 2009, the Oracle company bought the Sun company, which explains why the language now belongs to Oracle.

Java is described as being a multi-purpose, strongly typed, and [[Object-oriented programming]] (OOP) language.
## Features

Thanks to its excellent features, Java has become a popular and useful programming language. Sun characterized it as being:

- Compiled and Interpreted
- Platform Independent and Portable
- Object-Oriented
- Robust and Secure
- Distributed
- Familiar, Simple and Small
- Multi-threaded and Interactive
- High Performance
- Dynamic and Extensible

### 1. Compiled and Interpreted

Java combines the power of compiled languages with the flexibility of interpreted languages.

Compiler (**javac**) compiles the source code into **bytecode** then the Virtual Machine ([[JVM]]) executes this bytecode by transforming it into machine-readable code.
### 2. Platform Independent and Portable

The two-step compilation process is what lies behind Java’s most significant feature: platform independence, which allows for portability.

Being platform-independent means **a program compiled on one machine can be executed on any other machine, regardless of the OS**, as long as there is a JVM installed.

The portability feature refers to the ability to run a program on different machines. In fact, **the same code will run identically on different platforms**, regardless of hardware compatibility or operating systems, with no changes such as recompilation or tweaks to the source code.
### 4.3. Object-Oriented

Java strongly supports Object-Oriented Programming concepts such as encapsulation, abstraction, and inheritance.
All the instructions and data in a Java program have to be added inside a class or object.
### 4.4. Robust and Secure

Java includes several useful features that help us write robust and secure applications.
One of the most important ones is the memory management system, along with automatic [[Garbage collection]]. Compared to languages like C/C++, Java avoids the concept of explicit pointers, and doesn’t require programmers to manually manage the allocated memory.
Instead, the **GC** will take care of deleting unused objects to free up memory.
In addition, Java is a strongly-typed language, which is a feature that can help lower the number of bugs in an application, and provides error handling mechanisms.
### 4.5. Distributed

This feature is helpful when we develop large projects. We can split a program into many parts and store these parts on different computers. As a result, we can **easily create distributed and scalable applications that run on multiple nodes**.

We can achieve this using the concept of [RMI (Remote Method Invocation)](https://www.baeldung.com/java-rmi) and [EJB (Enterprise JavaBeans)](https://www.baeldung.com/ejb-intro).

### 4.6. Simple and Familiar

First, Java is simple thanks to its coding style, which is very clean and easy to understand. Also, it doesn’t use complex and difficult features of other languages, such as the concept of explicit pointers.

Finally, Java is familiar since it’s based on existing languages like C++ and incorporates many features from these languages.

### 4.7. Multi-Threaded and Interactive

Also known as Thread-based Multitasking, multithreading is a feature that allows executing multiple threads simultaneously.

In short, we can write Java programs that deal with many tasks at once by defining multiple threads. The advantage of multithreading is that it **doesn’t occupy memory for each thread – all threads share a common memory area**.

### 4.8. High Performance

Bytecodes that the compiler generates are highly optimized, so the Virtual Machine can execute them much faster. This is why Java is **faster than other traditional interpreted programming languages**.

### 4.9. Dynamic and Extensible

This feature gives the facility of dynamically linking new class libraries, methods, and objects. Java is highly dynamic as it can adapt to its evolving environment.