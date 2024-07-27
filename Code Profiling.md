Java Profiler is a tool that **monitors Java bytecode constructs and operations at the JVM level**. These code constructs and operations include object creation, iterative executions (including recursive calls), method executions, thread executions, and garbage collections.

Pros:

- Great for debugging tasks and tracking down memory leaks
- Show a large amount of details — but relevance may depend on debugging task

Features:

- Tracking all the JVM details (CPU, threads, memory, garbage collection)
- Track all method calls with memory usage and and allow dive into the call structure
- Track details of all memory usage with responsible classes/objects
- CPU sampling feature to track and aggregate CPU time by class and method to help zero in on hot spots
- Allow to manually run garbage collection and review memory consumption (great way to find classes and processes that are holding on to memory in error)

Implementation details:

- A direct connection to the JVM is used (not favorable in production use)
- May slow down the application or require downtime (not favorable in production use)

Examples: VisualVM, JProfiler, YourKit, Java Mission Control
## JProfiler

With an intuitive UI, JProfiler provides interfaces for viewing system performance, memory usage, potential memory leaks, and thread profiling. With this information, we can easily see what we need to optimize, eliminate, or change in the underlying system.

![[Pasted image 20240723212901.png]]

JProfiler also provides **advanced profiling for both SQL and NoSQL databases**. It provides specific support for profiling JDBC, JPA/Hibernate, MongoDB, Casandra, and HBase databases.

![[Pasted image 20240723213822.png]]

Live Memory is one feature of JProfiler that allows us to **see current memory usage by our application**. We can view memory usage for object declarations and instances, or for the full call tree.

![[Pasted image 20240723213918.png]]

## YourKit

Like JProfiler, YourKit has core features for visualizing threads, garbage collections, memory usage, and memory leaks with **support for local and remote profiling via ssh tunneling**.

## Java VisualVM

Java VisualVM is a simplified, yet robust profiling tool for Java applications. This is a free, open-source profiler. Now distributed as a standalone tool: [VisualVM Download](https://visualvm.github.io/download.html).
Its operation relies on other standalone tools provided in the JDK, such as _JConsole_, _jstat_, _jstack_, _jinfo_, and _jmap_.

Below we can see a simple overview interface of an ongoing profiling session using Java VisualVM:
![[Pasted image 20240723214506.png]]

One interesting advantage of Java VisualVM is that we can **extend it to develop new functionalities as plugins**. With the snapshot feature of Java VisualVM, we can **take snapshots of profiling sessions for later analysis**.



