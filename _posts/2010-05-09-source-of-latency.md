---
layout: post
title:  In-depth guide to sources of latency in distributed systems.
description: |
  Distributed systems involve various technologies and communication principles that bring overhead and latencies. The root cause of latencies varies from physical properties of the network to the quality of software components. The post discussed sources of latencies from the prism of infrastructure, protocol and application.

tags:
  - thoughts
  - non functional requirements
  - latency
  - networking
  - mobile network
  - microservices
  - protocols
  - tcp/ip
  - http
---

# In-depth guide to sources of latency in distributed systems.

The [previous post](/2010/04/12/latency-of-internet-services-user-expectation.html) discussed about The research on human-factors about latencies. It indicates that an application fails to meet consumer expectations if an interaction cannot be accomplished within 3 - 4 seconds. [Google and Bing claim](http://radar.oreilly.com/2009/06/bing-and-google-agree-slow-pag.html) - 500ms delay impact business metrics; [Amazon founds](http://blog.gigaspaces.com/amazon-found-every-100ms-of-latency-cost-them-1-in-sales/) - every 100ms of latency cost them 1% in sales. The latency is an important part of Quality of Service (QoS) that determines the degree of consumer satisfaction. The ultimate goal is the ability to quantitatively evaluate and trade-off this software quality attribute to ensure competitive end-to-end experience of consumer facing applications. 

Software development practices shall consider the end-to-end latency as one of user-oriented characteristics during the entire life-cycle of an application. This user-oriented metric shall be defined independently of underlying solutions or technologies and shall be used for quality assessment of delivered software. The *interactive* traffic is the classical data communication scheme employed by mobile applications. It is described as a request response pattern of users. Overall recommendation that interaction between human and remote equipment should not exceed 1 sec.


## History of distributed inter process communication

The latency challenge exists since the beginning of the distributed computing epoch. A transparent end-to-end communication involves various technologies and communication principles. Latency impacts distributed systems that demand a remote inter process communication. A transparent end-to-end communication involves various technologies and communication principles.

**1960s**
TCP/IP was used to build reliable client/server communication.

**1990s**
World Wide Web shift paradigm of client/server communication, HyperText Transmission protocol plays a major role.

**2000s**
Representational State Transfer (REST) is a style of software architecture for distributed hypermedia systems such as the World Wide Web.

**2005s**
Asynchronous JavaScript and XML (AJAX) is a group of technologies used to create interactive web applications. JavaScript Object Notation (JSON) becomes the de-facto format for data interchange.

**2010s**
WebSocket, HTML5 and Canvas are buzzing

**2014s**
Microservices


## Sources of latency

Let's discuss sources of latency, which are required to consider during application development; define measurable key performance indicators that are usable to reflect the degree of user satisfaction and talk about tooling to evaluate latency of microservices. These principles have evolved over the time as well as latency concerns. Let us illustrate a reference OSI protocol stack. It allows us to highlight sources of latencies that are observed by uses while their mobile device interacts with remote systems. These latencies belongs to one of following domains: **infrastructure**, **protocol** and **application**.

![Source of latency](/assets/images/2010-05-09-source-of-latency.svg)


## Infrastructure domain
	
A packet-switched communication network is a collection of nodes, which are interconnected through network switches and routers. The underlying network structure involves various technologies, interfaces and protocols on L1 - L3 of the OSI model. There is a limited ability to influence latency at network infrastructure level due to decentralized management architecture, cost of equipment, network ownership, etc.
	
The architectural decision about software shall account complexity of underlying network infrastructure. The Internet is not a single network, this is heterogeneous systems composed from backbones, Internet Service Providers, data centers and various edge/access networks. Core networks provide high-speed, low-latency packet switching while access networks have limited capacity shared among consumers. Design and best practices for latency optimization on L1 -- L3 of network stack is widely studied by Cisco. This post proposes to merge them into single **network delay** metric: 


**Propagation delay** is the time taken by the head of signal to travel from the sender to the receiver. The physical distances and speed of light are major constraints. The signal experiences about 5 microsecond delay per kilometer. Unfortunately, there is not any solution except to minimize the distance required by the signal to travel, measure and deal with this latency.


**Processing/Queuing delay** is a time used by network equipment to process packets. A packet-switched communication requires forwarding, segmentation, reassembly and application of value-added services (e.g. NAT, firewalls, load-balancing). The processing latency is visible on end-to-end communication especially in virtualized cloud environments despite the fact -- network gears use hardware solutions to carry-on required tasks.   
	  
	  
**Transmission delay** is a time required to serialize packet bits into the link. The modern communication systems are still impacted by it, especially mobile networks. The transmission time is constrained by a carrier signal, a modulation schema, used frequencies, medium access protocols and available bandwidth which is shared among thousands-and-thousands network subscribers. 


**Network delay**. is time from when a message delivery is requested until that message begins delivery at the remote end, often called One Way Delay (OWD). The time from when the message begins delivery until delivery is completed is the service time, denoted by `x` in the Queueing theory books. The analysis of infrastructure latencies brings significant overhead to software development practices, especially if you are designing consumer facing applications. The infrastructure appears as a system that makes peers wait for time `W`.  Mobile application considers two communication scenarios: short interactive request/response communication (`x << W`) -- concern initial network response time. Long file transfer (`x >> W`) -- network throughput is important.
	
## Protocol domain

The protocol domain covers communication on L3 -- L7 of the OSI model. The protocol domain covers communication on Layer 3-7 of OSI model. Latency optimization requires protocol tuning, new protocol development that causes standardization effort.

**Round-Trip Time** is the time taken to transmit a data packet to a remote host plus the acknowledgement time.

**Protocol signaling** demands message exchange with remote peers to acknowledge, establish connection, chatty protocols, etc.

**Protocol overhead** is the amount of bytes taken by protocol headers, error correction, etc.

**Blocking** is an effect of packet loss due to unavailability of resources at network equipment.


### Case of microservices

The development of REST and microservices architecture principles raised the significance of TCP/IP, TLS and HTTP protocols. It became important for mobile solutions to mitigate latencies introduced by them. The protocol overhead in terms of signaling carried over heterogeneous network environments became a major issue. The protocol internals and behavior in mobile networks is a complex subject that requires a dedicated post to depict protocols performance, when it is constrained by network delay and blocking effect (packet loss due to unavailability of resources at network equipment). Let's emphasize those issues. 

**TCP/IP** do not utilize a full bandwidth when connection is established. It gradually increases data flow to avoid overflowing the receiver or any other device in the data path. The protocol controls the packet injection rate which corresponds to the rate at which acknowledgements are returned by the remote host.

**TLS** protocol provides privacy and data integrity between two communication applications. A full TLS handshake demands at least two round trips in addition to TCP/IP connection setup.
	
**HTTP** is the core protocol in the REST interface. A reference client/server interaction involves two remote hosts. The first host (client) sends a request message to the second one (server) for processing purposes. Once a request is completed a response message is sent back.  
	
*Time to First Byte* (TTFB) is the duration from making a request to the first byte of the response being received. This metric assumes that the network appears to mobile applications as a system that makes it wait. This delay is influenced by request transfer delay, time required by remote host to process request, and delivery time of first byte/packet of the response. TTFB represents initial confirmation that the remote host is responding and the client application could proceed to rendering. A primary advantage of TTFB is that it is a concrete, consumer oriented metric easily measurable and is defined independently of underlying solutions or technologies. 

The time to display response is the most valuable metric from a consumer perspective. It is influenced by performance of network and client applications by itself. The time required for response rendering needs to be excluded from quality assessment due its dependency on user interface technology. Instead, *Time to Meaningful Response* (TTMR) is defined as network delay required to deliver response to client. 
	

## Application domain

The application domain covers communication on Layer 7 of OSI model and behavior of application software. The complex application is built from small independent SaaS instances that communicate with each other using REST principles, this architecture style is defined as microservices. Microservices style is subject to criticism -- the monolithic application complexity is shifted to macro-level. The software development deals with a new set of latency issues.  

**Value-added service** such as DNS, Proxy/Cache, NAT, firewall, load balancing brings extra latency.

**Execution environment** is an operating system, runtimes and set of application frameworks used for service provision.

**Service dependency** dependent components increase latency. If component A calls component B then the latency is the sum of the latency for each component. Service dependencies cause an avalanche effect of network latency when a single client request causes enormous internal interaction between microservices instances.

**System utilization** An average time in a system is an exponential-like function of utilization factor. Majority of systems hold predictable latency if utilization factor is kept below 70 - 75%.

**Hardware latency** often called hardware service time, is a time spent by hardware to fulfill the request. It includes processors, memory, storage and network.


## Afterwards

Any tech company has pressure to define a global architecture, purify and share concepts of core business in it. Microservices is an appropriate design style to achieve the goal, despite all challenges discussed above. It lets us evolve systems in parallel, make things to look uniform, design stable and consistent interfaces across the system. It just requires an end-to-end latency analysis solution to provide a measure of adequacy of a group of components under specified conditions. 

The latency analysis is the investigation of a component behavior using information gathered as the service sustains production or artificial load. It facilitates design of microservices as cost effectively as possible with a predefined grade of service. The analysis helps to solve a series of decision problem concerning both short-term and long-term arrangements: short term decisions include for example the determination of optimal software configuration, the number of servers, the number of concurrent connections; long term decisions include for example decisions concerning the development and extension of data and service architecture, choice of technology, runtime environment, etc. It defines methods for controlling that the actual end-to-end latency is fulfilling the requirements, and also to specify emergency actions when systems are overloaded or technical faults occur.
