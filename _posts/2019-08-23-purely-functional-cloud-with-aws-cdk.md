---
layout: post
title:  Purely Functional Cloud Components with AWS CDK
description: |
  A toolkit to develop purely functional and high-order cloud components with AWS CDK. AWS development kit do not implement a pure functional approach. The abstraction of cloud resources is exposed using class hierarchy, each type represents a "cloud component" and encapsulates everything AWS CloudFormation needs to create the component. A shift from category of classes to category of pure functions simplifies the development by scraping boilerplate. A pure function component of is a right approach to express semantic of Infrastructure as a Code.
---

# Purely Functional Cloud Components with AWS CDK

Let's continue [the discussion about composition](/2019/07/28/composable-cloud-components-with-aws-cdk.html) in AWS CDK. AWS development kit do not implement a pure functional approach. The abstraction of cloud resources is exposed using class hierarchy, each type represents a "cloud component" and encapsulates everything AWS CloudFormation needs to create the component. A shift from category of classes to category of pure functions simplifies the development by **scraping boilerplate**. A pure function component of type `IaaC<T>` is a right approach to express semantic of Infrastructure as a Code. These function takes a scope cdk.Construct and creates a new element.

```typescript
type IaaC<T> = (scope: cdk.Construct) => T
``` 

The composition becomes an issue of building efficiently parent-children relations between AWS CDK components. Previous post left the vague definition of composition in the domain of pure functions. Here, we considers important composition patterns and its comparison with classical approach.


## Create a stack

Typically each application defines single or few CloudFormation stacks. These stacks are bundled into the application. 

```typescript
class MyStack extends cdk.Stack {
  constructor(parent: cdk.App, name: string, props: cdk.StackProps) {
    super(parent, name, props);
    // Non boilerplate code is here
  }
}

const app = new cdk.App();
new MyStack(app, 'MyStack');
```

Purely functional semantic defines a `root` operator. It attaches the pure stack components to the root of CDK application.

```typescript
function MyStack(scope: cdk.Construct): cdk.Construct {
  // Non boilerplate code is here
}

const app = new cdk.App();
root(app, MyStack);
```

The spec of `root` operator is

```typescript
function root<T>(root: App, fn: IaaC<T>, name?: string): App
```


## Attach resource to stack

Stack constructor instantiates "cloud component". AWS CDK defines entire stack by a labelled graph. All resource are created within the scope of another resources. The root of hierarchy is application with stack nodes under it.

```typescript
class MyStack extends cdk.Stack {
  constructor(parent: cdk.App, name: string, props: cdk.StackProps) {
    super(parent, name, props);
    
    new ResourceA(this, 'ResourceA', {/* ... */})
    new ResourceB(this, 'ResourceB', {/* ... */})
  }
}
```

Purely functional semantic defines a `join` operator. It attaches the pure definition of resource to the graph nodes. The logical name of the attached resources is defined by the name of a function.

```typescript
function ResourceA(): cdk.Construct {/* ... */}
function ResourceB(): cdk.Construct {/* ... */}

function MyStack(scope: cdk.Construct): cdk.Construct {
  join(scope, ResourceA)
  join(scope, ResourceB)
}
```

The spec of `join` operator is

```typescript
function join<T>(scope: Construct, fn: IaaC<T>): T
```


## Create a resource

The abstraction of cloud resources is exposed using class hierarchy, each type represents a "cloud component" and encapsulates everything AWS CloudFormation needs to create the component. These classes defines a common constructor pattern, which takes a graph scope, nodes logical name and property of component.

```typescript
class Function extends ... {
  constructor(scope: Construct, id: string, props: FunctionProps)
}

function MyFunction(scope: Construct): Function {
  return new Function(scope, 'MyFunction', 
    {
      runtime: Runtime.NODEJS_10_X,
      code: new AssetCode('./src'),
      // ...
    }
  )
}
```
An overhead exists in class-based approach of resource definition. Firstly, the duplication of logical name - name of function and literal constant. Secondly, we can observe that category of cloud resource is bi-parted graph. The left side is "cloud components", the right side is they properties (e.g. `Function <-> FunctionProps`). It is possible to infer a type of "cloud components" by type of its property and visa verse using ad-hoc polymorphism. For example, in Scala any one can use implicit.

```scala
def iaac[T](props: T)(implicit resource: Resource[A]) = resource.construct(props)
```

Usage of similar technique helps us to reduce a definition purely function component to the definition of properties only. The example below is beautiful, it is a pure function.  

