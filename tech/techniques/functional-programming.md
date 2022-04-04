---
layout: default
title: Functional Programming
parent: Techniques
grand_parent: Technology Radar
nav_order: 2
description: |
  Functional Programming  
---

# Functional programming

[Abstract Types and Functors](https://www.cl.cam.ac.uk/~lp15/MLbook/PDF/chapter7.pdf) A modular structure makes a program easier to understand. Better still, the modules ought to serve as interchangeable parts: replacing one module by an improved version should not require changing the rest of the program. Standard ML’s abstract types and functors can help us meet this objective too.

[Modular Type Classes](https://people.mpi-sws.org/~dreyer/papers/mtc/main-long.pdf) - ML modules and Haskell type classes have proven to be highly effective tools for program structuring. Modules emphasize explicit configuration of program components and the use of data abstraction. Type classes emphasize implicit program construction and ad hoc polymorphism. In this paper, we show how the implicitly- typed style of type class programming may be supported within the framework of an explicitly-typed module language by viewing type classes as a particular mode of use of modules.

[Implementing, and Understanding Type Classes](http://okmij.org/ftp/Computation/typeclass.html) - This page looks behind the scenes of the abstraction of parametric overloading, also known as bounded polymorphism, or just `type classes`. Seeing the implementation makes type classes appear simpler, friendlier, more comfortable to use.

[Solving Embarrassingly Obvious Problems in Erlang](https://blog.usejournal.com/solving-embarrassingly-obvious-problems-in-erlang-e3f21a6203cc) - the article is helpful for any one how commits large function. It guides about decomposition of large functions using real life example. The technique is applicable to any language despite it refers to Erlang. 

[Inheritance vs Generics vs TypeClasses in Scala](https://dev.to/jmcclell/inheritance-vs-generics-vs-typeclasses-in-scala-20op) - A tutorial about polymorphism techniques in functional programming using Scala as an example. Answers the question about the difference between parametric polymorphism (generics) and ad-hoc polymorphism (type classes).

[Lightweight higher-kinded polymorphism](https://www.cl.cam.ac.uk/~jdy22/papers/lightweight-higher-kinded-polymorphism.pdf) Higher-kinded polymorphism is an essential component of many functional programming techniques such as monads, folds, and embedded DSLs. We show how to express higher-kinded polymorphism in OCaml with- out functors, using an abstract type app to represent type application, and opaque brands to denote abstractable type constructors. We demonstrate the flexibility of our approach by using it to translate a variety of standard higher-kinded programs into functor-free OCaml code.

