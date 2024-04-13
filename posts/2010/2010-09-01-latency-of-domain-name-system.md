# Domain Name System (DNS) and its contribution to communication latency

The Domain Name System (DNS) is a globally distributed host-name catalog that translates human-friendly names into network addresses associated with network equipment. For example the domain name www.example.com is translated into IPv4 address 93.184.216.34. Additionally DNS associates various control information with domain names. Often DNS is called the phone-book for the Internet.

Any human interactions with Internet services start with background DNS operations. Whether you’re typing http://gmail.com in a browser or starting your mail client, the application communicates first with the DNS infrastructure to resolve the network address and only then establishes communication with the desired Internet service. DNS provides information critical to the operation of most Internet services. Therefore DNS must be highly scalable, available and provide low latency responses.

## DNS architecture

The detailed design of the Internet DNS is specified by [RFC 1034](http://tools.ietf.org/html/rfc1034), [RFC 1035](http://tools.ietf.org/html/rfc1035) and other related RFCs. We summarize the high-level view.

DNS has a hierarchical distributed organization where each sub-domain can be independently administered, deployed and distributed among the network infrastructure. The root of the hierarchy is centrally administered by generic top-level domain servers. Sub-domains are delegated to other foreign servers that are authoritative for the subset of the name-space; and this repeats recursively.

![dns architecture](/assets/images/2010-09-01-dns-seq.svg)

Human-friendly name translation is an iterative distributed lookup among those servers where elements of DNS infrastructure either resolve the name into an IP address or point to another foreign server that might answer a query. DNS complexity is not exposed to end-user applications; they use a stub component built-in into any operating system (e.g. `gethostbyhame()` system call on Linux OS).

The stub implements only a local in-device cache; it offloads iterations through the DNS hierarchy to the resolver. The resolver is installed within the Internet Service Provider (ISP) access network. It deals with the distribution of domains, answers clients’ queries via recursive subroutines to foreign name server and the local cache. For example, lookup of www.example.com domain requires address resolution of authoritative server (ns.example.com), and secondly name lookup from it. 


## Latency of DNS

We have learned that resolution of human-friendly names into network addresses is a pre-requirement of any Internet service transaction. Therefore, latency of the DNS infrastructure contributes to the overall user-experience. A slow DNS gives the impression that the service is slow.

People have focused on DNS latency since 1992 in broadband networks. We have also studied the impact of DNS within cellular networks. In contrast to previous DNS traffic analysis, which is held at root name server (see "An analysis of wide-area name server traffic: A study of the Internet domain name system" by P. Danzig, et al), wide area network (see "Wide-area traffic patterns and characteristics" by K. Thompson), and fixed broadband IP network ("DNS Performance and the Effectiveness of Caching" by Jung J), we focus on DNS latency perceived by applications in mobile wireless networks. Instead of sniff-and-analysis traffic at network equipment, we have measured DNS lookup latency directly on the mobile terminal. That allowed us to aggregate end-to-end performance implications, including impact of wireless network.

**Latency of 90% queries do not exceed network delay more then 5%**

![dns latency at mobile network](/assets/images/2010-09-01-dns-mobile-latency.png)

## HowTo Measure DNS latency

Time taken to execute DNS transactions over wireless networks consists of UDP protocol latency, resolver latency, intermediate operations such as authoritative lookup and authoritative latency. A reliable measurement can be performed on client-side: latency to query ISP resolver (`TQ`) and latency to perform DNS transaction (`TTX`). Latency of authoritative server (`TA`) can be estimated (due to components ownership and access restrictions) by by measure a latency of DNS query to authoritative server performed on client-side and network round trip time. Our assumption is that latencies of intermediate operations can be neglected and latency of DNS transaction is `TTX = TQ + TA`. To proof the assumption, the following measurement is done:

**Latency of DNS transaction do not exceed latency of direct lookup on authoritative more then 5% measured by client**

![latency of dns transaction at mobile network](/assets/images/2010-09-01-dns-tx-latency.png)

Measure DNS latency using custom DNS stub component that generates DNS request via non-blocking datagram socket. The stub bypasses DNS services provided by OS such as `gethostbyhame()`, that allows us to avoid the effect of local in-device DNS cache.

**DNS Resolver latency**

Resolver latency is measured by issuing non-recursive queries (Request Desired bit is off).

```
dig @ip.of.resolver +norecurse A www.example.com
```

**DNS Transaction latency**

DNS transaction latency is measured by issue recursive queries (Request Desired bit is on)

```
dig @ip.of.resolver +recurse A www.example.com
```

**Authoritative latency**

The cost of DNS lookup from authoritative can be estimated, if client-side latency of DNS query to authoritative is measured. We need to know IP addresses of authoritative servers.

To discover an authoritative server

```
dig @ip.of.resolver +recurse NS www.example.com
```

To measure authoritative latency

```
dig @ip.of.authoritative +norecurse A www.example.com
```

**Resolver cache**

We assume DNS cache miss if resolver do not find a valid record in its local cache, further queries to foreign servers are required. The recursive lookup could be disabled if Request Desired bit in DNS request header is off. To study presence of particular domain(s) is ISP cache yo can do by:

```
dig @ip.of.resolver +norecurse A www.example.com
```

## Mitigate DNS latency

* Pre-fetch domain names
* Move DNS on the edge
* Watch out Time-to-Live for DNS records
* Limit number of “hot” DNS lookups
