One of the most common problems in multithreaded applications is the problem of race conditions.

Race condition is a condition of a program where its behavior depends on relative timing or interleaving of multiple threads or processes.
_Thread-safe_ is the term we use to describe a program, code, or data structure free of race conditions when accessed by multiple threads.

Given unpredictable thread scheduling, the order of specific steps is arbitrary.
## Types of race conditions

### 1. Check-Then-Act

This is the most common type of race condition. **It’s defined by a program flow where a potentially stale observation is used to decide what to do next.** We refer to the bugs produced by this condition as _Time-of-check to time-of-use_ or _TOCTOU_ bugs.

![[Pasted image 20240623231034.png]]

### 2. Read-Modify-Write

In most languages, the regular increment operator represents three sequential operations — read, modify, and write.

![[Pasted image 20240623231205.png]]

## Elimination
There’re two kinds of approaches to **fight race conditions**:
- Avoiding shared state
- Using synchronizations and atomic operations
### 1. Avoiding Shared State

Immutable objects, whose state cannot be changed after construction, are inherently thread-safe. **Using immutable objects as much as possible is always advisable.**

Thread-local variables, localized in such a way that each thread has its private copy, are also thread-safe since they are local to each thread.

For check-then-act race conditions, one general technique is to use exception handling instead of checking. In this approach, the failure of an assumption to hold is detected at use time, raising an exception. As the familiar saying goes, it’s easier to ask for forgiveness than permission.

More radical treatment is using a concurrency model prohibiting shared state altogether, such as actor-based concurrency.

### 2. Using Synchronizations and Atomic Operations

 _Lock_ is a synchronization mechanism to enforce critical section behavior at the thread level. A _mutex_ is the same abstraction existing across multiple system processes.

**While synchronization is the most powerful way to get rid of the race condition, it comes at a price.** Locks bring us a performance hit due to their overhead. They also may be tricky to handle. Locks do not compose, meaning it requires extra effort to preserve correctness when combining lock-based modules into a larger lock-based program. Finally, locks may introduce [[deadlocks]].

_Atomic operation_ implies its execution as a single unit of work. Specific guarantee depends on the language in use, but the main idea is that the implementation relies on hardware’s ability to prevent interrupts until the execution is over.

We may use atomic operations to implement higher-level lock-free abstractions. Software Transaction Memory – yet another concurrency model – leverages the concept to enable database-like transactions on the operations in memory.
