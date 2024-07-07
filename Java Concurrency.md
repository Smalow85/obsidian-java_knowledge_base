## What is concurrency?

Concurrency is the ability to run several programs or several parts of a program in parallel. If a time consuming task can be performed asynchronously or in parallel, this improves the throughput and the interactivity of the [[Java]] program.

A modern computer has several CPU’s or several cores within one CPU. The ability to leverage these multi-cores can be the key for a successful high-volume application.
### Process vs. threads

A _process_ runs independently and isolated of other processes. It cannot directly access shared data in other processes. The resources of the process, e.g. memory and CPU time, are allocated to it via the operating system.

A _thread_ is a so called lightweight process. It has its own call stack, but can access shared data of other threads in the same process. Every thread has its own memory cache. If a thread reads shared data, it stores this data in its own memory cache.

A thread can re-read the shared data.

### Concurrency issues

Threads have their own call stack, but can also access shared data. Therefore you have two basic problems, visibility and access problems.

A **visibility problem** occurs if thread A reads shared data which is later changed by thread B and thread A is unaware of this change.

An **access problem** can occur if several threads access and change the same shared data at the same time.


