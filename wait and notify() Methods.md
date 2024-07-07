In a [[Java Concurrency]] environment, multiple threads might try to modify the same resource. Not managing [[Thread]] properly will of course lead to consistency issues.

One tool we can use to coordinate actions of multiple threads in Java is guarded blocks. Such blocks keep a check for a particular condition before resuming the execution.

With that in mind, we’ll make use of the following:

- _[Object.wait()](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Object.html#wait())_ to suspend a thread
- _[Object.notify()](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Object.html#notify())_ to wake a thread up

![[Pasted image 20240624215406.png]]

## _wait()_ Method

Simply put, calling _wait()_ forces the current thread to wait until some other thread invokes _notify()_ or _notifyAll()_ on the same object.

For this, the current thread must own the object’s [[monitor]]. According to [Javadocs](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Object.html#notify()), this can happen in the following ways:

- when we’ve executed _synchronized_ instance method for the given object
- when we’ve executed the body of a _synchronized_ block on the given object
- by executing _synchronized static_ methods for objects of type _Class_

**Note that only one active thread can own an object’s monitor at a time.**

## _notify()_ and _notifyAll()

We use the _notify()_ method for waking up threads that are waiting for access to this object’s monitor.
For all threads waiting on this object’s monitor (by using any one of the _wait()_ methods), the method _notify()_ notifies any one of them to wake up arbitrarily. The choice of exactly which thread to wake is nondeterministic and depends upon the implementation.

Since _notify()_ wakes up a single random thread, we can use it to implement mutually exclusive locking where threads are doing similar tasks. But in most cases, it would be more viable to implement _notifyAll()_. The awakened threads will compete in the usual manner, like any other thread that is trying to synchronize on this object.

## Example

```java
public class Data {
    private String packet;
    
    // True if receiver should wait
    // False if sender should wait
    private boolean transfer = true;
 
    public synchronized String receive() {
        while (transfer) {
            try {
                wait();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); 
                System.err.println("Thread Interrupted");
            }
        }
        transfer = true;
        
        String returnPacket = packet;
        notifyAll();
        return returnPacket;
    }
 
    public synchronized void send(String packet) {
        while (!transfer) {
            try { 
                wait();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); 
                System.err.println("Thread Interrupted");
            }
        }
        transfer = false;
        
        this.packet = packet;
        notifyAll();
    }
}
```

Let’s break down what’s going on here:

- The _packet_ variable denotes the data that is being transferred over the network.
- We have a _boolean_ variable _transfer_, which the _Sender_ and _Receiver_ will use for synchronization:
    - If this variable is _true_, the _Receiver_ should wait for _Sender_ to send the message.
    - If it’s _false_, _Sender_ should wait for _Receiver_ to receive the message.
- The _Sender_ uses the _send()_ method to send data to the _Receiver_:
    - If _transfer_ is _false_, we’ll wait by calling _wait()_ on this thread.
    - But when it is _true_, we toggle the status, set our message, and call _notifyAll()_ to wake up other threads to specify that a significant event has occurred and they can check if they can continue execution.
- Similarly, the _Receiver_ will use the _receive()_ method:
    - If the _transfer_ was set to _false_ by _Sender_, only then will it proceed, otherwise we’ll call _wait()_ on this thread.
    - When the condition is met, we toggle the status, notify all waiting threads to wake up, and return the data packet that was received.

```java
public class Sender implements Runnable {
    private Data data;
 
    // standard constructors
 
    public void run() {
        String packets[] = {
          "First packet",
          "Second packet",
          "Third packet",
          "Fourth packet",
          "End"
        };
 
        for (String packet : packets) {
            data.send(packet);

            // Thread.sleep() to mimic heavy server-side processing
            try {
                Thread.sleep(ThreadLocalRandom.current().nextInt(1000, 5000));
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); 
                System.err.println("Thread Interrupted"); 
            }
        }
    }
}
```

Let’s take a closer look at this _Sender_:

- We’re creating some random data packets that will be sent across the network in _packets[]_ array.
- For each packet, we’re merely calling send().
- Then we’re calling _Thread.sleep()_ with random interval to mimic heavy server-side processing.

Finally, let’s implement our _Receiver_:

```java
public class Receiver implements Runnable {
    private Data load;
 
    // standard constructors
 
    public void run() {
        for(String receivedMessage = load.receive();
          !"End".equals(receivedMessage);
          receivedMessage = load.receive()) {
            
            System.out.println(receivedMessage);

            //Thread.sleep() to mimic heavy server-side processing
            try {
                Thread.sleep(ThreadLocalRandom.current().nextInt(1000, 5000));
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); 
                System.err.println("Thread Interrupted"); 
            }
        }
    }
}
```

Here, we’re simply calling _load.receive()_ in the loop until we get the last _“End”_ data packet.

Let’s now see this application in action:

```java
public static void main(String[] args) {
    Data data = new Data();
    Thread sender = new Thread(new Sender(data));
    Thread receiver = new Thread(new Receiver(data));
    
    sender.start();
    receiver.start();
}
```

