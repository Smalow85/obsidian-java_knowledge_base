
Data Definition Language (DDL) is a subset of [[SQL]]. Its primary function is to create, modify, and delete database structures, but not data. The commands in DDL are:

1. `CREATE`: This command is used to create the database or its objects (like table, index, function, views, store procedure, and triggers).
    
    ```
    CREATE TABLE table_name (
    column1 data_type(size),
    column2 data_type(size),
    ...
    );
    ```
    
2. `DROP`: This command is used to delete an existing database or table.
    
    ```
    DROP TABLE table_name;
    ```
    
3. `ALTER`: This is used to alter the structure of the database. It is used to add, delete/drop or modify columns in an existing table.
    
    ```
    ALTER TABLE table_name ADD column_name datatype;
    ALTER TABLE table_name DROP COLUMN column_name;
    ALTER TABLE table_name MODIFY COLUMN column_name datatype(size);
    ```
    
4. `TRUNCATE`: This is used to remove all records from a table, including all spaces allocated for the records which are removed.
    
    ```
    TRUNCATE TABLE table_name;
    ```
    
5. `RENAME`: This is used to rename an object in the database.
    
    ```
    RENAME TABLE old_table_name TO new_table_name;
    ```
    

Remember: In DDL operations, `COMMIT` and `ROLLBACK` statement cannot be performed because the MySQL engine automatically commits the changes.