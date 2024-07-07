# Numeric

[[SQL]] numeric functions are used to perform operations on numeric data types such as integer, decimal, and float. They’re fundamental in manipulating data in SQL commands and are commonly used in `SELECT`, `UPDATE`, `DELETE` and `INSERT` statements.
## Examples of SQL Numeric Functions:

1. **ABS() Function:** This function returns the absolute (positive) value of a number.
```sql
SELECT ABS(-243);
```
Output:
```sql
243
```
2. **Avg() Function:** This function returns the average value of a column.

```sql
SELECT AVG(price) FROM products;
```
3. **COUNT() Function:** This function returns the number of rows that matches a specified criterion.
```sql
SELECT COUNT(productID) FROM products;
```
4. **SUM() Function:** This function returns the total sum of a numeric column.
```sql
SELECT SUM(price) FROM products;
```
5. **MIN() & MAX() Functions:** MIN() function returns the smallest value of the selected column, and MAX() function returns the largest value of the selected column.
```sql
SELECT MIN(price) FROM products;
SELECT MAX(price) FROM products;
```
6. **ROUND() Function:** This function is used to round a numeric field to the nearest integer, you can, however, specify the number of decimals to be returned.
```sql
SELECT ROUND(price, 2) FROM products;
```
7. **CEILING() Function:** This function returns the smallest integer which is greater than, or equal to, the specified numeric expression.
```sql
SELECT CEILING(price) FROM products;
```
8. **FLOOR() Function:** This function returns the largest integer which is less than, or equal to, the specified numeric expression.
```sql
SELECT FLOOR(price) FROM products;
```
9. **SQRT() Function:** This function returns the square root of a number.
```sql
SELECT SQRT(price) FROM products;
```
10. **PI() Function:** This function returns the constant Pi.
```sql
SELECT PI();
```
These are just a few examples, SQL supports many more mathematical functions such as SIN, COS, TAN, COT, POWER, etc. Understanding and using these SQL numeric functions allows you to perform complex operations on the numeric data in your SQL tables.
# String Functions

In SQL, you can perform various operations on strings, including extracting a string, combining two or more strings, and converting a case of a string.
## CONCAT Function

The CONCAT function combines two or more strings into one string. The following is the syntax:
```sql
CONCAT(string1, string2, ...., string_n)
```
Example:
```sql
SELECT CONCAT('Hello ', 'World');
```
The output of the above SQL statement will be ‘Hello World’.
## SUBSTRING Function

The SUBSTRING function extracts a string from a given string. The syntax looks as follows:
```sql
SUBSTRING(string, start, length)
```
Example:
```sql
SELECT SUBSTRING('SQL Tutorial', 1, 3);
```
The output of the above query will be ‘SQL’.
## LENGTH Function

The LENGTH function returns the length of a string. The syntax is:
```sql
LENGTH(string)
```
Example:
```sql
SELECT LENGTH('Hello World');
```
The output of the above SQL statement will be 11.
## UPPER and LOWER Function

The UPPER function converts all the letters in a string to uppercase, whereas the LOWER function to lowercase.
Syntax:
```sql
UPPER(string)

LOWER(string)
```
Examples:
```sql
SELECT UPPER('Hello World');

SELECT LOWER('Hello World');
```
The output of the above SQL statements will be ‘HELLO WORLD’ and ‘hello world’ respectively.
## TRIM Function

The TRIM function removes leading and trailing spaces of a string. You can also remove other specified characters.

Syntax:
```sql
TRIM([LEADING|TRAILING|BOTH] [removal_string] FROM original_string)
```

Example:
```sql
SELECT TRIM('   Hello World   ');
SELECT TRIM('h' FROM 'hello');
```

The output of the first query will be ‘Hello World’ and that of the second query will be ‘ello’.

# Conditional

In SQL, Conditional expressions can be used in the SELECT statement, WHERE clause, and ORDER BY clause to evaluate multiple conditions. These are SQL’s version of the common if…then…else statement in other programming languages.

There are two kinds of conditional expressions in SQL:

1. **CASE** expression
    The `CASE` expression is a flow-control statement that allows you to add if-else logic to a query. It comes in two forms: simple and searched.
    
    Here is an example of a simple `CASE` expression:
    
    ```sql
    SELECT OrderID, Quantity,
        CASE
            WHEN Quantity > 30 THEN 'Over 30'
            ELSE 'Under 30'
        END AS QuantityText
    FROM OrderDetails;
    ```
    
    A searched `CASE` statement:
    
    ```sql
    SELECT FirstName, City,
        CASE
            WHEN City = 'Berlin' THEN 'Germany'
            WHEN City = 'Madrid' THEN 'Spain'
            ELSE 'Unknown'
        END AS Country
    FROM Customers;
    ```
    
2. **COALESCE** expression
    
    The `COALESCE` function returns the first non-null value in a list. It takes a comma-separated list of values and returns the first value that is not null.
    
    An example of a `COALESCE` statement:
    
    ```
    SELECT ProductName,
        COALESCE(UnitsOnOrder, 0) As UnitsOnOrder,
        COALESCE(UnitsInStock, 0) As UnitsInStock,
    FROM Products;
    ```
    
3. **NULLIF** expression
    
    `NULLIF` returns null if the two given expressions are equal.
    
    Example of using `NULLIF`:
    
    ```
    SELECT NULLIF(5,5) AS Same,
           NULLIF(5,7) AS Different;
    ```
    
