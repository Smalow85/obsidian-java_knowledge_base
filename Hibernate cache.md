### What is a first-level cache and how does it work?

- [[Hibernate]] session cache is also referred to as the **first-level cache**. The session cache is a cache within a database session’s scope.
- Whenever Hibernate retrieves an entity from the database, it stores a copy of that [[Hibernate entity]] in the first-level cache associated with the current session. If the application requests the same entity again within the same session, Hibernate can simply retrieve it from the first-level cache instead of querying the database again.
- This caching mechanism can significantly improve the performance of Hibernate applications because it **reduces the number of database queries** that need to be executed. In addition, since the first-level cache is associated with a single session, it ensures that **changes made to entities within one session are not visible to other sessions until they are committed to the database**.
- The first-level cache in Hibernate is implemented using an internal HashMap that maps the entity identifier to the corresponding entity instance. When Hibernate needs to retrieve an entity, it first checks the first-level cache to see if the entity is already present. If it is, Hibernate returns the cached entity instance instead of querying the database. If the entity is not present in the first-level cache, Hibernate queries the database and stores the retrieved entity in the first-level cache before returning it to the application.
- It’s worth noting that the **first-level cache is not configurable**, and it is always enabled by default in Hibernate.

### What is a second-level cache and how does it work?

Hibernate’s second-level cache sits between your application and the database. It stores objects across sessions, reducing the need to repeatedly fetch data from the database. This can lead to noticeable improvements in response times, especially for frequently accessed data.
##### Second-level cache providers in Hibernate

1. **Ehcache**: Ehcache is a popular open-source caching library that provides an efficient in-memory caching solution for Hibernate. It supports various features such as expiration policies, distributed caching, and persistent caching.
2. **Infinispan:** Infinispan is an open-source data grid platform that provides a distributed cache for Hibernate. It supports various caching modes such as local, replicated, and distributed caching and provides features such as expiration policies, transactions, and data eviction.
3. **Hazelcast:** Hazelcast is an open-source in-memory data grid that provides a distributed caching solution for Hibernate. It supports various features such as distributed caching, data partitioning, data replication, and automatic failover.
4. **JBoss Cache:** JBoss Cache is a popular open-source caching library that provides a scalable and distributed caching solution for Hibernate. It supports various features such as distributed caching, data replication, and transactional caching.
5. **Caffeine:** Caffeine is a high-performance in-memory caching library that provides a fast and efficient caching solution for Hibernate. It supports various features such as expiration policies, eviction policies, and asynchronous loading.

##### Configuration 2 level cache

- **Choose a cache provider:** You can choose a cache provider that meets your application requirements.
- **Add the caching provider to the classpath:** You need to add the caching provider library to the classpath of your Hibernate application.
- **Configure Hibernate properties:** You need to configure Hibernate properties to enable the second-level cache and specify the caching provider. For example, to enable the Ehcache provider, you can add the following properties to the hibernate.cfg.xml file:

```xml
<property name="hibernate.cache.use_second_level_cache">true</property>
<property name="hibernate.cache.region.factory_class">org.hibernate.cache.ehcache.EhCacheRegionFactory</property>
```

- **Configure entity caching:** You need to configure entity caching for specific entities in your Hibernate application. You can do this by adding the @Cacheable annotation to the entity class and specifying the cache region name. For example:

```java
@Entity
@Cacheable
@Cache(usage = CacheConcurrencyStrategy.READ_WRITE, region="myEntityCache")
public class MyEntity {
	// ...
}
```

