Java offers two types of [[Thread]]: user threads and daemon threads.

User threads are high-priority threads. **The JVM will wait for any user thread to complete its task before terminating it.**

On the other hand, **daemon threads are low-priority threads whose only role is to provide services to user threads.**

Daemon threads are useful for background supporting tasks such as garbage collection, releasing memory of unused objects and removing unwanted entries from the cache. Most of the JVM threads are daemon threads.

To set a thread to be a daemon thread, all we need to do is to call _Thread.setDaemon()._ In this example, we’ll use the _NewThread_ class which extends the _Thread_ class:

```java
NewThread daemonThread = new NewThread();
daemonThread.setDaemon(true);
daemonThread.start();
```

**Any thread inherits the daemon status of the thread that created it.** Since the main thread is a user thread, any thread that is created inside the main method is by default a user thread.