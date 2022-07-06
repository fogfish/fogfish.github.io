---
layout: post
title: "Assert Golang Errors For Behavior: Everything You Need To Know Before Making Robust and Scalable Error Handling"
description: |
  Golang articles advise sentinel errors and error types, which is not flexible enough. As a key learning, this solution leaks error types and its code across the packages. Only “assert errors for behavior” helps us to build a flexible solution. The post explains the package developers perspective on modelling errors using private data types to implement the respective behavior so that the library avoids a strong coupling with the caller, making for a nobrittle interface.
tags:
  - coding
  - golang
  - errors
---

# Assert Golang Errors For Behavior: Everything You Need To Know Before Making Robust and Scalable Error Handling  

Golang solves exceptions right! It enforces developers to assert the error behavior instead of its types. Long time ago, Dave Cheney made a thoughtful series of posts on the subject of error handling (the articles are enclosed at reference sections): 
* Why Go gets exceptions right
* Inspecting errors
* Don’t just check errors, handle them gracefully

These articles delivers _three important recommendations_ about implementation of the error path at Golang applications: 
1. “Assert errors for behavior, not type”
2. “Annotate errors with the context”
3. “Only handle errors once”

The Golang community repeats these recommendations as a mantra throughout various blog posts. However, the majority of these posts focus on errors-as-the-interface and deep-dives into Golang version 1.13 error handling features. We are still missing a holistic example of the error handling in the complex application, especially what does assert for behavior and which behavior means. In this post, we focus on how to implement these recommendations using a type safe approach in a scalable and maintainable way.


## Setting the mood about the problem

The scalable design of error modeling becomes an issue while developing a large application following principles of hexagon architecture. The loosely coupled application core, ports and adapters require a solution where an error originated by the adapter is handled once across the system.  

> As a developer I want to model errors using private data types to implement the respective interfaces (behavior) so that packages avoid a strong coupling with the caller, making for a nobrittle interface.

So far, error types and switches (see Dave’s article) were my preferred approach until I’ve started development of very large serverless applications following hexagon architecture style. The leakage of error types and its codes is the biggest mistake to avoid across the packages. 

Typically errors are originated by “driven adapters” as the reaction on the request from the application core. However, errors need to be recovered by “driving adapters”, next to “user experience” or “rest controllers”.  

