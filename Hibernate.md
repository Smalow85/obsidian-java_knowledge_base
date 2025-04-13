1. Hibernate supports **mapping of java classes to database tables** and vice versa. It provides features to perform CRUD operations across all the major relational databases.
2. Hibernate eliminates all the boiler-plate code that comes with JDBC and takes care of managing resources, so **we can focus on business use cases** rather than making sure that database operations are not causing resource leaks.
3. Hibernate supports **transaction management** and make sure there is no inconsistent data present in the system.
4. Since we use XML, property files or annotations for mapping java classes to database tables, it provides an **abstraction layer between application and database**.
5. Hibernate helps us in mapping joins, collections, inheritance objects and we can easily visualize how our model classes are representing database tables.
6. Hibernate provides a **powerful query language (HQL)** that is similar to SQL. However, HQL is fully object-oriented and understands concepts like inheritance, polymorphism and association.
7. Hibernate also offers integration with some external modules. For example Hibernate Validator is the reference implementation of Bean Validation (JSR 303).
8. Hibernate is an **open source project** from Red Hat Community and used worldwide. This makes it a better choice than others because learning curve is small and there are tons of online documentations and help is easily available in forums.
9. Hibernate is easy to integrate with other Java EE frameworks, it’s so popular that [[Spring Framework]] provides built-in support for integrating hibernate with Spring applications.
## Architecture

![[Pasted image 20240915155448.png]]

- **SessionFactory (org.hibernate.SessionFactory)**: SessionFactory is an immutable thread-safe cache of compiled mappings for a single database. We can get instance of `org.hibernate.Session` using `SessionFactory`.
- **Session (org.hibernate.Session)**: Session is a single-threaded, short-lived object representing a conversation between the application and the persistent store. It wraps JDBC `java.sql.Connection` and works as a factory for `org.hibernate.Transaction`.
- **Persistent objects**: Persistent objects are short-lived, single threaded objects that contains persistent state and business function. These can be ordinary JavaBeans/POJOs. They are associated with exactly one `org.hibernate.Session`.
- **Transient objects**: Transient objects are persistent classes instances that are not currently associated with a `org.hibernate.Session`. They may have been instantiated by the application and not yet persisted, or they may have been instantiated by a closed `org.hibernate.Session`.
- **Transaction (org.hibernate.Transaction)**: Transaction is a single-threaded, short-lived object used by the application to specify atomic units of work. It abstracts the application from the underlying JDBC or JTA transaction. A `org.hibernate.Session` might span multiple `org.hibernate.Transaction` in some cases.
- **ConnectionProvider (org.hibernate.connection.ConnectionProvider)**: ConnectionProvider is a factory for JDBC connections. It provides abstraction between the application and underlying `javax.sql.DataSource` or `java.sql.DriverManager`. It is not exposed to application, but it can be extended by the developer.
- **TransactionFactory (org.hibernate.TransactionFactory)**: A factory for `org.hibernate.Transaction` instances.

Hibernate provides implementation of **Java Persistence API**, so we can use [[JPA]] annotations with model beans and hibernate will take care of configuring it to be used in CRUD operations. We will look into this with annotations example.

