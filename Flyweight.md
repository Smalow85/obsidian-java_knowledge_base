**Flyweight** is a structural design pattern that lets you fit more objects into the available amount of RAM by sharing common parts of state between multiple objects instead of keeping all of the data in each object.

**RaceCar.java**

```java
public abstract class RaceCar {

	/*Intrinsic state stored and shared in the Flyweight object*/
	String name;
	int speed;
	int horsePower;
	
	/* Extrinsic state is stored or computed by client objects, and passed to the Flyweight.*/
	abstract void moveCar(int currentX, int currentY, int newX ,int newY);
}
```

**FlyweightMidgetCar.java**

```java
public class FlyweightMidgetCar extends RaceCar {

	/*Track number of flyweight instantiation*/
	public static int num;
	
	public FlyweightMidgetCar() {
		num++;
	}
	
	/*This method accepts car location (extrinsic). No reference to current
	*or new location is maintained inside the flyweight implementation*/
	@Override
	public void moveCar(int currentX, int currentY, int newX ,int newY) {
		System.out.println("New location of \n" );
		System.out.println(this.name + " is X" + newX + " - Y"  +newY);
	}
}
```

**FlyweightSprintCar.java**

```java
public class FlyweightSprintCar extends RaceCar{
	/*Track number of flyweight instantiation*/
	public static int num;
	public FlyweightSprintCar() {
		num++;
	}
	
	/*This method accepts car location (extrinsic). No reference to current
	*or new location is maintained inside the flyweight implementation*/
	@Override
	public void moveCar(int currentX, int currentY, int newX, int newY) {
		System.out.println("New location of \n" );
		System.out.println(this.name + " is X" + newX + " - Y"  +newY);
	}
}
```

**CarFactory.java**

```java
public class CarFactory {

	private static Map<String, RaceCar> flyweights = new HashMap<>();
	
	/*If key exist, return flyweight from Map*/
	public static RaceCar getRaceCar(String key) {
		if (flyweights.containsKey(key)) {
			return flyweights.get(key);
		}
		RaceCar raceCar;
		/*If key does not exist in Map, create flyweight, put it in Map, 
		and return the object*/
		switch (key) {
			case "Midget":
				raceCar = new FlyweightMidgetCar();
				raceCar.name="Midget Car";
				raceCar.speed=140;
				raceCar.horsePower=400;
				break;
			case "Sprint":
				raceCar = new FlyweightSprintCar();
				raceCar.name="Sprint Car";
				raceCar.speed=160;
				raceCar.horsePower=1000;
				break;
			default:
				throw new IllegalArgumentException("Unsupported car type.");
		}
		flyweights.put(key, raceCar);
		return raceCar;
	}
}
```

**RaceCarClient.java**

```java
public class RaceCarClient {

	private RaceCar raceCar;
	
	public RaceCarClient(String name) {
		/*Ask factory for a RaceCar*/
		raceCar = CarFactory.getRaceCar(name);
	}
	
	/**
	* The extrinsic state of the flyweight is maintained by the client
	*/
	private int currentX = 0;
	private int currentY=0;
	public void moveCar(int newX, int newY){
	
	/*Car movement is handled by the flyweight object*/
	raceCar.moveCar(currentX,currentY, newX, newY);
	currentX = newX;
	currentY = newY;
	}
}
```
