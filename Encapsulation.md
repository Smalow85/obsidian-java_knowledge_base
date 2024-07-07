In Java, encapsulation is achieved by declaring the instance variables of a class as private, which means they can only be accessed within the class. To allow outside access to the instance variables, public methods called getters and setters are defined, which are used to retrieve and modify the values of the instance variables, respectively.
#### How to achieve encapsulation in Java

- Declare the variables of a class as private.
    
- Provide public setter and getter methods to modify and view the variables values.
#### Benefits of Encapsulation

- The fields of a class can be made read-only or write-only.
    
- A class can have total control over what is stored in its fields.

```java
// Java Encapsulation
 
class Name {
    // Private is using to hide the data
    private int age;
 
    // getter
    public int getAge() { return age; }
 
    // setter
    public void setAge(int age) { this.age = age; }
}
 
// Driver Class
class GFG {
    // main function
    public static void main(String[] args)
    {
        Name n1 = new Name();
        n1.setAge(19);
        System.out.println("The age of the person is: "
                           + n1.getAge());
    }
}
```