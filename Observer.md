**Observer** is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.

**Subject.java**

```java
public interface Subject {
	public void registerObserver(Observer observer);
	public void removeObserver(Observer observer);
	public void notifyObservers();
	public void setBidAmount(Observer observer,BigDecimal newBidAmount);
}
```

**Product.java**

```java
public class Product implements Subject{
	private ArrayList<Observer> observers = new ArrayList<>();
	private String productName;
	private BigDecimal bidAmount;
	private Observer observer;
	
	public Product(String productName, BigDecimal bidAmount) {
		this.productName=productName;
		this.bidAmount=bidAmount;
	}
	
	@Override
	public void setBidAmount(Observer observer, BigDecimal newBidAmount) {
		int res = bidAmount.compareTo(newBidAmount);
		if(res == -1) {
			this.observer = observer;
			this.bidAmount = newBidAmount;
			notifyObservers();
		} else {
			System.out.println("New amount cannot be less or equal to: " + this.bidAmount);
		}
	}
	
	@Override
	public void registerObserver(Observer observer) {
		observers.add(observer);
	}
	
	@Override
	public void removeObserver(Observer observer) {
		observers.remove(observer);
		System.out.println("-----------------" + observer + " has withdrawn from bidding----------------");
	}
	
	@Override
	public void notifyObservers() {
		System.out.println("-----------------New bid placed----------------");
		for (Observer ob : observers) {
			ob.update(this.observer, this.productName, this.bidAmount );
		}
	}
}
```

**Observer.java**

```java
public interface Observer {
	public void update(Observer observer,String productName, BigDecimal bidAmount);
}
```



```java
public class Bidder implements Observer{
	String bidderName;
	
	public Bidder(String bidderName) {
		this.bidderName = bidderName;
	}
	
	@Override
	public void update(Observer observer,
						String productName, 
						BigDecimal bidAmount) {
		if (observer.toString().equals(bidderName)) {
			System.out.println("Hello " + bidderName + "! New bid of amount " + bidAmount + " has been placed on " + productName + " by you");
		
		}
		
		if(!observer.toString().equals(bidderName)) {
			System.out.println("Hello " + bidderName + "! New bid of amount " + bidAmount + " has been placed on " + productName + " by " + observer);
		}
	}
	
	@Override
	public String toString(){
		return bidderName;
	}
}
```