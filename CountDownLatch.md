By using a _CountDownLatch_ from [[Java Concurrency]] we can cause a thread to block until other threads have completed a given task. 
_CountDownLatch_ has a _counter_ field, which you can decrement as we require. We can then use it to block a calling thread until it’s been counted down to zero.

If we were doing some parallel processing, we could instantiate the _CountDownLatch_ with the same value for the counter as a number of threads we want to work across. Then, we could just call _countdown()_ after each thread finishes, guaranteeing that a dependent thread calling _await()_ will block until the worker threads are finished.

```java
public class Worker implements Runnable {
    private List<String> outputScraper;
    private CountDownLatch countDownLatch;

    public Worker(List<String> outputScraper, CountDownLatch countDownLatch) {
        this.outputScraper = outputScraper;
        this.countDownLatch = countDownLatch;
    }

    @Override
    public void run() {
        doSomeWork();
        outputScraper.add("Counted down");
        countDownLatch.countDown();
    }
}
```

Then, let’s create a test in order to prove that we can get a _CountDownLatch_ to wait for the _Worker_ instances to complete:

```java
@Test
public void whenParallelProcessing_thenMainThreadWillBlockUntilCompletion()
  throws InterruptedException {

    List<String> outputScraper = 
						    Collections.synchronizedList(new ArrayList<>());
    CountDownLatch countDownLatch = new CountDownLatch(5);
    List<Thread> workers = Stream
      .generate(() -> new Thread(new Worker(outputScraper, countDownLatch)))
      .limit(5)
      .collect(toList());

      workers.forEach(Thread::start);
      countDownLatch.await(); 
      outputScraper.add("Latch released");

      assertThat(outputScraper)
        .containsExactly(
          "Counted down",
          "Counted down",
          "Counted down",
          "Counted down",
          "Counted down",
          "Latch released"
        );
    }
```

A _CountDownLatch_ is a construct that a thread _wait_s on while other threads _count down_ on the latch until it reaches zero.

We can think of this like a dish at a restaurant that is being prepared. No matter which cook prepares however many of the _n_ items, the waiter must _wait_ until all the items are on the plate. If a plate takes _n_ items, any cook will _count down_ on the latch for each item she puts on the plate.

