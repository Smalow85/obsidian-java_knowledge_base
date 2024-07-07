Java 7 introduced the fork/join framework. It provides tools to help speed up parallel processing by attempting to use all available processor cores. It accomplishes this **through a divide and conquer approach.**

In practice, this means that **the framework first “forks,”** recursively breaking the task into smaller independent subtasks until they are simple enough to run asynchronously.

After that, **the “join” part begins.** The results of all subtasks are recursively joined into a single result. In the case of a task that returns void, the program simply waits until every subtask runs.

To provide effective parallel execution, the fork/join framework uses a [[Thread]] pool called the _ForkJoinPool_. This pool manages worker threads of type _ForkJoinWorkerThread_.

## ForkJoinPool

The _ForkJoinPool_ is the heart of the framework. It is an implementation of the Executor Service (see [[thread pools]]) that manages worker threads and provides us with tools to get information about the thread pool state and performance.

Worker threads can execute only one task at a time, but the _ForkJoinPool_ doesn’t create a separate thread for every single subtask. Instead, each thread in the pool has its own double-ended queue that stores tasks.

### Work-Stealing Algorithm

**Simply put, free threads try to “steal” work from deques of busy threads.**

By default, a worker thread gets tasks from the head of its own deque. When it is empty, the thread takes a task from the tail of the deque of another busy thread or from the global entry queue since this is where the biggest pieces of work are likely to be located.

This approach minimizes the possibility that threads will compete for tasks. It also reduces the number of times the thread will have to go looking for work, as it works on the biggest available chunks of work first.

```java
ForkJoinPool commonPool = ForkJoinPool.commonPool();
```

## ForkJoinTask

ForkJoinTask is the base type for tasks executed inside _ForkJoinPool_.
In practice, one of its two subclasses should be extended: the _RecursiveAction_ for _void_ tasks and the _RecursiveTask_ for tasks that return a value.

### 1. RecursiveAction

```java
public class CustomRecursiveAction extends RecursiveAction {

    private String workload = "";
    private static final int THRESHOLD = 4;

    private static Logger logger = 
      Logger.getAnonymousLogger();

    public CustomRecursiveAction(String workload) {
        this.workload = workload;
    }

    @Override
    protected void compute() {
        if (workload.length() > THRESHOLD) {
            ForkJoinTask.invokeAll(createSubtasks());
        } else {
           processing(workload);
        }
    }

    private List<CustomRecursiveAction> createSubtasks() {
        List<CustomRecursiveAction> subtasks = new ArrayList<>();

        String partOne = workload.substring(0, workload.length() / 2);
        String partTwo = workload.substring(workload.length() / 2, workload.length());

        subtasks.add(new CustomRecursiveAction(partOne));
        subtasks.add(new CustomRecursiveAction(partTwo));

        return subtasks;
    }

    private void processing(String work) {
        String result = work.toUpperCase();
        logger.info("This result - (" + result + ") - was processed by " 
          + Thread.currentThread().getName());
    }
}
```

### 2. RecursiveTask

The difference is that the result for each subtask is united in a single result:

```java
public class CustomRecursiveTask extends RecursiveTask<Integer> {
    private int[] arr;

    private static final int THRESHOLD = 20;

    public CustomRecursiveTask(int[] arr) {
        this.arr = arr;
    }

    @Override
    protected Integer compute() {
        if (arr.length > THRESHOLD) {
            return ForkJoinTask.invokeAll(createSubtasks())
              .stream()
              .mapToInt(ForkJoinTask::join)
              .sum();
        } else {
            return processing(arr);
        }
    }

    private Collection<CustomRecursiveTask> createSubtasks() {
        List<CustomRecursiveTask> dividedTasks = new ArrayList<>();
        dividedTasks.add(new CustomRecursiveTask(
          Arrays.copyOfRange(arr, 0, arr.length / 2)));
        dividedTasks.add(new CustomRecursiveTask(
          Arrays.copyOfRange(arr, arr.length / 2, arr.length)));
        return dividedTasks;
    }

    private Integer processing(int[] arr) {
        return Arrays.stream(arr)
          .filter(a -> a > 10 && a < 27)
          .map(a -> a * 10)
          .sum();
    }
}
```

## Submitting Tasks to the _ForkJoinPool_

We can use a few approaches to submit tasks to the thread pool.

Let’s start with the _submit()_ or _execute()_ method (their use cases are the same):

```java
forkJoinPool.execute(customRecursiveTask);
int result = customRecursiveTask.join();
```

The _invoke()_ method forks the task and waits for the result, and doesn’t need any manual joining:

```java
int result = forkJoinPool.invoke(customRecursiveTask);
```

The _invokeAll()_ method is the most convenient way to submit a sequence of _ForkJoinTasks_ to the _ForkJoinPool_. It takes tasks as parameters (two tasks, var args or a collection), forks and then returns a collection of _Future_ objects in the order in which they were produced.

Alternatively, we can use separate _fork()_ and _join()_ methods. The _fork()_ method submits a task to a pool, but it doesn’t trigger its execution. We must use the _join()_ method for this purpose.

```java
customRecursiveTaskFirst.fork();
result = customRecursiveTaskLast.join();
```

Using the fork/join framework can speed up processing of large tasks, but to achieve this outcome, we should follow some guidelines:

- **Use as few thread pools as possible.** In most cases, the best decision is to use one thread pool per application or system.
- **Use the default common thread pool** if no specific tuning is needed.
- **Use a reasonable threshold** for splitting _ForkJoinTask_ into subtasks.
- Avoid any **blocking in** _ForkJoinTasks_.