**Template Method** is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

**AlgorithmSkeleton.java**

```java
public abstract class PizzaMaker {

	public void makePizza() {
		preparePizzaDough();
		preBakeCrust();
		prepareIngredients();
		addToppings();
		bakePizza();
		
		if (customerWantsPackedPizza()) {
			packPizza();
		}
	}
	
	final void preparePizzaDough() {
		System.out.println("Preparing pizza dough.");
	}
	
	final void preBakeCrust() {
		System.out.println("Pre baking crust at 325 F for 3 minutes.");
	}
	
	abstract void prepareIngredients();
	
	abstract void addToppings();
	
	void bakePizza() {
		System.out.println("Baking pizza at 400 F for 12 minutes.");
	}
	
	void packPizza() {
		System.out.println("Packing pizza in pizza delivery box.");
	}
	
	boolean customerWantsPackedPizza() {
		return true;
	}
}
```

**VegPizzaMaker.java**

```java
public class VegPizzaMaker extends PizzaMaker {

	@Override
	public void prepareIngredients() {
		System.out.println("Preparing mushroom, tomato slices, onions.");
	}
	
	@Override
	public void addToppings() {
		System.out.println("Adding mozzerella cheese and tomato sauce.");
	}
}
```

**NonVegPizzaMaker.java**

```java
public class NonVegPizzaMaker extends PizzaMaker {

	@Override
	public void prepareIngredients() {
		System.out.println("Preparing chicken ham, onion, chicken sausages.");
	}
	
	@Override
	public void addToppings() {
		System.out.println("Adding cheese, pepper jelly, and BBQ sauce.");
	}
}
```



```java
public class InHouseAssortedPizzaMaker extends PizzaMaker {

	@Override
	public void prepareIngredients() {
		System.out.println("Preparing sweet corns,chicken sausage.");
	}
	
	@Override
	public void addToppings() {
		System.out.println("Adding cheddar cheese and bechamel sauce.");
	}
	
	@Override
	public boolean customerWantsPackedPizza() {
		return false;
	}
}
```

**Demo.java**

```java
public class Demo {

	public void testMakePizza() throws Exception {
	
		System.out.println("-----Making Veg Pizza-----");
		PizzaMaker vegPizzaMaker = new VegPizzaMaker();
		vegPizzaMaker.makePizza();
		
		System.out.println("-----Making Non Veg Pizza-----");
		PizzaMaker nonVegPizzaMaker = new NonVegPizzaMaker();
		nonVegPizzaMaker.makePizza();
		
		System.out.println("-----Making In-House Assorted Pizza-----");
		PizzaMaker inHouseAssortedPizzaMaker = new InHouseAssortedPizzaMaker();
		inHouseAssortedPizzaMaker.makePizza();
	}
}
```


