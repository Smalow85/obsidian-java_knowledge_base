![[Pasted image 20240616161516.png]]


In [[Java]], Map Interface is present in `java.util` package represents a mapping between a key and a value. Java Map interface is not a subtype of the [[Java Collections]]. Therefore it behaves a bit differently from the rest of the collection types. A map contains unique keys.

Maps are perfect to use for key-value association mapping such as dictionaries. The maps are used to perform lookups by keys or when someone wants to retrieve and update elements by keys.
### Characteristics of a Map Interface

1. A Map cannot contain duplicate keys and each key can map to at most one value. Some implementations allow null key and null values like the [[HashMap]] and [[LinkedHashMap]], but some do not like the [[TreeMap]].
2. The order of a map depends on the specific implementations. For example, *TreeMap* and LinkedHashMap have *predictable* orders, while *HashMap* does not.
3. There are two interfaces for implementing Map in Java. They are Map and *SortedMap*, and three classes: *HashMap*, *TreeMap*, and *LinkedHashMap*.
