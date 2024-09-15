- [[@Component]] is a generic stereotype for any Spring-managed component.
- `@Service` annotates classes at the service layer.
- [[@Repository]] annotates classes at the persistence layer, which will act as a database repository.

**We mark beans with @Service to indicate that they’re holding the business logic**. Besides being used in the service layer, there isn’t any other special use for this annotation.

