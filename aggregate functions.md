SQL aggregate functions are inbuilt functions that are used to perform some calculation on the data and return a single value. This is why they form the basis for “aggregate queries”. These functions operate on a set of rows and return a single summarized result.

==These functions ignore NULL values.==

## Common Aggregate Functions

**1. COUNT()**

Counts the number of rows.

```sql
SELECT COUNT(column_name) 
FROM table_name 
WHERE condition;
```

**2. SUM()**

Returns the sum of a numeric column.

```sql
SELECT SUM(column_name) 
FROM table_name 
WHERE condition;
```

**3. AVG()**

Returns the average value of a numeric column.

```sql
SELECT AVG(column_name) 
FROM table_name 
WHERE condition;
```

**4. MIN()**

Returns the smallest value of the selected column.

```sql
SELECT MIN(column_name) 
FROM table_name 
WHERE condition;
```

**5. MAX()**

Returns the largest value of the selected column.

```sql
SELECT MAX(column_name) 
FROM table_name 
WHERE condition;
```