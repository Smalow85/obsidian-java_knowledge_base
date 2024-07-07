**Facade** is a structural design pattern that provides a simplified interface to a library, a framework, or any other complex set of classes.

![[Pasted image 20240516000555.png]]

**Product.java**

```java
public class Product {

	public int productId;
	public String name;
	public Product(){}
	
	public Product(int productId, String name){
		this.productId=productId;
		this.name=name;
	}
}
```

**InventoryService.java**

```java
public class InventoryService {

	public static boolean isAvailable(Product product){
		/*Check Warehouse database for product availability*/
		return true;
	}
}
```

**PaymentService.java**

```java
public class PaymentService {

	public static boolean makePayment(){
		/*Connect with payment gateway for payment*/
		return true;
	}
}
```

**ShippingService.java**

```java
public class ShippingService {

	public static void shipProduct(Product product){
		/*Connect with external shipment service to ship product*/
	}
}
```

**OrderServiceFacade.java**

```java
public class OrderServiceFacadeImpl implements OrderServiceFacade{

	public boolean placeOrder(int pId){
		boolean orderFulfilled=false;
		Product product=new Product();
		product.productId=pId;
		
		if (InventoryService.isAvailable(product)) {
			System.out.println("Product with ID: " 
			+ product.productId+" is available.");
			boolean paymentConfirmed= PaymentService.makePayment();
			if (paymentConfirmed) {
				System.out.println("Payment confirmed...");
				ShippingService.shipProduct(product);
				System.out.println("Product shipped...");
				orderFulfilled=true;
			}
		}
		return orderFulfilled;
	}
}
```

