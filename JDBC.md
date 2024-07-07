Java database connectivity (JDBC) is the JavaSoft specification of a standard application programming interface (API) that allows [[Java]] programs to access database management systems.

Using these standard interfaces and classes, programmers can write applications that connect to databases, send queries written in structured query language (SQL), and process the results.

Since JDBC is a standard specification, one Java program that uses the JDBC API can connect to any database management system (DBMS), as long as a driver exists for that particular DBMS.

## JDBC Drivers

A JDBC driver is a JDBC API implementation used for connecting to a particular type of database. There are several types of JDBC drivers:

- Type 1 – contains a mapping to another data access API; an example of this is the JDBC-ODBC driver
- Type 2 – is an implementation that uses client-side libraries of the target database; also called a native-API driver
- Type 3 – uses middleware to convert JDBC calls into database-specific calls; also known as a network protocol driver
- Type 4 – connect directly to a database by converting JDBC calls into database-specific calls; known as database protocol drivers or thin drivers,

The most commonly used type is type 4, as it has the advantage of being **platform-independent**. Connecting directly to a database server provides better performance compared to other types. The downside of this type of driver is that it’s database-specific – given each database has its own specific protocol.

## Connecting to a Database

To connect to a database, we simply have to initialize the driver and open a database connection.

### 1. Registering the Driver

For our example, we will use a type 4 database protocol driver.

