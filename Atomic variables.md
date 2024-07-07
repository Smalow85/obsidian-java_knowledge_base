Shared mutable state very easily leads to problems when [[Java Concurrency]] is involved.
**If two threads try to get and update the value at the same time, it may result in lost updates.**

One of the ways to manage access to an object is to use locks. This can be achieved by using the [[Synchronized keyword]] in the _increment_ method signature. The _synchronized_ keyword ensures that only one thread can enter the method at one time.

**Using locks solves the problem. However, the performance takes a hit.**
**The process of suspending and then resuming a thread is very expensive** and affects the overall efficiency of the system.

## Atomic Operations

There is a branch of research focused on creating non-blocking algorithms for concurrent environments. These algorithms exploit low-level atomic machine instructions such as compare-and-swap (CAS), to ensure data integrity.

A typical CAS operation works on three operands:

1. The memory location on which to operate (M)
2. The existing expected value (A) of the variable
3. The new value (B) which needs to be set

**The CAS operation updates atomically the value in M to B, but only if the existing value in M matches A, otherwise no action is taken.**

In both cases, the existing value in M is returned. This combines three steps – getting the value, comparing the value, and updating the value – into a single machine level operation.

When multiple threads attempt to update the same value through CAS, one of them wins and updates the value. **However, unlike in the case of locks, no other thread gets suspended**; instead, they’re simply informed that they did not manage to update the value. The threads can then proceed to do further work and context switches are completely avoided.

## Atomic Variables in Java

The most commonly used atomic variable classes in Java are [AtomicInteger](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/atomic/AtomicInteger.html), [AtomicLong](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/atomic/AtomicLong.html), [AtomicBoolean](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/atomic/AtomicBoolean.html), and [AtomicReference](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/atomic/AtomicReference.html). These classes represent an _int_, _long_, _boolean,_ and object reference respectively which can be atomically updated. The main methods exposed by these classes are:

- _get()_ – gets the value from the memory, so that changes made by other threads are visible; equivalent to reading a variable, marked with [[Volatile keyword]] 
- _incrementAndGet() –_ Atomically increments by one the current value
- _set()_ – writes the value to memory, so that the change is visible to other threads; equivalent to writing a _volatile_ variable
- _lazySet()_ – eventually writes the value to memory, maybe reordered with subsequent relevant memory operations. One use case is nullifying references, for the sake of garbage collection, which is never going to be accessed again. In this case, better performance is achieved by delaying the null _volatile_ write
- _compareAndSet()_ – same as described in above section, returns true when it succeeds, else false
- _weakCompareAndSet()_ – same as described in above section, but weaker in the sense, that it does not create happens-before orderings. This means that it may not necessarily see updates made to other variables.

A thread-safe counter implemented with _AtomicInteger_ is shown in the example below:

```java
public class SafeCounterWithoutLock {
    private final AtomicInteger counter = new AtomicInteger(0);
    
    int getValue() {
        return counter.get();
    }
    
    void increment() {
        counter.incrementAndGet();
    }
}
```