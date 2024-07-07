_CompletableFuture_ offers an extensive API (see [[Future]] with only 4 methods) consisting of more than 50 methods. Many of these methods are available in two variants: _non-async_ and _async._
## thenApply()

When utilizing _thenApply()_, we pass a function as a parameter that takes the previous value of the _CompletableFuture_ as input, performs an operation, and returns a new value. Consequently, a fresh _CompletableFuture_ is created to encapsulate the resulting value.

```java
CompletableFuture<String> name = 
		CompletableFuture.supplyAsync(() -> "Baeldung"); 
CompletableFuture<Integer> nameLength = 
			name.thenApply(value -> { printCurrentThread(); });
```

The function passed as a parameter to _thenApply()_ will be executed by the thread that directly interacts with _CompletableFuture_‘s API.
## supplyAsync()

```java
CompletableFuture<String> name = 
		CompletableFuture.supplyAsync(() -> "Baeldung"); CompletableFuture<Integer> nameLength = 
		name.thenApplyAsync(value -> { printCurrentThread(); });
```

If we use the **async** methods without explicitly providing an _Executor_, the functions will be executed using _ForkJoinPool.commonPool()._

We can use overloaded method to pass executed code to ExecutorService.

```java
Executor testExecutor = Executors.newFixedThreadPool(5); CompletableFuture<String> name = 
		CompletableFuture.supplyAsync(() -> "Baeldung"); CompletableFuture<Integer> nameLength = 
		name.thenApplyAsync(value -> { printCurrentThread(); }, testExecutor);
```

When using the overloaded method, the CompletableFuture will no longer use the common _ForkJoinPool_.

## thenCompose()

We can also create a chain of sequential _Futures_. Suppose we have two factorial tasks, and the second needs input from the first one:

```java
CompletableFuture<BigInteger> completableFuture = CompletableFuture.supplyAsync(() -> factorial(BigInteger.valueOf(3)))
   .thenCompose(inputFromFirstTask -> CompletableFuture.supplyAsync(() -> factorial(inputFromFirstTask)));

BigInteger result = completableFuture.get();
```

