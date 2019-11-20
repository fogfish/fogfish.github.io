---
layout: default
title: Techniques
parent: Technology Radar
nav_order: 2
description: |
  Software development techniques, theories and fundamental aspects of computer science.
---

# Techniques

## Distributed System


#### Assess

[CAP Twelve Years Later: How the "Rules" Have Changed](https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed) - The CAP theorem asserts that any net­worked shared-data system can have only two of three desirable properties. Aspects of the CAP theorem are often misunderstood, particularly the scope of availability and consistency, which can lead to undesirable results. Articles discusses these misleading aspects.

[Base: An Acid Alternative](https://queue.acm.org/detail.cfm?id=1394128) - In partitioned databases, trading some consistency for availability can lead to dramatic improvements in scalability. It discusses an applicability of persistent message queues to achieve eventual consistency and recovery from failures.

[Peer-to-Peer Communication Across Network Address Translators](https://bford.info/pub/net/p2pnat/index.html) - Network Address Translation (NAT) causes well-known difficulties for peer-to-peer (P2P) communication, since the peers involved may not be reachable at any globally valid IP address. Several NAT traversal techniques are known, but their documentation is slim, and data about their robustness or relative merits is slimmer. This paper documents and analyzes one of the simplest but most robust and practical NAT traversal techniques, commonly known as “hole punching.”

#### Adopt

[Command Query Responsibility Segregation (CQRS) pattern](https://martinfowler.com/bliki/CQRS.html) - The technique defines an alternative approach to CURD. At its heart is the notion that you can use a different model to update information than the model you use to read information. CQRS patterns simplifies design, scalability and fault-tolerance of write stream especially when it accomplished by Event Sourcing (streaming). It impacts on the reader's consistency which requires special caching techniques if pattern is used in Web application. 

[Exposing CQRS Through a RESTful API](https://www.infoq.com/articles/rest-api-on-cqrs/) - Exposing a CQRS service through a REST API is not only possible but the richness of HTTP semantics allows for a fluent and efficient API to be built on top. This process involves building a public domain composed of commands, queries (input/output messages) and resources that are concurrency and caching aware. Also we need to map internal domain(s queries and commands to HTTP verbs and use status codes to convey state transitions and exceptions 

[Turning the database inside-out with Apache Samza](https://www.confluent.io/blog/turning-the-database-inside-out-with-apache-samza/) - think of a database as an always-growing collection of immutable facts. You can query it at some point in time — but that’s still old, imperative style thinking. A more fruitful approach is to take the streams of facts as they come in, and functionally process them in real-time.


## Machine Learning

#### Assess

[MIT Deep Learning Book](https://github.com/janishar/mit-deep-learning-book-pdf) - This is the most comprehensive book available on the deep learning. Written by three experts in the field, Deep Learning is the only comprehensive book on the subject.

[Breaking CAPTCHA Using Machine Learning in 0.05 Seconds](https://medium.com/towards-artificial-intelligence/breaking-captcha-using-machine-learning-in-0-05-seconds-9feefb997694) - technique uses GAN networks to synthesize process of CAPTCHA generation. THe solution is extremely interesting for other problem domains with absence of large datasets. 

#### Adopt

[The Rise of Generative Adversarial Networks](https://blog.usejournal.com/the-rise-of-generative-adversarial-networks-be52d424e517) - Overview of GAN networks. They can generate high-quality images, enhance photos, generate images from text, convert images from one domain to another, change the appearance of the face image as age progresses and many more. The list is endless.


## User Experience

#### Adopt

[Design Better Forms](https://uxdesign.cc/design-better-forms-96fadca0f49c) - Highlights a common mistakes designer makes on Web Forms and How to avoid them.

[Alternatives to Placeholder Text](https://uxdesign.cc/alternatives-to-placeholder-text-13f430abc56f) - Improve form usability by addressing the perils of placeholders. 

[4 Rules for Intuitive UX](https://learnui.design/blog/4-rules-intuitive-ux.html) - advice on improving the UX of your designs WITHOUT hours of user research sessions, paper prototyping playtime, or any other trendy UX buzzwords.

[5 способов найти фото для блога без нарушения авторских прав](https://zen.yandex.ru/media/id/5a940b5f3c50f7398387469d/5-sposobov-naiti-foto-dlia-bloga-bez-narusheniia-avtorskih-prav-5ae08a345f4967204194b6e3) - Поиск иллюстраций для статьи или короткого поста — постоянная головная боль блогера. Проверенные сервисы для поиска изображений и другие легальные источники картинок.

## Security

#### Assess

[Git Secret Scanner](https://shhgit.darkport.co.uk) - Analysis of GitHub, GitLab and BitBucket reports for accedential security commits.
