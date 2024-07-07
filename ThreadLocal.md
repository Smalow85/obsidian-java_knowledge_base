The _TheadLocal_ construct allows us to store data that will be **accessible only** by a specific [[Thread]].

```java
ThreadLocal<Integer> threadLocalValue = new ThreadLocal<>();
```

```java
public class ThreadLocalWithUserContext implements Runnable {
 
    private static ThreadLocal<Context> userContext 
      = new ThreadLocal<>();
    private Integer userId;
    private UserRepository userRepository = new UserRepository();

    @Override
    public void run() {
        String userName = userRepository.getUserNameForUserId(userId);
        userContext.set(new Context(userName));
        System.out.println("thread context for given userId: " 
          + userId + " is: " + userContext.get());
    }
    
    // standard constructor
}
```

We can test it by starting two threads that will execute the action for a given _userId_:

```java
ThreadLocalWithUserContext firstUser 
  = new ThreadLocalWithUserContext(1);
ThreadLocalWithUserContext secondUser 
  = new ThreadLocalWithUserContext(2);
new Thread(firstUser).start();
new Thread(secondUser).start();
```

After running this code, we’ll see on the standard output that _ThreadLocal_ was set per given thread:

```java
thread context for given userId: 1 is: Context{userNameSecret='18a78f8e-24d2-4abf-91d6-79eaa198123f'}
thread context for given userId: 2 is: Context{userNameSecret='e19f6a0a-253e-423e-8b2b-bca1f471ae5c'}
```
## ThreadLocal and Thread Pools

_ThreadLocal_ provides an easy-to-use API to confine some values to each thread. This is a reasonable way of achieving [[thread-safety]] in Java. However, we should be extra careful when we’re using ThreadLocal and [[thread pools]]

In order to better understand this possible caveat, let’s consider the following scenario:

1. First, the application borrows a thread from the pool.
2. Then it stores some thread-confined values into the current thread’s _ThreadLocal_.
3. Once the current execution finishes, the application returns the borrowed thread to the pool.
4. After a while, the application borrows the same thread to process another request.
5. **Since the application didn’t perform the necessary cleanups last time, it may re-use the same _ThreadLocal_ data for the new request.**

This may cause surprising consequences in highly concurrent applications.

One way to solve this problem is to manually remove each _ThreadLocal_ once we’re done using it. Because this approach needs rigorous code reviews, it can be error-prone.