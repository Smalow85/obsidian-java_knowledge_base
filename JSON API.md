JSON API employs JSON as its data format, making it human-readable and easy to work with. It defines a set of conventions for how resources are represented and relationships between them. These conventions include specifying URLs for resource endpoints, pagination, filtering, and sorting data.

#### Contract

Clients and servers **MUST** send all JSON:API payloads using the JSON:API media type in the `Content-Type` header.

Clients and servers **MUST** specify the `ext` media type parameter in the `Content-Type` header when they have applied one or more extensions to a JSON:API document.

Clients and servers **MUST** specify the `profile` media type parameters in the `Content-Type` header when they have applied one or more profiles to a JSON:API document.

A JSON object **MUST** be at the root of every JSON:API request and response document containing data. This object defines a document’s “top level”.

A document **MUST** contain at least one of the following top-level members:

`data`: the document’s “primary data”.

`errors`: an array of [error objects](https://jsonapi.org/format/#errors).

`meta`: a [meta object](https://jsonapi.org/format/#document-meta) that contains non-standard meta-information.

a member defined by an applied [extension](https://jsonapi.org/format/#extensions).

The members `data` and `errors` **MUST NOT** coexist in the same document.

A document **MAY** contain any of these top-level members:

`jsonapi`: an object describing the server’s implementation.

`links`: a [links object](https://jsonapi.org/format/#document-links) related to the primary data.

`included`: an array of [resource objects](https://jsonapi.org/format/#document-resource-objects) that are related to the primary data and/or each other (“included resources”).