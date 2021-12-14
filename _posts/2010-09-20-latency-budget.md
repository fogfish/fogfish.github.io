---
layout: post
title: Latency budget as the tool to address latency requirements
description: |
  Latency requirements determines the degree of user satisfaction about the web service. Software development practices shall consider the end-to-end latency as one of user-oriented characteristics during the entire life-cycle of an application. Latency budget defines latency target and non-functional requirements for distributed systems.

tags:
  - thoughts
  - non functional requirements
  - latency
  - internet
  - protocols
  - tcp/ip
  - dns
  - http
  - tls
---

# Latency budget as the tool to address latency requirements

End-to-end latency is one of user-oriented characteristics used for quality assessment of distributed software architecture. The ultimate goal is the ability to quantitatively evaluate and trade-off software quality attributes to ensure competitive end-to-end latency of Internet Services.

Latency belongs to Quality of Service (QoS) requirements that determine the degree of satisfaction of a user of a service. In this respect, it is an end-to-end user oriented metric, defined independently on underlying solutions or technologies. User experience studies and competitors benchmark are good sources to understand end-to-end latencies requirements. An application fails to meet consumer expectations if an interaction cannot be accomplished within 3 - 4 seconds. The ultimate goal is the ability to quantitatively evaluate and trade-off this software quality attribute to ensure competitive end-to-end experience of consumer facing applications.

Software development practices shall consider the end-to-end latency as one of user-oriented characteristics during the entire life-cycle of an application. This user-oriented metric shall be defined independently of underlying solutions or technologies and shall be used for quality assessment of delivered software.

## Latency Context

The figure below illustrates a typical client-server interaction, which is used to derive the terminology required to specify and observe quality metrics of client-server interaction.

![Client Server Interaction](/assets/images/2010-09-20-client-server.svg)

* Request (REQ) is a message sent to remote instances over network.
* Response (RSP) is a message sent back from remote host
* Transaction (TX) is collection of client-server calls that forms single task. A distribute systems denotes transaction as requests/response bundle, in which two or more network hosts are involved.
* Round Trip Time (RTT) is a time taken to transmit data packet to remote host plus the acknowledgement time.
* Transfer Delay (TD) is the sum of latencies that a message experience while transfered across the network. The transfer delay is influenced by the length of physical links, available bandwidth and time waiting at output link for transmission.
* Time to First Byte (TTFB) is the duration from making an request to the first byte of the response being received. This delay is influenced by request transfer delay time required by remote host to process request plus delivery time of first byte/packet of the response. A connection establishment and response transfer delay are not accounted here.
* Time to Meaningful Response (TTMR) is the duration to receive a response from the server for that client can unblock user experience. 


## Non-functional Requirement

Latency budget is a tool and collection of non-functional requirements to to meet competitive latency at interactive, user centric communication. Looking on the end-to-end client/server interaction we can define set of requirements:

![Complete Client Server Interaction](/assets/images/2010-09-20-client-server-tx.svg)


**Any remote host connections MUST be re-used.**
At least one round-trip is required to establish L4 (TCP) connection, an additionally multiple round-trip might be needed to establish L5-L7 connection. A network round-trip time is highly variable in radio access network. A cellular network RTT for 90% of cases is about 300 - 400 ms at 2G network, 160 - 290 ms at 3G network, 90 - 110 ms at 3.5G network. A congestion control (slow start), implemented by TCP, do not allow to fully utilize network bandwidth for a new connections. It gradually increases a data flow to avoid overflowing in the network.


**A connection time to remote host MUST be less then 300ms.**
A connection is established when first byte of L7 protocol request can be send from client to server. An operation to establish connection with remote host SHOULD require only 1 round-trip that allows to achieve desirable end-to-end latency on busy hour at slow access network where bandwidth is shared among terminals.


**Client SHOULD establish/warm-up connection with remote host 3 - 5 sec before data transmission.**
A cellular network requires a radio channel allocation before data transfer. It takes 3.6 2 sec depending on mobile network class and cell utilization. About 4 sec is elapsed before client can issue request. Predict-and-allocation connection for packet data communication allows to reduce a client/server communication latency.


**A transaction MUST involve 2 DNS lookups: 1 for API and 1 for CDN.**
Resolution of human-friendly names into network addresses is a pre-requirement of any Internet service transaction. Latency of the DNS infrastructure con- tributes to the overall user-experience. A slow DNS makes the impression that the service is slow. DNS resolution involves 1 round-trip time to ISP resolver and hierarchical address resolution if entry is not cached by ISP that takes 20 - 60 ms in addition to network RTT.

