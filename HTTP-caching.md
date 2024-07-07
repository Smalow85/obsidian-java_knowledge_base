HTTP caching is optional and not supported for all HTTP request methods and HTTP responses.
To manage cache, HTTP relies upon three primary concepts. These are _Freshness_, _Validation_, and _Invalidation_. Freshness is a relative measure between the time the resource was cached and its expiry date. Validation can be used to check to see whether a cached resource is still valid after it expires, or becomes stale. Invalidation is the intention of setting a cache to stale, and it normally happens as a side effect of executing an operation that is not safe.
### HTTP request directives

The following directives can be submitted as part of a client’s HTTP request.

- `max-age`
    
    Indicates the number of seconds before the client considers the HTTP response stale. If the _max-stale_ directive is not included then HTTP responses older than this will not be returned.
    
- `max-stale`
    
    If no argument is supplied then the client will accept a stale HTTP response of any age. However, if _max-stale_ is assigned a value then it will be the maximum number of seconds that a HTTP response can be stale for, and have the client still accept it.
    
- `min-fresh`
    
    Indicates that the client will only accept a HTTP response that will remain fresh for at least the specified number of seconds.
    
- `no-cache`
    
    Requires that the HTTP response cannot be retrieved from a cache unless it is first validated by the [origin](https://http.dev/origins "origin") server.
    
- `no-store`
    
    Directs that no part of the HTTP request or HTTP response be stored in a cache. This applies to caches of any type, including private caches. In cases where the information is stored unintentionally in volatile storage, well-behaved systems will attempt to remove it as soon as possible.
    
    Note: if a HTTP request containing the _no-store_ directive was retrieved from a cache, then the directive does not apply to the existing stored HTTP response.
    
- `no-transform`
    
    Directs that intermediaries cannot transform the message body, regardless of whether they support caching.
    
- `only-if-cached`
    
    This indicates that the client only wants to receive a stored HTTP response. A system that receives this directive will return a stored version of the HTTP response that meets all of the requirements specified by the HTTP request. If no such copy exists then it will respond with a [504 Gateway Timeout](https://http.dev/504 "504 Gateway Timeout") status code.
    

### HTTP response directives

The following directives are sent as part of the HTTP response from the server and direct the client on the requirements for caching the accompanying resources.

- `must-revalidate`
    
    This instructs the client that once a HTTP response is stale, the client cannot rely on the cached version before it is revalidated. With this directive present, if a client is unable to reach the [origin](https://http.dev/origins "origin") server to revalidate the cache then a [504 Gateway Timeout](https://http.dev/504 "504 Gateway Timeout") status code will accompany the HTTP response.
    
- `no-cache`
    
    Directs client that new HTTP requests cannot rely on a cached version of the resource unless it is first validated, even if stale HTTP responses are allowed.
    
- `no-store`
    
    Directs client and intermediaries not to intentionally store any part of the HTTP response in a cache. This differs from the _no-cache_ directive because HTTP response caching is allowed, but cannot be used unless first validated. The _no-store_ directive implies that a definite effort is made to eliminate all traces of this HTTP response from non-volatile storage as soon as possible.
    
- `no-transform`
    
    This instructs intermediaries not to transform the message body, regardless of whether a cache is involved.
    
- `public`
    
    Informs clients and intermediaries that the HTTP response may be cached, even in cases where it is normally non-cacheable or is restricted to a private cache.
    
- `private`
    
    This directive implies that this HTTP response cannot be stored in a shared cache. Furthermore, storing the HTTP response in a private cache is allowed, even in cases where the HTTP response is not normally cacheable.
    
- `proxy-revalidate`
    
    The directive is the same as _must-revalidate_ but it does not apply to HTTP responses stored in a private cache.
    
- `max-age`
    
    Indicates the number of seconds before the client considers the HTTP response stale.
    
- `s-maxage`
    
    This directive is the same as _max-age_ although it only refers to HTTP responses that are stored in a shared cache.

