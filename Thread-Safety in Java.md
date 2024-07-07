[[Java Concurrency]] is supported in Java out of the box. This means that by running bytecode concurrently in separate worker threads, the [[JVM]] is capable of improving application performance.

Although multithreading is a powerful feature, it comes at a price. In multithreaded environments, we need to write implementations in a thread-safe way. This means that different threads can access the same resources without exposing erroneous behavior or producing unpredictable results. **This programming methodology is known as “thread-safety.”**

## Ways of providing thread-safety

### 1. Stateless Implementations

```java
public class MathUtils {
    
    public static BigInteger factorial(int number) {
        BigInteger f = new BigInteger("1");
        for (int i = 2; i <= number; i++) {
            f = f.multiply(BigInteger.valueOf(i));
        }
        return f;
    }
}
```

The _factorial()_ method is a **stateless deterministic function.** Given a specific input, it always produces the same output.
All threads can safely call the _factorial()_ method and will get the expected result without interfering with each other and without altering the output that the method generates for other threads.

Therefore, **stateless implementations are the simplest way to achieve thread-safety.**

### 2. Immutable Implementations

**If we need to share state between different threads, we can create thread-safe classes by making them immutable.**

To put it simply, **a class instance is immutable when its internal state can’t be modified after it has been constructed.**

The easiest way to create an immutable class in Java is by declaring all the fields _private_ and _final_ and not providing setters:

```java
public class MessageService {
    
    private final String message;

    public MessageService(String message) {
        this.message = message;
    }
    
    // standard getter
    
}
```

Object is effectively immutable since its state can’t change after its construction. So, it’s thread-safe.
### 3. Thread-Local Fields

In [[Object-oriented programming]] (OOP), objects actually need to maintain state through fields and implement behavior through one or more methods.

**We can create thread-safe classes that don’t share state between threads by making their fields thread-local.**

We can easily create classes whose fields are thread-local by simply defining private fields in [[Thread]] classes.

We could define, for instance, a _Thread_ class that stores an _array_ of _integers_:

```java
public class ThreadA extends Thread {
    
    private final List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
    
    @Override
    public void run() {
        numbers.forEach(System.out::println);
    }
}
```

Meanwhile, another one might hold an _array_ of _strings_:

```java
public class ThreadB extends Thread {
    
    private final List<String> letters = Arrays.asList("a", "b", "c", "d", "e", "f");
    
    @Override
    public void run() {
        letters.forEach(System.out::println);
    }
}
```

**In both implementations, the classes have their own state, but it’s not shared with other threads. So, the classes are thread-safe.**

Similarly, we can create thread-local fields by assigning [[ThreadLocal]] instances to a field.

```java
public class ThreadState {
    
    public static final ThreadLocal<String> statePerThread = new ThreadLocal<String>() {
        
        @Override
        protected StateHolder initialValue() {
            return "active";  
        }
    };

    public static StateHolder getState() {
        return statePerThread.get();
    }
}
```
### 4. Synchronized Collections

We can easily create thread-safe collections by using the set of synchronization wrappers included within the [[Java Collections]] framework.

We can use, for instance, one of these synchronization wrappers to create a thread-safe collection:

```java
Collection<Integer> syncCollection = Collections.synchronizedCollection(new ArrayList<>());
Thread thread1 = new Thread(() -> syncCollection.addAll(Arrays.asList(1, 2, 3, 4, 5, 6)));
Thread thread2 = new Thread(() -> syncCollection.addAll(Arrays.asList(7, 8, 9, 10, 11, 12)));
thread1.start();
thread2.start();
```

Let’s keep in mind that synchronized collections use intrinsic locking in each method (we’ll look at intrinsic locking later).

**This means that the methods can be accessed by only one thread at a time, while other threads will be blocked until the method is unlocked by the first thread.**
### 5. Concurrent Collections

Alternatively to synchronized collections, we can use concurrent collections to create thread-safe collections.

Java provides the `java.util.concurrent` package, which contains several concurrent collections, such as `ConcurrentHashMap`:

```java
Map<String,String> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put("1", "one");
concurrentMap.put("2", "two");
concurrentMap.put("3", "three");
```

