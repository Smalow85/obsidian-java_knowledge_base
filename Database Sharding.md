[[Relational databases]] sharding is a technique for horizontal scaling of databases, where the data is split across multiple database instances, or shards, to improve performance and reduce the impact of large amounts of data on a single database.
## Types

### Key Based Sharding

- This technique is also known as **hash-based** sharding.
- Here, we take the value of an entity such as customer ID, customer email, IP address of a client, zip code, etc and we use this value as an input of the **hash function**.
- This process generates a **hash value** which is used to determine which shard we need to use to store the data.
- We need to keep in mind that the values entered into the hash function should all come from the same column (shard key) just to ensure that data is placed in the correct order and in a consistent manner.
- Basically, shard keys act like a primary key or a unique identifier for individual rows.
##### Advantages of Key Based Sharding:
1. *Predictable Data Distribution:*
Key-based sharding provides a predictable and deterministic way to distribute data across shards. Each unique key value corresponds to a specific shard, ensuring even and predictable distribution of data.
2. *Optimized Range Queries:*
If queries involve ranges of key values, key-based sharding can be optimized to handle these range queries efficiently. This is especially beneficial when dealing with operations that span a range of consecutive key values.
##### Disadvantages of Key Based Sharding:
1. *Uneven Data Distribution:*
Explanation: If the sharding key is not well-distributed or if certain key values are more frequently accessed than others, it may result in uneven data distribution across shards, leading to potential performance bottlenecks on specific shards.
2. *Limited Scalability with Specific Keys:*
The scalability of key-based sharding may be limited if certain keys experience high traffic or if the dataset is heavily skewed toward specific key ranges. Scaling may become challenging for specific subsets of data.
3. *Complex Key Selection:*
Selecting an appropriate sharding key is crucial for effective key-based sharding. Choosing the right key may require a deep understanding of the data and query patterns, and poor choices may lead to suboptimal performance.
### Horizontal or Range Based Sharding

- In this method, we split the data based on the **ranges** of a given value inherent in each entity.
![[Pasted image 20240512133353.png]]

## Vertical Sharding

- In this method, we split the entire column from the table and we put those columns into new distinct tables.
- Data is totally independent of one partition to the other ones.
- Also, each partition holds both distinct rows and columns.
- We can split different features of an entity in different shards on different machines.
![[Pasted image 20240512133544.png]]

