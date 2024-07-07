**Memento** is a behavioral design pattern that lets you save and restore the previous state of an object without revealing the details of its implementation.

**EmpOriginator.java**

```java
public class EmpOriginator {

	private int empId;
	private String empName;
	private String empPhoneNo;
	private String empDesignation;
	
	public EmpOriginator(int empId, 
						String empName, 
						String empPhoneNo,
						String empDesignation) {
	
		this.empId=empId;
		this.empName=empName;
		this.empPhoneNo=empPhoneNo;
		this.empDesignation=empDesignation;
	}
	
	// Getters and setters are skipped
	
	public EmpMemento saveToMemento() {
		EmpMemento empMemento = new EmpMemento(this.empId, 
											this.empName, 
											this.empPhoneNo, 
											this.empDesignation );
		return empMemento;
	}
	
	public void undoFromMemento(EmpMemento memento) {
		this.empId = memento.getEmpId();
		this.empName = memento.getEmpName();
		this.empPhoneNo = memento.getEmpPhoneNo();
		this.empDesignation = memento.getEmpDesignation();
	}
	
	public void printInfo()	{
		System.out.println("ID: "+ this.empId);
		System.out.println("Name: "+ this.empName);
		System.out.println("Phone Number: "+ this.empPhoneNo);
		System.out.println("Designation: "+ this.empDesignation);
	}
}
```

**EmpMemento.java**

```java
public class EmpMemento {
	private int empId;
	private String empName;
	private String empPhoneNo;
	private String empDesignation;
	
	public EmpMemento(int empId,
					String empName,
					String empPhoneNo,
					String empDesignation) {
		this.empId = empId;
		this.empName = empName;
		this.empPhoneNo = empPhoneNo;
		this.empDesignation = empDesignation;
	}
	
	// Getters and setters are skipped
}
```

**EmpCaretaker.java**

```java
public class EmpCaretaker {
	final Deque<EmpMemento> mementos = new ArrayDeque<>();

	public EmpMemento getMemento() {
		EmpMemento empMemento= mementos.pop();
		return empMemento;
	}
	
	public void addMemento(EmpMemento memento) {
		mementos.push(memento);
	}
}
```

Demo.java

```java
public class Demo {

	public void testMemento() throws Exception {
	
		EmpOriginator empOriginator= new EmpOriginator(306,
													"Mark Ferguson",
													"131011789610",
													"Sales Manager");
		
		EmpMemento empMemento = empOriginator.saveToMemento();
		EmpCaretaker empCaretaker = new EmpCaretaker();
		empCaretaker.addMemento(empMemento);
		
		empOriginator.setEmpPhoneNo("131011888886");
		
		empMemento = empOriginator.saveToMemento();
		empCaretaker.addMemento(empMemento);
		
		empOriginator.setEmpDesignation("Senior Sales Manager");
		empMemento = empOriginator.saveToMemento();
		empCaretaker.addMemento(empMemento);
		
		empMemento = empCaretaker.getMemento();
		empOriginator.undoFromMemento(empMemento);
		empMemento = empCaretaker.getMemento();
		empOriginator.undoFromMemento(empMemento);
	}
}
```