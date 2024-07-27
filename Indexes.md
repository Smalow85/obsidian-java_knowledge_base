A [[SQL]] index is a quick lookup table for finding records users need to search frequently. An index is small, fast, and optimized for quick lookups. It is very useful for connecting the relational tables and searching large tables.

```sql
CREATE INDEX name_of_Index ON name_Of_Table(Attribute1, Attribute2,...);
```

When a table is unindexed, the order of the rows will likely not be discernible by the query as optimized in any way, and your **query will therefore have to search through the rows linearly**. In other words, the queries will have to search through every row to find the rows matching the conditions.

![[Pasted image 20240329180326.png]]
## How does Indexing Work?

In reality the database table does not reorder itself every time the query conditions change in order to optimize the query performance: that would be unrealistic. In actuality, what happens is the index causes the database to create a data structure. The data structure type is very likely a [B-Tree](https://www.cs.cornell.edu/courses/cs3110/2012sp/recitations/rec25-B-trees/rec25.html). While the advantages of the B-Tree are numerous, the main advantage for our purposes is that it is **sortable**. When the data structure is sorted in order it makes our search more efficient for the obvious reasons we pointed out above.
## How Does the Database Know What Other Fields in the Table to Return?

Database indexes will also store pointers which are simply reference information for the location of the additional information in memory.

With that index, the query can search for only the rows in the `company_id` column that have 18 and then using the pointer can go into the table to find the specific row where that pointer lives. The query can then go into the table to retrieve the fields for the columns requested for the rows that meet the conditions.

If the search were presented visually, it would look like this:

![an indexed array lets a query directly find matching customer ids](https://chartio.com/assets/8dd496/tutorials/database-indexing/2fef5a846f4e7acd4b34713faba1f94855809c6b1287c14ece1067b934171f27/indexed-table.png)

## Types of index

- From the point of view of the characteristics of the index attribute:
    - Primary Index
    - Clustered Index
    - Secondary Index
- From the point of view of the number of index references to a data file:
    - Dense Index
    - Sparse Index
- Specialized indexes for highly specific scenarios:
    - Bitmap Index
    - Reverse Index
    - Hash Index
    - Filtered Index
    - Function-based Index
    - Spatial Index
### Clustered Index

One of the most common indexes available in all modern and full-fledged relational database systems is the clustered or clustering index. A clustered index defines the order in which data is **physically** stored on [DATA PAGES](https://docs.microsoft.com/en-us/sql/relational-databases/pages-and-extents-architecture-guide?view=sql-server-ver15#pages-and-extents) and implicitly in the table.
The purpose of a clustered index is to physically store the rows in ascending or descending order based on the column selected. The reason for creating such an index is to have the data always sorted, which helps very much in searching either for one or multiple values in a range. However, a clustered index shines best when we’re searching in a range.'

### Primary Index

Primary Index is an ordered file whose records are of fixed length with two fields. The first field of the index is the primary key of the data file in an ordered manner, and the second field of the ordered file contains a block pointer that points to the data block where a record containing the key is available.

### Secondary Index

A secondary index, put simply, is a way to efficiently access records in a database (the primary) by means of some piece of information other than the usual (primary) key.

```sql
CREATE TABLE students(
	student_id CHAR(4) NOT NULL,
	lastname CHAR(15), 
	firstname CHAR(15), 
	PRIMARY KEY(student_id));
CREATE INDEX lname ON students(lastname);
```

### Dense index 

**Subtype of primary index** - A dense index generates a separate entry for each value of the search key in the database. You can search more quickly, but you’ll need more room to keep index information. In this indexing, the method records have the value of the search key and the corresponding disc location.

### Sparse index

**Subtype of primary index** - In sparse index, index records are not created for every search key. An index record here contains a search key and an actual pointer to the data on the disk. To search a record, we first proceed by index record and reach at the actual location of the data. If the data we are looking for is not where we directly reach by following the index, then the system starts sequential search until the desired data is found.

### Bitmap Index

A **bitmap index** is a type of database index that uses bitmaps (bit arrays) to efficiently query and manage large datasets with many attributes, particularly those with low cardinality (a small number of distinct values). Here’s how it works:

1. **Structure**:
    - Each distinct value in a column is associated with a unique **bit vector**.
    - The bit vector has one bit for each row in the table.
    - A set bit (1) indicates the presence of the value in that row, while a cleared bit (0) indicates its absence.
2. **Advantages**:
    - **Space Efficiency**: Bitmap indexes use a compact binary representation, saving space.
    - **Fast Query Processing**: They enable quick answers to complex queries using set-based operations like AND, OR, and NOT.
    - **Low Maintenance Overhead**: Bitmap indexes can be updated incrementally, requiring less maintenance.
    - **Flexibility**: They can index both numerical and categorical data types.
3. **Use Cases**:
    - Ideal for **data warehousing** applications where fast query processing is essential.
    - Suitable for read-only systems with infrequent updates due to less efficiency with frequently updated columns.
4. **Limitations**:
    - Less efficient than traditional B-tree indexes for columns with frequent updates.
    - More often employed in systems specialized for fast querying rather than transactional databases.


### B-Tree index

B-tree indexing is the process of sorting large blocks of data to make searching through that data faster and easier. A B-tree stores data such that each node contains keys in ascending order. Each of these keys has two references to another two child nodes. The left side child node keys are less than the current keys, and the right side child node keys are more than the current keys. Its search time is **O(log(n))**

##### What is B-tree

A B-tree is a [data structure](https://builtin.com/data-science/data-structures) that provides sorted data and allows searches, sequential access, attachments and removals in sorted order. The B-tree is highly capable of storing systems that write large blocks of data. The B-tree simplifies the binary search tree by allowing nodes with more than two children. Below is a B-tree example.

![Illustration of a b-tree](https://builtin.com/sites/www.builtin.com/files/styles/ckeditor_optimize/public/inline-images/1_b-tree-indexing.jpg)

##### B+Tree vs. B-Tree: What’s the Difference?

B+ tree is another data structure that’s used to store data and looks almost the same as the B-tree. The only difference is that B+ tree stores data on the leaf nodes. This means that all non-leaf node values are duplicated in leaf nodes again. Below is a sample B+tree.

![An illustration of a B+tree](https://builtin.com/sites/www.builtin.com/files/styles/ckeditor_optimize/public/inline-images/5_b-tree-indexing.jpeg)

 The 13, 30, 9, 11, 16 and 38 non-leaf values are again repeated in leaf nodes. 

Leaf nodes include all values and all of the records are in sorted order. B+tree allows you to do the same search as B-tree, but it also allows you to  travel through all the values in a leaf node if we put a pointer to each leaf node as follows.

![Illustration of a B+tree with leaf node referencing](https://builtin.com/sites/www.builtin.com/files/styles/ckeditor_optimize/public/inline-images/6_b-tree-indexing.jpeg)

Illustration of a B+tree with leaf node referencing. | Image: Dhanushka Madushan

### Hash Index

Hash indexes use a hash function to map keys to an index position. This indexing algorithm is **most useful for exact-match queries, such as searching for a specific record based on a primary key value**. Hash indexing is commonly used in in-memory databases such as Redis.

Hash indexes work by **mapping each record in the table to a unique bucket based on its hash value.** The hash value is calculated using a **hash function,** which is a mathematical function that takes a data item as input and **returns a unique integer value**.

![](https://miro.medium.com/v2/resize:fit:1050/0*grNy-RKBPYjA1AKv.png)

To find a record in a hash index, **the database calculates the hash value of the search key and then looks up the corresponding bucket.** If the record is in the bucket, the database returns it. Otherwise, the database performs a full table scan.

