One to one represents in that a single entity is associated with a single instance of the other entity. An instance of a source entity can be at most mapped to one instance of the target entity.
## Foreign Key
The _address_id_ column in _users_ is the foreign key to _address_.

![[Pasted image 20240916162352.png]]
`User.class`

```java
@Entity
@Table(name = "users")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;
    //... 

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "address_id", referencedColumnName = "id")
    private Address address;

    // ... getters and setters
}
```

We place the `@OneToOne` annotation on the related entity field `Address`.
Also, we need to place [[@JoinColumn]] annotation to select a default one.

`Address.class`

```java
@Entity
@Table(name = "address")
public class Address {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;
    //...

    @OneToOne(mappedBy = "address")
    private User user;

    //... getters and setters
}
```

We won’t use the `@JoinColumn` annotation there. This is because we only need it on the owning side of the foreign key relationship. Simply put, whoever owns the foreign key column gets the `@JoinColumn` annotation. We also need to place the `@OneToOne` annotation here too. That’s because this is a **bidirectional relationship**. The address side of the relationship is called the non-owning side.
## Shared Primary Key
In this strategy, instead of creating a new column `address_id`, we’ll mark the primary key column (`user_id`) of the `address` table as the foreign key to the `users` table:
[![An ER diagram with Users Tied to Addresses where they share the same primary key values](https://www.baeldung.com/wp-content/uploads/2018/12/1-1-SK.png)
```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;

    //...

    @OneToOne(mappedBy = "user", cascade = CascadeType.ALL)
    @PrimaryKeyJoinColumn
    private Address address;

    //... getters and setters
}
```

```java
@Entity
@Table(name = "address")
public class Address {

    @Id
    @Column(name = "user_id")
    private Long id;

    //...

    @OneToOne
    @MapsId
    @JoinColumn(name = "user_id")
    private User user;
   
    //... getters and setters
}
```

The `mappedBy` attribute is now moved to the `User` class since the foreign key is now present in the `address` table. We’ve also added the `@PrimaryKeyJoinColumn` annotation, which indicates that the primary key of the `User` entity is used as the foreign key value for the associated `Address` entity.

We still have to define an `@Id` field in the `Address` class. But note that this references the `user_id` column, and it no longer uses the `@GeneratedValue` annotation. Also, on the field that references the `User`, we’ve added the `@MapsId` annotation, which indicates that the primary key values will be copied from the `User` entity.
## Join Table
One-to-one mappings can be of two types: optional and mandatory. **So far, we’ve seen only mandatory relationships.** Now let’s imagine that our employees get associated with a workstation. It’s one-to-one, but sometimes an employee might not have a workstation and vice versa.

![[Pasted image 20240916171525.png]]


```java
@Entity
@Table(name = "employee")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;

    //...

    @OneToOne(cascade = CascadeType.ALL)
    @JoinTable(name = "emp_workstation", 
      joinColumns = 
        { @JoinColumn(name = "employee_id", referencedColumnName = "id") },
      inverseJoinColumns = 
        { @JoinColumn(name = "workstation_id", referencedColumnName = "id") })
    private WorkStation workStation;

    //... getters and setters
}
```

```java
@Entity
@Table(name = "workstation")
public class WorkStation {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;

    //...

    @OneToOne(mappedBy = "workStation")
    private Employee employee;

    //... getters and setters
}
```

`@JoinTable` instructs Hibernate to employ the join table strategy while maintaining the relationship. Also, `Employee` is the owner of this relationship, as we chose to use the join table annotation on it. 



