_Semaphore_ is used for blocking thread level access to some part of the physical or logical resource in [[Java Concurrency]]. A [semaphore](https://www.baeldung.com/cs/semaphore) contains a set of permits; whenever a thread tries to enter the critical section, it needs to check the semaphore if a permit is available or not.

If a permit is not available (via _tryAcquire()_), the thread is not allowed to jump into the critical section; however, if the permit is available the access is granted, and the permit counter decreases.

Once the executing thread releases the critical section, again the permit counter increases (done by _release()_ method).

We can specify a timeout for acquiring access by using the _tryAcquire(long timeout, TimeUnit unit)_ method.

**We can also check the number of available permits or the number of threads waiting to acquire the semaphore.**

Following code snippet can be used to implement a semaphore:

```java
static Semaphore semaphore = new Semaphore(10);

public void execute() throws InterruptedException {

    LOG.info("Available permit : " + semaphore.availablePermits());
    LOG.info("Number of threads waiting to acquire: " + 
      semaphore.getQueueLength());

    if (semaphore.tryAcquire()) {
        try {
            // ...
        }
        finally {
            semaphore.release();
        }
    }

}
```