---
layout: default
title: Distributed Systems
parent: Technologies
grand_parent: Technology Radar
nav_order: 2
description: |
  Distributed Systems  
---

## Distributed Systems

[PACELC: Consistency Tradeoffs in Modern Distributed Database System Design](http://www.cs.umd.edu/~abadi/papers/abadi-pacelc.pdf) In theoretical computer science, the PACELC theorem is an extension to the CAP theorem. It states that in case of network partitioning (P) in a distributed computer system, one has to choose between availability (A) and consistency (C) (as per the CAP theorem), but else (E), even when the system is running normally in the absence of partitions, one has to choose between latency (L) and consistency (C).

[CAP Twelve Years Later: How the "Rules" Have Changed](https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed) - The CAP theorem asserts that any net­worked shared-data system can have only two of three desirable properties. Aspects of the CAP theorem are often misunderstood, particularly the scope of availability and consistency, which can lead to undesirable results. Articles discusses these misleading aspects.

[Base: An Acid Alternative](https://queue.acm.org/detail.cfm?id=1394128) - In partitioned databases, trading some consistency for availability can lead to dramatic improvements in scalability. It discusses an applicability of persistent message queues to achieve eventual consistency and recovery from failures.

[Peer-to-Peer Communication Across Network Address Translators](https://bford.info/pub/net/p2pnat/index.html) - Network Address Translation (NAT) causes well-known difficulties for peer-to-peer (P2P) communication, since the peers involved may not be reachable at any globally valid IP address. Several NAT traversal techniques are known, but their documentation is slim, and data about their robustness or relative merits is slimmer. This paper documents and analyzes one of the simplest but most robust and practical NAT traversal techniques, commonly known as “hole punching.”

[Command Query Responsibility Segregation (CQRS) pattern](https://martinfowler.com/bliki/CQRS.html) - The technique defines an alternative approach to CURD. At its heart is the notion that you can use a different model to update information than the model you use to read information. CQRS patterns simplifies design, scalability and fault-tolerance of write stream especially when it accomplished by Event Sourcing (streaming). It impacts on the reader's consistency which requires special caching techniques if pattern is used in Web application. 

[Exposing CQRS Through a RESTful API](https://www.infoq.com/articles/rest-api-on-cqrs/) - Exposing a CQRS service through a REST API is not only possible but the richness of HTTP semantics allows for a fluent and efficient API to be built on top. This process involves building a public domain composed of commands, queries (input/output messages) and resources that are concurrency and caching aware. Also we need to map internal domain(s queries and commands to HTTP verbs and use status codes to convey state transitions and exceptions 

[Turning the database inside-out with Apache Samza](https://www.confluent.io/blog/turning-the-database-inside-out-with-apache-samza/) - think of a database as an always-growing collection of immutable facts. You can query it at some point in time — but that’s still old, imperative style thinking. A more fruitful approach is to take the streams of facts as they come in, and functionally process them in real-time.

[Robust and Efficient Data Management for a Distributed Hash Table](https://pdfs.semanticscholar.org/6862/d2099203e4dcd4627ca2128115b4bd3d2fdb.pdf) - design and implementation of the distributed hash table based on erasure encoding. This design is both more robust and more efficient than the previous replication-based implementation.

[Kadmelia: A Peer-to-Peer Information System Based on the XOR metric](https://pdos.csail.mit.edu/~petar/papers/maymounkov-kademlia-lncs.pdf) - peer-to-peer DHT with consistency, fault tolerance. Uses XOR-based metric topology.  

[Don’t Build a Distributed Monolith](https://www.microservices.com/talks/dont-build-a-distributed-monolith/) tech talk about the system design principles using microservice pattern. 
