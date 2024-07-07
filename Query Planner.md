The PostgreSQL query execution mechanism is fairly intricate, but important to understand well in order to get the most out of your database. SQL queries are mostly declarative: you describe what data you would like to retrieve, Postgres figures out a plan for how to get it for you, then executes that plan.
## Query planning

Planning is determining the best arrangement of plan nodes for executing your query and depends on several factors.

First—and obviously most important—are the semantics of your query: the plan needs to return exactly the rows your query requested (though if there are different results that fulfill those semantics, you may get different rows every time: that’s why you cannot depend on a `SELECT * FROM my_table LIMIT 1` without an `ORDER BY`).

To come up with a plan, Postgres inspects your query and evaluates one or more alternatives given the above. It estimates a cost for each leaf node, and propagates those costs up the tree to calculate costs for internal nodes and eventually the root. It considers several plans and picks the cheapest based on cost settings, available indexes, and the heuristics based on collected database statistics.

As a quick example, let's utilize the [[EXPLAIN]] command to understand a simple query:

```sql
EXPLAIN SELECT * FROM databases WHERE id = 1;
```

```sql
                                    QUERY PLAN                                    
----------------------------------------------------------------------------------
 Index Scan using databases_pkey on databases  (cost=0.28..8.30 rows=1 width=126)
   Index Cond: (id = 1)
(2 rows)
```

## Basic Functionality of Query Planner

The Query Planner performs an essential role in the query execution process, which can be summarized into the following steps:

- **Parse the SQL query:** Validate the syntax of the SQL query and build an abstract parse tree.
- **Generate query paths:** Create and analyze different execution paths that can be used to answer the query.
- **Choose the best plan:** Determine the most optimal query plan based on the estimated costs of different paths.
- **Execute the selected plan:** Put the chosen plan into action and produce the desired result.

The query planner mainly focuses on steps 2 and 3, generating possible paths for the query to follow and choosing the most optimal path among them.

