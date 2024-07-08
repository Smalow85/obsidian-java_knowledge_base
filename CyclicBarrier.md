
_CyclicBarrier_ is a synchronizer (see [[Java Concurrency]]) that allows a set of threads to wait for each other to reach a common execution point, also called a _barrier_.

The constructor for a _CyclicBarrier_ is simple. It takes a single integer that denotes the number of threads that need to call the _await()_ method on the barrier instance to signify reaching the common execution point:

```java
public CyclicBarrier(int parties)
```

The threads that need to synchronize their execution are also called _parties_ and calling the _await()_ method is how we can register that a certain thread has reached the barrier point.

This call is synchronous and the thread calling this method suspends execution till a specified number of threads have called the same method on the barrier. This situation where the required number of threads have called _await()_, is called _tripping the barrier_.

Optionally, we can pass the second argument to the constructor, which is a _Runnable_ instance. This has logic that would be run by the last thread that trips the barrier:

```java
public CyclicBarrier(int parties, Runnable barrierAction)
```

![[Pasted image 20240707224454.png]]
## Example of implementation

```java
public class CyclicBarrierDemo {

    private CyclicBarrier cyclicBarrier;
    private List<List<Integer>> partialResults
     = Collections.synchronizedList(new ArrayList<>());
    private Random random = new Random();
    private int NUM_PARTIAL_RESULTS;
    private int NUM_WORKERS;

    // ...
	class NumberCruncherThread implements Runnable { 
		@Override public void run() { 
			String thisThreadName = Thread.currentThread().getName(); 
			List<Integer> partialResult = new ArrayList<>(); 
			// Crunch some numbers and store the partial result 
			for (int i = 0; i < NUM_PARTIAL_RESULTS; i++) { 
				Integer num = random.nextInt(10); 
				System.out.println(thisThreadName + ": Crunching some numbers! Final result - " + num); 
				partialResult.add(num); 
			} 
			partialResults.add(partialResult); 
			
			try { 
				System.out.println(thisThreadName + " waiting for others to reach barrier."); 
				cyclicBarrier.await(); 
			} catch (InterruptedException e) { 
			// ... 
			} catch (BrokenBarrierException e) {
			 // ... 
			 } 
		} 
	}

	public void runSimulation(int numWorkers, int numberOfPartialResults) {
		NUM_PARTIAL_RESULTS = numberOfPartialResults; 
		NUM_WORKERS = numWorkers; 
		cyclicBarrier = 
				new CyclicBarrier(NUM_WORKERS, new AggregatorThread());
		System.out.println("Spawning " + NUM_WORKERS + " worker threads to compute " + NUM_PARTIAL_RESULTS + " partial results each"); 
		for (int i = 0; i < NUM_WORKERS; i++) { 
			Thread worker = new Thread(new NumberCruncherThread());
			worker.setName("Thread " + i); worker.start(); 
		} 
	} 
	
	public static void main(String[] args) { 
		CyclicBarrierDemo demo = new CyclicBarrierDemo();
		demo.runSimulation(5, 3); 
	}
}
```

A _CyclicBarrier_ is a reusable construct where a group of threads _waits_ together until all of the threads _arrive_. At that point, the barrier is broken and an _action_ can optionally be taken.

We can think of this like a group of friends. Every time they plan to eat at a restaurant they decide a common point where they can meet. They _wait_ for each other there, and only when everyone _arrives_ can they go to the restaurant to eat together.

