In the factory method pattern, you provide an interface, which can be a *interface* or an *abstract class* to create objects. A factory method in the interface defers the object creation to one or more concrete subclasses at run time. The subclasses implement the factory method to select the class whose objects need to be created.

**Pizza.java**

```java
public abstract class Pizza {
	public abstract void addIngredients();
	public void bakePizza() {
}
```

**CheesePizza.java**

```java
public class CheesePizza extends Pizza {
	@Override
	public void addIngredients() {
		System.out.println("Preparing ingredients for cheese pizza.");
	}
}
```

**PepperoniPizza.java**

```java
public class PepperoniPizza extends Pizza {
	@Override
	public void addIngredients() {
		System.out.println("Preparing ingredients for pepperoni pizza.");
	}
}
```

**VeggiePizza.java**

```java
public class VeggiePizza extends Pizza {
	@Override
	public void addIngredients() {
		System.out.println("Preparing ingredients for veggie pizza.");
	}
}
```

**BasePizzaFactory.java**

```java
public abstract class BasePizzaFactory {
	public abstract Pizza createPizza(String type);
}
```

**PizzaFactory.java**

```java
public class PizzaFactory extends BasePizzaFactory{

	@Override
	public Pizza createPizza(String type){
		Pizza pizza;
		switch (type.toLowerCase()) {
			case "cheese":
				pizza = new CheesePizza();
				break;
			case "pepperoni":
				pizza = new PepperoniPizza();
				break;
			case "veggie":
				pizza = new VeggiePizza();
				break;
			default: 
				throw new IllegalArgumentException("No such pizza.");
		}
		pizza.addIngredients();
		pizza.bakePizza();
		return pizza;
	}
}
```


