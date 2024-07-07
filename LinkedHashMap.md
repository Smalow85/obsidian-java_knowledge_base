The _LinkedHashMap_ class is very similar to _HashMap_ in most aspects. However, the linked hash map is based on both hash table and linked list to enhance the functionality of hash map.

It maintains a doubly-linked list running through all its entries in addition to an underlying array of default size 16.

To maintain the order of elements, the linked hashmap modifies the _Map.Entry_ class of _HashMap_ by adding pointers to the next and previous entries:

```java
static class Entry<K,V> extends HashMap.Node<K,V> {
    Entry<K,V> before, after;
    Entry(int hash, K key, V value, Node<K,V> next) {
        super(hash, key, value, next);
    }
}
```
## Performance Considerations

Just like _HashMap_, _LinkedHashMap_ performs the basic _Map_ operations of add, remove and contains in constant-time, as long as the hash function is well-dimensioned. It also accepts a null key as well as null values.

However, this **constant-time performance of _LinkedHashMap_ is likely to be a little worse than the constant-time of _HashMap_** due to the added overhead of maintaining a doubly-linked list.

Iteration over collection views of _LinkedHashMap_ also takes linear time _O(n)_ similar to that of _HashMap_. On the flip side, **_LinkedHashMap_‘s linear time performance during iteration is better than _HashMap_‘s linear time**.

This is because, for _LinkedHashMap_, _n_ in _O(n)_ is only the number of entries in the map regardless of the capacity. Whereas, for _HashMap_, _n_ is capacity and the size summed up, _O(size+capacity)._

Load Factor and Initial Capacity are defined precisely as for _HashMap_. Note, however, that **the penalty for choosing an excessively high value for initial capacity is less severe for _LinkedHashMap_ than for _HashMap_**, as iteration times for this class are unaffected by capacity.