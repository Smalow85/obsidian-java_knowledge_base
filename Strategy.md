**Strategy** is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

**EncryptionStrategy.java**

```java
public interface EncryptionStrategy {
	void encryptData(String plainText);
}
```

**AesEncryptionStrategy.java**

```java
public class AesEncryptionStrategy implements EncryptionStrategy{

	@Override
	public void encryptData(String plaintext) {
		System.out.println("---Encrypting data using AES algorithm---");
		try {
			KeyGenerator keyGenerator = KeyGenerator.getInstance("AES");
			keyGenerator.init(128);
			SecretKey secretKey = keyGenerator.generateKey();
			byte[] plaintTextByteArray = plaintext.getBytes("UTF8");
			Cipher cipher = Cipher.getInstance("AES");
			cipher.init(Cipher.ENCRYPT_MODE, secretKey);
			byte[] cipherText = cipher.doFinal(plaintTextByteArray);
			
			System.out.println("Original data: " + plaintext);
			System.out.println("Encrypted data:");
			
			for (int i = 0; i < cipherText.length; i++) {
				System.out.print(cipherText[i] + " ");
			}
		} catch(Exception ex){
			ex.printStackTrace();
		}
	}
}
```

**BlowfishEncryptionStrategy.java**

```java
public class BlowfishEncryptionStrategy implements EncryptionStrategy{

	@Override
	public void encryptData(String plaintext) {
		System.out.println("\n---Encrypting data using Blowfish algorithm---");
		try {
			KeyGenerator keyGenerator = KeyGenerator.getInstance("Blowfish");
			keyGenerator.init(128);
			SecretKey secretKey = keyGenerator.generateKey();
			byte[] plaintTextByteArray = plaintext.getBytes("UTF8");
			Cipher cipher = Cipher.getInstance("Blowfish");
			cipher.init(Cipher.ENCRYPT_MODE, secretKey);
			byte[] cipherText = cipher.doFinal(plaintTextByteArray);
			
			System.out.println("Original data: " + plaintext);
			System.out.println("Encrypted data:");
			
			for (int i = 0; i < cipherText.length; i++) {
				System.out.print(cipherText[i] + " ");
			}
		} catch(Exception ex) {
			ex.printStackTrace();
		}
	}
}
```

**Encryptor.java**

```java
public class Encryptor {

	private EncryptionStrategy strategy;
	private String plainText;
	
	public Encryptor(EncryptionStrategy strategy){
		this.strategy = strategy;
	}
	
	public void encrypt() {
		strategy.encryptData(plainText);
	}
	
	public String getPlainText() {
		return plainText;
	}
	
	public void setPlainText(String plainText) {
		this.plainText = plainText;
	}
}
```

Demo.java

```java
public class EncryptorTest {

	public void testEncrypt() throws Exception {
		EncryptionStrategy aesStrategy = new AesEncryptionStrategy();
		Encryptor aesEncryptor = new Encryptor(aesStrategy);
		aesEncryptor.setPlainText("This is plain text");
		aesEncryptor.encrypt();
		EncryptionStrategy blowfishStrategy = new BlowfishEncryptionStrategy();
		Encryptor blowfishEncryptor = new Encryptor(blowfishStrategy);
		blowfishEncryptor.setPlainText("This is plain text");
		blowfishEncryptor.encrypt();
	}
}
```