_TreeMap_ is a map implementation that keeps its entries sorted according to the natural ordering of its keys or better still using a comparator if provided by the user at construction time.

_TreeMap_ implements _NavigableMap_ interface and bases its internal working on the principles of [red-black trees](https://www.baeldung.com/cs/red-black-trees):

![[Pasted image 20240616173718.png]]

**Red-Black Trees:**

![](https://media.geeksforgeeks.org/wp-content/uploads/20210112023659/redblacktree.png)

A red-black tree is a self-balancing binary search tree where each node has an extra bit, and that bit is often interpreted as the colour (red or black). These colours are used to ensure that the tree remains balanced during insertions and deletions. Although the balance of the tree is not perfect, it is good enough to reduce the searching time and maintain it around O(log n) time, where n is the total number of elements in the tree. It must be noted that as each node requires only 1 bit of space to store the colour information, these types of trees show identical memory footprint to the classic (uncoloured) binary search tree. 

1. As the name of the algorithm suggests colour of every node in the tree is either black or red.
2. The root node must be Black in colour.
3. The red node can not have a red colour neighbours node.
4. All paths from the root node to the null should consist of the same number of black nodes.

The above characteristics lead to certain properties of a node to possess which results out as follows:

1. The left elements are always less than the parent element.
2. Natural ordering is computed in logical comparison of the objects.
3. The right elements are always greater than or equal to the parent element.

_TreeMap_, unlike a hash map and linked hash map, does not employ the hashing principle anywhere since it does not use an array to store its entries.

### Default Sorting 

By default, _TreeMap_ sorts all its entries according to their natural ordering. For an integer, this would mean ascending order and for strings, alphabetical order.

### Custom Sorting

If we’re not satisfied with the natural ordering of _TreeMap_, we can also define our own rule for ordering by means of a comparator during construction of a tree map.

```java
TreeMap<Integer, String> map = new TreeMap<>(Comparator.reverseOrder());
```