Since we’re using a MySQL database, we need the [_mysql-connector-java_](https://mvnrepository.com/artifact/mysql/mysql-connector-java) dependency:

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.32</version>
</dependency>
```

Next, let’s register the driver using the _Class.forName()_ method, which dynamically loads the driver class:

```java
Class.forName("com.mysql.cj.jdbc.Driver");
```

In older versions of JDBC, before obtaining a connection, we first had to initialize the JDBC driver by calling the _Class.forName_ method. [As of JDBC 4.0](https://www.baeldung.com/java-jdbc-loading-drivers), **all drivers that are found in the classpath are automatically loaded**. Therefore, we won’t need this _Class.forName_ part in modern environments.

### 2. Creating the Connection

To open a connection, we can use the _getConnection()_ method of _DriverManager_ class. This method requires a connection URL _String_ parameter:

```java
try (Connection con = DriverManager
  .getConnection("jdbc:mysql://localhost:3306/myDb", "user1", "pass")) {
    // use con here
}
```

Since the _Connection_ is an **AutoCloseable** resource, we should use it inside a **try-with-resources**  block.

The syntax of the connection URL depends on the type of database used. Let’s take a look at a few examples:

```plaintext
jdbc:mysql://localhost:3306/myDb?user=user1&password=pass
jdbc:hsqldb:mem:myDb
jdbc:postgresql://localhost/myDb
```

To connect to the specified _myDb_ database, we will have to create the database and a user, and add grant necessary access:
## Executing SQL Statements

The send SQL instructions to the database, we can use instances of type _Statement_, _PreparedStatement,_ or _CallableStatement,_ which we can obtain using the _Connection_ object.

### 1. Statement

The _Statement_ interface contains the essential functions for executing SQL commands.

First, let’s create a _Statement_ object:

```java
try (Statement stmt = con.createStatement()) {
    // use stmt here
}
```

Again, we should work with Statements inside a _try-with-resources_ block for automatic resource management.

Anyway, executing SQL instructions can be done through the use of three methods:

- _executeQuery()_ for SELECT instructions
- _executeUpdate()_ for updating the data or the database structure
- _execute()_ can be used for both cases above when the result is unknown

Let’s use the _execute()_ method to add a _students_ table to our database:

```java
String tableSql = "CREATE TABLE IF NOT EXISTS employees" 
  + "(emp_id int PRIMARY KEY AUTO_INCREMENT, name varchar(30),"
  + "position varchar(30), salary double)";
stmt.execute(tableSql);
```

When using the _execute()_ method to update the data, then the _stmt.getUpdateCount()_ method returns the number of rows affected.
### 2. PreparedStatement

_PreparedStatement_ objects contain precompiled SQL sequences. They can have one or more parameters denoted by a question mark.

Let’s create a _PreparedStatement_ which updates records in the _employees_ table based on given parameters:

```java
String updatePositionSql = "UPDATE employees SET position=? WHERE emp_id=?";
try (PreparedStatement pstmt = con.prepareStatement(updatePositionSql)) {
    // use pstmt here
}
```

To add parameters to the _PreparedStatement_, we can use simple setters – _setX()_ – where X is the type of the parameter, and the method arguments are the order and value of the parameter:

```java
pstmt.setString(1, "lead developer");
pstmt.setInt(2, 1);
```

The statement is executed with one of the same three methods described before: _executeQuery(), executeUpdate(), execute()_ without the SQL _String_ parameter:

```java
int rowsAffected = pstmt.executeUpdate();
```

### 3. CallableStatement

The _CallableStatement_ interface allows calling stored procedures.

To create a _CallableStatement_ object, we can use the _prepareCall()_ method of _Connection_:

```java
String preparedSql = "{call insertEmployee(?,?,?,?)}";
try (CallableStatement cstmt = con.prepareCall(preparedSql)) {
    // use cstmt here
}
```

Setting input parameter values for the stored procedure is done like in the _PreparedStatement_ interface, using _setX()_ methods:

```java
cstmt.setString(2, "ana");
cstmt.setString(3, "tester");
cstmt.setDouble(4, 2000);
```

If the stored procedure has output parameters, we need to add them using the _registerOutParameter()_ method:

```java
cstmt.registerOutParameter(1, Types.INTEGER);
```

Then let’s execute the statement and retrieve the returned value using a corresponding _getX()_ method:

```java
cstmt.execute();
int new_id = cstmt.getInt(1);
```

For example to work, we need to create the stored procedure in our MySql database:

```sql
CREATE PROCEDURE insertEmployee(OUT emp_id int, 
  IN emp_name varchar(30), IN position varchar(30), IN salary double) 
BEGIN
INSERT INTO employees(name, position,salary) VALUES (emp_name,position,salary);
SET emp_id = LAST_INSERT_ID();
```

The _insertEmployee_ procedure above will insert a new record into the _employees_ table using the given parameters and return the id of the new record in the _emp_id_ out parameter.
## Parsing Query Results

After executing a query, the result is represented by a _ResultSet_ object, which has a structure similar to a table, with lines and columns.

### 1. ResultSet Interface

The _ResultSet_ uses the _next()_ method to move to the next line.

Let’s first create an _Employee_ class to store our retrieved records:

```java
public class Employee {
    private int id;
    private String name;
    private String position;
    private double salary;
 
    // standard constructor, getters, setters
}
```

Next, let’s traverse the _ResultSet_ and create an _Employee_ object for each record:

```java
String selectSql = "SELECT * FROM employees"; 
try (ResultSet resultSet = stmt.executeQuery(selectSql)) {
    List<Employee> employees = new ArrayList<>(); 
    while (resultSet.next()) { 
        Employee emp = new Employee(); 
        emp.setId(resultSet.getInt("emp_id")); 
        emp.setName(resultSet.getString("name")); 
        emp.setPosition(resultSet.getString("position")); 
        emp.setSalary(resultSet.getDouble("salary")); 
        employees.add(emp); 
    }
}
```

Retrieving the value for each table cell can be done using methods of type _getX(_) where X represents the type of the cell data.

The _getX()_ methods can be used with an _int_ parameter representing the order of the cell, or a _String_ parameter representing the name of the column. The latter option is preferable in case we change the order of the columns in the query.
### 2. Updatable ResultSet

Implicitly, a _ResultSet_ object can only be traversed forward and cannot be modified.

If we want to use the _ResultSet_ to update data and traverse it in both directions, we need to create the _Statement_ object with additional parameters:

```java
stmt = con.createStatement(
  ResultSet.TYPE_SCROLL_INSENSITIVE, 
  ResultSet.CONCUR_UPDATABLE
);
```

To navigate this type of _ResultSet_, we can use one of the methods:

- _first(), last(), beforeFirst(), beforeLast()_ – to move to the first or last line of a _ResultSet_ or to the line before these
- _next(), previous()_ – to navigate forward and backward in the _ResultSet_
- _getRow() –_ to obtain the current row number
- _moveToInsertRow(), moveToCurrentRow()_ – to move to a new empty row to insert and back to the current one if on a new row
- _absolute(int row) –_ to move to the specified row
- _relative(int nrRows)_ – to move the cursor the given number of rows

Updating the _ResultSet_ can be done using methods with the format _updateX()_ where X is the type of cell data. These methods only update the _ResultSet_ object and not the database tables.

To persist the _ResultSet_ changes to the database, we must further use one of the methods:

- _updateRow()_ – to persist the changes to the current row to the database
- _insertRow(), deleteRow()_ – to add a new row or delete the current one from the database
- _refreshRow()_ – to refresh the _ResultSet_ with any changes in the database
- _cancelRowUpdates()_ – to cancel changes made to the current row

Let’s take a look at an example of using some of these methods by updating data in the _employee’s_ table:

```java
try (Statement updatableStmt = con.createStatement(
  ResultSet.TYPE_SCROLL_INSENSITIVE, ResultSet.CONCUR_UPDATABLE)) {
    try (ResultSet updatableResultSet = updatableStmt.executeQuery(selectSql)) {
        updatableResultSet.moveToInsertRow();
        updatableResultSet.updateString("name", "mark");
        updatableResultSet.updateString("position", "analyst");
        updatableResultSet.updateDouble("salary", 2000);
        updatableResultSet.insertRow();
    }
}
```

## Handling Transactions

By default, each SQL statement is committed right after it is completed. However, it’s also possible to **control transactions programmatically**.

This may be necessary in cases when we want to preserve data consistency, for example when we only want to commit a transaction if a previous one has completed successfully.

First, we need to set the _autoCommit_ property of _Connection_ to _false_, then use the _commit()_ and _rollback()_ methods to control the transaction.

Let’s add a second update statement for the _salary_ column after the employee _position_ column update and wrap them both in a transaction. This way, the salary will be updated only if the position was successfully updated:

```java
String updatePositionSql = "UPDATE employees SET position=? WHERE emp_id=?";
PreparedStatement pstmt = con.prepareStatement(updatePositionSql);
pstmt.setString(1, "lead developer");
pstmt.setInt(2, 1);

String updateSalarySql = "UPDATE employees SET salary=? WHERE emp_id=?";
PreparedStatement pstmt2 = con.prepareStatement(updateSalarySql);
pstmt.setDouble(1, 3000);
pstmt.setInt(2, 1);

boolean autoCommit = con.getAutoCommit();
try {
    con.setAutoCommit(false);
    pstmt.executeUpdate();
    pstmt2.executeUpdate();
    con.commit();
} catch (SQLException exc) {
    con.rollback();
} finally {
    con.setAutoCommit(autoCommit);
}
```

For the sake of brevity, we omit the _try-with-resources_ blocks here.
## 8. Closing the Resources

When we’re no longer using it, **we need to close the connection to release database resources**.

We can do this using the _close()_ API:

```java
con.close();
```

However, if we’re using the resource in a _try-with-resources_ block, we don’t need to call the _close()_ method explicitly, as the _try-with-resources_ block does that for us automatically.