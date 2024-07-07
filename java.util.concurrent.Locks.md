Simply put, a lock is a more flexible and sophisticated thread synchronization mechanism than the standard _synchronized_ block.
## Differences Between Lock and Synchronized Block

There are a few differences between the use of synchronized _block_ and using _Lock_ APIs:

- A _synchronized block_ is fully contained within a method. We can have _Lock_ APIs _lock()_ and _unlock()_ operation in separate methods.
- A _synchronized block_ doesn’t support the fairness. Any thread can acquire the lock once released, and no preference can be specified. We can achieve fairness within the _Lock_ APIs by specifying the _fairness_ property. It makes sure that the longest waiting thread is given access to the lock.
- A thread gets blocked if it can’t get an access to the synchronized _block_. The _Lock_ API provides _tryLock()_ method. The thread acquires lock only if it’s available and not held by any other thread. This reduces blocking time of thread waiting for the lock.
- A thread that is in “waiting” state to acquire the access to _synchronized block_ can’t be interrupted. The _Lock_ API provides a method _lockInterruptibly()_ that can be used to interrupt the thread when it’s waiting for the lock.
## _Lock_ API

- `void lock()` – Acquire the lock if it’s available. If the lock isn’t available, a thread gets blocked until the lock is released.
- `void lockInterruptibly()` – This is similar to the _lock()_, but it allows the blocked thread to be interrupted and resume the execution through a thrown _java.lang.InterruptedException_.
- `boolean tryLock()` – This is a nonblocking version of _lock()_ method. It attempts to acquire the lock immediately, return true if locking succeeds.
- `boolean tryLock(long timeout, TimeUnit timeUnit)` – This is similar to _tryLock()_, except it waits up the given timeout before giving up trying to acquire the _Lock_.
- `void unlock()` unlocks the _Lock_ instance.

Locked instance should always be unlocked to avoid deadlock condition.
A recommended code block to use the lock should contain a _try/catch_ and _finally_ block:

```java
Lock lock = ...; 
lock.lock();
try {
    // access to the shared resource
} finally {
    lock.unlock();
}
```

## Lock Implementations

### 1. ReentrantLock

_ReentrantLock_ class implements the _Lock_ interface. It offers the same concurrency and memory semantics as the implicit monitor lock accessed using _synchronized_ methods and statements, with extended capabilities.

```java
public class SharedObjectWithLock {
    //...
    ReentrantLock lock = new ReentrantLock();
    int counter = 0;

    public void perform() {
        lock.lock();
        try {
            // Critical section here
            count++;
        } finally {
            lock.unlock();
        }
    }
    //...
}
```

We need to make sure that we are wrapping the _lock()_ and the _unlock()_ calls in the _try-finally_ block to avoid the deadlock situations.

Let’s see how the _tryLock()_ works:

```java
public void performTryLock(){
    //...
    boolean isLockAcquired = lock.tryLock(1, TimeUnit.SECONDS);
    
    if(isLockAcquired) {
        try {
            //Critical section here
        } finally {
            lock.unlock();
        }
    }
    //...
}
```

In this case, the thread calling _tryLock()_ will wait for one second and will give up waiting if the lock isn’t available.
### 2. ReentrantReadWriteLock

_ReentrantReadWriteLock_ class implements the _ReadWriteLock_ interface.

Let’s see the rules for acquiring the _ReadLock_ or _WriteLock_ by a thread:

- **Read Lock** – If no thread acquired the write lock or requested for it, multiple threads can acquire the read lock.
- **Write Lock** – If no threads are reading or writing, only one thread can acquire the write lock.

Let’s look at how to make use of the _ReadWriteLock_:

```java
public class SynchronizedHashMapWithReadWriteLock {

    Map<String,String> syncHashMap = new HashMap<>();
    ReadWriteLock lock = new ReentrantReadWriteLock();
    // ...
    Lock writeLock = lock.writeLock();

    public void put(String key, String value) {
        try {
            writeLock.lock();
            syncHashMap.put(key, value);
        } finally {
            writeLock.unlock();
        }
    }
    ...
    public String remove(String key){
        try {
            writeLock.lock();
            return syncHashMap.remove(key);
        } finally {
            writeLock.unlock();
        }
    }
    //...
}
```

For both write methods, we need to surround the critical section with the write lock — only one thread can get access to it:

```java
Lock readLock = lock.readLock();
//...
public String get(String key){
    try {
        readLock.lock();
        return syncHashMap.get(key);
    } finally {
        readLock.unlock();
    }
}

public boolean containsKey(String key) {
    try {
        readLock.lock();
        return syncHashMap.containsKey(key);
    } finally {
        readLock.unlock();
    }
}
```

For both read methods, we need to surround the critical section with the read lock. Multiple threads can get access to this section if no write operation is in progress.
### 3. StampedLock

_StampedLock_ is introduced in Java 8. It also supports both read and write locks.

However, lock acquisition methods return a stamp that is used to release a lock or to check if the lock is still valid:

```java
public class StampedLockDemo {
    Map<String,String> map = new HashMap<>();
    private StampedLock lock = new StampedLock();

    public void put(String key, String value){
        long stamp = lock.writeLock();
        try {
            map.put(key, value);
        } finally {
            lock.unlockWrite(stamp);
        }
    }

    public String get(String key) throws InterruptedException {
        long stamp = lock.readLock();
        try {
            return map.get(key);
        } finally {
            lock.unlockRead(stamp);
        }
    }
}
```

Another feature provided by _StampedLock_ is optimistic locking. Most of the time, read operations don’t need to wait for write operation completion, and as a result of this, the full-fledged read lock isn’t required.

Instead, we can upgrade to read lock:

```java
public String readWithOptimisticLock(String key) {
    long stamp = lock.tryOptimisticRead();
    String value = map.get(key);

    if(!lock.validate(stamp)) {
        stamp = lock.readLock();
        try {
            return map.get(key);
        } finally {
            lock.unlock(stamp);               
        }
    }
    return value;
}
```

