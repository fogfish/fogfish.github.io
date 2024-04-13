# Languages and Frameworks
These include programming languages like Go, Java and Python and frameworks Flutter, React.js and Gin, etc

- [Languages and Frameworks](#languages-and-frameworks)
  - [Golang](#golang)
  - [Flutter](#flutter)
  - [TypeScript](#typescript)
  - [JavaScript, React \& React Hooks](#javascript-react--react-hooks)
  - [C](#c)
  - [C++](#c-1)
  - [Bash, Zsh and Shell scripting](#bash-zsh-and-shell-scripting)
  - [Material UI](#material-ui)
  - [Python](#python)
  - [Datalog](#datalog)
  - [JSII](#jsii)
  - [GraphQL](#graphql)
  - [Erlang](#erlang)
  - [Elixir](#elixir)
  - [Hamler](#hamler)
  - [Scala](#scala)
  - [Haskell](#haskell)
  - [Java](#java)
  - [C#](#c-2)
  - [Perl](#perl)
  - [Octave](#octave)
  - [R](#r)
  - [PHP](#php)
  - [React Native](#react-native)
  - [BlueprintJS](#blueprintjs)
  - [UiKit](#uikit)
  - [Jekyll](#jekyll)
  - [Akka \& Typed Akka](#akka--typed-akka)


## Golang
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Golang (or Go) is a programming language renowned for its versatility in general-purpose, type-safe development across various domains. It excels in building backend services, cloud applications, and command-line utilities with its efficient concurrency model and robust standard library.

Backend Services: Golang is widely adopted for developing scalable and high-performance backend services. Its simplicity, speed, and built-in support for concurrency make it ideal for building microservices, RESTful APIs, and distributed systems. Gin and its middleware is the default selection. However, Go combinator library for building containerized and serverless HTTP services (https://github.com/fogfish/gouldian) has shown its potential.   

Cloud Applications: Golang's lightweight runtime and efficient memory management make it well-suited for cloud-native development. It seamlessly integrates with cloud platforms, enabling developers to build resilient and scalable applications for the cloud environment. Thus, Golang is the primary selection for serverless applications.

Command-Line Utilities: Golang's ease of use and powerful standard library make it an excellent choice for building command-line tools and utilities. Its cross-platform compatibility and efficient execution speed ensure that command-line applications written in Golang are fast and reliable. Cobra (https://github.com/spf13/cobra) and https://clig.dev/. GoReleaser (https://goreleaser.com) is the defacto standard solution for automation of cli applications.  

Type Safety: Golang's static typing system provides strong guarantees about the correctness of code at compile-time, reducing the likelihood of runtime errors and enhancing code maintainability. Addon libraries (e.g. https://github.com/fogfish/golem) provide various functional abstractions to improve type safety. Despite this, the  linter for Golang is a necessary tool - https://staticcheck.dev just does its job (golangci-lint does not support generics).  

Concurrency: Golang's native support for concurrency through goroutines and channels enables developers to write concurrent programs with ease, making it particularly suitable for building highly concurrent and parallel systems.

Overall, Golang's simplicity, performance, and concurrency support make it a popular choice for a wide range of development tasks, from backend services to cloud-native applications and command-line utilities. Its growing ecosystem and vibrant community continue to propel its adoption across various industries and use cases.

## Flutter
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Flutter is a versatile cross-platform front-end development framework renowned for its ability to support development across Web, iOS, and Android platforms. The major components of Flutter comprise the Dart platform, the Flutter engine, and the Foundation library. Additionally, Flutter offers a rich set of tools and widgets, facilitating rapid and efficient development of visually stunning and performant applications across various devices and platforms.

BLOC, which is a predictable state management library for Dart, is the default state management solution for me.  

## TypeScript
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

For me, TypeScript is a versatile language utilized for Infrastructure as Code (IaC) development and cloud integrations, employing pure-functional and higher-order type-safe techniques. Notably, AWS CDK leverages TypeScript to define target infrastructure, enabling developers to express cloud resources and configurations with clarity and precision.

The language accelerates development by providing robust static type checks at compile time, reducing the likelihood of errors and enhancing code reliability. Examples like Purely Functional and higher-order cloud components (https://github.com/fogfish/aws-cdk-pure) and Security-compliant AWS CDK components (https://github.com/SSHcom/c3) exemplify TypeScript libraries that extend the basic functionality of AWS CDK, offering enhanced features and capabilities for building cloud-native applications.

## JavaScript, React & React Hooks
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

JavaScript is only associated with front-end development in my cases, where it is often paired with frameworks like React to accelerate the development of web and hybrid mobile applications.

React is primarily utilized for User Experience (UX) development, emphasizing a pure functional approach to building interfaces. This involves leveraging React in a manner that eschews object-oriented programming paradigms, opting instead for a functional programming style that emphasizes the use of functions over classes.

React Hooks represents a significant advancement in React development, offering a technique for composing high-order components and moving away from class-based approaches. With Hooks, developers can create more modular and reusable components, facilitating the development of micro-frontends and enhancing code maintainability.

In addition to component composition, React Hooks also serve as a powerful tool for abstracting side effects in UX development. Projects like react-hook-oauth2 demonstrate how Hooks can be used to manage complex side effects, such as authentication flows, in a concise and reusable manner.

Overall, React and React Hooks enable developers to build dynamic and interactive user interfaces with a focus on simplicity, modularity, and maintainability. By embracing functional programming principles and leveraging Hooks for component composition and side-effect management, developers can create more robust and flexible UX solutions.

Personally, I’ve put Redux on-hold as a state management solution. React hooks + context is sufficient for small projects, the large one benefits from BLOC pattern.  

## C
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

No active development, mainly GCO bindings.
C is a tool for embedded systems, drivers development and Linux kernel development. It was one of the first ![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)ed languages but higher-order language like Go has substituted daily development on C.

## C++
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)
C++ old-good but Go is a preferred choice.

## Bash, Zsh and Shell scripting
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Shell scripting and Makefiles serve as fundamental tools for orchestrating heterogeneous workloads. Leveraging these tools helps avoid the need for custom domain-specific languages in continuous integration (CI) systems, enabling the implementation of testable automation pipelines. Their simplicity and versatility make them indispensable for managing complex tasks and streamlining development workflows across various environments and platforms.

## Material UI
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Material UI is a leading library for UI development, offering developers a multitude of advantages including simplicity, extensibility, visually appealing designs, and a comprehensive set of UI components. It serves well for Web (React) and Hybrid (Flutter) development.

## Python
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

In my perspective, Python stands out as the predominant language for machine learning and data science tasks, often paired with influential frameworks such as TensorFlow or Gensim. These tasks are typically executed within Jupyter notebooks or Databricks environments, enhancing the efficiency and flexibility of the development process.

TensorFlow stands as the de facto framework for constructing machine learning workloads, often paired with its high-level API Keras. Successfully delivering machine learning pipelines necessitates a blend of engineering and data science activities. Leveraging technologies like AWS Batch with GPU support proves instrumental in building robust production-grade ML training pipelines with TensorFlow.

## Datalog
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

Datalog is a versatile logic programming language that serves as a simplified database query tool. Its primary advantage lies in its ability to efficiently query linked data, graphs, and other recursive data structures. Datalog has demonstrated its power in production systems, particularly in deducing real-time consumer behavior, as a showcase of datastore like Datomic (https://www.datomic.com). The language is supported by multiple runtimes, including Datalog in Erlang (https://github.com/fogfish/datalog), which enhances its versatility and ![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)ion across different platforms. Thanks to its simplicity, Datalog can be seamlessly integrated as a query language into existing storage solutions such as Redis (https://github.com/fogfish/relog), DynamoDB, and the Elastic (ELK) Stack (https://github.com/fogfish/elasticlog), enabling developers to leverage its capabilities within their existing infrastructure.
- [Deduct consumer behavior in real-time](https://tech.fog.fish/2015/03/17/real-time-consumer-engagement.html)

## JSII
![Assess](https://img.shields.io/badge/assess-fbe6a8?style=flat-square)

Integrating TypeScript with other languages, such as Golang, often entails a significant amount of configuration and boilerplate code. However, in my recent experience, I've found that for Infrastructure as Code (IaC) development, I lean towards utilizing JSII with TypeScript, particularly for L3 library development.

JSII (JavaScript Interoperability Interface) simplifies the process of integrating TypeScript with other languages, allowing for seamless interoperability while minimizing the overhead of configuration and boilerplate code. This approach streamlines the development of infrastructure components, enabling developers to focus more on building robust and scalable solutions without being encumbered by tedious setup tasks.

By leveraging JSII with TypeScript, developers can harness the expressive power of TypeScript for IaC development while benefiting from the flexibility and interoperability provided by JSII. This combination facilitates efficient and effective development workflows, ultimately leading to the creation of high-quality infrastructure solutions with ease. 

## GraphQL
![Assess](https://img.shields.io/badge/assess-fbe6a8?style=flat-square)

Communities widely promote GraphQL as a modern API Gateway due to its advanced capabilities. Paired with micro frontend techniques, GraphQL greatly facilitates development within distributed teams, offering enhanced flexibility and efficiency in building and managing APIs across various services and applications.

## Erlang
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Suspended any active development.
Erlang stands out as an exceptional language for the development of scalable, fault-tolerant, and soft-real-time systems. Its unique features and capabilities make it well-suited for a variety of use cases:

Scalability: Erlang's lightweight processes, known as "actors," and built-in support for concurrency and distribution enable the creation of highly scalable systems. It excels in handling large numbers of concurrent connections and processing tasks efficiently.

Fault Tolerance: Erlang's "let it crash" philosophy, combined with its supervision tree architecture, empowers developers to build fault-tolerant systems that can recover from failures gracefully. This makes it ideal for building resilient applications that require high availability.

Soft real-time: Erlang's low-latency message passing and predictable garbage collection make it suitable for soft-real-time applications, where responsiveness and consistency are crucial, such as telecommunication systems and real-time collaboration platforms.

Distributed systems: Erlang's built-in support for distribution enables seamless communication between nodes, making it an excellent choice for building distributed systems. It is widely used in building distributed fault-tolerant storage solutions, distributed databases, and distributed message queues.

Time Series databases: Erlang's performance and fault-tolerance capabilities make it well-suited for building time series databases, which require efficient storage and retrieval of timestamped data points. Erlang-based databases like Riak Time Series leverage these features to provide scalable and reliable storage solutions for time-series data.

Workload management: Erlang's lightweight processes and efficient scheduling mechanisms make it ideal for building workload management systems that can handle high-throughput and bursty workloads while maintaining low latency and high availability.

Overall, Erlang's unique combination of features makes it a powerful tool for building robust, scalable, and fault-tolerant systems, particularly in domains where reliability, scalability, and real-time responsiveness are paramount. Its proven track record in building telecommunications systems, distributed databases, and real-time messaging platforms underscores its suitability for a wide range of demanding applications.

## Elixir
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Elixir is a possible "modern" replacement for Erlang but its Ruby syntax leaks alot of Erlang abstractions.

## Hamler
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Hamler (https://www.hamler-lang.org) is Haskell-style functional programming language running on Erlang VM.

## Scala
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Suspended any active development
Scala stands as a versatile language ideal for microservice development, offering several advantages over other JVM-based languages. With its expressive syntax and powerful features, Scala has emerged as the primary choice for building microservices in the Java ecosystem. Notably, Scala continues to evolve with advancements in pure functional and higher-order type abstractions, enabling developers to write concise, expressive, and type-safe code. Additionally, Scala's seamless interoperability with existing Java libraries and frameworks enhances its appeal for microservice architecture. Overall, Scala's combination of flexibility, scalability, and modern language features makes it a compelling option for developing robust and maintainable microservices. 

My Scala experience was only within the microservice domain, primary building solutions with Shapeless around Finch.  

Shapeless (https://github.com/milessabin/shapeless) was a default choice for me that facilitates generic programming in Scala, empowering developers to write code that operates efficiently across a wide range of data types and structures. By leveraging Shapeless, Scala programmers can achieve greater flexibility and expressiveness in their code, allowing for the creation of highly reusable and composable components. With its innovative approach to type-level programming and powerful features like heterogeneous lists and type-safe generic derivations, Shapeless opens up new possibilities for solving complex problems and building elegant, type-safe abstractions. Overall, Shapeless represents a significant advancement in Scala development, enabling developers to write more concise, modular, and maintainable code.

Finch (https://github.com/finagle/finch) is a powerful combinator library in Scala designed specifically for building HTTP services. With Finch, developers can leverage a rich set of combinators to define HTTP endpoints and handle requests and responses with ease. By composing these combinators, developers can create complex routing logic and middleware in a concise and expressive manner, making it ideal for building scalable and maintainable HTTP services. Additionally, Finch provides robust support for JSON serialization and deserialization, enabling seamless integration with modern web APIs. Overall, Finch offers a straightforward and type-safe approach to building HTTP services in Scala, making it a popular choice among developers for web development projects.

## Haskell
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Fun but unnecessary complexity.
For me, Haskell is  a primary reference to explore functional programming patterns and abstractions.

## Java
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Java is only applicable to maintain legacy codebases.

## C#
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

C# is only applicable to maintain legacy codebases.

## Perl
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Per is used to be a king for advanced shell scripting. Nowadays, usage of Go for cli development is a preferred choice.

## Octave
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Octave is an open source replacement for Matlab but modern data science is all about Python.

## R
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

R used to be dominant in data science communities a decade ago.

## PHP
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

PHP is the language for web app development. Lack of type system(s) makes it outdated in modern development.

## React Native
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Replaced by the Flutter due to unnecessary complexity and high community fragmentations.
React Native serves as a versatile framework tailored for hybrid application development across the iOS and Android mobile platforms. With its robust set of tools and components, React Native empowers developers to build cross-platform applications that offer a seamless user experience on both iOS and Android devices. By leveraging React Native, developers can streamline the development process, reduce code duplication, and reach a broader audience with their mobile applications.

## BlueprintJS
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Replaced by the Material UI, not well scalable for mobile.
BlueprintJS stands out as a prominent library for UI development, offering a comprehensive set of tools and components. Renowned for its versatility and ease of use, BlueprintJS has emerged as a top choice among developers, rivaling other popular UI development frameworks such as Bootstrap, Metro UI, and Dress Code. With its robust feature set, BlueprintJS continues to be a preferred solution for crafting intuitive and visually appealing web applications.

## UiKit
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Replaced by the Material UI, not well scalable for mobile.
UIKit (https://getuikit.com) is a lightweight and modular front-end framework for developing fast and powerful web interfaces.

## Jekyll
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Due to Ruby and inconsistency with Gems, it is hard to maintain a project if ruby is not your primary development environment.
Jekyll is a versatile framework renowned for its efficacy in creating static websites and online project documentations. Leveraging Jekyll empowers users to effortlessly generate and manage web content, making it an ideal choice for projects that prioritize simplicity, speed, and ease of maintenance. With its intuitive structure and powerful templating capabilities, Jekyll simplifies the process of building and updating websites, allowing users to focus more on content creation and less on technical intricacies. As a result, Jekyll remains a popular solution for individuals and organizations seeking a straightforward yet robust platform for their online presence.

## Akka & Typed Akka
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Unnecessary complexity to compare with Erlang, suspended active development.
Typed Akka represents a robust and type-safe actor system, offering developers a powerful framework for building highly scalable and resilient applications. With Typed Akka, developers can define actors with strong type safety, ensuring greater clarity and reliability in their code. This approach provides an intuitive and expressive way to define actors, making it easier for developers to reason about and manage complex concurrent systems. Additionally, Typed Akka offers advanced features such as supervision strategies and message protocols, further enhancing the robustness and resilience of actor-based applications. Overall, Typed Akka serves as a valuable tool for building fault-tolerant and scalable systems in a type-safe and intuitive manner.
