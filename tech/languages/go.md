---
layout: default
title: Go
parent: Languages
nav_order: 1
description: |
  Golang
---

# Go

#### Assess

[Go AST Viewer](http://goast.yuroyoro.net) - online tool for analysis and visualization of ast

[Basic AST Manipulation](https://zupzup.org/ast-manipulation-go/), [Basic AST Traversal in Go](https://zupzup.org/go-ast-traversal/) - simple technique to modify Go code using AST.

#### Adopt

[Type embedding in Go](https://travix.io/type-embedding-in-go-ba40dd4264df) - Go does not provide the typical, type-driven notion of subclassing, but it does have the ability to “borrow” pieces of an implementation by embedding types within a struct or interface. In fact, type embedding is product type composition. The article explains basics behind the embedding.

[Errors are values](https://blog.golang.org/errors-are-values) - Error handling in Go is different than other functional programming. Usually Either monad helps a lot. Go programmers miss a fundamental point about errors: Errors are values. The article shows a few patterns on error handling with Go. 

[Why doesn't Go have variance in its type system?](https://blog.merovius.de/2018/06/03/why-doesnt-go-have-variance-in.html) - It explain what co-, contra- and invariance are and what the implications for Go's type system would be. In particular, why it's impossible to have variance in slices.

[Go and Sum Data Types](https://eli.thegreenplace.net/2018/go-and-algebraic-data-types/) - Sum types is a neat feature of some programming languages that lets us specify that a value might take one of several related types, and includes convenient syntax for pattern matching on these types at run-time.

[Functional options on steroids](https://sagikazarmark.hu/blog/functional-options-on-steroids/) - Functional options is a paradigm in Go for clean and extensible APIs popularized by Dave Cheney and Rob Pike. This post is about the practices that appeared around the pattern since it was first introduced.

[JSON and struct composition in Go](https://attilaolah.eu/2014/09/10/json-and-struct-composition-in-go/) - Say you are decoding a JSON object into a Go struct. It comes from a service that is not under your control, so you cannot do much about the schema. However, you want to encode it differently.

[Standard Package Layout](https://medium.com/@benbjohnson/standard-package-layout-7cdbc8391fc1) - These are seen as big issues in the Go community but there’s another issue that’s rarely mentioned — application package layout.

[Go best practices, six years in](https://peter.bourgon.org/go-best-practices-2016/) - Hints and practices to manage Golang project.

[Don’t use Go’s default HTTP client](https://medium.com/@nate510/don-t-use-go-s-default-http-client-4804cb19f779) - Go’s http package doesn’t specify request timeouts by default, allowing services to hijack your goroutines. Always specify a custom http.Client when connecting to outside services.

