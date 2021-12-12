---
layout: post
title:  Effect of TCP/IP slow start on the communication latency
description: |
  TCP/IP do not utilizes a full bandwidth when connection is established. It gradually increases a data flow. Effect of TCP/IP slow start and its latency becomes visible for applications while mobile network is used. 

tags:
  - thoughts
  - latency
  - networking
  - internet
  - protocols
  - tcp/ip
---

# Effect of TCP/IP slow start on the communication latency

## TCP/IP protocol summary

* connection-oriented, TCP/IP handshake is required before data transmission. Handshake performance is constrained by Network delay.
* full duplex
* reliable, lost packets are re-transmitted.
* flow control, both sender/receiver flow control to avoid sending too much data.
* segmentation, application data is segmented into IP MTU.


## TCP/IP Slow Start

TCP/IP do not utilizes a full bandwidth when connection is established. It gradually increases a data flow this avoids overflowing the receiver or any other device in the data path. It operates by following rule

*the packet injection rate should correspond to the rate at which acknowledgements are returned by remote host*

The slow start adds another window to the sender, called the congestion window (cwnd). This window defines a number of segments sender is allowed to transmit.

Initial congestion window:
* was equal to 1 segment, defined by RFC 2581 in 1999
* is approx. 3 segments, defined by RFC 3390 in 2002
* proposed to 10 segments, in 2010 by Google

Every received acknowledgement increases the sender window by 1 until transmitted data size reaches the size of receive window. The sender starts by transmitting X segments and waiting for its ACK. When any of those X segments is acknowledged, the congestion window is doubled 2*X. This provides an exponential growth.

![tcp/ip slow start explained](/assets/images/2010-08-08-tcp-ip-slow-start.svg)

Effect of TCP/IP slow start and its latency becomes visible for applications while mobile network is used:

![tcp/ip slow latency](/assets/images/2010-08-08-tcp-ip-slow-start-latency.png)

**Mitigate TCP/IP Slow start**
* Multiplex L7 connection over single TCP/IP Keep-alive and re-use connection
* Transmit data in order of importance
* Align payload to congestion window
