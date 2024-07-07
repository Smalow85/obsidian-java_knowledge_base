GraphQL is a query language for your [[API]], and a server-side runtime for executing queries using a type system you define for your data.
## Schemas, resolvers, and other common GraphQL terms

API developers use GraphQL to create a **schema** to describe all the possible data that clients can query through that service.
A GraphQL schema is made up of object types, which define which kind of object you can request and what fields it has. 

As **queries** come in, GraphQL validates the queries against the schema. GraphQL then executes the validated queries.

The API developer attaches each field in a schema to a function called a **resolver**. During execution, the resolver is called to produce the value.
From the point of view of the client, the most common GraphQL operations are likely to be **queries** and **mutations**. If we were to think about them in terms of the _create, read, update and delete_ (CRUD) model, a query would be equivalent to _read_. All the others (_create, update,_ and _delete_) are handled by mutations.
#### Advantages

- A GraphQL schema sets a single source of truth in a GraphQL application. It offers an organization a way to federate its entire API.
- GraphQL calls are handled in a single round trip. Clients get what they request with no **overfetching**.
- Strongly defined data types reduce miscommunication between the client and the server. 
- GraphQL is introspective. A client can request a list of data types available. This is ideal for auto-generating documentation.
- GraphQL allows an application API to evolve without breaking existing queries.
- Many open source GraphQL extensions are available to offer features not available with REST APIs.
- GraphQL does not dictate a specific application architecture. It can be introduced on top of an existing REST API and can work with existing API management tools.

#### Disadvantages

- GraphQL presents a learning curve for developers familiar with REST APIs.
- GraphQL shifts much of the work of a data query to the server side, which adds complexity for server developers.
- Depending on how it is implemented, GraphQL might require different API management strategies than REST APIs, particularly when considering rate limits and pricing.
- Caching is more complex than with REST.
- API maintainers have the additional task of writing maintainable GraphQL schema.
### An example GraphQL query

Suppose you need to request a list of IDs, then request a series of records for each ID. With GraphQL, you could construct a query that pulls everything you want with a single API call. 

So this query:

```GraphQL
query HeroComparison($first: Int = 3) {
  leftComparison: hero(location: KANSAS) {
    ...comparisonFields
  }
  rightComparison: hero(location: OZ) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  friendsConnection(first: $first) {
    totalCount
    edges {
      node {
        name
      }
    }
  }
}
```

  
Might produce this result:

```GraphQL
{
  "data": {
    "leftComparison": {
      "name": "Dorothy",
      "friendsConnection": {
        "totalCount": 4,
        "edges": [
          {
            "node": {
              "name": "Aunt Em"
            }
          },
          {
            "node": {
              "name": "Uncle Henry"
            }
          },
          {
            "node": {
              "name": "Toto"
            }
          }
        ]
      }
    },
    "rightComparison": {
      "name": "Wizard",
      "friendsConnection": {
        "totalCount": 3,
        "edges": [
          {
            "node": {
              "name": "Scarecrow"
            }
          },
          {
            "node": {
              "name": "Tin Man"
            }
          },
          {
            "node": {
              "name": "Lion"
            }
          }
        ]
      }
    }
  }
}
```