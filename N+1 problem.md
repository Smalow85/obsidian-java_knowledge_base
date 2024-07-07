The N+1 query problem happens when your code executes N additional query statements to fetch the same data from [[Relational databases]] that could have been retrieved when executing the primary query.

The larger the value of N, the more queries will be executed, the larger the performance impact. And, unlike the slow query log that can help you find slow running queries, the N+1 issue won’t be spotted because each individual additional query runs sufficiently fast to not trigger the slow query log.

The problem is executing a large number of additional queries that, overall, take sufficient time to slow down response time.

## N+1 query problem with JPA and Hibernate

When using [[JPA]] and [[Hibernate]], there are several ways you can trigger the N+1 query issue, so it’s very important to know how you can avoid these situations.

For the next examples, consider we are mapping the `post` and `post_comments` tables to the following entities:
[![Post and PostComment entities](https://i.stack.imgur.com/rZJne.png)](https://i.stack.imgur.com/rZJne.png)

The JPA mappings look like this:

```java
@Entity(name = "Post")
@Table(name = "post")
public class Post {
 
    @Id
    private Long id;
 
    private String title;
 
    //Getters and setters omitted for brevity
}
 
@Entity(name = "PostComment")
@Table(name = "post_comment")
public class PostComment {
 
    @Id
    private Long id;
 
    @ManyToOne
    private Post post;
 
    private String review;
 
    //Getters and setters omitted for brevity
}
```

## `FetchType.EAGER`

Using `FetchType.EAGER` either implicitly or explicitly for your JPA associations is a bad idea because you are going to fetch way more data that you need. More, the `FetchType.EAGER` strategy is also prone to N+1 query issues.

Unfortunately, the `@ManyToOne` and `@OneToOne` associations use `FetchType.EAGER` by default, so if your mappings look like this:

```java
@ManyToOne
private Post post;
```

You are using the `FetchType.EAGER` strategy, and, every time you forget to use `JOIN FETCH` when loading some `PostComment` entities with a JPQL or Criteria API query:

```java
List<PostComment> comments = entityManager
.createQuery("""
    select pc
    from PostComment pc
    """, PostComment.class)
.getResultList();
```

You are going to trigger the N+1 query issue:

```sql
SELECT 
    pc.id AS id1_1_, 
    pc.post_id AS post_id3_1_, 
    pc.review AS review2_1_ 
FROM 
    post_comment pc

SELECT p.id AS id1_0_0_, p.title AS title2_0_0_ FROM post p WHERE p.id = 1
SELECT p.id AS id1_0_0_, p.title AS title2_0_0_ FROM post p WHERE p.id = 2
SELECT p.id AS id1_0_0_, p.title AS title2_0_0_ FROM post p WHERE p.id = 3
SELECT p.id AS id1_0_0_, p.title AS title2_0_0_ FROM post p WHERE p.id = 4
```

Notice the additional SELECT statements that are executed because the `post` association has to be fetched prior to returning the `List` of `PostComment` entities.

Unlike the default fetch plan, which you are using when calling the `find` method of the `EntityManager`, a JPQL or Criteria API query defines an explicit plan that Hibernate cannot change by injecting a JOIN FETCH automatically. So, you need to do it manually.

If you didn't need the `post` association at all, you are out of luck when using `FetchType.EAGER` because there is no way to avoid fetching it. That's why it's better to use `FetchType.LAZY` by default.

But, if you wanted to use `post` association, then you can use `JOIN FETCH` to avoid the N+1 query problem:

```java
List<PostComment> comments = entityManager.createQuery("""
    select pc
    from PostComment pc
    join fetch pc.post p
    """, PostComment.class)
.getResultList();

for(PostComment comment : comments) {
    LOGGER.info(
        "The Post '{}' got this review '{}'", 
        comment.getPost().getTitle(), 
        comment.getReview()
    );
}
```

This time, Hibernate will execute a single SQL statement:

```sql
SELECT 
    pc.id as id1_1_0_, 
    pc.post_id as post_id3_1_0_, 
    pc.review as review2_1_0_, 
    p.id as id1_0_1_, 
    p.title as title2_0_1_ 
FROM 
    post_comment pc 
INNER JOIN 
    post p ON pc.post_id = p.id
```

## `FetchType.LAZY`

Even if you switch to using `FetchType.LAZY` explicitly for all associations, you can still bump into the N+1 issue.

This time, the `post` association is mapped like this:

```java
@ManyToOne(fetch = FetchType.LAZY)
private Post post;
```

Now, when you fetch the `PostComment` entities:

```java
List<PostComment> comments = entityManager
.createQuery("""
    select pc
    from PostComment pc
    """, PostComment.class)
.getResultList();
```

Hibernate will execute a single SQL statement:

```sql
SELECT 
    pc.id AS id1_1_, 
    pc.post_id AS post_id3_1_, 
    pc.review AS review2_1_ 
FROM 
    post_comment pc
```

But, if afterward, you are going to reference the lazy-loaded `post` association:

```java
for(PostComment comment : comments) {
    LOGGER.info(
        "The Post '{}' got this review '{}'", 
        comment.getPost().getTitle(), 
        comment.getReview()
    );
}
```

You will get the N+1 query issue:

```sql
SELECT p.id AS id1_0_0_, p.title AS title2_0_0_ FROM post p WHERE p.id = 1

SELECT p.id AS id1_0_0_, p.title AS title2_0_0_ FROM post p WHERE p.id = 2

SELECT p.id AS id1_0_0_, p.title AS title2_0_0_ FROM post p WHERE p.id = 3


SELECT p.id AS id1_0_0_, p.title AS title2_0_0_ FROM post p WHERE p.id = 4
```

Because the `post` association is fetched lazily, a secondary SQL statement will be executed when accessing the lazy association in order to build the log message.

Again, the fix consists in adding a `JOIN FETCH` clause to the JPQL query:

```sql
List<PostComment> comments = entityManager.createQuery("""
    select pc
    from PostComment pc
    join fetch pc.post p
    """, PostComment.class)
.getResultList();

for(PostComment comment : comments) {
    LOGGER.info(
        "The Post '{}' got this review '{}'", 
        comment.getPost().getTitle(), 
        comment.getReview()
    );
}
```

And, just like in the `FetchType.EAGER` example, this JPQL query will generate a single SQL statement.

> Even if you are using `FetchType.LAZY` and don't reference the child association of a bidirectional `@OneToOne` JPA relationship, you can still trigger the N+1 query issue.

