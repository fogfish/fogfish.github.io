---
layout: post
title:  Impact of the network delay on protocols performance.
description: |
  Network is a system that make peers to wait. One-way delay shows the network performance from source to destination. Measure the delay and optimise protocols for number of round-trips.

tags:
  - thoughts
  - latency
  - networking
  - internet
  - protocols
---

# Impact of the network delay on protocols performance.

From client-server perspective, network appears as a system that make peers wait for time `W`. Wait time is is time from when a message delivery is requested until that message begins delivery at the remote end. The time from when the message begins delivery until delivery is completed is the service time, denoted by `x`.

Thus the primary performance metric is `T = W + x` be an average network delay.

We have to distinguish a two cases:
* short interactive message `x << W`, we are mainly concerned about initial network response time. It is often called One Way Delay (OWD).
* long file transfer `x >> W`, we are mainly concerned about the network throughput.

One-way delay (OWD) for reference packet shows the network performance from source to destination. OWD measurement is more accurate to compare with Round Trip Time (RTT) due to asymmetric communication (see [RFC 7679](https://datatracker.ietf.org/doc/html/rfc7679)):
* path from source to destination is often different then path from destination and back.
* different bandwidth at up/down links.
* different traffic policies for incoming/outgoing traffic.

The clock synchronization, packet collections and tooling aspects are the major obstacle to reliably measure OWD at live Internet Services. Thus, RTT is still valuable to estimate end-to-end network performance from client perspective:
* time required to establish TCP/IP connection with remote host
* time required to reliably deliver a message for chatty protocols

![Network delay](/assets/images/2010-06-22-network-delays.svg)

Easy way to measure RTT is a ping command line utility, it allows to sample latency of ICMP ECHO packets. The size of ICMP packet should not exceed either 40 bytes or 24 bytes that corresponds to TCP SYN and SYN,ACK messages, keep in mind that ICMP messages has 8 byte header:

```
ping -i 1000 -s 32 www.google.com
```

Any UDP-based measurement might not be accurate. ISP might enforce various traffic policies for UDP or TCP traffic therefore tools such as `tcpconnect` along with `tcpdump` or `tcptraceroute` allows us to estimate TCP/IP connection time.

**Mitigate the network delay**
* Align payload to data link frames
* Optimize number of round trips
* Watch-out protocol/application behavior
