As CPUs can carry many instructions per second, fetching from RAM isn’t ideal for them. To improve this situation, processors use tricks - [Out of Order Execution](https://en.wikipedia.org/wiki/Out-of-order_execution), [Branch Prediction](https://en.wikipedia.org/wiki/Branch_predictor), [Speculative Execution](https://en.wikipedia.org/wiki/Speculative_execution), and Caching.

This is where the following memory hierarchy comes into play:

![[Pasted image 20240624213255.png]]

As different cores execute more instructions and manipulate more data, they fill their caches with more relevant data and instructions. **This will improve the overall performance at the expense of introducing [cache coherence](https://en.wikipedia.org/wiki/Cache_coherence) challenges**.

Most modern processors write requests that won’t be applied right after they’re issued. Processors **tend to queue those writes in a special write buffer**. After a while, they’ll apply those writes to the main memory all at once.

This memory visibility may cause liveness issues in programs relying on visibility.

**Reordering** is an optimization technique for performance improvements. Interestingly, different components may apply this optimization:

- The processor may flush its write buffer in an order other than the program order.
- The processor may apply an out-of-order execution technique.
- The JIT compiler may optimize via reordering.
## _volatile_ Memory Order

We can use **volatile** to tackle the issues with Cache Coherence.

To ensure that updates to variables propagate predictably to other threads, we should apply the _volatile_ modifier to those variables.

This way, we can communicate with runtime and processor **to not reorder any instruction involving the volatile variable.** Also, processors understand that they should immediately flush any updates to these variables.
## _volatile_ and Thread Synchronization

Implementing [[Java Concurrency]], we need to ensure a couple of rules for consistent behavior:

- Mutual Exclusion – only one thread executes a critical section at a time
- Visibility – changes made by one thread to the shared data are visible to other threads to maintain data consistency

_synchronized_ methods and blocks provide both of the above properties at the cost of application performance.

_volatile_ is quite a useful keyword because it **can help ensure the visibility aspect of the data change without providing mutual exclusion**. Thus, it’s useful where we’re ok with multiple threads executing a block of code in parallel, but we need to ensure the visibility property.



