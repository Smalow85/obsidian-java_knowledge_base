A monitor is a synchronization mechanism that allows threads to have:

- _mutual exclusion_ – only one thread can execute the method at a certain point in time, using _locks_
- _cooperation_ – the ability to make threads wait for certain conditions to be met

Why is this feature called “monitor”? Because **it monitors how threads access some resources.**
## Monitor Features

Monitors provide three main features to the concurrent programming:
- only one thread at a time has mutually exclusive access to a critical code section
- threads running in a monitor could be blocked while they’re waiting for certain conditions to be met
- one thread can notify other threads when conditions they’re waiting on are met

## How Does Java Implement Monitors?

A _critical section_ is a part of the code that accesses the same data through different threads.

In Java, we use the [[Synchronized keyword]] to mark critical sections. We can use it to mark methods (also called synchronized methods) or even smaller portions of code (synchronized statements).

In Java, there’s a logical connection between the monitor and every object or class. Hence, they cover instance and also static methods. Mutual exclusion is accomplished with a lock associated with every object and class. This lock is a binary semaphore called a _mutex_.

### _wait()_ and _notify()

[[wait and notify() Methods]] are key methods in Java used in synchronized blocks that enable collaboration between threads.

_wait()_ orders the calling thread to release the monitor and go to sleep until some other thread enters this monitor and calls _notify()_.  Also, _notify()_ wakes up the first thread that called _wait()_ on the specific object.