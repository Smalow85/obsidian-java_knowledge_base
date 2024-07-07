_Future_ class represents a future result of an asynchronous computation. This result will eventually appear in the _Future_ after the processing is complete.

_Future_ class represents a future result of an asynchronous computation. This result will eventually appear in the _Future_ after the processing is complete.
Long running methods are good candidates for asynchronous processing and the _Future_ interface because we can execute other processes while we’re waiting for the task encapsulated in the _Future_ to complete.

- computational intensive processes (mathematical and scientific calculations)
- manipulating large data structures (big data)
- remote method calls (downloading files, HTML scrapping, web services)

```java
public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning)
    V       get();
    V       get(long timeout, TimeUnit unit);
    boolean isCancelled();
    boolean isDone();
}
```
## Creating _Future_

_Future_ class represents a future result of an asynchronous computation. This result will eventually appear in the _Future_ after the processing is complete.

```java
Future<String> future = executor.submit(new Callable<String>() {  
	@Override  
	public String call() throws Exception {  
		Thread.sleep(1000);  
		return "Hello World!";  
	}  
});
```

_Callable_ is an interface representing a task that returns a result, and has a single _call()_ method.
Creating an instance of _Callable_ doesn’t take us anywhere; we still have to pass this instance to an executor that will take care of starting the task in a new thread, and give us back the valuable _Future_ object. That’s where _ExecutorService_ (see [[thread pools]]) comes in.
## Consuming _Future_

```java
Integer result = future.get(500, TimeUnit.MILLISECONDS);
```

The method _get()_ will block the execution until the task is complete. Method _get()_ has an overloaded version that takes a timeout and a [_TimeUnit_](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/TimeUnit.html) as arguments.

## Cancelling _Future_

You can cancel a `Future` task using the `cancel()` method. This method returns `true` if the task was successfully cancelled, and `false` if the task could not be cancelled or had already completed.

```java
if (future.cancel(true)) {  
    System.out.println("Task cancelled successfully");  
} else {  
    System.out.println("Task could not be cancelled");  
}
```

## Overview of _ForkJoinTask_

_[ForkJoinTask](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/ForkJoinTask.html)_ is an abstract class which implements _Future,_ and is capable of running a large number of tasks hosted by a small number of actual threads in [[Fork Join Pool]].

The main characteristic of a _ForkJoinTask_ is that it will usually spawn new subtasks as part of the work required to complete its main task. It generates new tasks by calling `fork()`, and it gathers all results with `join()`, thus the name of the class.

There are two abstract classes that implement _ForkJoinTask_: `RecursiveTask`, which returns a value upon completion, and `RecursiveAction`, which doesn’t return anything. As their names imply, these classes are to be used for recursive tasks, such as file-system navigation or complex mathematical computation.

```java
public class FactorialSquareCalculator extends RecursiveTask<Integer> { 
	private Integer n; 
	public FactorialSquareCalculator(Integer n) { 
		this.n = n; 
	} 
	
	@Override protected Integer compute() { 
		if (n <= 1) { 
			return n; 
		} 
		FactorialSquareCalculator calculator = 
			new FactorialSquareCalculator(n - 1); 
		calculator.fork(); 
		return n * n + calculator.join(); 
	}
}
```

By calling _fork()_, a non-blocking method, we ask _ForkJoinPool_ to initiate the execution of this subtask. The _join()_ method will return the result from that calculation, to which we’ll add the square of the number we’re currently visiting.

Now we just need to create a _ForkJoinPool_ to handle the execution and thread management:

```java
ForkJoinPool forkJoinPool = new ForkJoinPool();

FactorialSquareCalculator calculator = new FactorialSquareCalculator(10);

forkJoinPool.execute(calculator);
```








