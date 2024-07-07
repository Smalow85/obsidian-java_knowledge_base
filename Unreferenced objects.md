The process of destroying unreferenced objects is called a Garbage Collection(GC). Once an object is unreferenced it is considered as an unused object, hence JVM automatically destroys that object.

There are various ways to make an object eligible for GC.
## By nullifying a reference to an object

We can set all the available object references to "null" once the purpose of creating an object is served.

```java
public class GCTest1 { 
	public static void main(String [] args) { 
		String str = "Welcome to TutorialsPoint"; // String object referenced
												 // by variable str and it is 
												 // not eligible for GC yet. 
		str = null;                          //String object referenced by
											//variable str is eligible for GC. 
		System.out.println("str eligible for GC: " + str); 
	} 
}
```
## By reassigning the reference variable to some other object

We can make the reference variable to refer to another object. Decouple the reference variable from the object and set it to refer to another object, so the object which was referring to before reassigning is eligible for GC.

```java
public class GCTest2 { 
	public static void main(String [] args) { 
		String str1 = "Welcome to TutorialsPoint"; 
		String str2 = "Welcome to Tutorix"; 
		// String object referenced by variable str1 and str2 and is not
		// eligible for GC yet. 
		str1 = str2; 
		// String object referenced by variable str1 is eligible for GC.
		System.out.println("str1: " + str1); 
	} 
}
```