```typescript
function MyFunction(scope: Construct): FunctionProps {
  return {
    runtime: Runtime.NODEJS_10_X,
    code: new AssetCode('./src'),
    // ...
  }
}
```

Unfortunately, implicit is not available in TypeScript and methods of [implicit simulation](/2019/08/10/adhoc-polymorphism-in-typescript.html) is not acceptable for AWS CDK because it requires maintenance of giant type mapping dictionaries.

Instead, purely functional semantic defines `iaac` operator - type safe factory. It takes a class constructor of "cloud component" as input and returns another function, which builds a type-safe association between "cloud component" and its property.

```typescript
// type of lambda is (iaac: IaaC<FunctionProp>) => IaaC<Function>
const lambda = iaac(Function)
lambda(MyFunction)
```

A shift from category of classes to category of pure functions simplifies the development by **scraping boilerplate**. The spec of `iaac` operator is

```typescript
type Node<Prop, Type> = new (scope: Construct, id: string, props: Prop) => Type

function iaac<Prop, Type>(f: Node<Prop, Type>): (fn: IaaC<Prop>) => IaaC<Type>
```


## Integrations and Targets

Often, AWS CDK development requires integration of "cloud components". As an example, usage of Lambda within API Gateway requires a packaging of resource into another class. 

```typescript
const restapi = new new RestApi(parent, 'MyApi', {/* ... */})
const lambda  = new Function(scope, 'MyFunction', {/* ... */})
const method  = new LambdaIntegration(lambda)

restapi.root.addResource('test').addMethod('GET', method)
```

So far, each "cloud components" is pure function then special composition techniques is required. Purely functional semantic defines a `wrap` operator. Its behavior almost identical to `iaac`. Its input is the constructor of integration category, the output is type safe factory.

```typescript
const use = wrap(LambdaIntegration)
const method = use(lambda(MyFunction))
```

The spec of `warp` operator is

```typescript
type Wrap<Prop, TypeA, TypeB> = new (scope: TypeA, props?: Prop) => TypeB

function wrap<Prop, TypeA, TypeB>(f: Wrap<Prop, TypeA, TypeB>): (fn: IaaC<TypeA>) => IaaC<TypeB> {
  return (iaac) => (scope) => new f(iaac(scope))
}
```

## Effects

`iaac` and `wrap` are primary composition operator used for application development. There is a challenge to use these operators along with native AWS CDK API because operators works with `IaaC<T>` category.

```typescript
namespace cloud {
  export const restapi = iaac(RestApi)
  export const lambda  = iaac(Function)
  export const method  = wrap(LambdaIntegration)
}

const restapi = cloud.restapi(MyApi)
const method  = cloud.method(cloud.lambda(MyFunction))

restapi.root.addResource('test').addMethod('GET', method)
```

For example, the code fails to compile - `addMethod` requires an `Integration` type. Purely functional semantic resolves this issue with concept of `effects` which are applicable over `IaaC<T>`.

```typescript
use({ restapi, method })
  .effect(x => x.restapi.root.addResource('test').addMethod('GET', x.method))
  .yield('restapi')
```

The effect is a type-class that operates with product of individual `IaaC<T>`. It implements methods to apply effects to product of "cloud components" and yields the result back. The effect function operates with pure types `T`. The effect returns always `IaaC<T>`. You have to flatten it before attach it to the stack with `flat: IaaC<IaaC<T>> => IaaC<T>`.

```typescript
function MyApi(): IaaC<RestApi> {/* ... */}

function MyStack(stack: cdk.Construct) {
  join(stack, flat(MyApi))
}
```

The spec of effects is

```typescript
type Product<T> = {[K in keyof T]: IaaC<T[K]>}
type Pairs<T> = {[K in keyof T]: T[K]}

class Effect<T extends Pairs<T>> {
  value: IaaC<T>
  constructor(x: IaaC<T>){this.value = x}

  effect(f: (x:T) => void): Effect<T> {/* ... */}

  yield<K extends keyof T>(k: K): IaaC<T[K]> {/* ... */} 
}

function use<T extends Pairs<T>>(resources: Product<T>): Effect<T>
```

## Afterwords

The discussed notations complements AWS CDK with the *purely functional composition*. This style of development builds a new things from small reusable "cloud components". A shift from category of classes to category of pure functions simplifies the development by **scraping boilerplate**.

The features discussed here are implemented at [aws-cdk-pure library](https://github.com/fogfish/aws-cdk-pure).  

Feel free to share your comments and thought on [Twitter](https://twitter.com/_fogfish_/status/1164894358321139712) or raise and issue to [GitHub](https://github.com/aws/aws-cdk-pure).
