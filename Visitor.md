**Visitor** is a behavioral design pattern that lets you separate algorithms from the objects on which they operate.

**MailClient.java**

```java
public interface MailClient {
	void sendMail(String[] mailInfo);
	void receiveMail(String[] mailInfo);
	boolean configureForMac();
	boolean configureForWindows();
}
```

**OperaMailClient.java**

```java
public class OperaMailClient implements MailClient {

	@Override
	public void sendMail(String[] mailInfo) {
		System.out.println(" OperaMailClient: Sending mail");
	}
	
	@Override
	public void receiveMail(String[] mailInfo) {
		System.out.println(" OperaMailClient: Receiving mail");
	}
	
	@Override
	public boolean configureForMac() {
		System.out.println("Configuration of Opera mail client complete");
		return true;
	}
	
	@Override
	public boolean configureForWindows() {
		System.out.println("Configuration of Opera mail client");
		return true;
	}
}
```

**SquirrelMailClient.java**

```java
public class SquirrelMailClient implements MailClient {
	
	@Override
	public void sendMail(String[] mailInfo) {
		System.out.println("SquirrelMailClient: Sending mail");
	}
	
	@Override
	public void receiveMail(String[] mailInfo) {
		System.out.println("SquirrelMailClient: Receiving mail");
	}
	
	@Override
	public boolean configureForMac() {
		System.out.println("Configuration of Squirrel mail client complete");
		return true;
	}
	
	@Override
	public boolean configureForWindows() {
		System.out.println("Configuration of Squirrel mail client complete");
		return true;
	}
}
```

**ZimbraMailClient.java**

```java
public class ZimbraMailClient implements MailClient{

	@Override
	public void sendMail(String[] mailInfo) {
		System.out.println(" ZimbraMailClient: Sending mail");
	}
	
	@Override
	public void receiveMail(String[] mailInfo) {
		System.out.println(" ZimbraMailClient: Receiving mail");
	}
	
	@Override
	public boolean accept(MailClientVisitor visitor) {
		visitor.visit(this);
		return true;
	}
}
```

**MailClientVisitor.java**

```java
public interface MailClientVisitor {
	void visit(OperaMailClient mailClient);
	void visit(SquirrelMailClient mailClient);
	void visit(ZimbraMailClient mailClient);
}
```

**WindowsMailClientVisitor.java**

```java
public class WindowsMailClientVisitor implements MailClientVisitor{

	@Override
	public void visit(OperaMailClient mailClient) {
		System.out.println("Configuration of Opera mail client");
	}
	
	@Override
	public void visit(SquirrelMailClient mailClient) {
		System.out.println("Configuration of Squirrel mail client");
	}
	
	@Override
	public void visit(ZimbraMailClient mailClient) {
		System.out.println("Configuration of Zimbra mail client");
	}
}
```

**MacMailClientVisitor.java**

```java
public class MacMailClientVisitor implements MailClientVisitor{

	@Override
	public void visit(OperaMailClient mailClient) {
		System.out.println("Configuration of Opera mail client");
	}
	
	@Override
	public void visit(SquirrelMailClient mailClient) {
	System.out.println("Configuration of Squirrel mail client");
	}
	
	@Override
	public void visit(ZimbraMailClient mailClient) {
		System.out.println("Configuration of Zimbra mail client");
	}
}
```


```java
public class LinuxMailClientVisitor implements MailClientVisitor{

	@Override
	public void visit(OperaMailClient mailClient) {
		System.out.println("Configuration of Opera mail client");
	}
	
	@Override
	public void visit(SquirrelMailClient mailClient) {
		System.out.println("Configuration of Squirrel mail client");
	}
	
	@Override
	public void visit(ZimbraMailClient mailClient) {
		System.out.println("Configuration of Zimbra mail client");
	}
}
```

**Demo.java**

```java
public class MailClientVisitorTest {

	private MacMailClientVisitor macVisitor;
	private LinuxMailClientVisitor linuxVisitor;
	private WindowsMailClientVisitor windowsVisitor;
	private OperaMailClient operaMailClient;
	private SquirrelMailClient squirrelMailClient;
	private ZimbraMailClient zimbraMailClient;
	
	public void test() {
	
		macVisitor=new MacMailClientVisitor();
		linuxVisitor=new LinuxMailClientVisitor();
		windowsVisitor=new WindowsMailClientVisitor();
		operaMailClient = new OperaMailClient();
		squirrelMailClient=new SquirrelMailClient();
		zimbraMailClient=new ZimbraMailClient();
		
		System.out.println("-----Testing Opera Mail Client-----");
		operaMailClient.accept(macVisitor);
		operaMailClient.accept(linuxVisitor);
		operaMailClient.accept(windowsVisitor);
	
		System.out.println("-----Testing Squirrel Mail Client-----");
	
		squirrelMailClient.accept(macVisitor));
		squirrelMailClient.accept(linuxVisitor));
		squirrelMailClient.accept(windowsVisitor));
	
		System.out.println("-----Testing Zimbra Mail Client-----");
		
		zimbraMailClient.accept(macVisitor);
		zimbraMailClient.accept(linuxVisitor);
		zimbraMailClient.accept(windowsVisitor);
	}
}
```