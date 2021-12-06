---
layout: post
title:  Network Infrastructure and its impact on communication latencies.
description: |
  The network infrastructure involves various technologies, interfaces and protocols along the communication path. It is conveniently partitioned into domains: Internet, Data Center and Internet Service Provider. The post gives an introduction to high-level IP network architecture and its impact on communication latencies. 

tags:
  - thoughts
  - latency
  - networking
  - mobile network
  - internet
  - tcp ip
---

# Network Infrastructure and its impact on communication latencies

A packet-switched communication network is a collection of nodes, which are interconnected through network switches and/or routers. The underlying network structure involves various technologies, interfaces and protocols along the path. Thus, it is conveniently partitioned into domains: *Internet*, *Data Center* and *Internet Service Provider* (ISP).

## Internet

Over the 25 years since the ARPANET started; backbone speed increased by 1000 number of hosts by 1 000 000.

![Internet topology](/assets/images/2010-05-26-internet-topology.png)

The complexity of the Internet belongs at the edges, and the IP layer of the Internet should remain as simple as possible.

*Tier 1* is the key to global connectivity is the inter-networking layer. It is built by settlement-free peering. Tier 1 networks usually have only a small number of peers.

*Tier 2* do peering with other networks, but who still purchases IP transit to reach some portion of the Internet. Tier 2 networks are motivated to peer with many other Tier 2 and end-user networks. Tier 2 networks with good peering is frequently much ”closer” to most end users or content.

*Tier 3* solely purchase IP transit from other networks.

![Architecture of IP network](/assets/images/2010-05-26-ip-net-architecture.svg)

**Major latency concern** is Propagation and Queue delays.

## Data Center

The data center is a place for computation resources, storage, and other appliances. It facilitates the content or passes it through to end users.

The network design is based on a layered approach. It looks to improve scalability, performance, flexibility and maintenance in reality it is primarily optimized for cost.

*Access network* where servers physically attach to the network. It provides L2 - L3 topologies.

*Aggregation network* provides value-added service integration, L2 domain definitions and default gateway. It facilitates multi-tier traffic with fire-walling and load balancing; additionally implements L5-L7 services such as content switch- ing, SSL termination, traffic shaping, etc.

*Core network* provides the high-speed packet switching for all inbound/outbound traffic flows.

**Major latency concern** is Processing and Queue delays

## Internet Service Provider

**Broadband Access Network**
* ADSL over copper telephony line (last mile)
* Distributed over short distances less than 4 kilometers
* ADSL2+ down-link 24.0 Mbps uplink 3.3 Mbps
* Fixed terminals

**Radio Access Network**
* Refers to various cellular technologies: WCDMA, CDMA, WiMAX, LTE
* Radio Cells facilitates wireless communication between user equipment
and network.
* Enables terminal mobility

**ISP Core Network**
* Central part of ISP infrastructure
* Transports large amounts of telephone calls and data traffic over fiber links.
* Services provisioning (e.g. packet-switched service)
* Gateways to access other networks
* Network layer is IP-based

Data-link layer is ATM over SONET/SDH; Synchronous Optical Networking (SONET) and Synchronous Digital Hierarchy (SDH); Data passing through equipment can be delayed by at most 0.032 ms, compared to a frame rate of 0.125 ms. SONET standard is defined by ANSI; SDH is originally defined by the ETSI.


Data communication is encapsulated either by Asynchronous Transfer Mode (ATM) or Packet over SONET/SDH; ATM encodes data into small fixed-sized cells, a small data cells reduce jitter. ATM 155 Mbps (designed) a typical MTU (1500 byte) transmission takes 0.077ms. In low speed links 1.544 Mbps takes up to 7.8 ms. ATM has cost of segmentation and reassembly, fixed 48-byte cell payload is not suitable for underlying IP. Uses virtual circuits for traffic engineering: QoS, shaping and policing.

Decreasing complexity, CAPEX, OPEX is the major driver for evolution of transport efficiency for IP: IP over ATM over SONET; IP over SONET over WDM IP over WDM; IP over Ethernet.

