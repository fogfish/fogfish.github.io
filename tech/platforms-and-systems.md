# Platforms and Systems

Things that we build software on top of such as cloud platforms, operating systems, or generic kinds of platforms like distributes or actor systems.

- [Platforms and Systems](#platforms-and-systems)
  - [AWS Cloud Computing](#aws-cloud-computing)
  - [AWS Serverless](#aws-serverless)
  - [Linux](#linux)
  - [Containerization](#containerization)
  - [Elastic (ELK) stack](#elastic-elk-stack)
  - [Robotic OS](#robotic-os)
  - [Actor Systems](#actor-systems)
  - [iOS](#ios)
  - [Android](#android)
  - [Nokia Smartphone Platform](#nokia-smartphone-platform)
  - [TelCo (5G, LTE, 3G and Tetra)](#telco-5g-lte-3g-and-tetra)


## AWS Cloud Computing
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

AWS Cloud Computing services serve as a foundational platform for delivering workloads in the cloud. This versatile platform caters to a diverse array of workloads, ranging from batch processing and stream processing to online transaction processing, content distribution, machine learning, and beyond.

## AWS Serverless
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

AWS Serverless consolidates a myriad of techniques for deploying and executing workloads in the cloud, eliminating the need for managing virtual machines. This encompassing approach includes serverless functions, batch processing, delivery pipelines, storage solutions, and more. A standout feature of AWS Serverless is the extensibility of serverless function runtimes, enabling the integration of custom runtimes to cater to diverse programming needs and use cases.
Serverless Runtime for Erlang https://github.com/fogfish/serverless

## Linux
![Adopt](https://img.shields.io/badge/adopt-d9ecc0?style=flat-square)

Linux is a versatile open-source operating system renowned for its monolithic kernel architecture. Low-level system development often involves patching the Linux kernel to accommodate specific requirements due to its monolithic nature. Proficiency in Linux kernel internals is crucial for tasks such as driver development, advanced IP networking, and routing.

Originally, application development for Linux was primarily accomplished using the GNU toolchain, which included essential components like the GNU Compiler Collection (gcc). However, in modern times, programming languages have evolved to offer cross-platform implementations specifically tailored for Linux environments, further enhancing the development experience and enabling seamless integration with the operating system's ecosystem.

## Containerization
![Trial](https://img.shields.io/badge/trial-cadae0?style=flat-square)

Containerization effectively tackles challenges related to transparent compute infrastructure utilization, workload co-allocation, and streamlining development pipelines. Despite their continued prominence in software delivery pipelines, questions have arisen regarding the suitability of containers in cloud environments.

To address this, AWS provides a range of container services, including Elastic Container Service (ECS), Fargate, and Elastic Kubernetes Service (EKS), offering flexibility and scalability for containerized workloads. However, containers have faced criticism for their limitations in deploying storage solutions effectively.

The rise of serverless solutions presents a formidable challenge to the dominance of containers in workload management. Serverless computing offers a compelling alternative by abstracting away infrastructure management and providing on-demand execution of code, thus intensifying the competition for container workloads in the cloud.

## Elastic (ELK) stack
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Search is invalidated by LLM, AI and Vector DBs.
The Elastic Stack is a highly scalable platform designed for developing robust analytics, search, and data visualization solutions. Renowned for its adaptability, the platform has demonstrated exceptional suitability for constructing diverse applications such as data meshes, Geographic Information System (GIS) tools, and Consumer Search applications. Its flexibility and comprehensive feature set make it a preferred choice for organizations seeking powerful and customizable solutions for managing and analyzing their data effectively.

## Robotic OS
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

[Robotic OS (ROS)](https://www.ros.org) is a versatile and comprehensive framework designed for developing robot software. It comprises a diverse array of tools, libraries, and conventions meticulously crafted to streamline the creation of sophisticated and resilient robot behaviors. ROS not only simplifies the development process but also fosters interoperability and scalability, enabling seamless integration across a broad spectrum of robotic hardware platforms. Moreover, ROS facilitates collaboration within the robotics community by providing standardized interfaces and protocols, fostering innovation and accelerating advancements in the field of robotics.


## Actor Systems
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

Serverless is considered as the replacement of actor systems.
Actor Systems serve as indispensable platforms for addressing scalability challenges in various domains such as data processing, connectivity, and Internet of Things (IoT). These systems, exemplified by frameworks like Erlang, Akka, and Vert.X, provide a powerful paradigm for building highly scalable and fault-tolerant applications.

However, with the advent of serverless computing, there has been a shift in priorities, leading to a deprioritization of the traditional homogenous actor systems. Serverless functions, which are essentially heterogeneous actors, leverage scalable connectivity fabric provided by cloud providers to achieve similar scalability benefits without the need for managing infrastructure.

Despite this shift, actor systems continue to play a vital role in certain use cases where fine-grained control over concurrency, fault tolerance, and distributed processing is required. Moreover, the principles underlying actor systems, such as message-passing concurrency and isolation, remain relevant and influential in the design of modern distributed systems.

## iOS
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

The priority is shifted towards multi platform development with Flutter
iOS, developed by Apple, is a proprietary operating system and integrated environment tailored for application development exclusively on Apple devices.

## Android
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

The priority is shifted towards multi platform development with Flutter
The Android platform serves as the mobile operating system and development environment for Android devices. Originally conceived by Google in response to Nokia's smartphone initiatives, Android has since become one of the most widely adopted mobile platforms worldwide.

## Nokia Smartphone Platform
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

The world's first mobile OS for smartphones, Symbian OS, was designed and developed by Nokia as a proprietary solution. The platform served as the foundation for Nokia's family of S60 devices and was also licensed to other manufacturers.

## TelCo (5G, LTE, 3G and Tetra)
![Hold](https://img.shields.io/badge/hold-f7d1ca?style=flat-square)

5G, the fifth generation of wireless technology, promises significantly faster data speeds, lower latency, and greater network capacity compared to its predecessors.

LTE, or Long-Term Evolution, is a standard for high-speed wireless communication, often referred to as 4G technology, which offers improved performance and efficiency over previous generations.

3G, or third-generation technology, marked a significant advancement in mobile communication, introducing faster data transfer speeds and enabling services such as video calling and mobile internet browsing.

Tetra, short for Terrestrial Trunked Radio, is a digital mobile radio communication system widely used by public safety and emergency services for secure and reliable voice and data communication.

Each of these technologies represents a milestone in the evolution of mobile communication, offering unique capabilities and catering to diverse needs across different industries and sectors.
