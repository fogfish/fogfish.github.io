# Techniques and Theories

These include elements of a software engineering process, such as theories (e.g. queueing, functional programming), architecture practices and patterns, experience design, etc.

- [Techniques and Theories](#techniques-and-theories)
  - [Functional, Typesafe Programming](#functional-typesafe-programming)
  - [Protocols Engineering](#protocols-engineering)
  - [Distributed Systems](#distributed-systems)
  - [Event-driven architectures](#event-driven-architectures)
  - [Highly Scalable Systems](#highly-scalable-systems)
  - [Fault Tolerant Systems](#fault-tolerant-systems)
  - [Security and Privacy of Internet Services](#security-and-privacy-of-internet-services)
  - [Microservices](#microservices)
  - [Infrastructure as a Code](#infrastructure-as-a-code)
  - [“Everything is Continuous”](#everything-is-continuous)
  - [Open Source Software](#open-source-software)
  - [Technical Hiring](#technical-hiring)
  - [Test Driven Development](#test-driven-development)
  - [Service Design \& Agile Methodology](#service-design--agile-methodology)
  - [Procurement and Technology acquisition](#procurement-and-technology-acquisition)
  - [User Experience](#user-experience)
  - [Observability](#observability)
  - [Linked Data](#linked-data)
  - [Well Architected Review](#well-architected-review)
  - [“Code as Crime Scene”](#code-as-crime-scene)
  - [Semantic, Knowledge and Ontology](#semantic-knowledge-and-ontology)
  - [Data Engineering](#data-engineering)
  - [Teletraffic \& Performance Engineering](#teletraffic--performance-engineering)
  - [Software as a Service](#software-as-a-service)


## Functional, Typesafe Programming
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Functional Programming is a declarative style of software development that relies on side-effect-free functions to articulate solutions within the problem domain. Its foundational principles include first-class and higher-order functions, along with immutability. Additionally, composition plays a pivotal role in functional programming, enabling the construction of new components from smaller, reusable elements. Remarkably, this approach is not confined to purely functional languages but can also be effectively applied in imperative ones. Scala serves as a prime illustration, leveraging an imperative runtime while offering data structure implementations that internally safeguard against mutation.

Static type compilers serve as a robust defense against type errors and the detection of undesirable program behavior during compilation. Through the utilization of type classes abstractions and higher-order types, developers can formalize the core domain in algebraic terms, providing a means to "prove" the correctness of algorithms and thus ensure type safety throughout implementation.

## Protocols Engineering
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Protocols stand as pivotal components in constructing interoperable distributed systems. They establish the essential communication frameworks necessary for seamless interaction between disparate entities. Among the plethora of protocols imperative to distributed systems are the likes of IP, TCP, UDP, TLS, HTTP/1, HTTP/2, HTTP/3, WebSocket, OAuth2, MQTT, and CDN, each serving specific functions tailored to diverse network requirements.

In addition to their functional attributes, non-functional aspects such as latency play a crucial role in protocol design, particularly in the context of heterogeneous wireless networks. Ensuring efficient transmission and reception of data across varying network conditions demands meticulous consideration of latency, packet loss, and bandwidth constraints. The adaptability of protocols to accommodate these challenges significantly influences the overall performance and reliability of distributed systems, thereby emphasizing the paramount importance of incorporating non-functional aspects into protocol design methodologies.

## Distributed Systems
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Addressing consensus challenges within a network of unreliable processors stands as a fundamental hurdle in distributed computing. Protocols such as Paxos and Raft have emerged as cornerstone solutions tailored to tackle coordination and synchronization obstacles inherent in distributed systems. These protocols facilitate the establishment of consensus, ensuring coherence among distributed nodes despite potential failures or discrepancies.

Complementing traditional consensus mechanisms, Conflict-Free Replicated Data Types (CRDTs) offer a novel approach to achieving strong eventual consistency without the need for explicit synchronization. Mathematically proven to resolve concurrent updates across disparate replicas, CRDTs provide a robust foundation for maintaining data integrity in distributed environments.

The absence of a global clock poses a significant challenge in distributed systems. Approaches such as Lamport and Vector clocks employ partial ordering to navigate temporal discrepancies and establish a coherent sequence of events across distributed nodes. Additionally, innovative techniques like Twitter Snowflake and k-ordered numbers offer decentralized event ordering, enabling efficient coordination and synchronization without relying on a central authority.

By leveraging a diverse array of consensus protocols and synchronization mechanisms, distributed systems can overcome the inherent complexities of unreliable processors and ensure robustness, coherence, and consistency across networked environments.

- [CRDT in Erlang](https://github.com/fogfish/crdts)
- [K-ordered unique identifiers in lock-free and decentralized manner](https://github.com/fogfish/guid)
- [Consistent hashing data structure](https://github.com/fogfish/ring)

## Event-driven architectures
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Event-driven architecture (EDA) represents a software architecture paradigm focused on the generation, propagation, and handling of events, as well as the underlying principles governing their processing. Central to EDA is the concept of asynchronous processing, which serves as a fundamental abstraction necessary for designing resilient and scalable systems. By decoupling components and enabling non-blocking communication between them, asynchronous processing facilitates responsiveness and fault tolerance, making it indispensable in modern system designs.

Additionally, event-driven architectures empower systems to respond dynamically to changing conditions and user interactions, enabling real-time processing and decision-making. This adaptability is achieved through the continuous flow of events, which trigger actions and workflows in a loosely coupled and decentralized manner.

In essence, embracing event-driven architecture enables organizations to build agile, scalable, and responsive systems that can effectively handle the complexities of modern applications and meet the evolving demands of users and stakeholders.

- [Go channels for distributed queueing and event-driven systems](https://github.com/fogfish/swarm)

## Highly Scalable Systems
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Scalability is commonly defined as "the property of a system to handle a growing amount of work by adding resources to the system." This essential characteristic underpins the engineering of systems that can dynamically accommodate increasing workloads while maintaining performance and availability. Achieving scalability involves the design and implementation of horizontally scalable architectures, which facilitate the seamless addition of resources to meet growing demands. Additionally, ensuring high availability is paramount, necessitating robust redundancy and fault tolerance mechanisms.

Central to scalability is the practice of workload decomposition, wherein complex tasks are partitioned into smaller, manageable units that can be distributed across multiple nodes or resources. This approach enables efficient parallel processing and resource utilization, thereby enhancing system performance and responsiveness.

Traffic shaping also plays a crucial role in scalability, allowing organizations to regulate the flow of data and requests within the system. By implementing traffic shaping techniques such as rate limiting and prioritization, service providers can prevent bottlenecks and ensure equitable resource allocation, even under heavy loads.

Furthermore, the principles of scalable design and validation must permeate the daily activities of service teams. Continuous monitoring, performance testing, and capacity planning are essential practices that ensure the scalability and resilience of distributed systems. By adhering to these principles, organizations can effectively address the evolving needs of their users and adapt to changing market conditions while maintaining optimal performance and reliability.

## Fault Tolerant Systems
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Software systems inevitably encounter failures over time; it's an inherent aspect of their operation. These failures often manifest inconsistently, and observers typically acquire imperfect information about their occurrence. Such failures are categorized as Byzantine faults, presenting significant challenges to system reliability and performance.

Fault tolerance is the measure of a computer system's ability to operate effectively and maintain functionality in the face of these adversities. It encompasses a range of techniques and strategies designed to mitigate the impact of failures and ensure system resilience. These measures include redundancy, error detection and correction mechanisms, graceful degradation, and failover capabilities, among others.

In essence, fault tolerance is a critical aspect of system design and engineering, particularly in mission-critical environments where system downtime or failure can have severe consequences. By implementing robust fault tolerance mechanisms, software engineers can enhance system dependability and minimize disruptions caused by unforeseen failures.

## Security and Privacy of Internet Services
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

The increasing adoption of SaaS as an operating model has underscored the paramount importance of prioritizing security and privacy throughout the development process. Cybersecurity is pivotal in mitigating a multitude of threats, including data breaches, theft, and privacy violations. Fortunately, a dedicated cadre of security experts continuously assess and address these risks. They have devised robust controls and benchmark protocols to safeguard against these challenges.

However, leveraging these tools often demands specialized knowledge and effort from software engineers. Compliance with mandatory controls such as those outlined by the Center for Internet Security (CIS) and the General Data Protection Regulation (GDPR) is imperative. Furthermore, integrating security threat automated scanning tools into the deployment pipeline has become a standard practice. These tools serve as proactive measures, running automatically to detect and address vulnerabilities throughout the development lifecycle.

## Microservices
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Microservices have evolved into a foundational design approach for shaping modern system architectures. They offer a methodical framework to distill and encapsulate core business concepts, enabling the evolution of solutions in parallel while promoting uniformity and consistency across the entire system. This architectural paradigm proves particularly advantageous for large enterprises, empowering them to decompose intricate systems into manageable and cohesive units. This facilitates parallel evolutions and enhances scalability, enabling large companies to navigate complex software landscapes with agility and efficiency.

Conversely, for smaller organizations, microservices provide the agility to pivot swiftly and efficiently dispose of components as needed. This flexibility enables small businesses to adapt quickly to changing market dynamics and seize emerging opportunities with nimbleness.

Effective utilization of microservice patterns demands a holistic consideration of various factors. Domain-driven design principles are paramount for defining clear service boundaries aligned with business objectives. Establishing a common data model fosters interoperability and facilitates seamless communication between microservices. Thoughtful interface design is crucial for defining stable and consistent interfaces that facilitate integration and maintainability.

Furthermore, attention to non-functional requirements such as scalability, reliability, and security is essential. Practices such as immutable deployments, where services are deployed as self-contained units, enhance deployment agility and reduce the risk of configuration drift or unintended side effects.

In essence, microservices represent a transformative approach to software architecture, empowering organizations of all sizes to build resilient, scalable, and agile systems. By embracing domain-driven design, adhering to common data models, and prioritizing non-functional requirements, businesses can unlock the full potential of microservices to drive innovation and deliver value to their customers.

- [Migrate Your Application To Cloud Age](https://tech.fog.fish/2017/05/24/migrate-you-application-to-cloud.html)
- [Microservices](https://martinfowler.com/articles/microservices.html)

## Infrastructure as a Code
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

The paradigm of strictly implying that everything is pure code, developed with a general-purpose programming language, is pivotal in modern infrastructure management. In this approach, any changes to the infrastructure must be defined within the code itself. Declarative Infrastructure as Code (IaC) describes the desired state of the infrastructure without explicitly defining the commands needed to achieve it. Developers utilize generic programming languages to articulate target configurations, which are subsequently materialized by the IaC engine.

The adoption of tools like AWS CDK with TypeScript exemplifies how infrastructure development can be elevated to a typesafe, purely functional, and higher-order process. Leveraging these technologies, developers can design cloud components with increased confidence, ensuring reliability and scalability while maintaining code quality and consistency.

In the realm of software engineering, continuous delivery pipelines have become mainstream, serving as a cornerstone for efficient development workflows. Extending these principles to DevOps represents a natural progression, enabling organizations to seamlessly integrate development and operations activities. Continuous Integration/Continuous Deployment (CI/CD) pipelines for IaC play a crucial role in ensuring quality validation and automation during environment provisioning. By automating the testing and deployment of infrastructure changes, CI/CD pipelines streamline development processes, minimize errors, and accelerate time-to-market, ultimately driving greater agility and innovation within organizations.

IaC enables us with Immutable deployment to address critical aspects of availability and fault tolerance by mitigating the risks associated with software regression. The key principle is to refrain from altering infrastructure or application configurations on running systems. Instead, deploying a new copy as a parallel stack ensures stability and resilience, minimizing the impact of potential failures and downtime.

## “Everything is Continuous”
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Continuous Integration, Continuous Delivery, and Continuous Deployment are the cornerstones of modern software development. The ethos of "Everything is Continuous" epitomizes the philosophy and dedication to maintaining code and services in a perpetually release-ready state. This approach entails the implementation of pipelines to automatically deploy every commit to a feature sandbox environment, with subsequent promotion to production.

Major players in enabling these practices within cloud-native environments include GitHub Actions and AWS CodeBuild. These platforms facilitate seamless integration and automation of CI/CD pipelines, empowering teams to streamline development processes, accelerate delivery cycles, and ensure the reliability and scalability of their software solutions.

## Open Source Software
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Open Source development and contribution to communities.

## Technical Hiring
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Practices to attract techy talents into companies and processes to validate their skills-and-abilities.

## Test Driven Development
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

KISS ("keep it simple, stupid") is a guiding principle that, when coupled with test-driven development (TDD), serves as a powerful technique for maintaining control over the quality, addressing regressions, and ensuring the maintainability of software products.

## Service Design & Agile Methodology
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

In the rapidly evolving landscape of software engineering, delivering accessible and user-friendly services presents a significant challenge. Service Design encompasses a collection of engineering patterns that span across cloud infrastructure, databases, and protocols. These patterns are meticulously crafted to address the complexities of modern software systems.

Service design processes guide the entire product lifecycle, employing agile methodologies to adapt to changing requirements and facilitate greenfield development. By leveraging these processes, software engineers can navigate the dynamic nature of their projects and ensure the effective delivery of accessible and usable services to users.

Continuing on the theme of service design, Continuous Delivery involves the iterative delivery of small increments, fostering continuous learning and adaptation to customer demand. This approach emphasizes the continuous evolution of products and processes, embracing the principles of "everything" and "everywhere."

In the realm of Agile development, the Kanban style stands out as particularly well-suited for service teams. With its emphasis on real-time communication, full transparency regarding units of work, and a philosophy that acknowledges the potential for change at any moment, Kanban provides an ideal framework for navigating the dynamic nature of service-oriented projects within the context of service design.

## Procurement and Technology acquisition
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Collection of processes to make a selection and on-boarding the technology into the company's portfolio from external sources. It involves licensing, acquiring, technology deep-dives, proof-of-concepts and other tech study activities.

## User Experience
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Full-stack polyglot service developers are expected to possess the proficiency to craft and simulate User Experiences effectively. A deep understanding of usability principles not only enhances the development process but also ensures the creation of intuitive and user-friendly internal and consumer-facing applications.

In the realm of software engineering, the command line remains a ubiquitous and indispensable tool. It holds unparalleled significance and familiarity among developers. Thus, it is imperative that we strive to optimize its functionality and ease of access, maximizing its utility and empowering developers to accomplish tasks efficiently.

- [Command Line Interface Guidelines](https://clig.dev)
- [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines)

## Observability
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

Observability, rooted in control theory, is a fundamental property of a system that gauges its capacity to infer internal states from external outputs. To achieve effective observability, applications must actively externalize key events through logs, metrics, and alerts, facilitating comprehensive monitoring strategies. While cloud-based applications can leverage unified solutions like AWS CloudWatch and X-Ray for event collection, other environments rely on tools such as Prometheus, ELK Logstash, ChaosSearch, or ServiceNow.

For visualization and analysis of collected data, Grafana and Kibana serve as powerful tools for building dashboards and visualizing metrics, enabling stakeholders to gain insights into system behavior. Additionally, low-level toolkits like D3.js (https://d3js.org) and Cubism.js (https://bost.ocks.org/mike/cubism/intro/#0) provide options for constructing custom dashboards tailored to specific requirements, further enhancing the observability and diagnostic capabilities of the system.

## Linked Data
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

Linked Data serves as a prevalent method for disseminating structured information, enabling its interconnection and machine interpretation. Typically, data domains are constructed and processed as sets of grounded facts, encapsulating knowledge statements that leverage machine-interpretable vocabularies such as schema.org. These vocabularies provide a standardized framework for defining and organizing data elements, facilitating seamless integration and comprehension by automated systems.

In the realm of representing Linked Data, protocols such as RDF (Resource Description Framework) and JSON-LD (JSON for Linked Data) play integral roles. RDF offers a flexible and extensible model for expressing relationships between resources in a graph format, while JSON-LD provides a lightweight and easily parsable serialization format that aligns with the conventions of JSON, making it particularly suitable for web-based applications. The adoption of these protocols underscores the importance of interoperability and accessibility in the dissemination and utilization of Linked Data across diverse digital ecosystems.

- [Method and apparatus for providing edge-based interoperability for data and computations](https://patents.google.com/patent/US20140067758A1/en)

## Well Architected Review
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

Well-Architected Review is a comprehensive framework that equips cloud architects with the necessary techniques to craft infrastructure that is secure, high-performing, resilient, and efficient, capable of supporting a wide range of applications and workloads. By adhering to these best practices, architects ensure that the designed workload is meticulously prepared to handle production traffic seamlessly.

## “Code as Crime Scene”
![Assess](https://img.shields.io/badge/assess-fbe6a8?style=flat-square)

"Code as Crime Scene" is a methodology developed by Adam Tornhill. It borrows principles from forensic science and applies them to software development to understand and improve code quality and maintainability. The central idea behind "Code as Crime Scene" is that software repositories contain valuable insights into the health, structure, and evolution of a codebase, much like how a crime scene contains clues that can help investigators understand what happened. By analyzing various aspects of a codebase, such as code churn (frequency of changes), code complexity, and code ownership, developers can uncover patterns and trends that reveal areas of high risk, technical debt, or inefficiency.

To apply "Code as Crime Scene" effectively, developers use tools and techniques to gather and analyze data from version control systems, issue trackers, code repositories, and other sources. By visualizing this data and identifying patterns, developers can prioritize areas for refactoring, bug fixing, or improvement, ultimately leading to a more maintainable and resilient codebase.

Overall, "Code as Crime Scene" provides developers with a systematic approach to understanding and improving code quality, enabling them to make informed decisions and drive continuous improvement in software development processes.

## Semantic, Knowledge and Ontology
![Assess](https://img.shields.io/badge/assess-fbe6a8?style=flat-square)

Semantic and Ontology methodologies represent powerful techniques for mitigating the information retrieval complexities inherent in unstructured knowledge. These approaches offer effective solutions to the challenges encountered in the extraction, storage, retrieval, and equivalence matching of facts within diverse production systems spanning eCommerce, Fashion, AppStore, Consumer Behavior, and the Search domain. Leveraged notably by IBM Watson, these methodologies provide invaluable frameworks for structuring and harnessing unstructured data, enhancing the efficiency and efficacy of information processing and decision-making processes across various industries.

## Data Engineering
![Assess](https://img.shields.io/badge/assess-fbe6a8?style=flat-square)

Data engineering is a field within computer science and information technology that focuses on designing, building, and maintaining systems and infrastructure to support the processing, storage, and analysis of large volumes of data. It encompasses a wide range of tasks, including data ingestion, transformation, storage, and retrieval. At its core, data engineering involves managing the lifecycle of data within an organization, from its acquisition or generation to its storage, processing, and analysis. Focus on security measures to protect sensitive data from unauthorized access or breaches and ensure compliance with relevant regulations such as GDPR or HIPAA.

## Teletraffic & Performance Engineering
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Modern software engineering focuses on empirical approached.
Teletraffic engineering harnesses statistical analysis, queueing systems, and simulations to accurately forecast the capacity requirements of network equipment. This multidisciplinary approach is essential for ensuring the optimal performance and scalability of telecommunications infrastructure. In many cases, service teams complement these techniques with performance engineering—an empirical method that addresses specific challenges identified by various branches of teletraffic engineering.

Analytical models play a crucial role in understanding the dynamics of highly loaded Internet services. By establishing relationships between business requirements and system capacity, organizations can leverage these models as valuable tools for investment planning. This strategic approach enables informed decision-making regarding infrastructure upgrades and resource allocation, ultimately enhancing the reliability and efficiency of telecommunications networks.

## Software as a Service
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Suspended development of SaaS solutions
Software as a Service (SaaS) epitomizes a delivery model in which web-based software is hosted and managed by a dedicated service team. Stemming from the realm of cloud computing, SaaS emerged alongside infrastructure as a service (IaaS) and platform as a service (PaaS), revolutionizing the way software is accessed, deployed, and maintained.

