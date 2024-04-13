# Tools and Technologies
Standalone software components such as databases, software development tools, such as versions' control systems; or more generic categories of productivity tools.

- [Tools and Technologies](#tools-and-technologies)
  - [AWS Cloud Development Kit](#aws-cloud-development-kit)
  - [Relational Database Management Systems](#relational-database-management-systems)
  - [Key Value Datastores (NoSQL)](#key-value-datastores-nosql)
  - [Real-time event processing](#real-time-event-processing)
  - [Reactive computing](#reactive-computing)
  - [Data Meshes and Warehouse](#data-meshes-and-warehouse)
  - [Identity and Access Management](#identity-and-access-management)
  - [Graph Databases](#graph-databases)
  - [Batch processing](#batch-processing)
  - [Geographic Information Systems](#geographic-information-systems)
  - [Zero Trust](#zero-trust)
  - [Artificial Intelligence \& Machine Learning](#artificial-intelligence--machine-learning)
  - [Vector Databases \& Embeddings](#vector-databases--embeddings)
  - [Distributed Hash Table](#distributed-hash-table)
  - [Peer-to-Peer Networking](#peer-to-peer-networking)
  - [Messaging and Queuing Systems](#messaging-and-queuing-systems)
  - [Embedded Queuing Systems](#embedded-queuing-systems)
  - [Consumer Search and Discovery](#consumer-search-and-discovery)
  - [Apache Solr](#apache-solr)
  - [Embedded Databases](#embedded-databases)
  - [Smart Sockets](#smart-sockets)
  - [LiDAR](#lidar)
  - [OpenGL ES](#opengl-es)


## AWS Cloud Development Kit
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

The AWS Cloud Development Kit (CDK) harnesses Infrastructure as Code (IaC) principles to achieve idempotence, ensuring that regardless of how many times the code is executed, it consistently yields the same outcome. Developed using TypeScript, a strictly type-safe language, CDK allows developers to precisely declare the target infrastructure. Employing type-safe constructs, each corresponding to an AWS concept, facilitates efficient development. Familiar tools and techniques expedite the process, while compile-time static type checks help mitigate potential errors. Additionally, CDK transpiles TypeScript code into AWS CloudFormation, enabling seamless orchestration of cloud component provisioning.

## Relational Database Management Systems
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Relational databases leverage relational algebra as a foundational framework for modeling data, with the majority of RDBMS employing B+ Tree indexing and SQL for data management tasks. Nevertheless, certain implementations opt for alternative indexing approaches like log-merge or fractals to optimize performance and scalability. Postgres stands as a prominent open-source example of an RDBMS, renowned for its robustness, extensibility, and support for advanced features (e.g. JSONB, full-text search, vector search, etc).

## Key Value Datastores (NoSQL)
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

The rapid evolution of scalable Internet services has revolutionized data retrieval requirements, sparking a resurgence in NoSQL technology around 2010. Unlike traditional RDBMS systems, which rely heavily on B+ tree indexing, Key-Value Storages offer a distinct approach, leveraging associative arrays (hash-tables) for low-latency data access. This technology has seen widespread adoption, with platforms like AWS DynamoDB, AWS S3, Redis, Memcached, Coherence, LevelDB, RocksDB, Riak, and MySQL Handler Socket leading the charge.

The absence of relational structures necessitates a sophisticated approach to data modeling. To address this challenge, leveraging Linked Data patterns proves invaluable in defining complex data domains and structuring data in a meaningful way. This enables organizations to efficiently manage and query diverse datasets while maximizing the performance and scalability of their systems.

- [Key-value abstraction to store algebraic, linked-data data types](https://github.com/fogfish/dynamo)
- [Method and apparatus for providing applications with shared scalable caching](https://patents.google.com/patent/US8977717B2/en)

## Real-time event processing
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

The technology represents a natural evolution of queueing systems, incorporating elements of the publish/subscribe paradigm and asynchronous communication. Event systems have emerged in response to the demand for reactive computing and the proliferation of decentralized systems development. These systems often serve as the most effective approach to scaling data intake pipelines, facilitating real-time processing and analysis of vast volumes of data.

It's crucial to highlight key patterns such as Event Sourcing, Command Query Responsibility Segregation (CQRS), and Inside-out databases within event-driven architectures. These patterns offer valuable strategies for managing data flow, maintaining consistency, and enabling efficient processing of events.

When considering implementations, Apache Kafka, AWS Kinesis, and AWS EventBridge stand out as robust solutions that provide scalable and reliable event processing capabilities. However, traditional queueing systems like AWS SQS can serve as lightweight alternatives to event systems, particularly when event ordering is not critical for applications.

In the realm of programming abstractions, Go channels emerge as fundamental tools for building event processing systems, offering lightweight and efficient mechanisms for communication and synchronization between concurrent processes.

- [Inside-out database](https://www.confluent.io/blog/turning-the-database-inside-out-with-apache-samza/)
- [Command Query Responsibility Segregation](https://martinfowler.com/bliki/CQRS.html)
- [Go channels for distributed queueing and event-driven systems](https://github.com/fogfish/swarm)

## Reactive computing
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Reactive computing can be conceptualized as the client-side counterpart of real-time event processing, where data flows through a series of operations, reacting to changes in real-time. At the core of reactive computing lies the concept of streams (http://srfi.schemers.org/srfi-41/srfi-41.html), which are sequential data structures containing computed elements on demand. These streams enable developers to model and process data flows in a reactive manner, facilitating dynamic and responsive applications.

Frameworks such as [Apache Camel](https://camel.apache.org), Akka Streams, and [RxJS](https://rxjs-dev.firebaseapp.com) are widely recognized for their support of reactive programming paradigms. These frameworks provide developers with powerful tools and abstractions for working with streams and managing asynchronous data processing.

An important aspect of streams is that they adhere to the monad laws, providing consistency and predictability in how data is transformed and manipulated. By enforcing functional purity, streams abstract away the complexities of data processing, eliminating side effects and facilitating composability and maintainability in codebases.
- [The stream monad](https://patternsinfp.wordpress.com/2010/12/31/stream-monad/)
- [Pure functional stream implementation](https://github.com/fogfish/datum/blob/master/src/stream/stream.erl)
- [Standard I/O build over streams](https://github.com/fogfish/stdio)

## Data Meshes and Warehouse
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Analytics and System of Records (SoR) represent the core use cases for Data Warehouses, historically posing challenges in terms of scalability and management. Decades ago, on-premises Data Warehouses necessitated the ![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)ion of complex community-centric solutions like MapR or Apache Spark to address scalability issues. However, with the emergence of cloud computing, solutions such as Data Bricks (Delta Lake) have revolutionized Big Data Analytics for SoR systems. Leveraging services like AWS S3, EFS, and RedShift, organizations can now effortlessly establish data warehouses in the cloud, streamlining scalability and management processes.

Moreover, the evolution of technology has transitioned Data Warehouses from traditional warehouses to dynamic data lakes and, more recently, towards the concept of data meshes. This evolution emphasizes the significance of linked data and distributed data mesh architectures in addressing the complexities of modern data management and analytics. By ![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)ing these advanced paradigms, organizations can harness the power of interconnected data ecosystems to drive innovation, efficiency, and insight generation.
- [Distributed Data Meshes](https://martinfowler.com/articles/data-monolith-to-mesh.html)

## Identity and Access Management
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Identity and Access Management (IAM) serves as a comprehensive framework encompassing policies and technologies designed to guarantee that authorized users within an enterprise ecosystem have suitable access to technology resources. Leading IAM solutions such as AWS Cognito, Auth0, Google Account, OAuth2, and Keycloak exemplify effective implementations of this framework. These platforms offer robust features for managing user identities, authentication, and authorization, ensuring seamless and secure access to resources while maintaining strict control over user privileges. By leveraging IAM solutions, organizations can safeguard sensitive data, streamline user access workflows, and fortify their overall cybersecurity posture.

## Graph Databases
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

Graph databases constitute a specialized class of storage solutions finely tuned for executing semantic queries rooted in graph theory concepts such as nodes, edges, and properties. Unlike traditional relational models that rely on strict schemas and data normalization to support ACID transactions, graphs facilitate the exploration of associative data sets without the overhead of costly join operations. Furthermore, they offer greater flexibility by relaxing schema definition requirements.

This storage technology has given rise to various applications, including Knowledge Graphs, Intent Graphs, and Consumption Graphs. Knowledge Graphs are instrumental in advancing search solutions by enabling a deeper understanding of relationships between entities. Intent and Consumption Graphs facilitate real-time consumer analytics, enabling organizations to track user behavior and make immediate decisions based on interactions with user experiences. Through the seamless integration of graph databases, organizations can unlock valuable insights, drive innovation, and enhance user experiences across diverse domains.

## Batch processing
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

When discussing batch processing, our minds often gravitate towards on-premises technologies like Hadoop, Spark, and Flink, which demand extensive infrastructure, dedicated engineering teams, and complex management. However, cloud computing, particularly AWS, has transformed batch processing into lightweight ETL (Extract, Transform, Load) pipelines that operate seamlessly with ad-hoc provisioned infrastructure. AWS Batch stands as a prime example of a service designed to construct and manage batch processing pipelines efficiently in the cloud. This paradigm shift offers organizations the flexibility, scalability, and cost-effectiveness needed to handle batch processing tasks effectively without the burdens associated with traditional on-premises solutions.

## Geographic Information Systems
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

Many applications, particularly those in environmental domains, extensively process spatial data. These applications encompass the capture, storage, analysis, and visualization of geographical information. Horizontally scalable data meshes (warehouses) for GIS are constructed using the Elastic Stack, enabling efficient management and analysis of spatial datasets. AWS Location Services is an off the shelf cloud alternative. Tile38 (https://tile38.com) is an ultra fast geospatial database & geofencing server. It is an in-memory geolocation data store, spatial index, and real time geofence.

For browser-based web applications, tools like LeafletJS and MapBox are commonly employed to visualize geospatial data, providing interactive and intuitive maps. Hybrid applications leverage technologies such as React Native Maps for similar visualization capabilities across different platforms. However, recent experimentation advises me to focus exclusively on Flutter for building cross platform apps (including GIS domain). 

Custom indexing of geospatial data often requires dimension transformation techniques like Z-Curve, GeoHash, or S2 (https://s2geometry.io) geometry. These methodologies facilitate efficient storage and retrieval of spatial information, ensuring optimal performance in spatial data processing applications.

The presence of GIS platforms significantly enhances the capability to develop robust applications. However, it's crucial to resist the temptation of relying solely on a single GIS provider. While major players like MapBox, Here, Google, AWS, and Apple offer valuable tools and services, none of them offers a comprehensive solution. Instead, each provider excels in specific use-cases; for instance, Google is renowned for navigation services, MapBox for its map tiles and customization, Here for GIS expertise, AWS for geofencing capabilities, and Apple for cost-effectiveness. Consequently, effective GIS engineering necessitates an integration effort across multiple vendors, leveraging the strengths of each to create a more versatile and resilient application.

## Zero Trust
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

Zero Trust is a security framework based on the principle of "never trust, always verify." In essence, it assumes that both internal and external threats may exist within a network and mandates strict verification of every user, device, and application attempting to connect to resources, regardless of their location or network environment. Unlike traditional security models that rely on perimeter-based defenses, Zero Trust operates on the premise that trust should not be granted implicitly based on network location or user credentials alone. Instead, it requires continuous authentication and authorization mechanisms, granular access controls, and robust encryption to ensure that only authenticated and authorized entities can access sensitive resources. This approach helps organizations strengthen their security posture, mitigate the risk of data breaches, and protect against insider threats, making Zero Trust a vital framework in today's dynamic and evolving cybersecurity landscape.

## Artificial Intelligence & Machine Learning
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

Artificial Intelligence encompasses various disciplines, including advanced web search engines, recommendation systems, and generative tools. My expertise and interest lies in knowledge engineering, which involves organizing concepts within a domain and their relationships. The goal is to enable AI programs to intelligently answer questions and deduce real-world facts. Formal knowledge representations (ontologies) play a crucial role in content-based indexing and retrieval. An ontology defines the objects, relations, concepts, and properties specific to a domain of knowledge.

Successfully adopting AI necessitates establishing MLOps, short for Machine Learning Operations. MLOps is a paradigm designed to “deploy and maintain machine learning models in production reliably and efficiently”. My objective is to facilitate the creation of machine learning products by leveraging best practices of engineering.

The integration of pre-trained commercial and open-source models has become indispensable in contemporary software engineering, enabling the development of intelligent experiences across various applications. Leveraging established solutions like OpenAI and AWS Bedrock models offers a swift and efficient route to construct and scale generative AI applications. These pre-trained models provide a solid foundation, empowering developers to focus on application-specific functionalities while benefiting from cutting-edge AI capabilities.

Alternatively, for those seeking open-source alternatives, platforms like [Llama 2](https://llama.meta.com/llama2/) and projects like [Kandinsky](https://github.com/ai-forever/Kandinsky-3) offer viable options. These open-source initiatives provide flexibility and transparency, allowing developers to tailor models to their specific needs and contribute to the wider AI community. Whether opting for commercial or open-source solutions, the availability of pre-trained models significantly accelerates the development cycle and enhances the overall intelligence of software systems.

## Vector Databases & Embeddings
![Assess](https://img.shields.io/badge/assess-fbe6a8?style=flat-square)

Vector databases are specifically designed to efficiently store and retrieve vector embeddings, which serve as mathematical representations of data in a high-dimensional space. These embeddings encapsulate various forms of content such as images, text, videos, and more, converting them into fixed-length numerical arrays. At the heart of vector database architecture lies the utilization of AI models for content encoding.

One fundamental indexing technique employed within vector databases is the Hierarchical Navigable Small World (HNSW) algorithm (https://arxiv.org/pdf/1603.09320.pdf), belonging to the class of approximate nearest-neighbor search (ANN) algorithms. This algorithm plays a central role akin to the B+ tree in traditional relational database management systems (RDBMS), facilitating efficient search operations within the vast repositories of vector embeddings. In the Open Source, there are the following solutions https://weaviate.io, https://milvus.io, https://github.com/pgvector/pgvector and https://github.com/facebookresearch/faiss. AWS Aurora for Postgres supports vector search out of the box. 

The capability to conduct similarity searches within massive datasets stands out as a pivotal feature of vector databases. Such databases find extensive utility across various domains, including but not limited to recommendations, content retrieval, semantic search, retrieval-augmented-generation (RAG), classification, and anomaly detection.

Word2vec, Doc2vec, and http://rdf2vec.org represent open-source solutions tailored for calculating embeddings. These methods provide efficient means to transform textual data into dense numerical representations, facilitating various downstream tasks such as semantic analysis, clustering, and recommendation systems.

In addition to these open-source solutions, numerous cloud providers offer commercial-grade embedding services. For instance, offerings such as OpenAI Embeddings and AWS Bedrock Titan Text model are specifically engineered to handle large-scale embedding computations efficiently. These commercial solutions often leverage advanced machine learning models and infrastructure optimizations to deliver robust performance and scalability for diverse applications in natural language processing and beyond.
- [Blazing Fast Text Embedding With Word2Vec in Golang to Power Extensibility of Large Language Models (LLMs)](https://medium.com/@dmkolesnikov/blazing-fast-text-embedding-calculation-with-word2vec-in-golang-to-power-extensibility-of-large-ed05625dea62)
- [adapter over various popular vector embedding interfaces: AWS BedRock, OpenAI, word2vec](https://github.com/kshard/embeddings)

## Distributed Hash Table
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

due commercially available NoSQL
A Distributed Hash Table (DHT) is a type of KeyValue storage system deployed across a distributed environment, enabling efficient data storage and retrieval across multiple nodes. Key implementations of DHT technology include platforms such as AWS DynamoDB, Cassandra, and Riak. These systems distribute data across a network of nodes, allowing for scalable and fault-tolerant data management. By leveraging DHT, organizations can achieve high availability, resilience, and performance in handling large volumes of data in distributed environments.
Dynamo: Amazon’s Highly Available Key-value Store https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf
Robust and Efficient Data Management for a Distributed Hash Table https://pdfs.semanticscholar.org/6862/d2099203e4dcd4627ca2128115b4bd3d2fdb.pdf

## Peer-to-Peer Networking
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

due to L7 protocols (e.g. HTTP/3)
Topology management plays a pivotal role in peer-to-peer networking, ensuring efficient communication and resource utilization among interconnected nodes. Several algorithms have been developed to facilitate effective peer management in such networks. Consistent Hashing, Chord, Kadmelia, and BitTorrent are among the prominent algorithms utilized for this purpose. These algorithms enable nodes to efficiently locate and communicate with one another, contributing to the scalability, robustness, and performance of peer-to-peer networks. Additionally, they help optimize resource allocation and facilitate load balancing, further enhancing the overall efficiency of the network.

## Messaging and Queuing Systems
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

The rise of event processing de-prioritise importance of queueing systems at the data plane.
Queueing systems are essential components in computer systems, protocols, and I/O operations, enabling the implementation of "waiting" lines to manage tasks and requests efficiently. Beyond mere task management, queues also play a crucial role in enabling reliable asynchronous communication through messaging systems. Platforms such as RabbitMQ, eJabberd, and MongooseIM exemplify messaging systems, facilitating the seamless exchange of messages between distributed components. By leveraging queues and messaging systems, organizations can enhance the reliability, scalability, and responsiveness of their systems while promoting decoupling and flexibility in communication architectures. Despite the family of queuing systems having got on-![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square), AWS SQS is still a prominent technology. 

## Embedded Queuing Systems
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Embedded Queueing Systems operate similarly to embedded databases, functioning within the application process itself without the need for external processes. These systems are implemented as linkable libraries, seamlessly integrated into the application's runtime environment. By leveraging this technology, applications can efficiently communicate with peers and orchestrate traffic flows without the overhead of managing separate processes.

Several reference implementations of Embedded Queueing Systems exist, including [ZeroMQ](https://zeromq.org), [nanomsg](https://nanomsg.org), and [Erlang Simple Queue](https://github.com/fogfish/esq). These implementations provide developers with versatile tools for building scalable, resilient, and efficient communication pipelines within their applications. Whether handling inter-process communication, distributed messaging, or asynchronous task coordination, Embedded Queueing Systems offer a lightweight and flexible solution for a wide range of use cases.

## Consumer Search and Discovery
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

The rise of vector databases eliminates traditional search solutions, chatbots powered by LLM are a game changer of search and its experience .
Search and Discovery are two concepts frequently intertwined, yet they serve distinct purposes. Discovery facilitates direct access to product catalogs, offering an implicit contextual search triggered programmatically in response to consumer interaction. On the other hand, Search transforms explicit consumer intent into precise or fuzzy pattern match requirements, facilitating the retrieval of relevant items from the catalog.
Using microservices to power fashion search and discovery | https://engineering.zalando.com/posts/2017/02/using-microservices-to-power-fashion-search-and-discovery.html

## Apache Solr
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Solr and SolrCloud are open-source search engines developed by the Apache community. These powerful tools are widely utilized for building a variety of search solutions across various domains, including but not limited to fashion, forensic investigations, and more. With their robust features and flexibility, Solr and SolrCloud empower developers to create sophisticated search applications tailored to meet diverse needs and requirements.

## Embedded Databases
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Databases typically operate within isolated processes, with applications communicating with them via communication sockets to query or store data. Embedded databases, however, are implemented as libraries linked directly to the same process as the application code. While this reduces communication overhead, it also prohibits the sharing of data between multiple instances of the same application. Embedded databases find utility in standalone applications or serve as storage toolkits in distributed systems, offering lightweight and efficient data management solutions.

Examples of embedded databases include SQLite, LevelDB, RocksDB, Apache Lucene, and ETS, each offering unique features and performance characteristics tailored to different use cases.

## Smart Sockets
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Real-time event processing has substitute needs for advanced smart socket concepts.
Smart Sockets represent an innovative technology designed to abstract Layer 4 (L4) and higher communication protocols through a unified interface, resembling either BSD sockets or message queuing systems. Notably, WebSocket, gRPC, NanoMsg, and ZeroMQ are commonly cited examples of smart sockets. These technologies facilitate efficient and seamless communication between distributed systems, enabling real-time data exchange and interoperability across diverse network environments.

Additionally, smart sockets offer numerous advantages, including enhanced performance, scalability, and flexibility in handling various communication patterns. They empower developers to build robust and resilient applications by providing reliable communication channels and simplifying the integration of complex networking functionalities. With their versatility and ease of use, smart sockets play a pivotal role in modernizing communication infrastructures and driving innovation in distributed systems architecture.

## LiDAR
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Suspended any active development.
LiDAR, short for Light Detection and Ranging, is a cutting-edge technology utilized to create precise 3D representations of physical targets. Its end-to-end data processing pipeline enables the construction of advanced environmental analytics. Particularly prevalent in robotic and autonomous vehicles, including self-driving cars, LiDAR plays a pivotal role in enhancing spatial awareness and navigation capabilities.

In environmental analytics, leveraging LiDAR demands sophisticated pipelines for processing and normalizing raw data. This encompasses tasks such as point cloud storage, indexing, search visualization, and 3D machine learning techniques like feature extraction, segmentation, object recognition, and classification. By harnessing LiDAR technology, organizations can gain invaluable insights into complex environmental landscapes and drive innovation in various fields, from urban planning to natural resource management.

## OpenGL ES
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

OpenGL ES, short for OpenGL for Embedded Systems, is a specialized graphic hardware acceleration technology tailored for embedded platforms, such as smartphones. It serves as a powerful tool for implementing immersive 3D graphics in various applications, notably including 3D games. Additionally, OpenGL ES finds application in creating gamified user experiences, such as interactive social networking platforms reminiscent of Second Life. By leveraging OpenGL ES, developers can unlock the full potential of embedded systems, delivering captivating visual experiences and innovative user interfaces across a range of mobile and embedded devices.
