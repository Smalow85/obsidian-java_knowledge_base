![[Pasted image 20240615171138.png]]

There are three main categories of exceptional conditions in [[Java]]:

- Checked exceptions
- Unchecked exceptions / Runtime exceptions
- Errors

### Checked Exceptions

Checked exceptions are exceptions that the Java compiler requires us to handle. We have to either declaratively throw the exception up the call stack, or we have to handle it ourselves.

A couple of examples of checked exceptions are `IOException` and `ServletException`.

### Unchecked Exceptions

Unchecked exceptions are exceptions that the Java compiler **does NOT require us to handle**.

Simply put, if we create an exception that extends _RuntimeException_, it will be unchecked; otherwise, it will be checked.

[Oracle’s documentation](https://docs.oracle.com/javase/tutorial/essential/exceptions/runtime.html) tells us that there are good reasons for both concepts, like differentiating between a *situational error* (**checked**) and a *usage error* (**unchecked**).

### Errors

Errors represent serious and usually irrecoverable conditions like a library incompatibility, infinite recursion, or memory leaks.

And even though they don’t extend _RuntimeException_, they are also unchecked.

## Handling Exceptions

### throws

The simplest way to “handle” an exception is to rethrow it:

```java
public int getPlayerScore(String playerFile)
  throws FileNotFoundException {
 
    Scanner contents = new Scanner(new File(playerFile));
    return Integer.parseInt(contents.nextLine());
}
```

### try-catch

If we want to try and handle the exception ourselves, we can use a _try-catch_ block. We can handle it by rethrowing our exception:

```java
public int getPlayerScore(String playerFile) {
    try {
        Scanner contents = new Scanner(new File(playerFile));
        return Integer.parseInt(contents.nextLine());
    } catch ( FileNotFoundException noFile ) {
        logger.warn("File not found, resetting score.");
        return 0;
    }
}
```

### finally

There are times when we have code that needs to execute regardless of whether an exception occurs, and this is where the _finally_ keyword comes in.

```java
public int getPlayerScore(String playerFile)
  throws FileNotFoundException {
    Scanner contents = null;
    try {
        contents = new Scanner(new File(playerFile));
        return Integer.parseInt(contents.nextLine());
    } finally {
        if (contents != null) {
            contents.close();
        }
    }
}
```

```java
public int getPlayerScore(String playerFile) {
    Scanner contents;
    try {
        contents = new Scanner(new File(playerFile));
        return Integer.parseInt(contents.nextLine());
    } catch (FileNotFoundException noFile ) {
        logger.warn("File not found, resetting score.");
        return 0; 
    } finally {
        try {
            if (contents != null) {
                contents.close();
            }
        } catch (IOException io) {
            logger.error("Couldn't close the reader!", io);
        }
    }
}
```

### try-with-resources

As of Java 7, we can simplify the above syntax when working with things that extend _AutoCloseable_:

```java
public int getPlayerScore(String playerFile) {
    try (Scanner contents = new Scanner(new File(playerFile))) {
      return Integer.parseInt(contents.nextLine());
    } catch (FileNotFoundException e ) {
      logger.warn("File not found, resetting score.");
      return 0;
    }
}
```

### Multiple _catch_ Blocks

```java
public int getPlayerScore(String playerFile) {
    try (Scanner contents = new Scanner(new File(playerFile))) {
        return Integer.parseInt(contents.nextLine());
    } catch (IOException e) {
        logger.warn("Player file wouldn't load!", e);
        return 0;
    } catch (NumberFormatException e) {
        logger.warn("Player file was corrupted!", e);
        return 0;
    }
}
```

### Union _catch_ Blocks

```java
public int getPlayerScore(String playerFile) {
    try (Scanner contents = new Scanner(new File(playerFile))) {
        return Integer.parseInt(contents.nextLine());
    } catch (IOException | NumberFormatException e) {
        logger.warn("Failed to load score!", e);
        return 0;
    }
}
```

### Wrapping and Rethrowing

We can also choose to rethrow an exception we’ve caught:

```java
public List<Player> loadAllPlayers(String playersFile) 
  throws IOException {
    try { 
        // ...
    } catch (IOException io) { 		
        throw io;
    }
}
```

Or do a wrap and rethrow:

```java
public List<Player> loadAllPlayers(String playersFile) 
  throws PlayerLoadException {
    try { 
        // ...
    } catch (IOException io) { 		
        throw new PlayerLoadException(io);
    }
}
```

## Anti-Patterns

### Swallowing Exceptions

```java
public int getPlayerScore(String playerFile) {
    try {
        // ...
    } catch (Exception e) {} // <== catch and swallow
    return 0;
}
```

### Using _return_ in a _finally_ Block

Another way to swallow exceptions is to _return_ from the _finally_ block. This is bad because, by returning abruptly, the JVM will drop the exception, even if it was thrown from by our code:

```java
public int getPlayerScore(String playerFile) {
    int score = 0;
    try {
        throw new IOException();
    } finally {
        return score; // <== the IOException is dropped
    }
}
```

### Using _throw_ in a _finally_ Block

Similar to using _return_ in a _finally_ block, the exception thrown in a _finally_ block will take precedence over the exception that arises in the catch block.

This will “erase” the original exception from the _try_ block, and we lose all of that valuable information:

```java
public int getPlayerScore(String playerFile) {
    try {
        // ...
    } catch ( IOException io ) {
        throw new IllegalStateException(io); // <== eaten by the finally
    } finally {
        throw new OtherException();
    }
}
```