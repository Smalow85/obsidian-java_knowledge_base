Simply put, in a multi-threaded environment, a [[race condition]] occurs when two or more threads attempt to update mutable shared data at the same time. Java offers a mechanism to avoid race conditions by synchronizing thread access to shared data.

A piece of logic marked with _synchronized_ becomes a synchronized block, **allowing only one thread to execute at any given time**.

We can use the _synchronized_ keyword on different levels:
- Instance and static methods
- Code blocks

We can add the _synchronized_ keyword in the method declaration to make the method synchronized:

```java
public synchronized void synchronisedCalculate() {
    setSum(getSum() + 1);
}
```

Sometimes we don’t want to synchronize the entire method, only some instructions within it. We can achieve this by _applying_ synchronized to a block:

```java
public void performSynchronisedTask() {
    synchronized (this) {
        setCount(getCount()+1);
    }
}
```

 ==!== Notice that we passed a parameter _this_ to the _synchronized_ block. This is the **monitor** object. The code inside the block gets synchronized on the monitor object. Simply put, only one thread per monitor object can execute inside that code block.

If the method was _static_, we would pass the class name in place of the object reference, and the class would be a monitor for synchronization of the block:

```java
public static void performStaticSyncTask(){
    synchronized (SynchronisedBlocks.class) {
        setStaticCount(getStaticCount() + 1);
    }
}
```

The lock behind the _synchronized_ methods and blocks is a **reentrant**. This means the current thread can acquire the same _synchronized_ lock over and over again while holding it