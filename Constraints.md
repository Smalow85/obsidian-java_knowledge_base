Data constraints in [[SQL]] are used to specify rules for the data in a table. Constraints are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the table.

## Types of SQL Data Constraints

1. `NOT NULL`: Ensures that a column cannot have a NULL value.
    
    ```sql
    CREATE TABLE Students (
        ID int NOT NULL,
        Name varchar(255) NOT NULL,
        Age int
    );
    ```
    
2. `UNIQUE`: Ensures that all values in a column are different.
    
    ```sql
    CREATE TABLE Students (
        ID int NOT NULL UNIQUE,
        Name varchar(255) NOT NULL,
        Age int
    );
    ```
    
3. `PRIMARY KEY`: Uniquely identifies each record in a database table. Primary keys must contain `UNIQUE` values. Exactly the same as the UNIQUE constraint but there can be many unique constraints in a table, but only one PRIMARY KEY constraint per table.

    ```sql
    CREATE TABLE Students (
        ID int NOT NULL,
        Name varchar(255) NOT NULL,
        Age int,
        PRIMARY KEY (ID)
    );
    ```
    
4. `FOREIGN KEY`: Prevents actions that would destroy links between tables. A FOREIGN KEY is a field (or collection of fields) in one table that refers to the `PRIMARY KEY` in another table.    
    ```sql
    CREATE TABLE Orders (
        OrderID int NOT NULL,
        OrderNumber int NOT NULL,
        ID int,
        PRIMARY KEY (OrderID),
        FOREIGN KEY (ID) REFERENCES Students(ID)
    );
    ```
    
5. `CHECK`: The CHECK constraint ensures that all values in a column satisfies certain conditions.    
    ```sql
    CREATE TABLE Students (
        ID int NOT NULL,
        Name varchar(255) NOT NULL,
        Age int,
        CHECK (Age>=18)
    );
    ```
    
6. `DEFAULT`: Provides a default value for a column when none is specified.
    ```sql
    CREATE TABLE Students (
        ID int NOT NULL,
        Name varchar(255) NOT NULL,
        Age int,
        City varchar(255) DEFAULT 'Unknown'
    );
    ```
    
7. `INDEX`: Used to create and retrieve data from the database very quickly.
    ```sql
    CREATE INDEX idx_name 
    ON Students (Name);
    ```
    
    ==Note==: Indexes are not a part of the SQL standard and are not supported by all databases.
