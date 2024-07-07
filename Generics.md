JDK 5.0 introduced [[Java]] Generics with the aim of reducing bugs and adding an extra layer of abstraction over types.
#### Type Parameters and Representation

In Java, generics use type parameters, which are placeholders for specific data types. These type parameters can be represented in two common ways:

1. **Single Letter Convention**: Typically, single uppercase letters are used to distinguish from regular class or interface names. The most commonly used parameters are  
    `E` - Element (used extensively by the Java Collections Framework)  
    `K` - Key  
    `N` - Number  
    `T` - Type  
    `V` - Value  
    They don't carry any special meaning; they're just placeholders.  
    For example, the class `java.util.HashMap<K, V>` has two type parameters, `K` and `V`, representing the type of the keys and values, respectively, stored in the map. The interface `java.util.List<E>` has a single type parameter `E` representing the type of the elements stored in the list.
2. **Descriptive Names**: Sometimes, more descriptive names like `**KeyType**` or `**ValueType**` are used for type parameters, especially when clarity is important, just as you might use labels with descriptive names for your jars.
#### Advantage of Java Generics
- No scarification of **type-safety**
- No requirement of **type-casting**
- **Compile-time** checking
- Code **reusability** and improved performance

## Types of Java Generics

### Generic Methods

You can write a single generic method declaration that can be called with arguments of different types. Based on the types of the arguments passed to the generic method, the compiler handles each method call appropriately.

**Rules to Define Generic Methods**

- All generic method declarations have a type parameter section delimited by angle brackets (< and >) that precedes the method's return type ( < E > in the next example).
    
- Each type parameter section contains one or more type parameters separated by commas. A type parameter, also known as a type [variable](https://www.tutorialspoint.com/java/java_variable_types.htm), is an identifier that specifies a generic type name.
    
- The type parameters can be used to declare the return type and act as placeholders for the types of the arguments passed to the generic method, which are known as actual type arguments.
    
- A generic method's body is declared like that of any other method. Note that type parameters can represent only reference types, not primitive types (like int, double and char).

```java
public class GenericMethodTest { 
	public static < E > void printArray( E[] inputArray ) { 
	// Display array elements 
		for (E element : inputArray) { 
			System.out.printf("%s ", element); 
		} System.out.println(); 
	}

	public static void main(String args[]) { 
		// Create arrays of Integer, Double and Character 
		Integer[] intArray = { 1, 2, 3, 4, 5 }; 
		Double[] doubleArray = { 1.1, 2.2, 3.3, 4.4 }; 
		Character[] charArray = { 'H', 'E', 'L', 'L', 'O' };
		
		System.out.println("Array integerArray contains:"); 
		printArray(intArray); // pass an Integer array 
		System.out.println("\nArray doubleArray contains:"); 
		printArray(doubleArray); // pass a Double array 
		System.out.println("\nArray characterArray contains:"); 
		printArray(charArray); // pass a Character array 
	}
}
```

### Generic Classes

A generic class declaration looks like a non-generic class declaration, except that the class name is followed by a type parameter section.

```java
public class Box<T> { 
	private T t; 
	
	public void add(T t) { 
		this.t = t; 
	}
	 
	public T get() { 
		return t; 
	} 
	
	public static void main(String[] args) { 
		Box<Integer> integerBox = new Box<Integer>(); 
		Box<String> stringBox = new Box<String>(); 
		
		integerBox.add(new Integer(10)); 
		stringBox.add(new String("Hello World")); 
		
		System.out.printf("Integer Value :%d\n\n", integerBox.get()); 
		System.out.printf("String Value :%s\n", stringBox.get()); 
	} 
}
```

### Generic Interfaces

Similar to creating Generic Classes and Generic Methods, we can also create interfaces in Java that can be used with any type of data by using generics. This type of method is known as a Generic Interface.

```java
interface InterfaceName<T1, T2, ..., Tn> {  
	// block of code  
}  
  
class ClassName<T1, T2, ..., Tn> implements InterfaceName<T1, T2, ..., Tn> {  
	// block of code  
}  
  
// Example Syntax -  
  
interface Pair<K, V> {  
	K getKey();  
	V getValue();  
}
```

## Bounded Generics

Remember that type parameters can be bounded. Bounded means “restricted,” and we can restrict the types that a method accepts.

For example, we can specify that a method accepts a type and all its subclasses (upper bound) or a type and all its superclasses (lower bound).

```java
public <T extends Number> List<T> fromArrayToList(T[] a) { 
	... 
}
```

## Using Wildcards With Generics

Wildcards are represented by the question mark _?_ in Java, and we use them to refer to an unknown type. Wildcards are particularly useful with generics and can be used as a parameter type.

```java
public static void doSomething(List<? extends T> buildings) {
    ...
}
```

This is called an upper-bounded wildcard, where type **T** is the **upper bound**.

We can also specify wildcards with a lower bound, where the unknown type has to be a supertype of the specified type. **Lower bounds** can be specified using the _super_ keyword followed by the specific type.

```java
public static void doSomething(List<? super T> buildings) {
    ...
}
```

## Type Erasure

Generics were added to Java to ensure type safety. And to ensure that generics won’t cause overhead at runtime, the compiler applies a process called _type erasure_ on generics at compile time.

Type erasure removes all type parameters and replaces them with their bounds or with _Object_ if the type parameter is unbounded. This way, the bytecode after compilation contains only normal classes, interfaces and methods, ensuring that no new types are produced. Proper casting is applied as well to the _Object_ type at compile time.

## Generics and Primitive Data Types

**One restriction of generics in Java is that the type parameter cannot be a primitive type.**

For example, the following doesn’t compile:

```java
List<int> list = new ArrayList<>();
list.add(17);
```

To understand why primitive data types don’t work, let’s remember that **generics are a compile-time feature**, meaning the type parameter is erased and all generic types are implemented as type _Object_.

So, if we want to create a list that can hold integers, we can use wrapper class:

```java
List<Integer> list = new ArrayList<>();
list.add(17);
int first = list.get(0);
```



