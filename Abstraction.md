It allows us ==*to hide the implementation complexities just by providing functionalities via simpler interfaces*.== In Java, we achieve abstraction by using either an **interface** or an **abstract class**.

### Interface vs. Abstract Class

Abstract classes have no restrictions on field and method modifiers, while in an interface, all are public by default. We can have instance and static initialization blocks in an abstract class, whereas we can never have them in the interface. Abstract classes may also have constructors which will get executed during the child object’s instantiation.

**Abstract classes are analogous to interfaces in some ways:**

- **We can’t instantiate either of them.** i.e., we cannot use the statement _new TypeName()_ directly to instantiate an object. If we used the aforementioned statement, we have to override all the methods using an [[anonymous class]].
- **They both might contain a set of methods declared and defined with or without their implementation.** i.e., static & default methods(defined) in an interface, instance methods(defined) in abstract class, abstract methods(declared) in both of them

## When to Use an Interface

- If the problem needs to be solved using multiple inheritances and is composed of different class hierarchies
- When unrelated classes implement our interface. For example, [Comparable](https://www.baeldung.com/java-comparator-comparable#comparable) provides the _compareTo()_ method that can be overridden to compare two objects
- When application functionalities have to be defined as a contract, but not concerned about who implements the behavior. i.e., third-party vendors need to implement it fully
## When to Use an Abstract Class

- When trying to use the inheritance concept in code (share code among many related classes), by providing common base class methods that the subclasses override
- If we have specified requirements and only partial implementation details
- While classes that extend abstract classes have several common fields or methods (that require non-public modifiers)
- If one wants to have non-final or non-static methods to modify the states of an object