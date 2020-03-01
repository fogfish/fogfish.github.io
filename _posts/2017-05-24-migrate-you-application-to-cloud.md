---
layout: post
title:  Migrate Your Application To Cloud Age
description: |
  Microservices became a design style to implement applications in the cloud.
  It define system architectures, purify core business concepts, evolve solutions in parallel.
---

# Migrate Your Application To Cloud Age

The talk at Accenture Architecture Tech-Day, 2017, Helsinki

<iframe width="560" height="315" src="https://www.youtube.com/embed/8Ejt9rQcCuc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Microservices in Action

Microservices became a design style to define system architectures, purify core business concepts, evolve solutions in parallel, make things look uniform, and implement stable and consistent interfaces across systems.

There are not common journey toward microservices. It is different for each company. There are no hard and fast solution, only trade offs.

Let's discuss principles for microservice development of stateful solutions. The stateful miscorervices is a hardest area. Fashion search and discovery is a promising field that we are excited to evolve to be a microservice solutions.

Autonomy of development and operation is the key concept behind microservices. What is the role of architect then? If delivery teams work independently they goes toward “death star”. The architect is responsible to prevent it by answering and coaching team on
- System decomposition: Split monolith to swarm of microservices
- Non-functional requirements: scalability, availability and fault tolerance
- Ensure the system and product evolution for business needs

Let me tell you a my story - How to Migrate Application To Cloud Age!


## Fashion Search as Swarm of Microservices

Search is the primary entry point for consumers to interact with Fashion Shop. It is responsible to offer a user experience to capture consumer’s search intent. The intent is then translated into pattern match requirements for the catalog.

The quality of search has direct impact on sales. Our search solution handles millions of search queries per day. The long lasting outages has significant impact on business.

The search user experience in fashion differs from web-style search. “Full width, responsive” search box do not work. We need to inspire consumers who has vague ideas and answers specific consumers intent.

Why is that?


## How to Call a Fashion Things?

The fashion things are called by designers and fashion experts. The average consumer do not know names of fashionable items, how to spell the name properly. Thus, we made an inspiring search user experience.


## Search Consumer Journey

Consumer journey is the basement for decomposition ​once you emphasize important consumer-oriented use cases addressed by the solution. Use cases help us to build functional boundaries as an initial step in system design and decomposition. This sounds as statement from SOA book.

Let’s consider an example of search consumer journey. It is used by us to build the search experience:

<dl>
   <dt>Discovery</dt>
   <dd>provides direct access to product catalogs, using the category tree as the primary entry point.</dd>

   <dt>Search</dt>
   <dd>matches documents from fashion catalogs using consumer defined keywords.</dd>

   <dt>Refinement</dt>
   <dd>is a process to narrow down a large set of search hits using filters and facets, also known as faceted navigation.</dd>

   <dt>Relevance sorting</dt>
   <dd>provides a collection of algorithms to rank search results depending on the weight of certain content aspects.</dd>

   <dt>Intent analysis</dt>
   <dd>transforms unstructured consumer intent (e.g. keywords, visual search) into semi-structured queries using feature extraction techniques.</dd>

   <dt>Personalization​</dt>
   <dd>is required to efficiently respond to consumer intent.</dd>
</dl>

The enterprise architecture might see this search solution as monolith due to historical reason. Search has not been ever a primary business in enterprise domain. It uses vendor appliances, integrations with 3rd party providers and SaaS solutions.

Some Executives has similar opinion, they see search as feature ~ single monolithic service.

“The truth is out there”

Search is swarm of microservice... The swarm of stateful microservices... The hardest swarm...

There are no common blueprint on microservice decomposition. You usually cut microservice by functional boundaries, ignoring the aspects of technology.

This sounds as “dirty thing” for traditional architecture but you do not have a middleware. A common technology might supports two use-cases “discovery” and “search” but we split them into independent microservices.


## Domain Driven Design

The data is hardest problem when designing microservices. We need to build a microservice as collection of stateless processes, backing services and admin processes (using 12 factor terminology). You might find this as ​recurring pattern if you design stateful swarms.

Layers helps us define operational boundaries, leveraging issues of stateless/stateful service delivery and operation.

The storage layer is a set of stateful components (backing service) that hold your state. It is built around off-the-shelf open source technologies. The usage of cloud value-added services (e.g AWS) helps us streamline operational processes and improve data durability and service availability. The availability of the storage layer has a high impact on customer experience and sales.

The storage layer is not sharable database to swarm. Each microservice uses it own data domain. The storage ensures autonomous team operation and decision making.

The development of stateful microservices requires a data driven design. You need to reason about you data and design storage around it. The usage of consumer journey and understanding of business value helps you to draw boundaries around data and its transactions. It is required to identify a smallest unit of atomicity use by microservice and materialize into design of storage layer.

Do not expect 100% data consistency within your swarm because of CAP theorem. Use asynchronous and event-based techniques for data distribution. Strong Eventual Consistency is a good target.

As an example, we have used consumer journey as an input to identify our search data boundaries. We derived them from our use-cases and business criteria.

