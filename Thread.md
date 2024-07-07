A thread in [[Java]] is the direction or path that is taken while a program is being executed. Generally, all the programs have at least one thread, known as the main thread, that is provided by the JVM at the starting of the program’s execution. At this point, when the main thread is provided, the main() method is invoked by the main thread.

Multiple threads of execution can be used to provide [[Java Concurrency]] by an application running on the Java Virtual Machine. The priority of each thread varies. Higher priority threads are executed before lower priority threads.

Thread is critical in the program because it enables multiple operations to take place within a single method. Each thread in the program often has its own program counter, stack, and local variable.

A thread in Java can be created in the following two ways:

```java
class Multi extends Thread{  
	public void run() {  
		System.out.println("thread is running...");  
	}  

	public static void main(String args[]){  
		Multi t1=new Multi();  
		t1.start();  
	}
}
```

```java
class Multi3 implements Runnable{  
	public void run(){  
		System.out.println("thread is running...");  
	}  

	public static void main(String args[]){  
		Multi3 m1=new Multi3(); 
		// Using the constructor Thread(Runnable r)   
		Thread t1 =new Thread(m1);   
		t1.start();  
	}  
}
```

## Lifecycle of a Thread in Java

The Life Cycle of a Thread in Java refers to the state transformations of a thread that begins with its birth and ends with its death. When a thread instance is generated and executed by calling the start() method of the Thread class, the thread enters the runnable state. When the sleep() or wait() methods of the Thread class are called, the thread enters a non-runnable mode.

Thread returns from non-runnable state to runnable state and starts statement execution. The thread dies when it exits the run() process. In Java these thread state transformations are referred to as the Thread life cycle.

There are basically 4 stages in the lifecycle of a thread, as given below:

1. **New**
2. **Runnable**
3. **Running**
4. **Blocked (Non-runnable state)**
5. **Dead**

![lifecycleofathread](https://www.simplilearn.com/ice9/free_resources_article_thumb/lifecycleofathread.png)

- ### New State
As we use the Thread class to construct a thread entity, the thread is born and is defined as being in the New state. That is, when a thread is created, it enters a new state, but the start() method on the instance has not yet been invoked.
- ### Runnable State
A thread in the runnable state is prepared to execute the code. When a new thread's start() function is called, it enters a runnable state.
In the runnable environment, the thread is ready for execution and is awaiting the processor's availability (CPU time). That is, the thread has entered the queue (line) of threads waiting for execution.
- ### Running State
Running implies that the processor (CPU) has assigned a time slot to the thread for execution. When a thread from the runnable state is chosen for execution by the thread scheduler, it joins the running state.
In the running state, the processor allots time to the thread for execution and runs its run procedure. This is the state in which the thread directly executes its operations. Only from the runnable state will a thread enter the running state.
- ### Blocked State
When the thread is alive, i.e., the thread class object persists, but it cannot be selected for execution by the scheduler. It is now inactive
- ### Dead State
When a thread's `run()` function ends the execution of sentences, it automatically dies or enters the dead state. That is, when a thread exits the run() process, it is terminated or killed. When the stop() function is invoked, a thread will also go dead.
## Java Thread Priorities

The number of services assigned to a given thread is referred to as its priority. Any thread generated in the JVM is given a priority. The priority scale runs from 1 to 10.

`1` is known as the lowest priority = `Thread.MAX_PRIORITY`

`5` is known as standard priority  = `Thread.NORM_PRIORITY`

`10` represents the highest level of priority = `Thread.MIN_PRIORITY`








