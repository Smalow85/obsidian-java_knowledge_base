In the abstract factory pattern you provide an interface to create families of related or dependent objects, but you do not specify the concrete classes of the objects to create. From the client point of view, it means that a client can create a family of related objects without knowing about the object definitions and their concrete class names.

To apply the abstract factory pattern in the pizza store application, let us first create the products that the factories will produce.

**Cheese.java**

```java
public interface Cheese {
	void prepareCheese();
}
```

**GoatCheese.java**

```java
public class GoatCheese implements Cheese {
	
	public GoatCheese() {
		prepareCheese();
	}
	
	@Override
	public void prepareCheese(){
		System.out.println("Preparing goat cheese...");
	}
}
```

**MozzarellaCheese.java**

```java
public class MozzarellaCheese implements Cheese {
	
	public MozzarellaCheese() {
		prepareCheese();
	}

	@Override
	public void prepareCheese() {
		System.out.println("Preparing mozzarella cheese...");
	}
}
```

**Sauce.java**

```java
public interface Sauce {
	void prepareSauce();
}
```

**TomatoSauce.java**

```java
public class TomatoSauce implements Sauce {
	public TomatoSauce() {
		prepareSauce();
	}

	@Override
	public void prepareSauce() {
		System.out.println("Preparing tomato sauce..");
	}
}
```

**CaliforniaOilSauce.java**

```java
public class CaliforniaOilSauce implements Sauce {

	public CaliforniaOilSauce() {
		prepareSauce();
	}
	
	@Override
	public void prepareSauce() {
		System.out.println("Preparing california oil sauce..");
	}
}
```

In the above examples, we wrote the **Cheese** interface, which is an AbstractProduct. Then we wrote the **GoatCheese** and **MozzarellaCheese** classes, which are the ConcreteProduct to implement **Cheese**. Similarly for the pizza sauce, we wrote the **Sauce** interface and the **TomatoSauce** and **CaliforniaOilSauce** implementation classes.

Next, we will write the factories that will create the products. We will start with the abstract factory.

**BaseToppingFactory.java**

```java
import guru.springframework.gof.abstractFactory.topping.Cheese;
import guru.springframework.gof.abstractFactory.topping.Sauce;

public abstract class BaseToppingFactory {
	public abstract Cheese createCheese();
	public abstract Sauce createSauce();
}
```

In the example above, we wrote the **BaseToppingFactory** abstract class, the abstract factory of our application. in the abstract factory, we declare the *createCheese()* and *createSauce()* abstract methods that return **Cheese** and **Product** objects respectively. As stated in the definition of abstract factory earlier, our abstract factory (**BaseToppingFactory**) is providing an *“interface to create families of related or dependent objects“*. The related objects here are **Cheese** and **Sauce**, both of which are together used to create toppings. The definition also states *“…but you do not specify the concrete classes of the objects to create“.* As you can notice in the **BaseToppingFactory** code, our abstract factory is not concerned with any of the concrete products: **GoatCheese**, **MozzarellaCheese**, **TomatoSauce**, or **CaliforniaOilSauce**. Let us now write the ConcreteFactory implementations.

**SicillianToppingFactory.java**

```java
import guru.springframework.gof.abstractFactory.topping.Cheese;
import guru.springframework.gof.abstractFactory.topping.MozzarellaCheese;
import guru.springframework.gof.abstractFactory.topping.Sauce;
import guru.springframework.gof.abstractFactory.topping.TomatoSauce;

public class SicilianToppingFactory extends BaseToppingFactory{
    @Override
    public  Cheese createCheese(){return new MozzarellaCheese();}
    @Override
    public  Sauce createSauce(){return new TomatoSauce();}
}
```

**GourmetToppingFactory.java**

```java
import guru.springframework.gof.abstractFactory.topping.CaliforniaOilSauce;
import guru.springframework.gof.abstractFactory.topping.Cheese;
import guru.springframework.gof.abstractFactory.topping.GoatCheese;
import guru.springframework.gof.abstractFactory.topping.Sauce;

public class GourmetToppingFactory extends BaseToppingFactory{
    @Override
    public Cheese createCheese(){return new GoatCheese();}
    @Override
    public Sauce createSauce(){return new CaliforniaOilSauce();}
}
```
 