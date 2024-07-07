Currently, by far, the most used programming paradigm is [[Object-oriented programming ]](OOP). It allows us, at the design level, to represent real-world entities as objects and their relationships. Objects contain data (attributes) and code (methods). It’s really great once we get the hang of it.

An Object-Relational Mapping tool, **ORM**, is a framework that can help and simplify the translation between the two paradigms: [[Object-oriented programming]] and [[Relational databases]] tables. It can use class definitions (models) to create, maintain and provide full access to objects’ data and their database persistence. The figure shows an extract of how an ORM would map a set of classes into relational tables:

![[Pasted image 20240512220923.png]]

## Advantages

The first advantage is that **we can use the backend’s own language to code all data access**.
Classes that deal with object-relational mappings are called **Models**. Those classes use as, a template, a class from the ORM’s library of model classes. That way, it inherits all methods needed to create, retrieve, update, and delete from the database.

The **models are weakly bounded with the rest of the application**. Meaning that changing them shall not do much harm to other components. Moreover, the table relationships are represented in the model classes. That way, foreign table keys will lead to class attributes pointing to the referenced objects.

Also, **the ORM can keep track of any changes in our own model classes, synchronizing them to the actual database structure, a process called Migration.** Besides, **it abstracts the database backend**, so we can easily switch databases or write applications compliant with a large number of database vendors and versions.

Finally, ORMs have utility functions to help backup, restore, or recreate the database. This is great to help writing testing functions, among other use cases.

## Disadvantages

On the other hand, **to properly use ORM, we must go through its steep learning curve**. Even though it uses the same language our code is based on, ORM has its own usage semantics and syntax.

The ORM ultimately takes care to translate our code into SQL. However, even though most of the time it does a nice job, it can’t compete, on complex queries, against well-written SQL.

Also, even if tries its best to create the best possible SQL clauses, it can go wrong. Especially because sometimes it is hard to gather what the ORM is doing behind the scenes.

## Should We Use ORM?

In short, yes, **we should use an ORM whenever possible**. The advantages, for most applications, exceed by far the disadvantages. Besides, we can still use, if needed, raw SQL clauses, though it is highly unadvised.

[[Hibernate]] de-facto is a standard for ORM in Java world.