![Illustration of hexagon architecture style](https://herbertograca.files.wordpress.com/2018/11/070-explicit-architecture-svg.png)

Let’s consider a typical error handling path at serverless application
1. DynamoDB fails to fulfill the request;
2. Golang AWS SDK v2 executes I/O with the service and returns the exception to persistence adapter;
3. The persistence adapter signals error case to application core, which consequently aborts the business transaction; 
4. The failure of business transaction causes reporting of problem details to the “user interface” port;
5. Execution of AWS Lambda is terminated, problem details are communicated to AWS API Gateway;
6. The application shows the non ideal state experience to the user.

It is not possible to accommodate all possible errors and its codes into the application core or anywhere else in the system without making cross package dependencies (e.g. injecting knowledge about DynamoDB error codes into the REST API controller).

Better technique to assert error is required. The technique which helps to eliminate cross package dependencies - **“assert errors for behavior, not type”** is the solution but firstly it requires us to understand the error landscape in the application.


## Taxonomy of Errors

The implementation of error handling strategy requires us to build a taxonomy of errors for an application by splitting errors into following buckets, where each bucket implements its own error handling method. 

* **Emergency Errors** - software is not usable, panic execution of current routine or application, it is not possible to gracefully terminate it.
* **Opaque Errors** - software is failed, unable to recover from error. The failure does not have global catastrophic impacts but local functionality is impaired, execution of current control flow is terminated and incorrect results are returned.
* **Recoverable Errors** - the failure is ignored and the application is still capable of delivering incomplete but correct results, in-the-best case application is able to fully recover from error.
* **Non Ideal States** - software is failed, error is recoverable, no impact.

Like an application development requires definition of its domain model, the scopes and sources of errors need to be understood to be fault tolerant.   

Let’s consider each category in depth and define a pattern to deal with errors! 


## Emergency Errors

Essentially, the emergency is Golang’s `panic`, as Dave said: “It’s game over man”. There is no magic to implement these errors. It only requires identifying all causes that lead software to unusable state. In case of serverless application, the bootstrap sequence of Lambda function is a classical root case for emergency.

```go
func main() {
  cfg, err := config.LoadDefaultConfig(context.Background())
  if err != nil {
    panic(fmt.Errorf("failed to config aws access: %w", err))
  } 
}
```

Please refer to official Golang’s “Defer, Panic, and Recover” blog post for details. Nothing extra can be added here.


## Opaque Errors

Opaque errors is a classical scenario of error handling in Golang, which is advertised by the majority of resources. The code block knows that an error occurred. It does not have the ability to inspect error details, it only knows about the successful or unsuccessful completion of the called function. This error bubbles along the call stack until it is logged by “driving adapters”.      

```go
func foo() (*Bar, error) {
  val, err := db.dynamo.GetItem(ctx, req)
  if err != nil {
    return nil, err
  }
  // continue happy path
}
```

The debugging of opaque error handling becomes a difficult job because it violates **“annotate errors with the context”** recommendation. The Go Programming Language recommends inclusion of the context to the error path using `fmt.Errorf`.

```go
func foo() (*Bar, error) {
  val, err := db.dynamo.GetItem(ctx, req)
  if err != nil {
    return nil, fmt.Errorf("[foo] dynamodb i/o failed: %w", err)
  }
}
```

In the context of a large application, `fmt.Errorf("[foo] dynamodb i/o failed: %w", err)` would be repeated a gazillion times for each service call to dynamo, any refactoring of the error text or context structure becomes a tedious job - the type system would not help at all because `fmt.Errorf` is not type safe. Usage of a pure function to define a type safe wrapping of errors is the better approach to annotate error context:

```go
func errDynamoIO(err error) error {
  return fmt.Errorf("dynamodb i/o failed: %w", err)
}

val, err := db.dynamo.GetItem(ctx, req)
if err != nil {
  return nil, errDynamoIO(err)
}
```

With help of the `runtime` package, function context can be injected into the error itself

```go
func errDynamoIO(err error) error {
  var name string

  if pc, _, _, ok := runtime.Caller(1); ok {
    name = runtime.FuncForPC(pc).Name()
  }

  return fmt.Errorf("[%s] dynamodb i/o failed: %w", name, err)
}
```

If you are developing a highly loaded system, usage of pure function wrapper might cause about 10% of the loss of the error path capacity and about 75% when the `runtime` package is used to discover function context.    

Declare categories of errors using private functions `func errXXX(error) error` within the application core, ports and adapters. It makes a maintainable pattern to annotate errors with the current context. Further on, these functions help to adapt opaque errors into recoverable one. 

Despite the type safe approach being used to “construct” errors, the error handler is still unable to recover it due to missing the ability to interpret the error. However, the rich context allows to log the error and aid debugging of the problem. “Driving adapters” are still capable of output `500 Internal Server Error` together with HTTP Problem Details Object (RFC 7807) that refers to the unique identity of the error in the logging system.

```json
{
  "instance":"NhzMqjdM2utt4V.1",
  "type":"https://httpstatuses.com/500",
  "status":500,
  "title":"Internal Server Error",
}
```

Opaque errors aids debugging of the problem but leads to generic error messages toward “user experience”, it is just indication of unexpected conditions encountered.


## Recoverable Errors

The development of error handling requires us to build the ability to inspect the error details. Usage of sentinel errors and error types is the most obvious but inflexible solution, only **“assert errors for behavior”** helps us to build a flexible solution. Behavior helps us to inspect error details “without knowing anything about the type, or the original source of the error value” due to the implicit nature of Go’s interfaces. The classical example from Dave’s article on recovery timeout errors.

```go
func recoverTimeout(err errors) bool {
  var e interface{ Timeout() bool }

  ok := errors.As(err, &e)
  return ok && e.Timeout()
}
```

Golang native package `errors.As` provides us the ability to focus inside wrapped errors to check if any of the inner errors implement the interfaces.    

So far, Golang provides us with type safe abilities to

1. declare error type and annotates its behavior using built-in `error` type and well-known `interface{ Error() string }`;
2. opaque pass errors through the call stack and annotate its context with`fmt.Errorf` together with type safe pure functional wrappers;
3. recover from error at “single” error handling code block using only behavior notation with help of `errors.As`. 

Only the final secret ingredient is missing from the formula! What is the error behavior? How to define it? Unfortunately the behavior is the contract, which must be obeyed by both parties. It is the responsibility of the application developers to identify error behavioral contracts as part of the error taxonomy analysis within the problem domain. Highly likely, “driven adapters” would warp errors of the external system into the contracts before the global and well-known taxonomies are developed and widely accepted by the Golang community.   

Let’s consider a few examples of behavioral contracts in the order to help with understanding the pattern!  

**Ephemeral** errors have a short lifespan, repeating the function call for a few attempts is a good recovery strategy. Use the behavior `interface{ Ephemeral() bool }` to match the category of these errors. Furthermore, service invocation in the distributed environment might fail due to communication timeout. Timeouts are ephemeral errors. An application package might implement the custom error:      

```go
type timeout struct {
  t   time.Duration
  err errors

}

func errTimeout(err error) error { return &timeout{err} }

func (e *timeout) Error() string { return fmt.Sprintf("timeout: %s", e.err) }

func (e *timeout) Unwrap() error { return e.err }

func (e *timeout) Ephemeral() bool { return true }

func (e *timeout) Timeout() time.Duration { return e.t }
```

Any code block is capable of recovering from error using either `interface{ Ephemeral() bool }` or `interface{ Timeout() time.Duration }` behaviorals.  

**Error codes** are still alive despite its criticism. For example, AWS SDK uses `interface{ ErrorCode() string }` behavior. This behavior can be used instead of the recommended error types approach by the oficial SDK documentation. Semantic of the error codes still make indirect dependency to the solution behind the adapter thus error codes shall be used with caution as last resort on error handling strategy. The further expansion of error codes toward semantic design of error ontologies, and usage of Internationalized Resource Identifiers instead of error code helps to resolve indirect dependency. For example, HTTP Vocabulary defined the StatusCodeClass  (https://www.w3.org/TR/HTTP-in-RDF10/#StatusCodeClass), its instances are well-known codes `http:StatusCode#InternalServerError` that requires no dependency to `net/http` package on processing the error. Similarly, AWS SDK error codes can be twisted into compact IRIs `dynamodb:ResourceNotFoundException`. The behavior `interface{ ErrorType() string }` is a logical enhancement of `ErrorCode()` towards an IRI reference - the primary identifier for the problem type. 

**PreCondition** failed is a common scenario to implement optimistic locking in a distributed system. HTTP Protocol supports reporting status of optimistic locking through a number of built-in status codes:  `409 Conflict`, `410 Gone` and `412 Precondition Failed`. The definition of corresponding behavioral contracts helps to pass-through DynamoDB’s ConditionalCheckFailedException through the call stack to error handling logic at “driving adapters”. An application package might implement the custom error:

```go
type preConditionFailed struct { /* ... */ }

func (e *preConditionFailed) PreConditionFailed() bool { return true }

func (e *preConditionFailed) Conflict() bool { return e.conflict }

func (e *preConditionFailed) Gone() bool { return e.gone }
```

Now, REST API controller is able to “recover” errors by communicating status of the transaction using one of the following behavior `interface { PreConditionFailed() bool }`, `interface{ Conflict() bool }` or `interface{ Gone() bool }` 

**HTTP Problem Details Object** is defined by RFC 7807 as a standard approach to communicate errors from public interfaces. Direct mapping of any error to this JSON object is not possible and also leads to a risk of disclosure of sensitive information. Well crafted behavior helps safely translate error into the object.

```go
type ProblemDetails interface {
  ErrorCode() string
  ErrorType() string
  ErrorInstance() string
  ErrorTitle() string
  ErrorDetail() string
}
```

Implementing this behavior by errors make it easy to handle errors at the REST controllers.   


## Non Ideal State

Non Ideal State does not really differ a lot from recoverable errors, it is essentially a subcategory of those. The category of Non Ideal State errors helps to distinguish errors caused by the unacceptable input from clients. One of the classical examples is the “Not Found” error. Typically, storage fails with an error if requested items do not exist. The application cannot recover from the error, it is only passed through the error to “user experience”. Use `interface { NotFound() string }` behavior for this purpose. Any other Non Ideal State can be modeled similarly.  


## Evolving Opaque Errors

Please note that opaque errors would cover about 80% of cases for the application, only 20% of cases would require a careful development of dedicated behaviors. Implement the development of error recovery strategy using an iterative approach. 

Firstly, specify and implement all categories of opaque errors as a pure function. This is the foundation to conduct further analysis on error recovery strategy.

```go
func errDynamoIO(error) error { /* ... */ }

func errCodecItem(error) error { /* ... */ }

// ...
```

In the consequent iterations, introduce recoverable errors by defining consistent and reusable behaviors.

```go
type preConditionFailed struct { ... }

func errPreConditionFailed(error) error { ... }

func (e *preConditionFailed) PreConditionFailed() bool { return true }

func (e *preConditionFailed) Conflict() bool { ... }

func (e *preConditionFailed) Gone() bool { ... }
```

Then implement a recovery strategy, following the “only handle errors once” recommendation.

```go
func recoverPreConditionFailed(err errors) bool {
  var e interface{ PreConditionFailed() bool }

  ok := errors.As(err, &e)
  return ok && e.PreConditionFailed()

}

func recoverConflict(err errors) bool {
  var e interface{ Conflict() bool }

  ok := errors.As(err, &e)
  return ok && e.Conflict()

}
```

and so on.

The final advice, keep all errors private for the package in single place (e.g.`errors.go` file) but document implemented behaviors and how to recover. 


## Afterwords

Often, Golang articles advise sentinel errors and error types, which is not flexible enough. As a key learning, this solution leaks error types and its code across the packages. Only **“assert errors for behavior”** helps us to build a flexible solution.

As a developer I want to model errors using private data types to implement the respective behavior so that the library avoids a strong coupling with the caller, making for a nobrittle interface.

Begin the implementation of error handling strategy with building a taxonomy of errors by splitting them into following buckets: **Emergency Errors**, **Opaque Errors**, **Recoverable Errors** and **Non Ideal States**.

Then, continue to implement defined taxonomy using type safe pure functions to manage error in the application, making each error as `func errXXX(error) error`. Declare private types to wrap errors from 3rd party libraries and annotate it with desired behaviors.

Finally, implement recovery routines from error at “single” error handling code block using only behavior notation with help of `errors.As`.

This simple technique makes a robust and scalable error handling path in your application, while adhering to three important recommendations: “Assert errors for behavior, not type”, “Annotate errors with the context” and “Only handle errors once”. 

The short summary of usable behavioral is published in [**the gist**](https://gist.github.com/fogfish/061c0da30394eb0abbc0952764a2aaf9) 

## References

1. [Why Go gets exceptions right](https://dave.cheney.net/2012/01/18/why-go-gets-exceptions-right)
2. [Inspecting errors](https://dave.cheney.net/2014/12/24/inspecting-errors)
3. [Don’t just check errors, handle them gracefully](https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully)
4. [Defer, Panic, and Recover](https://go.dev/blog/defer-panic-and-recover)
