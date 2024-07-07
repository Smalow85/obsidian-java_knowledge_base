**Mediator** is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.
![[Pasted image 20240519122459.png]]
**VS**
![[Pasted image 20240519122510.png]]

**CommanderMediator.java**

```java
public interface CommanderMediator {

	void registerArmedUnits(ArmedUnit soldierUnit, ArmedUnit tankUnit);
	void setAttackStatus(boolean attackStatus);
	boolean canAttack();
	void startAttack(ArmedUnit armedUnit);
	void ceaseAttack(ArmedUnit armedUnit);
}
```

**CommanderMediatorImpl.java**

```java
public class CommanderMediatorImpl implements Commander {
	ArmedUnit soldierUnit, tankUnit;
	boolean attackStatus = true;
	
	@Override
	public void registerArmedUnits(ArmedUnit soldierUnit, ArmedUnit tankUnit) {
		this.soldierUnit = soldierUnit;
		this.tankUnit = tankUnit;
	}
	
	@Override
	public void setAttackStatus(boolean attackStatus) {
		this.attackStatus = attackStatus;
	}
	
	@Override
	public boolean canAttack() {
		return attackStatus;
	}
	
	@Override
	public void startAttack(ArmedUnit armedUnit) {
		armedUnit.attack();
	}
	
	@Override
	public void ceaseAttack(ArmedUnit armedUnit) {
		armedUnit.stopAttack();
	}
}
```

**ArmedUnit.java**

```java
public interface ArmedUnit {
	void attack();
	void stopAttack();
}
```

**SoldierUnit.java**

```java
public class SoldierUnit implements ArmedUnit{
	private Commander commander;
	
	public SoldierUnit(Commander commander) {
		this.commander=commander;
	}
	
	@Override
	public void attack() {
		if(commander.canAttack()) {
			commander.setAttackStatus(false);
		} else {
			System.out.println("SoldierUnit: Cannot attack now.");
		}
	}
	
	@Override
	public void stopAttack(){
		System.out.println("SoldierUnit: Stopped Attacking.....");
		commander.setAttackStatus(true);
	}
}
```

**TankUnit.java**

```java
public class TankUnit implements ArmedUnit{

	private Commander commander;
	public TankUnit(Commander commander) {
		this.commander=commander;
	}
	
	@Override
	public void attack() {
		if(commander.canAttack()) {
			System.out.println("TankUnit: Attacking.....");
			commander.setAttackStatus(false);
		} else {
			System.out.println("TankUnit: Cannot attack now.");
		}
	}
	
	@Override
	public void stopAttack(){
		System.out.println("TankUnit: Stopped attacking.....");
		commander.setAttackStatus(true);
	}
}
```

**Demo.java**

```java
public class Demo {

	public void testMediator() throws Exception {
		Commander commander= new CommanderImpl();
		ArmedUnit soldierUnit=new SoldierUnit(commander);
		ArmedUnit tankUnit=new TankUnit(commander);
		commander.registerArmedUnits(soldierUnit, tankUnit);
		commander.startAttack(soldierUnit);
		commander.startAttack(tankUnit);
		commander.ceaseAttack(soldierUnit);
		commander.startAttack(tankUnit);
	}
}
```





