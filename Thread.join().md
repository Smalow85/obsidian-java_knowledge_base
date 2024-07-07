When we invoke the _join()_ method on a [[Thread]], the calling thread goes into a waiting state. It remains in a waiting state until the referenced thread terminates.

Like the  [[wait and notify() Methods]], _join()_ is another mechanism of inter-thread synchronization.

**When we invoke the _join()_ method on a thread, the calling thread goes into a waiting state. It remains in a waiting state until the referenced thread terminates.**

```java
class SampleThread extends Thread {
    public int processingCount = 0;

    SampleThread(int processingCount) {
        this.processingCount = processingCount;
        LOGGER.info("Thread Created");
    }

    @Override
    public void run() {
        LOGGER.info("Thread " + this.getName() + " started");
        while (processingCount > 0) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                LOGGER.info("Thread " + this.getName() + " interrupted");
            }
            processingCount--;
            LOGGER.info("Inside Thread " + this.getName() + ", processingCount = " + processingCount);
        }
        LOGGER.info("Thread " + this.getName() + " exiting");
    }
}

@Test
public void givenStartedThread_whenJoinCalled_waitsTillCompletion() 
  throws InterruptedException {
    Thread t2 = new SampleThread(1);
    t2.start();
    LOGGER.info("Invoking join");
    t2.join();
    LOGGER.info("Returned from join");
    assertFalse(t2.isAlive());
}
```

We should expect results similar to the following when executing the code:

```plaintext
[main] INFO: Thread Thread-1 Created
[main] INFO: Invoking join
[Thread-1] INFO: Thread Thread-1 started
[Thread-1] INFO: Inside Thread Thread-1, processingCount = 0
[Thread-1] INFO: Thread Thread-1 exiting
[main] INFO: Returned from join
```

## _Thread.join()_ Methods and Synchronization

In addition to waiting until termination, calling the _join()_ method has a synchronization effect. *join()* creates a [[happens-before]] relationship:

This means that when a thread t1 calls t2.join(), all changes done by t2 are visible in t1 on return. However, if we do not invoke _join()_ or use other synchronization mechanisms, we do not have any guarantee that changes in the other thread will be visible to the current thread even if the other thread has been completed.

