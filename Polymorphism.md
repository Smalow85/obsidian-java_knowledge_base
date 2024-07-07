### How is Polymorphism achieved in Java?****

Polymorphism in Java is accomplished through two distinct methods: **method overloading** and **method overriding**.

There are two types of polymorphism:

- **Compile-time polymorphism** (also called static polymorphism or *early binding*) occurs when the type of the object that is going to be acted upon is determined at compile-time. This is achieved through **method overloading**, which allows multiple methods to have the same name but different parameters within the same class.
- **Run-time polymorphism** (also called dynamic polymorphism or *late binding*) occurs when the type of the object is determined at run-time. This is achieved through **method overriding**, which allows a child class to provide a specific implementation of a method that is already defined in its parent class.

Polymorphism meaning in Java refers to the ability of objects to take on many forms. In other words, it allows different objects to respond to the same message or method call in multiple ways.

Polymorphism allows coders to write code that can work with objects of multiple classes in a generic way without knowing the specific class of each object.
#### Characteristics of Polymorphism

- It allows the same name for variables and methods with different data types. These variables are known as polymorphic variables or parameters. 
- The behavior of a method depends on the data provided. 
- It supports implicit type conversion to prevent type errors during compilation time. Implicit type conversion is when the compiler automatically converts one data type to another without loss of data. 
- The functionality of a method changes according to different scenarios.

```java
class Shape { 
	String draw() { 
		return "Drawing a shape"; 
	} 
} 

class Circle extends Shape { 
	@Override String draw() { 
		return "Drawing a circle"; 
	} 
} 

class Square extends Shape { 
	@Override String draw() { 
		return "Drawing a square"; 
	} 
} 

public class ShapeDemo { 
	public static void main(String[] args) { 
		Shape myShape = new Circle(); 
		// At runtime, Circle's draw() is called
		System.out.println(myShape.draw()); myShape = new Square(); 
		// At runtime, Square's draw() is called
		 System.out.println(myShape.draw()); 
	} 
}
```