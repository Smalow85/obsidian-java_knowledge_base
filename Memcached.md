**The key features of Memcached are as follows −**
- It is open source.
- Memcached server is a big hash table.
- It significantly reduces the database load
- It is perfectly efficient for websites with high database load.
- It is distributed under Berkeley Software Distribution (BSD) license.
- It is a client-server application over TCP or UDP.

**Memcached is not −**
- a persistent data store
- a database
- application-specific
- a large object cache
- fault-tolerant or highly available

Memcached is a general-purpose **distributed memory-caching system**. It is often used to speed up dynamic database-driven websites by [[Caching]] data and objects **in RAM** to reduce the number of times an external data source (such as a database or API) must be read. Memcached is free and open-source software, licensed under the Revised BSD license.

Memcached’s APIs provide a very large hash table distributed across multiple machines. When the table is full, subsequent inserts cause older data to be purged in the least recently used (LRU) order. Applications using Memcached typically layer requests and additions into RAM before falling back on a slower backing store, such as a database.

Memcached has no internal mechanism to track misses which may happen. However, some third-party utilities provide this functionality.