**Time-to-live (TTL) for DNS records MUST be about 12 - 24 hours.**
DNS caches evicts records based on TTL policy associated with DNS record. A long TTL improves record caching on infrastructure and terminal sides. A short TTL leads frequently DNS resolution from authoritative servers. However, the requirement contradicts to "availability".

**Domain name SHOULD be pre-fetched from DNS infrastructure.**
A domain name resolution takes takes 20 - 60 ms in addition to network RTT. A RRT for first 3 packets is higher then average in cellular network due to radio channel allocation procedure. DNS pre-fetch allows to avoid an DNS resolution over ”slow” network.

**Perform DNS resolution on the edge.**
Having a full DNS resolution on the edge allows to minimizes DNS latency to network RTT time.

**A SSL handshake MUST be a background operation from user perspective.**
SSL handshake requires 2 - 3 round trips and 2 - 4K of data transfer. An average time for SSL handshake is about 1000ms on cellular network.

**A size of certificate chain SHOULD be about 1024 bytes and MUST not exceed 4096 bytes.**
A SSL handshake procedure involves a certificate exchange. The sender follows congestion control algorithms while transmits a certificate chain. The initial congestion window is about 4K. Thus any certificate chain over 4K involves extra round-trip as the sender waits acknowledgement from receiver. A SSL handshake procedure is faster if certificate chain fits into MTU thus less impacted by transmission delay.

**A TLS Record MUST NOT be fragmented to multiple MTU**
TLS would not decrypt/validate record until whole record has been received. A transfer of large records would involve extra round-trips before congestion window opens up.

**HTTP Redirect MUST NOT be used.**
A redirect triggers a DNS resolution, TCP/IP connection set up and request/response transfer. In worts-case scenario, TLS handshake is involved. HTTP redirect bring from 1 to 2 second latency on cellular network.

**An overhead of HTTP and underlying protocols MUST NOT exceed 400 byte.**
A transmission delay for first packets is higher then average in cellular network due to radio channel allocation procedure. Thus a small request allow to minimize transaction latency.

**Minify and compress HTTP payload**
Minification and compression of HTTP payload reduces a transmission delay.

**Interactive transaction MUST be accomplished with 3000 ms.**
Research on human-factors indicates that system fails to meet user need if a transaction cannon be accomplished within 3 - 4 seconds.

**Align payload to state of TCP/IP congestion window.**
TCP/IP do not utilizes a full bandwidth when connection is established. It gradually increases a data flow to avoids in-network overflowing. The packet injection rate should correspond to the rate at which acknowledgements are returned by remote host. The rate is controlled by the congestion window that defines a number of segments sender is allowed to transmit (initial window is 3 segments). Every received acknowledgement increases the sender window by 1 until transmitted data size reaches the size of receive window.

**Application request (including protocols overhead) MUST be less then 1400 bytes and transfer delay MUST NOT exceed 200ms**
A RRT and transmission delay for first 3 packets is higher then average in cellular network due to radio channel allocation procedure.

**A remote host delay MUST NOT exceed 150ms**
Research on human-factors indicates that system fails to meet user need if a transaction cannot be accomplished with 3 - 4 seconds. A typical time of a single client to server request is 10 - 20% of total transaction time. Therefore interactive response perceived as bad when remote server feedback (TTFB) takes longer then 500ms. A cellular network RTT for 90% of cases is about 160 - 290 ms at 3G network. Thus remote host delay MUST NOT exceed 150ms.

**Application MUST pipeline data processing**
Application MUST NOT wait util payload is fully received. It MUST progressively receive, parse and render data chunks. E.g. 10KB transmission take about 2 s over 3G network.


## Afterwords

Any tech company has pressure to define a global architecture, purify and share concepts of core business in it. Microservices is an appropriate design style to achieve the goal, despite all challenges discussed above. It lets us evolve systems in parallel, make things to look uniform, design stable and consistent interfaces across the system. It just requires an end-to-end latency analysis solution to provide a measure of adequacy of a group of components under specified conditions. 

The latency analysis is the investigation of a component behavior using information gathered as the service sustains production or artificial load. It facilitates design of microservices as cost effectively as possible with a predefined grade of service. The analysis helps to solve a series of decision problem concerning both short-term and long-term arrangements: short term decisions include for example the determination of optimal software configuration, the number of servers, the number of concurrent connections; long term decisions include for example decisions concerning the development and extension of data and service architecture, choice of technology, runtime environment, etc. It defines methods for controlling that the actual end-to-end latency is fulfilling the requirements, and also to specify emergency actions when systems are overloaded or technical faults occur.
