---
layout: post
title:  Composable Cloud Components with AWS CDK
description: |
  The composition is a style of development to build a new things from small reusable elements. AWS CDK makes an opportunity to deliver Infrastructure as a Code. The article discusses various patterns of cloud components composition and emphasis a purely functional method as a solution to scarp a boilerplate code. 
---

# Composable Cloud Components with AWS CDK

> Composition is the essence of programming...

The *composition* is a style of development to build a new things from small reusable elements. The composition is a key feature in functional programming. Functional code looks great only if functions clearly describe your problem. Usually lines of code per function is only a single metric that reflects quality of the code.

> If your functions have more than a few lines of code (a maximum of four to five lines per function is a good benchmark), you need to look more closely — chances are you have an opportunity to factor them into smaller, more tightly focused functions.

This is because functions describe the solution to your problem. If your code contains many lines then highly likely you are solving few problems without articulating them. A critical thinking is the process of software development. 

A recent release of [AWS CDK](https://github.com/aws/aws-cdk) not only made an opportunity to deliver Infrastructure as a true Code but also gave a capability to make it from decomposable elements. Unfortunately, the development kit ignores a pure functional approach. The abstraction of cloud resources is exposed using class hierarchy.

```typescript
import * as cdk from '@aws-cdk/core'

class MyStack extends cdk.Stack {
  constructor(app: cdk.App, id: string) {
    super(app, id)
    ...
  }
}

const app = new cdk.App()
new MyStack(app, 'MyStack')
app.synth()
```

You are going to produce terrible code if you put entire stack definition to construction. No one will be able to understand at a glance what is going on there.


## Compose with cdk.Construct

> Constructs are the basic building blocks of AWS CDK apps. A construct represents a "cloud component" and encapsulates everything AWS CloudFormation needs to create the component.
> -- [AWS CDK](https://docs.aws.amazon.com/cdk/latest/guide/constructs.html)

Use this class to define code blocks that solves a single problem - just create a single cloud resource.

```typescript
import * as cdk from '@aws-cdk/core'

class MyResource extends cdk.Construct {
  constructor(scope: cdk.Construct, id: string) {
    super(scope, id)
    ...
  }
}

class MyStack extends cdk.Stack {
  constructor(app: cdk.App, id: string) {
    super(app, id)
    new MyResource(this, 'MyResource')
  }
}

const app = new cdk.App()
new MyStack(app, 'MyStack')
app.synth()
```

You'll be able to build reusable cloud component library and automate infrastructure patterns with real code. The usage of `cdk.Construct` only suffers from boilerplate code. 

## Pure functions

Let's try to define pure functions to solve embarrassingly obvious composition problem. We need to shift our perspective from category of classes to category of pure functions.

Let's abstract our Infrastructure as a Code in terms of pure functions. Functions that takes a parent cdk.Construct and creates a new element.

```typescript
type IaaC<T> = (parent: cdk.Construct) => T
```

The composition is about building efficiently parent-children relations due to nature of AWS CDK framework.

```typescript
type Compose = <T>(parent: cdk.Construct, children: IaaC<T>) => cdk.Construct
```

Finally, we are able to define `_` as compose operator

```typescript
function _<T>(parent: cdk.Construct, fn: IaaC<T>): cdk.Construct {
  root instanceof cdk.App
    ? fn(new cdk.Stack(parent, fn.name))
    : fn(new cdk.Construct(parent, fn.name))
  return parent
}
```

The definition of our cloud resources becomes reflected in terms of pure functions

```typescript
function MyResource(scope: cdk.Construct): cdk.Construct {
   return ...
}

function MyStack(stack: cdk.Construct): cdk.Construct {
   return _(stack, MyResource)
}

const app = new cdk.App()
_(app, MyStack)
app.synth()
```

Now, the infrastructure is decomposable using small function. Each obviously correct and self explainable. This example eliminates boilerplate code of class constructions.

Here are some of the benefits:
* Infrastructure intent is clear and obvious
* It becomes testable and easier to reason about
* Easier to debug and maintain 

See the gist of [`pure.ts`](https://gist.github.com/fogfish/e97ef042db0afe011149873a56f79d93).

Feel free to share your comments either to [Twitter](https://twitter.com/_fogfish_/status/1156165926619963392) or [GitHub issue](https://github.com/aws/aws-cdk/issues/3481).
