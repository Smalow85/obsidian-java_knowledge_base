Java garbage collection is the process by which Java programs perform automatic memory management. Java programs compile to bytecode that can be run on a Java Virtual Machine, or JVM for short. When Java programs run on the JVM, objects are created on the heap, which is a portion of memory dedicated to the program. Eventually, some objects will no longer be needed. The garbage collector finds these unused objects and deletes them to free up memory.
## How Java Garbage Collection Works

[[Java]] garbage collection is an automatic process. The programmer does not need to explicitly mark objects to be deleted. The garbage collection implementation lives in the JVM. Every JVM can implement garbage collection however it pleases. The only requirement is that it should meet the JVM specification.
## What are the Various Steps During the Garbage Collection?

While [[JVM]] has multiple garbage collectors that are optimized for various use cases, all its garbage collectors follow the same basic process. 
In the first step, [[Unreferenced objects]] are identified and marked as ready for garbage collection. 
In the second step, marked objects are deleted. Optionally, memory can be compacted after the garbage collector deletes objects, so remaining objects are in a contiguous block at the start of the heap. The compaction process makes it easier to allocate memory to new objects sequentially after the JVM allocates the memory blocks to existing objects.

Basically, _GC_ works in two simple steps, known as Mark and Sweep:

- **Mark –** this is where the garbage collector identifies which pieces of memory are in use and which aren’t.
- **Sweep –** this step removes objects identified during the “mark” phase.
## How Generational Garbage Collection Strategy Works

All of JVM’s garbage collectors implement a generational garbage collection strategy that categorizes objects by age. The rationale behind generational garbage collection is that most objects are short-lived and will be ready for garbage collection soon after creation.

![[Pasted image 20240615141425.png]]

### What are Different Classification of Objects by Garbage Collector?

We can divide the heap into [three sections](https://plumbr.eu/handbook/garbage-collection-in-java):

- **Young Generation**: Newly created objects start in the Young Generation. The garbage collector further subdivides Young Generation into an Eden space, where all new objects start, and two Survivor spaces, where it moves objects from Eden after surviving one garbage collection cycle. When objects are garbage collected from the Young Generation, it is a minor garbage collection event.
- **Old Generation:** Eventually, the garbage collector moves the long-lived objects from the Young Generation to the Old Generation. When objects are garbage collected from the Old Generation, it is a major garbage collection event.
- **Permanent Generation:** The JVM stores the metadata, such as classes and methods, in the Permanent Generation. JVM garbage collects the classes from the Permanent Generation that are no longer in use.

### What are Different Types of Garbage Collector?

Oracle's HotSpot has four garbage collectors:

- [[Serial GC]]: All garbage collection events are conducted serially in one thread. JVM executes the compaction after each garbage collection.
- [[Parallel GC]]: JVM uses multiple threads for minor garbage collection. It uses a single thread for major garbage collection and Old Generation compaction. Alternatively, the Parallel Old variant uses multiple threads for major garbage collection and Old Generation compaction.
- [[CMS (Concurrent Mark Sweep)]]: Multiple threads are used for minor garbage collection using the same algorithm as Parallel. Major garbage collection is multi-threaded, like Parallel Old. Still CMS runs concurrently alongside application processes to minimize “stop the world” events (i.e., when the garbage collector running stops the application). Here, the JVM does not perform compaction of memory.
- [[G1 (Garbage First)]]: The newest garbage collector is intended as a replacement for CMS. It is parallel and concurrent, like CMS. However, it works quite differently under the hood than older garbage collectors.

## What Triggers Garbage Collection?

The Garbage Collection process is triggered by a variety of events that signal to the Garbage Collector that memory needs to be reclaimed.

Here are some common events that trigger Java Garbage Collection:

1. **Allocation Failure:** When an object cannot be allocated in the heap because there is not enough contiguous free space available, the JVM triggers the Garbage Collection to free up memory.
2. **Heap Size:** When the heap reaches a certain capacity threshold, the JVM triggers Garbage Collection to reclaim memory and prevent an OutOfMemoryError.
3. **System.gc():** Calling the System.gc()  method can trigger Garbage Collection, although it does not guarantee that Garbage Collection will occur.
4. **Time-Based:** Some Garbage Collection algorithms, such as G1 Garbage Collection, use time-based triggers to initiate Garbage Collection.