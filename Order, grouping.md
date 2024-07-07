### ORDER BY

The `ORDER BY` clause in SQL is used to sort the result-set from a SELECT statement in ascending or descending order. It sorts the records in ascending order by default. If you want to sort the records in descending order, you have to use the `DESC` keyword.

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC;
```

You can also sort by multiple columns. Sort the table by the `AGE` column in ascending order and then `SALARY` in descending order:

```sql
SELECT * FROM Customers
ORDER BY AGE ASC, SALARY DESC;
```

### GROUP BY

 `GROUP BY` is a clause in SQL that is used to arrange identical data into groups.

```sql
SELECT column1, column2
FROM table_name
GROUP BY column1, column2;
```
##### Example:

Assume we have a “Sales” table. This table has three columns: ID, Item, and Amount.

```sql
ID     Item    Amount
---   ------   ------
1      A        150
2      B        200
3      A        100
4      B        50
5      A        200
6      A        100
7      B        150
```

Execute the following SQL statement…

```sql
SELECT Item, SUM(Amount)
FROM Sales
GROUP BY Item;
```

This will concatenate, or “group”, all items that are the same into one row, applying the SUM() function on their respective Amounts. The output will then be:

```sql
Item    SUM(Amount)
------  ----------
A        550
B        400
```

## Group By with Having Clause

 `GROUP BY` clause can also be used with the `HAVING` keyword. `HAVING` keyword allows you to filter the results of the group function.

For example:

```sql
SELECT Item, SUM(Amount)
FROM Sales
GROUP BY Item
HAVING SUM(Amount) > 150;
```

This will return all grouped items where the total amount is more than 150. Hence, the result will be:

```sql
Item    SUM(Amount)
------  ----------
A        550
B        400
```