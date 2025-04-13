Before getting into the topic of entity lifecycle, first, we need to understand the **persistence context**.

Simply put, the _persistence context_ sits between client code and data store. It’s a staging area where persistent data is converted to entities, ready to be read and altered by client code.

[[JPA]] `EntityManager` and [[Hibernate]] `Session` are an implementation of the _persistence context_ concept.

![[Pasted image 20240915215355.png]]
## Managed Entity

**A managed entity is a representation of a database table row** (although that row doesn’t have to exist in the database yet). 

On call to transaction _commit()_ or _flush()_, the _Session_ will find any _dirty_ entities from its tracking list and synchronize the state to the database.

Notice that we didn’t need to call any method to notify _Session_ that we changed something in our entity – since it’s a managed entity, all changes are propagated to the database automatically.

A managed entity is always a persistent entity – it must have a database identifier, even though the database row representation is not yet created i.e. the INSERT statement is pending the end of the unit of work.
## Detached Entity

**A detached entity is just an ordinary entity POJO** whose identity value corresponds to a database row. The difference from a managed entity is that it’s **not tracked anymore by any persistence context**.

An entity can become detached when the _Session_ used to load it was closed, or when we call _Session.evict(entity)_ or _Session.clear()_.

## Transient Entity

**A transient entity is simply an entity object that has no representation in the persistent store** and is not managed by any _Session_.

A typical example of a transient entity would be instantiating a new entity via its constructor.

To make a transient entity _persistent_, we need to call _Session.save(entity)_ or _Session.saveOrUpdate(entity).
# JPA Entity Lifecycle Statuses

Entities in JPA can exist in one of the following statuses:

1. New
2. Managed
3. Detached
4. Removed

- An entity starts as New when first created.
- It becomes Managed when associated with a persistence context, e.g., after being persisted or retrieved from the database.
- An entity can be Detached from the context, which means it’s no longer tracked for changes.
- Lastly, an entity can be marked as Removed, indicating it will be deleted from the database.

When working with [[JPA]] and Spring Data, it’s wise to know when to explicitly call methods such as `save()`and when it's redundant. Improper handling of save operations can lead to issues like unintended data updates, reduced performance, and potential version conflicts.
#### When to call `save()` and other methods:
1. Entities in the “New” state: When you have a new entity (i.e., it has not been saved to the database yet), an explicit call to `save()` is necessary to persist it.
2. Detached entities: If you have an entity that was previously fetched but is currently not associated with the active persistence context (i.e., it’s in the “Detached” state), an explicit call to `save()` or `merge()` is required to save any modifications.
3. Deleting an entity: Use the `delete()` method to remove an entity from the database.
#### When not to call `save()`:
1. Entities in the “Managed” state: If an entity is already in a managed state and is associated with the current persistence context, any changes to it will be automatically synchronized with the database upon transaction completion. In such cases, explicitly calling `save()` is redundant.
#### Risks of calling `save()` when it's not required:
1. Performance: Excessive `save()` calls can lead to additional system overhead, as each call might initiate a database query.
2. Unintended updates: Calling `save()` on an already managed entity can result in unintended updates of all fields of the entity, even if they were not modified.
3. Version conflicts: If versioning mechanisms are in use (for example, using the `@Version` annotation), improper save operations can lead to version conflicts.
4. Unexpected states: Depending on your JPA provider’s settings and the use case, calling `save()` on an entity might lead to various side effects, such as re-saving associated entities.