4. **IIF** expression
    
    `IIF` function returns value_true if the condition is TRUE, or value_false if the condition is FALSE.
    
    Example of using `IIF`:
    
    ```
    SELECT IIF (1>0, 'One is greater than zero', 'One is not greater than zero');
    ```
    

These are essential constructs that can greatly increase the flexibility and functionality of your SQL code, particularly when dealing with elaborate conditions and specific data selections.

# Date and Time

In SQL, the `DateTime` data type is used to work with dates and times. SQL Server comes with numerous functions for processing dates and times. Some of these include `GETDATE()`, `DATEDIFF()`, `DATEADD()`, `CONVERT()`, and so forth.
## GETDATE()

`GETDATE()` returns the current date and time as a DateTime datatype. It does not require any arguments.

```sql
SELECT GETDATE() AS CurrentDateTime;
```
## DATEDIFF()

`DATEDIFF()` returns the difference between two date values based on the unit of time you want to use. The syntax is `DATEDIFF(datepart, startdate, enddate)`.
```sql
SELECT DATEDIFF(day, '2022-01-01', '2022-01-15') AS DiffInDays;
```
## DATEADD()

`DATEADD()` adds or subtracts a specified time interval from a date. Its syntax is `DATEADD(datepart, number, date)`.

```
SELECT DATEADD(year, 1, '2022-01-01') AS NewDate;
```

## CONVERT()

`CONVERT()` is used to convert from one data type to another, and it is commonly used to format DateTime values. Its syntax is `CONVERT(data_type(length), expression, style)`.

```
SELECT CONVERT(VARCHAR(19), GETDATE()) AS FormattedDateTime;
```

Remember to replace `date` with your date in above queries.

## DateTime Format

By using appropriate format codes, SQL allows us to present dates and times in various formats.

```
SELECT FORMAT(GETDATE(), 'MM/dd/yyyy') AS DateFormatted;
```

Also, by using specific column names instead of `GETDATE()`, the same patterns can be applied to DateTime values in your data.

Note: All dates are stored as numeric values under the hood, with the integer portion representing the date and the decimal portion representing the time. Also, different database systems may use slightly different functions for handling dates and times, so be sure to check the documentation for your specific DBMS.

# JSON AND JSONB

## Selecting Data

The `->` and `->>` operators are used to access object fields and array elements in a JSONB column. The `->` operator returns JSONB objects/arrays, while `->>` returns text.

```sql
SELECT details->'specs' FROM products;
```
## Filtering Data

The `@>` operator checks if the left JSONB value contains the right JSONB path/value entries at the top level.

```sql
SELECT * FROM products WHERE details @> '{"category": "Electronics"}';
```
## Aggregating
#### jsonb_agg

Aggregates values from a set of JSONB values into a single JSON array.

```sql
SELECT jsonb_agg(details) FROM products;
```
#### jsonb_object_agg

Aggregates JSONB values into a single JSON object, using a key and value.

```sql
SELECT jsonb_object_agg(details->>'name', details->>'price') FROM products;
```
## Expansion

#### jsonb_each

Expands the outermost JSON object into a set of key-value pairs.

```sql
SELECT jsonb_each(details) FROM products;
```
#### jsonb_each_text

Similar to jsonb_each, but returns all values as text.

```sql
SELECT jsonb_each_text(details) FROM products;
```

# JSONB Query Examples
## Filtering by Top-Level Attribute Value
Filter records where a jsonb column contains a specified value at its top level.
```sql
SELECT * FROM products WHERE details->>'brand' = 'Apple';
```
## Selecting Specific Attribute Value from Items
Select a particular attribute’s value from a jsonb column.
```sql
SELECT details->>'price' AS price FROM products;
```
## Filtering Items Containing Specific Attribute

Filter records that include a certain attribute in a jsonb column.
```sql
SELECT * FROM products WHERE details ? 'warranty';
```
## Filtering by Nested Attribute Value

Filter records where a jsonb column contains a specified value in a nested object.
```sql
SELECT * FROM products WHERE details#>>'{specs, memory}' = '16GB';
```
## Filtering by Attribute in Array
Filter records where a jsonb array contains an object with a specific attribute value.
```sql
SELECT * FROM products WHERE details->'colors' @> '["red"]';
```
## Using IN Operator on Attributes
Check if the value of a jsonb attribute is within a set of values.
```sql
SELECT * FROM products WHERE details->>'category' IN ('Smartphone', 'Tablet');
```
## Inserting JSON Object
Add a new record with a jsonb column containing a complete JSON object.
```sql
INSERT INTO products (details) VALUES ('{"name": "Smart Watch", "price": 250}');
```
## Updating/Inserting Attribute
Modify an existing attribute or add a new one in a jsonb column.

```sql
UPDATE products SET details = jsonb_set(details, '{sale}', 'true', true) WHERE details->>'category' = 'Electronics';
```
## Deleting Attribute
Delete a specific attribute from a jsonb column.
```sql
UPDATE products SET details = details - 'sale';
```
## Joining Tables by JSONB Attribute
Perform a SQL join where a condition involves a jsonb attribute.
```sql
SELECT * FROM orders JOIN products ON orders.product_id = (products.details->>'id')::uuid;
```
