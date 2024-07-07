In Java, [[Thread]] is mapped to system-level threads, which are the operating system’s resources. If we create threads uncontrollably, we may run out of these resources quickly.

The operating system does the context switching between threads as well — in order to emulate parallelism. A simplistic view is that the more threads we spawn, the less time each thread spends doing actual work.

The Thread Pool pattern helps to save resources in a multithreaded application and to contain the parallelism in certain predefined limits.

When we use a thread pool, we **write our concurrent code in the form of parallel tasks and submit them for execution to an instance of a thread pool.** This instance controls several re-used threads for executing these tasks.

![[Pasted image 20240625225526.png]]

The pattern allows us to **control the number of threads the application creates** and their life cycle. We’re also able to schedule tasks’ execution and keep incoming tasks in a queue.
### Executors, Executor and ExecutorService

_Executors_ helper class contains several methods for the creation of preconfigured thread pool instances.

We use the _Executor_ and _ExecutorService_ interfaces to work with different thread pool implementations in Java. Usually, we should **keep our code decoupled from the actual implementation of the thread pool** and use these interfaces throughout our application.
#### Executor

The _Executor_ interface has a single _execute_ method to submit _Runnable_ instances for execution.

```java
Executor executor = Executors.newSingleThreadExecutor();
executor.execute(() -> System.out.println("Hello World"));
```
#### ExecutorService

_ExecutorService_ interface contains a large number of methods to **control the progress of the tasks and manage the termination of the service.** Using this interface, we can submit the tasks for execution and also control their execution using the returned _Future_ instance.

```java
ExecutorService executorService = Executors.newFixedThreadPool(10);
Future<String> future = executorService.submit(() -> "Hello World");
// some operations
String result = future.get();
```

### ThreadPoolExecutor

The _ThreadPoolExecutor_ is an extensible thread pool implementation with lots of parameters and hooks for fine-tuning.

The main configuration parameters that we’ll discuss here are _corePoolSize_, _maximumPoolSize_ and _keepAliveTime_. 

#### _newFixedThreadPool_

```java
ThreadPoolExecutor executor = 
  (ThreadPoolExecutor) Executors.newFixedThreadPool(2);
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});

assertEquals(2, executor.getPoolSize());
assertEquals(1, executor.getQueue().size());
```

#### _newCachedThreadPool_

```java
ThreadPoolExecutor executor = 
  (ThreadPoolExecutor) Executors.newCachedThreadPool();
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});

assertEquals(3, executor.getPoolSize());
assertEquals(0, executor.getQueue().size());
```

These parameter values mean that **the cached thread pool may grow without bounds to accommodate any number of submitted tasks.** But when the threads are not needed anymore, they will be disposed of after 60 seconds of inactivity. A typical use case is when we have a lot of short-living tasks in our application.

#### _newSingleThreadExecutor()_

**The single thread executor is ideal for creating an event loop.**

```java
AtomicInteger counter = new AtomicInteger();

ExecutorService executor = Executors.newSingleThreadExecutor();
executor.submit(() -> {
    counter.set(1);
});
executor.submit(() -> {
    counter.compareAndSet(1, 2);
});
```


