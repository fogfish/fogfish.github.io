---
layout: post
title:  Real-time consumer engagement
---

# Real-time consumer engagement

The age of digital content consumption requires efficiently respond to consumer demand. There are needs to engage and create relevant experience for each individual consumer. The interactive marketing is not just about BigData, it is all about knowing consumers, getting to known consumer preferences and emerging trends from both consumer, retailer and content provider. A high resolution, real-time consumer engagement is an opportunity to enhance traditional ETL analytics portfolio.

The successful implementation of real-time consumer engagement depends on the ability to access consumer behavior, deduct relevant knowledge and accumulate consumer profile. The consumer profile leverages individual offering from "visit our store" to "personalized discounts" or "coupons".

The collaborative (social) filtering is a popular technique for making recommendations. In collaborative filtering we use the power of community to deduct knowledge about products (it based on segmentation and data clustering techniques). Despite many advantages, the recommendation systems based on collaborative filtering tends to recommend already popular products. As an extreme case, the individual preferences of consumers are degraded. The classification based on consumer attribution bypasses this issue.

The development of a consumer-centric retail offering becomes easy when consumer behavior mapped to discrete signal of called feature vectors, marketologs calls it consumer  DNA. DNA enables delivery of relevant communication; boost conversion rates and ensure personalized experience.

High-resolution consumer engagement traces consumer behavior while he/she interacts with digital user experience. The state-of-the-art log management solution is used for traditional long-term, off-line analytics. The interactive analytic immediately streams consumer behavior using communication sockets. The atomic knowledge facts about consumer behavior map digital footprints to construct consumer genome. The consumer genome is a collection of attributes to depict consumer behavior using dimension and decay properties in discrete manner. It empowers with incisive insights on various facets of the consumer.

The efficient implementation of consumer profile requires an advanced knowledge management system. The complexity of knowledge relation and variety of knowledge sources demands extensibility, interoperability and maintainability despite other non-functional requirements.

The development of consumer DNA portfolio requires no impact on internal data structure and deployed data flows. The further expansion of knowledge sources requires minimal integration effort with zero impact on existing relations. The consumer knowledge management system requires the ability to absorb knowledge from external sources (data enrichment) while offering uniform knowledge exchange protocol to internal components. The architectural design of knowledge management system facilitates the isolation of defects on facts and knowledge sources. The atomic repair principle is mandatory where the faulty replace without spread of side effects. The maintenance of knowledge source and data processing performed by independently each other.

The technology choice that require complex and expensive integration impact on time-to-market. The utilization of relational technologies with explicit schema management delays an adaptation on increased knowledge velocity and fragmentation; it makes incorporation of consumer insights into interactive marketing a time-consuming process.

The inflexibility of relation technology is sole based on they methods of knowledge management, which is represented as n-ary tuple. Each tuple accumulates multiple facts about the subject. On the other hand, the nature of knowledge facts is binary relation. The subject knowledge is a collection of ordered pairs of elements arbitrary sets (or classes) that form a knowledge graph.

In contrast to n-ary schema, the binary relation literally allow to maintain knowledge facts independently each other. The knowledge management system augments these facts with probability and time dimension with less cost than relational system.

The independence property is vital for real-time consumer engagement. It allows to decompose the intake procedure on independent data flows and parallelize knowledge management and discovery. The consumer inside storage uses graph technology to persist knowledge instead of relation technology. The n-ary relations are materialized at query time to address a business logic needs. This addresses extensibility, interoperability and maintainability.

The discussed solution maps to high-level stream processing architecture that is built from few side-effect free stages: streaming part ingests events, enrichment part augments events with additional knowledge and deducts part that build a genome from event and prior knowledge.  The core part of this solution is a semantical storage that holds consumer behavior and data integration interface Datalog. The proof-of-concept delivery shows the storage and query language in actions.





