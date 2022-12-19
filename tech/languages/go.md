---
layout: default
title: Go
parent: Languages
grand_parent: Technology Radar
nav_order: 1
description: |
  Golang
---

# Go

[Organising Database Access in Go](https://www.alexedwards.net/blog/organising-database-access) In the context of a web application what would you consider a Go best practice for accessing the database in (HTTP or other) handlers? The post analyses various design pattern they pros and cons.

[Standard Package Layout](https://medium.com/@benbjohnson/standard-package-layout-7cdbc8391fc1) - These are seen as big issues in the Go community but there’s another issue that’s rarely mentioned — application package layout.

[Go best practices, six years in](https://peter.bourgon.org/go-best-practices-2016/) - Hints and practices to manage Golang project.

[Don’t use Go’s default HTTP client](https://medium.com/@nate510/don-t-use-go-s-default-http-client-4804cb19f779) - Go’s http package doesn’t specify request timeouts by default, allowing services to hijack your goroutines. Always specify a custom http.Client when connecting to outside services.

[A Simple Web Scraper in Go](https://schier.co/blog/a-simple-web-scraper-in-go) and [Web Scrapping With Golang](https://ednsquare.com/story/web-scrapping-with-golang-------yvPXsw) are easy technic to parse HTML output of website. Most of the online bots are based on same technic to get required information

[Iterating Over Slices In Go](https://www.ardanlabs.com/blog/2013/09/iterating-over-slices-in-go.html) In Go everything is pass by value. We can pass by value the address of an object or pass by value a copy of an object. That includes function parameters, return values and when iterating over a slice, map or channel. 

[A Survey of Iterator (or Generator) Patterns in golang](https://ewencp.org/blog/golang-iterators/) what the canonical iterator pattern in Go is: what’s the interface and the implementation? The post covers the basic options for a forward-only iterator (which admittedly simplifies the problem, mainly focusing on implementation rather than the appropriate interface, but this is also the most common type of iterator we need).

[Golang RegExp uses linear time](https://github.com/golang/go/blob/master/src/regexp/regexp.go) The regexp implementation provided by Go is guaranteed to run in time linear in the size of the input. This is a property not guaranteed by most open source implementations of regular expressions.

[Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments) collects common comments made during reviews of Go code, so that a single detailed explanation can be referred to by shorthands.

[Go Style Decisions](https://google.github.io/styleguide/go/decisions) This document contains style decisions intended to unify and provide standard guidance, explanations, and examples for the advice given by the Go readability mentors.

## Type System

[Type embedding in Go](https://travix.io/type-embedding-in-go-ba40dd4264df) - Go does not provide the typical, type-driven notion of subclassing, but it does have the ability to “borrow” pieces of an implementation by embedding types within a struct or interface. In fact, type embedding is product type composition. The article explains basics behind the embedding.

[Why doesn't Go have variance in its type system?](https://blog.merovius.de/2018/06/03/why-doesnt-go-have-variance-in.html) - It explain what co-, contra- and invariance are and what the implications for Go's type system would be. In particular, why it's impossible to have variance in slices.

[Go and Sum Data Types](https://eli.thegreenplace.net/2018/go-and-algebraic-data-types/) - Sum types is a neat feature of some programming languages that lets us specify that a value might take one of several related types, and includes convenient syntax for pattern matching on these types at run-time.

[Functional options on steroids](https://sagikazarmark.hu/blog/functional-options-on-steroids/) - Functional options is a paradigm in Go for clean and extensible APIs popularized by Dave Cheney and Rob Pike. This post is about the practices that appeared around the pattern since it was first introduced.

[Golang Unsafe Type Conversions and Memory Access](https://hackernoon.com/golang-unsafe-type-conversions-and-memory-access-odz3yrl) Go is a strongly typed, you have to convert variable from one type to another to use in different parts of application. But sometimes you need to step around this type safety. It could be needed to optimization of bottle necks in high load systems, where every tick counts. Unsafe operation potentially could safe a lot of allocations. Unsafe also allows to hack into any struct field, including slices, strings, maps etc.


## JSON

[JSON and struct composition in Go](https://attilaolah.eu/2014/09/10/json-and-struct-composition-in-go/) - Say you are decoding a JSON object into a Go struct. It comes from a service that is not under your control, so you cannot do much about the schema. However, you want to encode it differently.

[Dealing with JSON with non-homogeneous types in GO](https://engineering.bitnami.com/articles/dealing-with-json-with-non-homogeneous-types-in-go.html) - Dynamic languages supports non-homogeneous schemas out-of-the-box (e.g. `[ { "value": "123" }, { "value": 123 } ]`). The post explains how to achieve this with type-safe approach in Golang.

[Golang Parsing JSON into Interfaces](https://endophage.com/post/golang-parse-to-interface/) - Interfaces are an incredibly powerful feature in Go, and JSON is one of the most ubiquitous serialization formats in use today. The post explains how to handle sum-type support in JSON with interfaces.


## Channels

[The Behavior Of Channels](https://www.ardanlabs.com/blog/2017/10/the-behavior-of-channels.html) Any one who working with Go’s channels for the first time can make mistake of thinking about channels as a data structure. It’s best to forget about how channels are structured and focus on how they behave. Do not think about channels as a queue, think about one thing: signaling. A channel allows one goroutine to signal another goroutine about a particular event. Signaling is at the core of everything you should be doing with channels. The article depicts attributes of signalling system: Guarantee Of Delivery, State and With or Without Data. It relates to the question: 

[Do buffered channels maintain order?](https://stackoverflow.com/questions/25795131/do-buffered-channels-maintain-order) 

[Building an Unbounded Channel in Go](https://medium.com/capital-one-tech/building-an-unbounded-channel-in-go-789e175cd2cd) 
The default un-buffered channels pass values from one goroutine to another, one at a time. The goroutine that writes to the channel blocks until another goroutine reads from the same channel. A buffered channel gives writers a bit more flexibility. When a channel is buffered, a set number of values can be written to the channel and not read before the channel blocks. It behaves like a synchronized queue with a bounded size. The post explains how to build Erlang's style unbounded queues.

[Go channels across many machines](https://medium.com/@matryer/introducing-vice-go-channels-across-many-machines-bcac1147d7e2) Concurrency is a great way to get more stuff done faster. Go channels are perfect for enabling multiple concurrent goroutines to safely communicate within a single process, but if we want to let multiple machines/nodes communicate in a similar way, we have to write completely different code and integrate with messaging queues, gRPC, or something else.

## Language & Runtime

[Scheduling In Go : Part I - OS Scheduler](https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part1.html), [Scheduling In Go : Part II - Go Scheduler](https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part2.html) and [Scheduling In Go : Part III - Concurrency](https://www.ardanlabs.com/blog/2018/12/scheduling-in-go-part3.html) provide an understanding of the mechanics and semantics behind the scheduler in Go

[Go AST Viewer](http://goast.yuroyoro.net) - online tool for analysis and visualization of ast

[Basic AST Manipulation](https://zupzup.org/ast-manipulation-go/), [Basic AST Traversal in Go](https://zupzup.org/go-ast-traversal/) - simple technique to modify Go code using AST.

## Performance

[High Performance Go Workshop](https://dave.cheney.net/high-performance-go-workshop/dotgo-paris.html) - The goal for this workshop is to give you the tools you need to diagnose performance problems in your Go applications and fix them. Through the day we’ll work from the small — learning how to write benchmarks, then profiling a small piece of code. Then step out and talk about the execution tracer, the garbage collector and tracing running applications. 

[Recursion And Tail Calls In Go](https://www.ardanlabs.com/blog/2013/09/recursion-and-tail-calls-in-go_26.html) Go does not optimize for recursion, even if tail calls are explicit. The post explains in details a Golang design of recursive calls and how to improve the memory consumption in recursive algorithms.


## Errors

[Why Go gets exceptions right](https://dave.cheney.net/2012/01/18/why-go-gets-exceptions-right) explains how the error handling made right in Go to compare with C, C++ and Java.

[Errors are values](https://blog.golang.org/errors-are-values) - Error handling in Go is different than other functional programming. Usually Either monad helps a lot. Go programmers miss a fundamental point about errors: Errors are values. The article shows a few patterns on error handling with Go.

[Constant errors](https://dave.cheney.net/2016/04/07/constant-errors) post show up the issue with sentinel error values in Go, it proposes alternative and type-safe solution to express errors using `const`. The approach advances error declaration and handling towards behaviour-based approach to [inspect errors](https://dave.cheney.net/2014/12/24/inspecting-errors). 

[Inspecting errors](https://dave.cheney.net/2014/12/24/inspecting-errors) Don’t assert errors for type, assert for behaviour. For package authors, if your package generates errors of a temporary nature, ensure you return error types that implement the respective interface methods. For package users, if you need to inspect an error, use interfaces to assert the behaviour you expect, not the error’s type.

[Don’t just check errors, handle them gracefully](https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully) in-depth explanation about error handling strategies.  


