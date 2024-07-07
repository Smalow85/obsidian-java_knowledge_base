- Stream does not store elements. It simply conveys elements from a source such as a data structure, an array, or an I/O channel, through a pipeline of computational operations.
- Stream is functional in nature. Operations performed on a stream does not modify it's source. For example, filtering a Stream obtained from a collection produces a new Stream without the filtered elements, rather than removing elements from the source collection.
- Stream is lazy and evaluates code only when required.
- The elements of a stream are only visited once during the life of a stream. Like an Iterator, a new stream must be generated to revisit the same elements of the source.

#### Stream Creation

```java
Stream<String> streamEmpty = Stream.empty();
```

```java
public Stream<String> streamOf(List<String> list) { 
	return list == null || list.isEmpty() ? Stream.empty() : list.stream(); 
}
```

```java
Collection<String> collection = Arrays.asList("a", "b", "c"); 
Stream<String> streamOfCollection = collection.stream();
```

```java
Stream<String> streamBuilder = Stream.<String>builder().add("a").add("b").add("c").build();
```

```java
Stream<String> streamGenerated = Stream.generate(() -> "element").limit(10);
```

```java
Stream<Integer> streamIterated = Stream.iterate(40, n -> n + 2).limit(20);
```

#### Stream of Primitives

```java
IntStream intStream = IntStream.range(1, 3); 
LongStream longStream = LongStream.rangeClosed(1, 3);
```

#### Referencing a Stream

We can instantiate a stream, and have an accessible reference to it, as long as only intermediate operations are called. Executing a **terminal operation** makes a stream inaccessible. [[Java 8]] **streams can’t be reused.**

This kind of behavior is logical. We designed streams to apply a finite sequence of operations to the source of elements in a functional style, not to store elements.
#### Stream Pipeline

To perform a sequence of operations over the elements of the data source and aggregate their results, we need three parts: the **source**, **intermediate operation(s)** and a **terminal operation.**

Intermediate operations return a new modified stream:

```java
Stream<String> onceModifiedStream = Stream.of("abcd", "bbcd", "cbcd").skip(1);
```

If we need more than one modification, we can chain intermediate operations.

A stream by itself is worthless; the user is interested in the result of the terminal operation, which can be a value of some type or an action applied to every element of the stream. **We can only use one terminal operation per stream.**

The correct and most convenient way to use streams is by a **stream pipeline, which is a chain of the stream source, intermediate operations, and a terminal operation.

#### Lazy Invocation

**Intermediate operations are lazy.** This means that **they will be invoked only if it is necessary for the terminal operation execution.**

#### Order of Execution

From the performance point of view, **the right order is one of the most important aspects of chaining operations in the stream pipeline**. This brings us to the following rule: **intermediate operations which reduce the size of the stream should be placed before operations which are applying to each element.** So we need to keep methods such as `skip(),` `filter()` and `distinct()` at the top of our stream pipeline.
#### reduce() Method

**Reduction stream operations allow us to produce one single result from a sequence of elements**, by repeatedly applying a combining operation to the elements in the sequence.

Before we look deeper into using the _Stream.reduce()_ operation, let’s break down the operation’s participant elements into separate blocks. That way, we’ll understand more easily the role that each one plays.

- _Identity_ – an element that is the initial value of the reduction operation and the default result if the stream is empty
- _Accumulator_ – a function that takes two parameters: a partial result of the reduction operation and the next element of the stream
- _Combiner_ – a function used to combine the partial result of the reduction operation when the reduction is parallelized or when there’s a mismatch between the types of the accumulator arguments and the types of the accumulator implementation

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6); 
int result = numbers .stream() 
					.reduce(0, (subtotal, element) -> subtotal + element);
```

Here `0` is identity, and `(subtotal, element) -> subtotal + element` is accumulator.

When a stream executes in parallel, the Java runtime splits the stream into multiple sub-streams. In such cases, **we need to use a function to combine the results of the sub-streams into a single one.** **This is the role of the combiner** — in the below snippet, it’s the `Integer::sum` method reference.

```java
List<Integer> ages = Arrays.asList(25, 30, 45, 28, 32); 
int computedAges = ages.parallelStream()
						.reduce(0, (a, b) -> a + b, Integer::sum);
```

We can also use `Stream.reduce()` with custom objects that contain non-primitive fields.

```java
Rating averageRating = users.stream()
					.reduce(new Rating(), 
					(rating, user) -> Rating.average(rating, user.getRating()),
					Rating::average);
```

#### collect() method

`Stream.collect()` is one of the Java 8’s _Stream API_‘s terminal methods. It allows us to perform mutable fold operations (repackaging elements to some data structures and applying some additional logic, concatenating them, etc.) on data elements held in a _Stream_ instance.

```java
List<String> result = givenList.stream().collect(toList());
List<String> result = givenList.stream() .collect(toUnmodifiableList());

Set<String> result = givenList.stream() .collect(toSet());
List<String> result = givenList.stream() .collect(toCollection(LinkedList::new))
```

The `toMap` collector can be used to collect _Stream_ elements into a `Map` instance. To do this, we need to provide two functions:

- keyMapper
- valueMapper

We’ll use _keyMapper_ to extract a _Map_ key from a _Stream_ element, and _valueMapper_ to extract a value associated with a given key.

```java
Map<String, Integer> result = 
	givenList.stream().collect(toMap(Function.identity(), String::length))
```

Note that _toMap_ doesn’t even evaluate whether the values are also equal. If it sees duplicate keys, it immediately throws an _IllegalStateException_.

In such cases with key collision, we should use _toMap_ with another signature:

```java
Map<String, Integer> result = givenList.stream()
  .collect(toMap(Function.identity(), String::length, (item, identicalItem) -> item));
```

_GroupingBy_ collector is used for grouping objects by some property, and then storing the results in a _Map_ instance.

```java
Map<Integer, Set<String>> result = givenList.stream()
								.collect(groupingBy(String::length, toSet()));
```

_PartitioningBy_ is a specialized case of _groupingBy_ that accepts a _Predicate_ instance, and then collects _Stream_ elements into a _Map_ instance that stores _Boolean_ values as keys and collections as values. Under the “true” key, we can find a collection of elements matching the given _Predicate_, and under the “false” key, we can find a collection of elements not matching the given _Predicate_.

```java
Map<Boolean, List<String>> result = givenList.stream()
					.collect(partitioningBy(s -> s.length() > 2))
```

Examples of other collectors:

```java
List<String> result = givenList.stream()
					.collect(collectingAndThen(toList(), 
					ImmutableList::copyOf))

String result = givenList.stream() .collect(joining(" "));

Long result = givenList.stream() .collect(counting());

Double result = givenList.stream() .collect(averagingDouble(String::length));

Double result = givenList.stream() .collect(summingDouble(String::length));

Optional<String> result = givenList.stream()
									.collect(maxBy(Comparator.naturalOrder()));
```