Unlike their synchronized counterparts, **concurrent collections achieve thread-safety by dividing their data into segments.** In a _ConcurrentHashMap_, for example, several threads can acquire locks on different map segments, so multiple threads can access the _Map_ at the same time.

**Concurrent collections are** **much more performant than synchronized collections**, due to the inherent advantages of concurrent thread access.

### 6. Atomic Objects

[[Atomic variables]] classes allow us to perform atomic operations, which are thread-safe, without using synchronization. An atomic operation is executed in one single machine-level operation.

```java
public class AtomicCounter {
    
    private final AtomicInteger counter = new AtomicInteger();
    
    public void incrementCounter() {
        counter.incrementAndGet();
    }
    
    public int getCounter() {
        return counter.get();
    }
}
```

### 7. Synchronized Methods

The earlier approaches are very good for collections and primitives, but we’ll sometimes need greater control than that. So, another common approach that we can use for achieving thread-safety is implementing synchronized methods.

```java
public synchronized void incrementCounter() {
    counter += 1;
}
```

**Synchronized methods rely on the use of “intrinsic locks” or “monitor locks.”** An intrinsic lock is an implicit internal entity associated with a particular class instance.

In a multithreaded context, the term [[monitor]] is just a reference to the role that the lock performs on the associated object, as it enforces exclusive access to a set of specified methods or statements.

**When a thread calls a synchronized method, it acquires the intrinsic lock.** After the thread finishes executing the method, it releases the lock, which allows other threads to acquire the lock and get access to the method.

We can implement synchronization in instance methods, static methods and statements (synchronized statements).

### 8. Volatile Fields

The values of regular class fields might be cached by the CPU. Hence, consequent updates to a particular field, even if they’re synchronized, might not be visible to other threads.

To prevent this situation, we can use [[Volatile keyword]] for class fields:

```java
public class Counter {

    private volatile int counter;

    // standard constructors / getter
    
}
```

With the _volatile_ keyword, we instruct the JVM and the compiler to store the _counter_ variable in the main memory. That way, we make sure that every time the JVM reads the value of the _counter_ variable, it will actually read it from the main memory, instead of from the CPU cache.
###  9. Reentrant Locks

Java provides an improved set of [[java.util.concurrent.Locks]] implementations whose behavior is slightly more sophisticated than the intrinsic locks discussed above.

**With intrinsic locks, the lock acquisition model is rather rigid**: One thread acquires the lock, then executes a method or code block, and finally releases the lock so other threads can acquire it and access the method.

`ReentrantLock` instances allow us to do exactly that, preventing queued threads from suffering some types of resource starvation:

```java
public class ReentrantLockCounter {

    private int counter;
    private final ReentrantLock reLock = new ReentrantLock(true);
    
    public void incrementCounter() {
        reLock.lock();
        try {
            counter += 1;
        } finally {
            reLock.unlock();
        }
    }
    
    // standard constructors / getter
    
}
```

The _ReentrantLock_ constructor takes an optional _fairness_ _boolean_ parameter. When set to `true` and multiple threads are trying to acquire a lock, **the JVM will give priority to the longest waiting thread and grant access to the lock.**

### 10. Read/Write Locks

Another powerful mechanism that we can use for achieving thread-safety is the use of `ReadWriteLock` implementations.

A `ReadWriteLock` lock actually uses a pair of associated locks, one for read-only operations and the other for writing operations.

As a result, **it’s possible to have many threads reading a resource, as long as there’s no thread writing to it. Moreover, the thread writing to the resource will prevent other threads from reading it.**

Here’s how we can use a _ReadWriteLock_ lock:

```java
public class ReentrantReadWriteLockCounter {
    
    private int counter;
    private final ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();
    private final Lock readLock = rwLock.readLock();
    private final Lock writeLock = rwLock.writeLock();
    
    public void incrementCounter() {
        writeLock.lock();
        try {
            counter += 1;
        } finally {
            writeLock.unlock();
        }
    }
    
    public int getCounter() {
        readLock.lock();
        try {
            return counter;
        } finally {
            readLock.unlock();
        }
    }

   // standard constructors
   
}
```