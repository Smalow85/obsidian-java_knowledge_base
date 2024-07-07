## Key Concepts

- **Migration**: A migration in [[SQL]] databases is a single unit of change that affects the schema or data in a database. Each migration encapsulates an operation such as creating, altering, or dropping tables, indices, or constraints.
- **Migration History**: The sequence of applied migrations is the migration history, and it helps you keep track of the transformations applied to the schema over time. Typically, migrations are tracked using a dedicated table in the database that logs applied migrations and their order.
- **Up and Down Migrations**: Each migration typically consists of two operations – an “up” operation that applies the change, and a “down” operation that rolls back the change if needed. The up operation moves the schema forward, while the down operation reverts it.
## Benefits of Migrations

- **Version Control**: Migrations help to version control your database schema, making it easier to collaborate with team members and review schema changes in the same way you review application code.
- **Consistency**: Migrations promote a consistent and reproducible approach to managing schema changes across various environments (e.g., development, testing, production).
- **Testability**: Migrations allow you to test the effect of schema changes in isolated environments before deploying them to production.
- **Deployability**: Migrations facilitate automated deployment processes and help reduce the risk of human error during database schema updates.