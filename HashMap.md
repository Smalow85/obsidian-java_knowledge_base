Simply put, the _HashMap_ stores values by key and provides APIs for adding, retrieving and manipulating stored data in various ways. The implementation is **based on the the principles of a hashtable**.

Key-value pairs are stored in what is known as **buckets** which together make up what is called a table, which is actually an internal array.
Once we know the key under which an object is stored or is to be stored, **storage and retrieval operations occur in constant time**, _O(1)_ in a well-dimensioned hash map.

To store a value in a hash map, we call the _put_ API which takes two parameters; a key and the corresponding value:

```java
V put(K key, V value);
```

When a value is added to the map under a key, the _hashCode()_ API of the key object is called to retrieve what is known as the initial hash value.

The _hash_ function of _HashMap_ looks like this:

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

What we should note here is only the use of the hash code from the key object to compute a final hash value.

While inside the _put_ function, the final hash value is used like this:

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```

HashMap contains an **array of Node** and Node can represent a class having the following objects : 

1. int hash
2. K key
3. V value
4. Node next
**Buckets**: A bucket is an element of the HashMap array. It is used to store nodes. Two or more nodes can have the same bucket. In that case, a link list structure is used to connect the nodes. Buckets are different in capacity.

****Index Calculation in Hashmap****

Generated hash code may be in the range of integer and if we create arrays for such a range, then it will easily cause `outOfMemoryException`. So we generate an index to minimize the size of the array. The following operation is performed to calculate the index. 

```java
int index = hashCode(key) & (n-1).
```

where `n` is the number of buckets or the size of the array. In our example, I will consider n as the default size which is 16.
## Process of inserting key-value pair

```java
HashMap map = new HashMap();
```

![empty_hasharray](https://media.geeksforgeeks.org/wp-content/uploads/Hashmap_working.jpg)

```java
map.put(new Key("vishal"), 20);
```
Steps:
    1. Calculate hash code of Key {“vishal”}. It will be generated as 118.
    2. Calculate index by using index method it will be 6.
    3. Create a Node object as : 
```
{  
  int hash = 118  
  // {"vishal"} is not a string but   
  // an object of class Key  
  Key key = {"vishal"}  
  Integer value = 20  
  Node next = null  
}
```
1. Place this object at index 6, if no other object is presented there.

Inserting another Key-Value Pair:**** Now, putting the other pair that is, 

```java
map.put(new Key("sachin"), 30);
```
Steps:
    1. Calculate hashCode of Key {“sachin”}. It will be generated as 115.
    2. Calculate index by using index method it will be 3.
    3. Create a node object as :   
```
{  
  int hash = 115  
  Key key = {"sachin"}  
  Integer value = 30  
  Node next = null  
}
```

In Case of collision:**** Now, putting another pair that is,   
```java
map.put(new Key("vaibhav"), 40);

```
Steps:
    1. Calculate hash code of Key {“vaibhav”}. It will be generated as 118.
    2. Calculate index by using index method it will be 6.
    3. Create a node object as :

```
 {  
  int hash = 118  
  Key key = {"vaibhav"}  
  Integer value = 40  
  Node next = null  
}
```

1. Place this object at index 6 if no other object is presented there.
2. In this case, a node object is found at index 6 – this is a **case of collision**.
3. In that case, check via the `hashCode()` and `equals()` method if both the keys are the same.
4. If keys are the same, replace the value with the current value.
5. Otherwise, connect this node object to the previous node object via linked list and both are stored at index 6.   

Now HashMap becomes :

![3_hasharray](https://media.geeksforgeeks.org/wp-content/uploads/Hashmap_working_3.jpg)

## Get data from map by key

Now let’s try some get methods to get a value. get(K key) method is used to get a value by its key. If you don’t know the key then it is not possible to fetch a value. 

```java
map.get(new Key("sachin"));
```
Steps:
    1. Calculate hash code of Key {“sachin”}. It will be generated as 115.
    2. Calculate index by using index method it will be 3.
    3. Go to index 3 of the array and compare the first element’s key with the given key. If both are equals then return the value, otherwise, check for the next element if it exists.
    4. In our case, it is found as the first element and the returned value is 30.

```java
map.get(new Key("vaibhav"));
```
Steps:
    1. Calculate hash code of Key {“vaibhav”}. It will be generated as 118.
    2. Calculate index by using index method it will be 6.
    3. Go to index 6 of the array and compare the first element’s key with the given key. If both are equals then return the value, otherwise, check for the next element if it exists.
    4. In our case, it is not found as the first element and the next node object is not null.
    5. If the next node is null then return null.
    6. If the next of node is not null traverse to the second element and repeat process 3 until the key is not found or next is not null.
    7. Time complexity is almost constant for the put and the get method until rehashing is not done.
    8. In case of collision, i.e. index of two or more nodes are the same, nodes are joined by a link list i.e. the second node is referenced by the first node and the third by the second, and so on.
    9. If the key given already exist in HashMap, the value is replaced with the new value.
    10. hash code of the null key is 0.
    11. When getting an object with its key, the linked list is traversed until the key matches or null is found on the next field.