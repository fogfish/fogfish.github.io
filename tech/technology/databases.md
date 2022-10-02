---
layout: default
title: Databases
parent: Technologies
grand_parent: Technology Radar
nav_order: 1
description: |
  Foundation of Database
---

# Databases

[ActorDB – Distributed SQL database](https://news.ycombinator.com/item?id=17331904) Hacker News discussion about the open source product.


## Stream

[A theory of stream queries](https://www.microsoft.com/en-us/research/wp-content/uploads/2017/01/189.pdf) Data streams are modeled as infinite or finite sequences of data elements coming from an arbitrary but fixed universe. Stream queries are modeled as functions from streams to streams. Issues investigated include abstract definitions of computability of stream queries; the connection between abstract computability, continuity, monotonicity, and non-blocking operators; and bounded memory computability of stream queries using abstract state machines (ASMs).

[Why query planning for streaming systems is hard](https://www.scattered-thoughts.net/writing/why-query-planning-for-streaming-systems-is-hard) Many groups are working on running sql queries in incremental/streaming systems. Query planning in this context is not a well understood problem. This post is a quick pointer towards the many open problems.

[Streams](https://srfi.schemers.org/srfi-41/srfi-41.html) Streams, sometimes called lazy lists, are a sequential data structure containing elements computed only on demand. A stream is either null or is a pair with a stream in its cdr. Since elements of a stream are computed only when accessed, streams can be infinite. Once computed, the value of a stream element is cached in case it is needed again.

## Graph

[Hexastore: Sextuple Indexing for Semantic Web Data Management](http://karras.rutgers.edu/hexastore.pdf) Despite the intense interest towards realizing the Semantic Web vision, most existing RDF data management schemes are constrained in terms of efficiency and scalability. In this paper, we propose an RDF storage scheme that uses the triple nature of RDF as an as-set. RDF data is indexed in six possible ways, one for each possible ordering of the three RDF elements.

