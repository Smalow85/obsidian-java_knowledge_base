[[PostgreSQL]] devises a _query plan_ for each query it receives. Choosing the right plan to match the query structure and the properties of the data is absolutely critical for good performance, so the system includes a complex _planner_ that tries to choose good plans. You can use the `EXPLAIN` command to see what query plan the planner creates for any query.

```sql
EXPLAIN SELECT * FROM tenk1;

                         QUERY PLAN
-------------------------------------------------------------
 Seq Scan on tenk1  (cost=0.00..458.00 rows=10000 width=244)
```

