**Data Query Language (DQL)** is a fundamental sublanguage within [[SQL]] (Structured Query Language) that focuses on retrieving data from a database. Let’s explore its key characteristics:

1. **Purpose of DQL**:
    
    - DQL statements are used for **querying** data within schema objects.
    - The primary purpose is to **retrieve specific data** based on the query provided.
    - DQL allows you to impose order on the data and filter results according to specified conditions.
2. **DQL Commands**:

    - The most common DQL command is the `SELECT` statement.
    - When you execute a SELECT statement against one or more tables, it retrieves data from those tables.
    - The result of a SELECT query is compiled into a temporary table.
3. **SELECT Statement**:
    
    - The `SELECT` statement allows you to:
        - Specify which columns (attributes) you want to retrieve.
        - Define [[conditions]] (using WHERE clauses) to filter the data.
        - [[Order, grouping]] the results.
        - [[Joins]] multiple tables to combine related data.
        - Perform [[aggregate functions]] (e.g., SUM, COUNT, AVG) on the data.
4. **Example of a SELECT Query**:

```
SELECT FirstName, LastName, Salary
FROM Employees
WHERE Salary > 50000;
```