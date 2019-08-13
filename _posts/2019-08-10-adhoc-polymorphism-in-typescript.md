---
layout: post
title:  Ad-Hoc polymorphism in TypeScript with implicit context
---

# Ad-hoc polymorphism in TypeScript with implicit context.

> Ad􏰀hoc polymorphism occurs when a function is defined over several different types􏰌 acting in a dif􏰀ferent way for each type􏰍.

Overloading is a most simplest example of ad-hoc polymorphism. It associates a single function with many implementations. TypeScript supports only dynamic [overloading](https://www.typescriptlang.org/docs/handbook/functions.html#overloads) - you have to execute runtime checks of types and choose appropriate behavior using if-else syntax. TypeScript compilation output a pure JavaScript code. Its dynamic nature do not allow to implement overloading similarly to other languages.

TypeScript overloading requires usage of `typeof` and `instanceof` checks, which are considered as anti-patterns in type-safe environments. Usually, type systems offers a better options - [Type-Classes](https://en.wikipedia.org/wiki/Type_class). Type-classes has been designed by P. Wadler and S. Blott as a [new approach to ad-hoc polymorphism](http://homepages.inf.ed.ac.uk/wadler/papers/class/class.ps). Haskell is one of the first languages that adopted this technique.

The type class just defines trait to a group of types. This trait is a contract, it can be implemented without any changes to the original code. One could say that inheritance does same, but no one needs to predict inheritance demand with type classes before hand. Please see the comparison of [inheritance vs type classes](https://dev.to/jmcclell/inheritance-vs-generics-vs-typeclasses-in-scala-20op).

Let's look into a classical example of type class polymorphism with Scala: 

```scala
trait Show[A] {
  def show(a: A): String
}

object Show {
  def show[A](a: A)(implicit sh: Show[A]) = sh.show(a)
}

implicit val intCanShow: Show[Int] =
  new Show[Int] {
    def show(int: Int): String = s"int $int"
  }

implicit val strCanShow: Show[String] =
  new Show[String] {
    def show(str: String): String = s"string $str"
  }

Show.show(10)      // "int 10"
Show.show("hello") // "string hello"
Show.show(true)    // compile time error implicit value Show[Boolean] not found
```

That is basically a type class! There is a behavior trait that describes the functionality and implementations for each required type. Important highlight, the shown functionality is defined outside of each specific type definition. It could be implemented in a different modules or libraries than the behavior trait. In Scala, the `implicit` mechanism does a matching of class instances with code that uses them. The compiler infers a right implementation using Scala's `implicit` feature while executing a generic `show` algorithm.

There is a wish about [implicit in TypeScript](https://github.com/Microsoft/TypeScript/issues/10844) but there is a doubt that it will ever be materialized.

Let's implement type classes for TypeScript at "user-side". 


## Demystifying type classes

Knowing the implementation details behind type classes makes them comfortable to use. There is a great [post](http://okmij.org/ftp/Computation/typeclass.html) that 

> looks behind the scenes of the abstraction of parametric overloading, also known as bounded polymorphism, or just type classes.

I'd like to emphasis on two techniques, which are relevant for TypeScript context: **dictionary passing** and **intensional type analysis**.

Dictionary passing is most well-known techniques of implementing type classes. It has been implemented in Haskell with a greatest advantage to other languages - Haskell can do dictionary passing automatically. Compiler translates type-class declaration to records in the dictionary. Each method has corresponding dictionary record, which is straightforwardly used by compiler to infer a corresponding types.

<!--
Monomorphization uses code re-write techniques. It takes a type-safe code and outputs the code with no type classes and no bounded polymorphism. All overloading and other constrains are resolved at compile time. Monomorphization takes expression-by-expression re-writes source expressions into the target and possibly updating the internal state. It recursively repeats it. Sounds like TypeScript to JavaScript compilation.
-->

Intensional type analysis is complementary to compile-time type class resolution. The appropriate overloading operation is chosen at run-time using type identification. Intuitively, TypeScript/JavaScript uses the runtime technique to implement ad-hoc polymorphism. The intensional type analysis takes expression-by-expression and eliminates all reference of type classes and they constrains using type annotation of all identifiers.


## Attempt on Type Classes implementation

Let's sketch a type class implements in TypeScript using dictionary and intensional type analysis technique. 

```typescript
interface Show<A> {
  show(a: A): string
}

class IntCanShow implements Show<number> {
  show(int: number): string {
    return `int ${int}`
  }
}

class StrCanShow implements Show<string> {
  show(str: string): string {
    return `string ${str}`
  }
}

namespace Show {
  export const Number = new IntCanShow
  export const String = new StrCanShow
}

function show(x: any): string {
  return Show[x.constructor.name as keyof typeof Show].show(x)
}

show(10)         // int 10
show('hello')    // string hello
show(true)       // runtime error Cannot read property 'show' of undefined
```

Here, `namespace Show` is a dictionary that support a run-time matching of class instances with code that uses them, `x.constructor.name` (or other runtime reflection) resolves a type identification. 

Unfortunately, the proposed implementation do not implements compile-time type safeness. The code fails at runtime if supplied type is not implemented by the dictionary `namespace Show`.

A few brute force ideas to improve it

```typescript
function show<T>(x: T): string {
  return Show[x.constructor.name as keyof typeof Show].show(x)
}

//
// fails at compile time with error and do not guarantee type safeness
// Property 'constructor' does not exist on type 'T'.
```

Usage of overloads solves a problem of type safeness but we are loosing a flexible code decoupling. All supported types have to be declared in single module. It is impossible to offload a functionality outside of each specific type definition, implement it in different modules or libraries. 

```typescript
function show(x: number): string;
function show(x: string): string;
function show(x: any): string {
  return Show[x.constructor.name as keyof typeof Show].show(x)
}
```

## Type Classes in TypeScript

Fortunately, the solution exists by enforcing type safety of generic function `show<T>` while delivering a type identification:

```typescript
function show<T>(x: T): string {
  return Show[x.constructor.name as keyof typeof Show].show(x)
}
```

In order to fulfill our type safety requirement, the type `T` must reflect required types and only accept types defined in implicit dictionary (e.g. `namespace Show`). The implementation of latter constrain is easy.

```typescript
namespace Show { ... }

type TShow = keyof typeof Show

function show<T extends TShow>(...) { ... }
```

The implementation of both constrains requires definition of generic type `Cat<T>`. It constrains a required type of instance `x` and ensures that these types are implemented by implicit dictionary.

```typescript
function show<T extends TShow>(x: Cat<T>) { ... }
```

The implementation of `Cat<T>` is inspired by higher-kinded polymorphism feature from [fp-ts library](https://github.com/gcanti/fp-ts) but it fully relies on TypeScript native index type query and indexed access operators [features](https://www.typescriptlang.org/docs/handbook/advanced-types.html#index-types). 

```typescript
type Cat<T> = Cats[T]
```

We just projects a type T, which is eventually type of all know keys of implicit context, to an required type. Unfortunately, it required definition on another dictionary `interface Cats{}` to store all known types. The final definition of type `Cat<T>` is

```typescript
interface Cats {}

type Cat<T extends keyof Cats> = Cats[T]
```

The final implementation of ad-hoc polymorphism (type-class)  with TypeScript looks

```typescript
//
// Generic algorithms
interface Cats {}

type Cat<T extends keyof Cats> = Cats[T]

namespace Show {}
type TShow = keyof typeof Show

interface Show<A> {
  show(a: A): string
}

function show<T extends TShow>(x: Cat<T>): string {
  return (<any>Show[x.constructor.name as TShow]).show(x)
}

//
// Int
class IntCanShow implements Show<number> {
  show(int: number): string {
    return `int ${int}`
  }
}

namespace Show {export const Number = new IntCanShow}
interface Cats {Number: number}

//
// String
class StrCanShow implements Show<string> {
  show(str: string): string {
    return `string ${str}`
  }
}

namespace Show {export const String = new StrCanShow}
interface Cats {String: string}

//
// Usage
console.log(show(10))      // int 10
console.log(show('hello')) // string hello
console.log(show(true))    // compile time error  
                           // Argument of type 'true' is not assignable 
                           // to parameter of type 'string | number'
```

A few times in this post, I've pitched a requirements to offload a functionality outside of each specific type definition and implement it in different modules or libraries. TypeScript supports a [declaration merging](https://www.typescriptlang.org/docs/handbook/declaration-merging.html). This features helps us to build a library of type classes.


## Conclusion

TypeClasses are not first-class citizen in the TypeScript language. Additionally, the language do not provide a concept of implicit that remains to be a good solution for ad-hoc polymorphism in Scala. However, TypeScript has advanced type system that allow us to write type classes. The syntax and implementation is not straight forwards at the first glance but depicted technique helps to achieve it.


## References

1. [How to make ad􏰀-hoc polymorphism less ad-hoc](http://homepages.inf.ed.ac.uk/wadler/papers/class/class.ps)
2. [Types and Programming Languages](http://www.cis.upenn.edu/~bcpierce/tapl/)
