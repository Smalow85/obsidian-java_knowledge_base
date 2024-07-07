
The **java.util** package contains all the classes and interfaces for the Collection framework.

![[Pasted image 20240615175307.png]]

## Iterable Interface

The Iterable interface is the root interface for all the collection classes in [[Java]]. The Collection interface extends the Iterable interface and therefore all the subclasses of Collection interface also implement the Iterable interface.

```java
public interface Iterable<T> {  
    /**  
     * Returns an iterator over elements of type {@code T}.  
     *     * @return an Iterator.  
     */    Iterator<T> iterator();
}
```

## Collection Interface

The Collection interface is the interface which is implemented by all the classes in the collection framework. **It declares the methods that every collection will have.** In other words, we can say that the Collection interface builds the foundation on which the collection framework depends.

Examples of basic methods:

```java
int size();
boolean isEmpty();
boolean contains(Object o);
boolean add(E e);
boolean remove(Object o);
boolean containsAll(Collection<?> c);
boolean addAll(Collection<? extends E> c);
boolean removeAll(Collection<?> c);
boolean retainAll(Collection<?> c);
default Stream<E> stream();
```

## List Interface

- Elements can be inserted or accessed by their position in the list, using a zero-based index.
- A list may contain duplicate elements.
- Several of the list methods will throw an UnsupportedOperationException if the collection cannot be modified, and a ClassCastException is generated when one object is incompatible with another.

### ArrayList
_ArrayList_ is one of the _List_ implementations built atop an array, which is able to dynamically grow and shrink as you add/remove elements. Elements could be easily accessed by their indexes starting from zero. This implementation has the following properties:

- Random access takes _O(1)_ time
- Adding element takes amortized constant time _O(1)_
- Inserting/Deleting takes _O(n)_ time
- Searching takes _O(n)_ time for unsorted array and _O(log n)_ for a sorted one
### LinkedList
_LinkedList_ is a doubly-linked list implementation of the *List* and *Deque* interfaces. It implements all optional list operations and permits all elements (including _null_).
Operations that index into the list will traverse the list from the beginning or the end, whichever is closer to the specified index
- It is not synchronized
- Its *Iterator* and *ListIterator* iterators are **fail-fast** (which means that after the iterator’s creation, if the list is modified, a **ConcurrentModificationException** will be thrown)
- Every element is a node, which keeps a reference to the next and previous ones
- It maintains insertion order.

## Set Interface
A **Set** is a *Collection*  that cannot contain duplicate elements.
Set also adds a stronger contract on the behavior of the equals and hashCode operations, allowing Set instances to be compared meaningfully even if their implementation types differ.

### HashSet
_HashSet_ is one of the fundamental data structures in the Java Collections API.
- It stores unique elements and permits nulls
- It’s backed by a _HashMap_
- It doesn’t maintain insertion order
- It’s not thread-safe
Note that this internal _HashMap_ gets initialized when an instance of the _HashSet_ is created:

```java
public HashSet() {
    map = new HashMap<>();
}
```
The performance of a _HashSet_ is affected mainly by two parameters – its _Initial Capacity_ and the _Load Factor_.
The expected time complexity of adding an element to a set is _O(1)_ which can drop to _O(n)_ in the worst case scenario (only one bucket present) – therefore, it’s essential to maintain the right HashSet capacity.

### TreeSet
_TreeSet_ is a sorted collection that extends the _AbstractSet_ class and implements the _NavigableSet_ interface.
- It stores unique elements
- It doesn’t preserve the insertion order of the elements
- It sorts the elements in ascending order
- It’s not thread-safe
- Not supports nulls

**In this implementation, objects are sorted and stored in ascending order according to their natural order**. The _TreeSet_ uses a self-balancing binary search tree, more specifically [[Red-Black tree]].
When compared to a _HashSet_ the performance of a _TreeSet_ is on the lower side. Operations like _add_, _remove_ and _search_ take _O(log n)_ time while operations like printing _n_ elements in sorted order require _O(n)_ time. _TreeSet_ should be our primary choice if we want to keep our entries sorted.

### LinkedHashSet
The **LinkedHashSet** is an ordered version of HashSet that maintains a doubly-linked List across all elements. When the iteration order is needed to be maintained this class is used. When iterating through a [HashSet](https://www.geeksforgeeks.org/hashset-in-java/) the order is unpredictable, while a LinkedHashSet lets us iterate through the elements in the order in which they were inserted. When cycling through LinkedHashSet using an iterator, the elements will be returned in the order in which they were inserted.

## Queue Interface

After we declare our _Queue,_ we can add new elements to the back, and remove them from the front or end (FIFO and LIFO).
1. `offer()` – Inserts a new element onto the _Queue_
2. `poll()` – Removes an element from the front of the _Queue_
3. `peek()` – Inspects the element at the front of the _Queue,_ without removing it

Generally, the _Queue_ interface is inherited by 3 main sub-interfaces: Blocking Queues, Transfer Queues and Deques.

*BlockingQueue* interface **supports additional operations which force threads to wait on the** **Queue** depending on the current state. A thread may wait on the Queue to be non-empty when attempting a retrieval, or for it to become empty when adding a new element.

*TransferQueue* interface extends the BlockingQueue interface but is tailored toward the **producer-consumer pattern**. It controls the flow of information from producer to consumer, creating backpressure in the system.

_Deque_ is short for **D**ouble-**E**nded **Que**ue and is analogous to a deck of cards – elements may be taken from both the start and end of the _Deque_. Much like the traditional _Queue,_ the Deque provides methods to add, retrieve and peek at elements held at both the top and bottom.
### Priority Queues
When new elements are inserted into the Priority Queue, they are ordered based on their **natural ordering**, or by a defined **Comparator** provided when we construct _Priority__Queue_.




