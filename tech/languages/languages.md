---
layout: default
title: Programming Languages
parent: Technology Radar
nav_order: 4
description: |
  References to programming languages, development tricks and other useful techniques.
---

# Programming Languages


## Functional programming

#### Assess

[Modular Type Classes](https://people.mpi-sws.org/~dreyer/papers/mtc/main-long.pdf) - ML modules and Haskell type classes have proven to be highly effective tools for program structuring. Modules emphasize explicit configuration of program components and the use of data abstraction. Type classes emphasize implicit program construction and ad hoc polymorphism. In this paper, we show how the implicitly- typed style of type class programming may be supported within the framework of an explicitly-typed module language by viewing type classes as a particular mode of use of modules.


#### Adopt

[Implementing, and Understanding Type Classes](http://okmij.org/ftp/Computation/typeclass.html) - This page looks behind the scenes of the abstraction of parametric overloading, also known as bounded polymorphism, or just `type classes`. Seeing the implementation makes type classes appear simpler, friendlier, more comfortable to use.

[Solving Embarrassingly Obvious Problems in Erlang](https://blog.usejournal.com/solving-embarrassingly-obvious-problems-in-erlang-e3f21a6203cc) - the article is helpful for any one how commits large function. It guides about decomposition of large functions using real life example. The technique is applicable to any language despite it refers to Erlang. 

[Inheritance vs Generics vs TypeClasses in Scala](https://dev.to/jmcclell/inheritance-vs-generics-vs-typeclasses-in-scala-20op) - A tutorial about polymorphism techniques in functional programming using Scala as an example. Answers the question about the difference between parametric polymorphism (generics) and ad-hoc polymorphism (type classes).

## Scala

#### Adopt

[Type classes in Scala](https://scalac.io/typeclasses-in-scala/) - Type classes are a powerful and flexible concept that adds ad-hoc polymorphism to Scala. They are not a first-class citizen in the language, but other built-in mechanisms allow to writing them in Scala. This is the reason why they are not so obvious to spot in code and one can have some confusion over what the ‘correct’ way of writing them is. This blog post summarizes the idea behind type classes, how they work and the way of coding them in Scala.

[Shapeless 101](https://harrylaou.com/slides/shapeless101.pdf) - Shapeless is an advanced functional programming library for the scala language. The presentation talks about few shapeless patterns such as generic, polymorphic functions, aux pattern, product and coproduct, witness, singleton.

[The Type Astronaut's Guide to Shapeless Book](https://underscore.io/books/shapeless-guide/) - aimed at experienced Scala hitch hikers with an interest in generic programming and boilerplate elimination. The book walks you through one of the main use cases for shapeless – automatic, boilerplate-free derivation of type class instances.  

[Building a REST API with Finch and Finagle](https://andrew-jones.com/blog/building-a-rest-api-with-finch-and-finagle/) - step-by-step tutorial to create a simplest proxy endpoint with Finch and Finagle.


## TypeScript

#### Adopt

[TYPESCRIPT TYPE INFERENCE GUIDE](http://ducin.it/typescript-type-inference-guide) - Type Inference is one of the most important features we should master, as we progress with using TypeScript. One of the most useful resources when getting started with TypeScript is the official handbook. However, it doesn't go too deep into type inference and doesn't provide practical tips on how to leverage it. 

[Notes on TypeScript](https://dev.to/busypeoples/notes-on-typescript-pick-exclude-and-higher-order-components-40cp) - a series of posts about TypeScript development techniques. 

[TypeScript: Working with JSON](http://choly.ca/post/typescript-json/) - TypeScript do not support at type safeness on external JSON object. The blog gives hints how to enforce type safeness without generics or 3rd party libraries


