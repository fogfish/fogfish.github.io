# The 13 Type Safe Combinators To Build Networking I/O In Distributed Systems

Microservices have become a design style to evolve system architecture in parallel, implement stable and consistent interfaces. This architecture style brings additional complexity and new problems. What would be the expressive language to depict the variety of network communication use-cases? So far, `curl -v` implements the most human-friendly syntax to depict networking I/O:

```
> GET /example HTTP/1.1
> Host: example.com
> User-Agent: curl/7.54.0
> Accept: application/json
>
< HTTP/1.1 200 OK
< Content-Type: text/html; charset=UTF-8
< Server: ECS (phd/FD58)
< ...
```

The given semantic provides an intuitive approach to specify requests and responses, using Input/Process/Output paradigm. Adoption of this syntax as Go native code provides rich capabilities for network programming. One of the posts [“A Guide To Pure Type Combinators in Golang”](https://tech.fog.fish/2022/03/27/golang-type-combinator.html) discusses an abstract concept of type safe combinators.

The combinators fit very well to express intent of communication behavior. It gives rich abstractions to hide the networking complexity and help us to compose a chain of network operations and represent them as pure computation, building new things from small reusable elements.

## Combinators

System of combinators has been known for 100 years since Moses Schönfinkel developed a universal computation system that has been researched since together with mathematical logic, lambda calculus and category theory. Combinators open up an opportunity to depict computation problems in terms of fundamental elements like physics talks about the universe in terms of particles. The only definite purpose of combinators are building blocks for composition of “atomic” functions into computational structures from concrete problem “domain”. So far, combinators remain as powerful symbolic expressions in computational languages.

Combinators are simple and do not involve any advanced math in its definition. A combinator builds new “things” from previously defined “things”. Since, “thing” can be any computational “element” including functions and other combinators. It delivers powerful combinator patterns for functional programming - a style of declaring a small set of primitive abstractions and collection of combinators to define advanced structures. Golang, like any other languages, supports first class functions. It allows functions to be assigned to variables, passed as arguments to other functions and returned from other functions. Therefore, combinators of pure functions is a given fact for any Golang application.

Let’s formalize principles that help us to define our own abstraction applicable in functional programming through composition. The composition becomes a fundamental operation: the codomain of 𝒇 be the domain of 𝒈 so that the composite operation 𝒇 ◦ 𝒈 is defined. Our formalism uses `Arrow: IO ⟼ IO` as a key abstraction of networking combinators. It is a pure function that takes an abstraction of the protocol context, called IO category and applies morphism as an "invisible" side-effect of the composition.

Following the Input/Process/Output protocols paradigm, the two classes of arrows are defined:
1. The first class is writer (emitter) morphism combinators, denoted by the symbol ø. It focuses inside the protocol stack and reshapes requests. In the context of HTTP protocol, the writer morphism is used to declare HTTP method, destination URL, request headers and payload. 
2. Second one is reader (matcher) morphism combinators, denoted by the symbol ƒ. It focuses on the side-effects of the protocol stack. The reader morphism is a pattern matcher, and is used to match response code, headers and response payload, etc. Its major property is “fail fast” with error if the received value does not match the expected pattern.

Given combinator abstraction (ø, ƒ and ◦), we implements a pure functional, declarative style to express communication behavior by hiding the networking complexity using combinators.

```golang
http.Join(
  ø...,         // > GET /example HTTP/1.1
  ø...,         // > Host: example.com
  ø...,         // > User-Agent: curl/7.54.0
  ø...,         // > Accept: application/json

  ƒ...,         // < HTTP/1.1 200 OK
  ƒ...,         // < Content-Type: text/html; charset=UTF-8
  ƒ...,         // < Server: ECS (phd/FD58)
)
```

It decorates communication pipeline(s) with "programmable commas", allowing to make http requests with few interesting properties such as composition and laziness.

## Networking “algebra”

### Compose operator

Compose operation ◦ becomes a fundamental abstraction over `Arrow` type. `Arrow` can be composed with another `Arrow` into a new high-order `Arrow` and so on. The operator builds a strict product type of `Arrow: A ◦ B ◦ C ◦ ... ⟼ D`. The product type takes the IO category (a protocol context) and applies "morphism" sequentially unless some step fails. Ease of the composition is one of the major reasons why the concept deviates from the standard Golang interface. Instances of  `Arrow` type are composable “promises” of networking I/O. The networking becomes a set of `Arrow` functions that allow anyone to build a complex networking scenario from a small reusable block.

```golang
// Join composes HTTP arrows to high-order function
// (a ⟼ b, b ⟼ c, c ⟼ d) ⤇ a ⟼ d
func Join(arrows ...http.Arrow) http.Arrow

var a: Arrow = /* ... */
var b: Arrow = /* ... */
var c: Arrow = /* ... */
var d := Join(a, b, c)
```

The composition of `Join` leads to results of the same `Arrow` type and so on `Join(..., Join(/* ... */), Join(/* ... */), ...)`.

### Showcase of HTTP combinators

By the definition, ø and ƒ combinators depend on the protocol. Classical client / server interaction is a simplest scenario to illustrate combinators in actions. The table below defines a minimal set of combinators for HTTP protocol implementation. 

| Protocol Primitive | Client-side                                                                                                                                                                                                                                                                                                             | Server-side                                                                                                                                                                                                                                                                                                                                                                |
| :----------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **request**        | **writer morphism**                                                                                                                                                                                                                                                                                                     | **reader morphism**                                                                                                                                                                                                                                                                                                                                                        |
| HTTP Method        | `ø.GET`<br/>declares the verb of HTTP request                                                                                                                                                                                                                                                                           | `ƒ.GET`<br/> matches the verb of HTTP request and fails with error if the verb does not match the expected one.                                                                                                                                                                                                                                                            |
| URI                | `ø.URI(string)`<br/>specifies target URI for HTTP request. The combinator uses absolute URI to specify target host and endpoint.                                                                                                                                                                                        | `ƒ.URI(string)`<br/>matches URL path from HTTP request. The combinator considers the URI path as an ordered sequence of segments, which are matched against a given pattern or uses a lens to extract value into the context. It fails if the path does not match the expected one.                                                                                        |
| URI Query          | `ø.Params(any)`<br/> `ø.Param[T Literals](string, T)`<br/> lifts the flat structure or individual values into query parameters of specified URI.                                                                                                                                                                        | `ƒ.Params[T Lens](T)`<br/> `ƒ.Param[T Lens](string, T)`<br/>matches the URL query string from an HTTP request. It either matches literal value or uses a lens to extract value.  It fails if the query does not match the expected one.                                                                                                                                    |
| Headers            | `ø.Header[T Literals](string, T)`<br/> `ø.ContentType.ApplicationJSON`<br/> `ø.ContentType.Set(string)`<br/>declares headers and its values into HTTP requests. The [standard HTTP headers](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields) are accomplished by a dedicated combinator making it typesafe.    | `ƒ.Header[T Lens](string, T)`<br/> `ƒ.ContentType.ApplicationJSON`<br/> `ƒ.ContentType.Is(string)`<br/> `ƒ.ContentType.To(Lens)`<br/>matches HTTP headers from the request. It either matches literal value or uses a lens to extract value. The standard HTTP headers are accomplished by a dedicated combinator. It fails if the header does not match the expected one. |
| Body               | `ø.Send(any)`<br/>transmits the payload to the destination URI. The combinator takes standard data types (e.g. maps, struct, etc) and encodes it to binary using Content-Type header as a hint.                                                                                                                         | `ƒ.Body[T Lens](T)`<br/> `ƒ.Bytes([]byte)`<br/> `ƒ.Match(Pattern)`<br/>consumes payload from HTTP requests and decodes the value into the type associated with the lens using Content-Type header as a hint. It fails if the body cannot be consumed.                                                                                                                      |
| **response**       | **reader morphism**                                                                                                                                                                                                                                                                                                     | **writer morphism**                                                                                                                                                                                                                                                                                                                                                        |
| Status Code        | `ƒ.Status.OK`<br/> `ƒ.Code(200)`<br/> checks the code in HTTP response and fails with error if the status code does not match the expected one. The all well-known HTTP status codes are accomplished by a dedicated combinator making it typesafe.                                                                     | `ø.Status.OK`<br/>declares the status of HTTP response.                                                                                                                                                                                                                                                                                                                    |
| Headers            | `ƒ.Header[T Lens](string, T)`<br/> `ƒ.ContentType.ApplicationJSON`<br/> `ƒ.ContentType.Is(string)`<br/> `ƒ.ContentType.To(Lens)`<br/> matches the presence of HTTP header and its value in the response. The matching fails if the response is missing the header or its value does not correspond to the expected one. | `ø.Header[T Literals](string, T)`<br/> `ø.ContentType.ApplicationJSON`<br/> `ø.ContentType.Set(string)`<br/> declares headers and its values into HTTP response.                                                                                                                                                                                                           |
| Payload            | `ƒ.Body[T Lens](T)`<br/> `ƒ.Bytes([]byte)`<br/> `ƒ.Match(Pattern)`<br/> consumes payload from HTTP requests and decodes the value into the type associated with the lens using Content-Type header as a hint. It fails if the body cannot be consumed.                                                                  | `ø.Send(any)`<br/>transmits the payload as the response on HTTP request. The combinator takes standard data types (e.g. maps, struct, etc) and encodes it to binary using Content-Type header as a hint.                                                                                                                                                                   |


Using these combinators, the implementation of client / server interaction becomes straightforward. The syntax is identical to actual protocol flow, which reduces cognitive load while doing the system implementation.

**HTTP Request**
```
> GET /example HTTP/1.1
> Host: example.com
> User-Agent: curl/7.54.0
> Accept: application/json
>
< HTTP/1.1 200 OK
< Content-Type: text/html; charset=UTF-8
< Server: ECS (phd/FD58)
< ...
```

**Client-Side**
```golang
http.GET(
  ø.URI("http://example.com/example"),
  ø.UserAgent.Set("curl/7.54.0"),
  ø.Accept.ApplicationJSON,

  ƒ.Status.OK,
  ƒ.ContentType.TextHTML,
  ƒ.Server.Is("ECS (phd/FD58)"),
  ƒ.Body(/* ... */)
)
```

**Server-Side**
```golang
http.GET(
  ƒ.URI("/example"),
  ƒ.UserAgent.Is("curl/7.54.0"),
  ƒ.Accept.ApplicationJSON,

  ø.Status.OK,
  ø.ContentType.TextHTML,
  ø.Server.Set("ECS (phd/FD58)"),
  ø.Send(/* ... */)
)
```

### Generic combinators

Combinators are pure functions as simple, which either fails with an error or successfully morphs the protocol context:

```go
func Combinator(/* args */) Arrow { return func(*Context) error { /* body */ } }
```

However, the composition of these pure functions into a chain of networking operations is formalized through the lens of category theory. A category is a concept that is defined in abstract terms of objects, arrows together with two functions composition ◦ and identity 𝒊𝒅. These functions shall be compliant with category laws:
1. Associativity : (𝒇 ◦ 𝒈) ◦ 𝒉 = 𝒇 ◦ (𝒈 ◦ 𝒉)
2. Left identity : 𝒊𝒅 ◦ 𝒇 = 𝒇
3. Right identity : 𝒇 ◦ 𝒊𝒅 = 𝒇
The category leaves the definition of these objects, arrows, composition and identity to us, which gives a powerful abstraction! This gives a powerful abstraction in functional programming and makes a functional programming language look very much like a category. In the context of networking, objects are protocol context and arrows are a set of morphisms allowed by the protocol specification `Arrow: IO ⟼ IO`.


## What problems are solved by networking combinators?

The combinators fit very well to express intent of communication behavior. It gives rich abstractions to hide the networking complexity and help us to compose a chain of network operations and represent them as pure computation, building new things from small reusable elements. It provides a higher order abstraction or domain specific language (DSL) to describe networking, thus reducing the cognitive load. This pure abstract concept has given the birth of multiple practical applications that improves developers productivity:

**Networking client library with declarative style** of development is an obvious application for combinators. [**ᵍ🆄🆁🅻 library**](https://github.com/fogfish/gurl) implements a pure functional style to express communication. So far, it decorates http i/o pipeline(s) with "programmable commas", allowing to make http requests with few interesting properties such as composition and laziness.

**Building containerized and serverless services** with a combinator library is a well-known technique introduced by Scala’s Finch and its successor [**Gouldian**](https://github.com/fogfish/gouldian) (The Gouldian finch or the rainbow finch is a colorful passerine bird). The library is a thin layer of purely functional abstractions to build HTTP services. In contrast with other HTTP routers, the library resolves a challenge of building simple and declarative api implementations in the absence of pattern matching at Golang.

**Testing Cloud Application In Production** emphasizes continuous proofs of the quality as a key feature along the deployment pipelines to eliminate defects at earlier phases of the feature lifecycle. It impacts on engineering teams philosophy and commitments, ensuring that your microservice(s) are always in a release-ready state. The utility [**https://assay.it**](https://assay.it) is designed for testing of loosely coupled topologies such as serverless applications, microservices and other systems that rely on interfaces, protocols and its behaviors. It does unit-like testing but in distributed environments. It checks the correctness of deployments using type safe and pure functional test specification of protocol endpoints using combinators.


## Afterwords

The post has defined the basic principles of composing networking I/O. It follows Hilbert’s axiomatic method “to build everything from as few notions as possible”. All defined combinators use standard Golang notations from which other combinator notations are constructed. Domain Specific Language built over combinators shows that any networking could be reduced to an expression purely in terms of combinators. The crucial idea here is the computational language, which delivers abstractions, where anyone can declare things and then reuse them without having to think about how they’re built inside.

In the end, combinators are fundamentally computational constructs. It is surprising, just how simple the combinator systems can be. Combinators make a bridge to the way humans think, allowing anyone to represent anything using structured symbolic expressions.
