1. **S**ingle Responsibility
2. **O**pen/Closed
3. **L**iskov Substitution
4. **I**nterface Segregation
5. **D**ependency Inversion

## Single Responsibility

This principle states that **a class should only have one responsibility. Furthermore, it should only have one reason to change.**

1. **Testing** – A class with one responsibility will have far fewer test cases.
2. **Lower coupling** – Less functionality in a single class will have fewer dependencies.
3. **Organization** – Smaller, well-organized classes are easier to search than monolithic ones.

```java
public class Book {

    private String name;
    private String author;
    private String text;

    //constructor, getters and setters

	// methods that directly relate to the book properties 
	public String replaceWordInText(String word, String replacementWord) {
		return text.replaceAll(word, replacementWord); 
	} 
	
	public boolean isWordInText(String word) { 
		return text.contains(word); 
	}
}
```

```java
public class BookPrinter {

    // methods for outputting text
    void printTextToConsole(String text){
        //our code for formatting and printing the text
    }

    void printTextToAnotherMedium(String text){
        // code for writing to any other location..
    }
}
```

## Open / Closed Principle

**Classes should be open for extension but closed for modification.** **In doing so, we** **stop ourselves from modifying existing code and causing potential new bugs** in an otherwise happy application.

```java
public class Guitar {

    private String make;
    private String model;
    private int volume;

    //Constructors, getters & setters
}
```

```java
public class SuperCoolGuitarWithFlames extends Guitar {

    private String flameColor;

    //constructor, getters + setters
}
```

## Liskov Substitution

**If class _A_ is a subtype of class _B_, we should be able to replace _B_ with _A_ without disrupting the behavior of our program.**

```java
public interface Car {

    void turnOnEngine();
    void accelerate();
}
```

```java
public class MotorCar implements Car {

    private Engine engine;

    //Constructors, getters + setters

    public void turnOnEngine() {
        //turn on the engine!
        engine.on();
    }

    public void accelerate() {
        //move forward!
        engine.powerOn(1000);
    }
}
```

But wait — we are now living in the era of electric cars:

```java
public class ElectricCar implements Car {

    public void turnOnEngine() {
        throw new AssertionError("I don't have an engine!");
    }

    public void accelerate() {
        //this acceleration is crazy!
    }
}
```

By throwing a car without an engine into the mix, we are inherently changing the behavior of our program. This is **a blatant violation of Liskov substitution and is a bit harder to fix than our previous two principles.** One possible solution would be to rework our model into interfaces that take into account the engine-less state of our _Car_.
##  Interface Segregation

**Larger interfaces should be split into smaller ones. By doing so, we can ensure that implementing classes only need to be concerned about the methods that are of interest to them.**

```java
public interface BearKeeper {
    void washTheBear();
    void feedTheBear();
    void petTheBear();
}
```

Let’s **fix this by splitting our large interface into three separate ones**:

```java
public interface BearCleaner {
    void washTheBear();
}

public interface BearFeeder {
    void feedTheBear();
}

public interface BearPetter {
    void petTheBear();
}
```

Now, thanks to interface segregation, we’re free to implement only the methods that matter to us:

```java
public class BearCarer implements BearCleaner, BearFeeder {

    public void washTheBear() {
        //I think we missed a spot...
    }

    public void feedTheBear() {
        //Tuna Tuesdays...
    }
}
```

And finally, we can leave the dangerous stuff to the reckless people:

```java
public class CrazyPerson implements BearPetter {

    public void petTheBear() {
        //Good luck with that!
    }
}
```

## Dependency Inversion

**The principle of dependency inversion refers to the decoupling of software modules. This way, instead of high-level modules depending on low-level modules, both will depend on abstractions.**

```java
public class Windows98Machine {

    private final StandardKeyboard keyboard;
    private final Monitor monitor;

    public Windows98Machine() {
        monitor = new Monitor();
        keyboard = new StandardKeyboard();
    }

}
```

This code will work, and we’ll be able to use the _StandardKeyboard_ and _Monitor_ freely within our _Windows98Computer_ class.  But, by declaring the `StandardKeyboard` and `Monitor` with the `new` keyword, we’ve tightly coupled these three classes together.

Let’s decouple our machine from the _StandardKeyboard_ by adding a more general _Keyboard_ interface and using this in our class:

```java
public interface Keyboard { }
```

```java
public class Windows98Machine{

    private final Keyboard keyboard;
    private final Monitor monitor;

    public Windows98Machine(Keyboard keyboard, Monitor monitor) {
        this.keyboard = keyboard;
        this.monitor = monitor;
    }
}
```

Here, we’re using the dependency injection pattern to facilitate adding the _Keyboard_ dependency into the _Windows98Machine_ class.

Let’s also modify our _StandardKeyboard_ class to implement the _Keyboard_ interface so that it’s suitable for injecting into the _Windows98Machine_ class:

```java
public class StandardKeyboard implements Keyboard { }
```

Now our classes are decoupled and communicate through the _Keyboard_ abstraction. If we want, we can easily switch out the type of keyboard in our machine with a different implementation of the interface. We can follow the same principle for the _Monitor_ class.