The **intake layer** is a traditional layer to ​[extract, transform, and load processes](https://en.wikipedia.org/wiki/Extract,_transform,_load) ​– we call it an event-driven content integration pipe. It is used to ​extract content from a heterogeneous source using both synchronous and asynchronous communication patterns. Usually, it reads content either from ground truth business sources hosted by the e-commerce platform or content extensions driven by a consumer facing application. The content ​transformation stage is a series of purely functional operations that join data from multiple sources and prepare the smallest unit of atomicity required by search use cases. The content is asynchronously loaded​ to microservices at the storage layer, enabling article discovery.

The **business logic layer** is built from consumer-oriented microservices that delivery a search experience (use-cases). It solely focuses on the consumer journey as collection of value added services. The layer is a pure, stateless microservice adjusted for execution of a “single” use case.


## Interfaces

The successful development of a large system using microservice patterns requires the definition of interfaces between functional elements. We learn that traditional REST principles do not suit each microservice involved in large swarms. We have made a choice to use different API technology according to our needs. Let’s elaborate on them.

<dl>
   <dt>REST</dt>
   <dd>is a standard of communication between microservices using REST principles. The communication pattern follows typical client-server interaction over HTTP protocol, leveraging JSON as the primary payload.</dd>

   <dt>QUEUE</dt>
   <dd>is an asynchronous communication protocol that involves message-oriented technologies. This interface indicates a publisher-subscriber communication paradigm between components. The implementation technique might vary from plain socket to high-level client library.</dd>

   <dt>KEY/VALUE</dt>
   <dd>is an interface for storing, retrieving, and managing associative arrays (hashmap). The interface provides access to a collection of binary objects a.k.a blobs​. These blobs are stored and retrieved using a unique identifier (key).</dd>

   <dt>QUERY LANG</dt>
   <dd​>is a vendor dependent interface used for data retrieval purposes.</dd​>

   <dt>LOG</dt>
   <dd>is a stream oriented event publishing interface.</dd>
</dl>


## Common Data Model

The Common Data Model defines generic vocabulary terms of layout content into generic structures. This model guarantees an interoperability baseline between microservices, in the absence of strong content negotiation techniques. The interpretation of data semantics allows us to build solutions that are isolated from the evolution of the individual data models. Simultaneously, it is able to aggregate and interleave content from various independent sources.

#### Interoperability​

Consumer facing applications are key stakeholders for our search solution. The major concern is compatibility and interoperability of interfaces as well as data contracts supplied by the search solution. Core interfaces are built around a common data model that follows common metadata and provides the baseline for interoperability between various microservices and its evolution.

#### Evolution​

As fashion metadata is distributed, our dynamic environment operates with multiple content sources: Product metadata, availability information, partner content. The solution should ensure the continuity of data access to consumer facing applications associated with old versions of data contracts. Simultaneously, it should not prevent the evolution of these contracts to cover new metadata aspects. The evolution must ensure the development of search architecture that allows for improvements of the indexing algorithm and technological adaptation.

#### Consistency​

The smallest unit of atomicity is a snippet and attributes - the snippet is an opaque data structure used by frontend applications to visualize search results; attributes facilitate discovery and refinement processes. This ensures we follow domain driven design. We cannot guarantee 100% consistency of product metadata within a system-of-records or search indexes due to ​[natural limitations of distributed systems​](https://en.wikipedia.org/wiki/CAP_theorem).


## Non functional requirements
Non-functional requirements (NFR) are set of constraints that are used to assert system design. These requirements are detailed during system design and architecture phases. They are technology initiatives necessary to evolve solution to support current and future business needs.

#### Scalability​

The scalability is the system’s ability to handle the required amount of work and its potential to be enlarged to accommodate growing traffic and data. The technology should not be a limiting factor for business growth or limit integration of new data sources. The horizontal scalability of the storage layer is a major consideration for architecture decisions. Interface scalability is achieved by using state of the art principles of microservice design.

#### Latency​

Software development practices consider end-to-end latency as a user-oriented characteristic during the entire lifecycle of an application. This user-oriented metric shall be defined independently of underlying solutions or technologies and will be used for quality assessment of the delivered solution. The interactive traffic is the classical data communication scheme employed by mobile search. This pattern gives a recommendation that interaction between human and remote equipment should not exceed 1 second. It makes an implication on the latency of individual microservice. Often, the latency of consumer facing interfaces should not exceed 100 ms due to ​latency challenges in microservices​. The system design considers infrastructure, used protocols, and other microservices.

#### Availability​

Availability here concerns data availability and durability. The availability concerns search-and-discovery interfaces, as it defines uptime requirements on system external interfaces. Durability reflects the risks of losing data during maintenance or downtimes. Durability is the ultimate requirement for storage solutions built from event-based data sources.


## Immutable deployment

The immutable deployment answers aspects of availability and fault tolerance due to software regression.

Never change your infrastructure and application configuration at running systems. Deploy a new copy as parallel stack.

The microservice configuration (including infrastructure) shall be scriptable and reproducible using automated deployment.

You need to deliver “Infrastructures as a Code”.

As the result, you’ll get a “time machine” to rollback defective software in matter of minutes. Another advantage is the ability to split traffic between parallel deployments for quality assurance. Finally, you’ll get rid of staging environment for read-only services.

There is a challenge of this approach. It requires a solid (proven) tooling to address deployment. This option exposes a higher costs at beginning. There are no straightforward techniques to apply immutable deployment for storage services (thus we isolate into own layer).

We built the immutable deployment with help of AWS Cloud Formation, Route 53, Elastic Load Balancing and custom tooling to orchestrate bits and pieces.

## Graceful degradation

Graceful degradation is about of making users aware of stability issues but ensure general usability of the product.

Rule of thumb: disable feature / use-case but keep system (user experience) available. Provide an alternative version of functionality.

It requires careful system decomposition on feature levels and isolation of these feature.

Here is an example of feature decomposition in search.
* Faceting is isolated from search.
* The search consists of query planning (advanced feature with NLP) and basic
structured search.
* Caching of data backs up temporary unavailability of storage.
* Personalized sorting is independent component that handles post processing of
search results.

## Black Friday

The Black Friday is an exam of your works. The sustainability of business during this day depends on decision you drive as part of you microservice development.

