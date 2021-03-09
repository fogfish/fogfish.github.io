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

#### Assess

[Go AST Viewer](http://goast.yuroyoro.net) - online tool for analysis and visualization of ast

[Basic AST Manipulation](https://zupzup.org/ast-manipulation-go/), [Basic AST Traversal in Go](https://zupzup.org/go-ast-traversal/) - simple technique to modify Go code using AST.

[Building an Unbounded Channel in Go](https://medium.com/capital-one-tech/building-an-unbounded-channel-in-go-789e175cd2cd) 
The default un-buffered channels pass values from one goroutine to another, one at a time. The goroutine that writes to the channel blocks until another goroutine reads from the same channel. A buffered channel gives writers a bit more flexibility. When a channel is buffered, a set number of values can be written to the channel and not read before the channel blocks. It behaves like a synchronized queue with a bounded size. The post explains how to build Erlang's style unbounded queues.

[Organising Database Access in Go](https://www.alexedwards.net/blog/organising-database-access) In the context of a web application what would you consider a Go best practice for accessing the database in (HTTP or other) handlers? The post analyses various design pattern they pros and cons.

#### Adopt

[Standard Package Layout](https://medium.com/@benbjohnson/standard-package-layout-7cdbc8391fc1) - These are seen as big issues in the Go community but there’s another issue that’s rarely mentioned — application package layout.

[Go best practices, six years in](https://peter.bourgon.org/go-best-practices-2016/) - Hints and practices to manage Golang project.

[Type embedding in Go](https://travix.io/type-embedding-in-go-ba40dd4264df) - Go does not provide the typical, type-driven notion of subclassing, but it does have the ability to “borrow” pieces of an implementation by embedding types within a struct or interface. In fact, type embedding is product type composition. The article explains basics behind the embedding.

[Errors are values](https://blog.golang.org/errors-are-values) - Error handling in Go is different than other functional programming. Usually Either monad helps a lot. Go programmers miss a fundamental point about errors: Errors are values. The article shows a few patterns on error handling with Go.

[Constant errors](https://dave.cheney.net/2016/04/07/constant-errors) post show up the issue with sentinel error values in Go, it proposes alternative and type-safe solution to express errors using `const`. The approach advances error declaration and handling towards behaviour-based approach to [inspect errors](https://dave.cheney.net/2014/12/24/inspecting-errors). 


[Why doesn't Go have variance in its type system?](https://blog.merovius.de/2018/06/03/why-doesnt-go-have-variance-in.html) - It explain what co-, contra- and invariance are and what the implications for Go's type system would be. In particular, why it's impossible to have variance in slices.

[Go and Sum Data Types](https://eli.thegreenplace.net/2018/go-and-algebraic-data-types/) - Sum types is a neat feature of some programming languages that lets us specify that a value might take one of several related types, and includes convenient syntax for pattern matching on these types at run-time.

[Functional options on steroids](https://sagikazarmark.hu/blog/functional-options-on-steroids/) - Functional options is a paradigm in Go for clean and extensible APIs popularized by Dave Cheney and Rob Pike. This post is about the practices that appeared around the pattern since it was first introduced.

[JSON and struct composition in Go](https://attilaolah.eu/2014/09/10/json-and-struct-composition-in-go/) - Say you are decoding a JSON object into a Go struct. It comes from a service that is not under your control, so you cannot do much about the schema. However, you want to encode it differently.

[Dealing with JSON with non-homogeneous types in GO](https://engineering.bitnami.com/articles/dealing-with-json-with-non-homogeneous-types-in-go.html) - Dynamic languages supports non-homogeneous schemas out-of-the-box (e.g. `[ { "value": "123" }, { "value": 123 } ]`). The post explains how to achieve this with type-safe approach in Golang.

[Golang Parsing JSON into Interfaces](https://endophage.com/post/golang-parse-to-interface/) - Interfaces are an incredibly powerful feature in Go, and JSON is one of the most ubiquitous serialization formats in use today. The post explains how to handle sum-type support in JSON with interfaces.

[Don’t use Go’s default HTTP client](https://medium.com/@nate510/don-t-use-go-s-default-http-client-4804cb19f779) - Go’s http package doesn’t specify request timeouts by default, allowing services to hijack your goroutines. Always specify a custom http.Client when connecting to outside services.

[A Simple Web Scraper in Go](https://schier.co/blog/a-simple-web-scraper-in-go) and [Web Scrapping With Golang](https://ednsquare.com/story/web-scrapping-with-golang-------yvPXsw) are easy technic to parse HTML output of website. Most of the online bots are based on same technic to get required information

[Iterating Over Slices In Go](https://www.ardanlabs.com/blog/2013/09/iterating-over-slices-in-go.html) In Go everything is pass by value. We can pass by value the address of an object or pass by value a copy of an object. That includes function parameters, return values and when iterating over a slice, map or channel. 

[Recursion And Tail Calls In Go](https://www.ardanlabs.com/blog/2013/09/recursion-and-tail-calls-in-go_26.html) Go does not optimize for recursion, even if tail calls are explicit. The post explains in details a Golang design of recursive calls and how to improve the memory consumption in recursive algorithms.
