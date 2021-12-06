---
layout: post
title:  What is the user's expectation about the latency of Internet Services?
description: |
  End-to-end latency is one of user-oriented characteristics used for quality assessment of distributed software architecture. Research indicates that a service fails to meet user needs if a transaction cannot be accomplished within 3 - 4 seconds.
tags:
  - thoughts
  - non functional requirements
  - latency
  - networking
  - mobile network
  - consumer journey
  - user experience
  - microservices
---

# What is the user's expectation about the latency of Internet Services?

Design and best practices for latency optimization on layer 1 through layer 3 of network stack is widely studies by Cisco in the paper "Design Best Practices for Latency Optimization" and IP Packet Delay Variation Metric for IP Performance Metrics, [RFC 3393](https://datatracker.ietf.org/doc/html/rfc3393). Souder's book "High Performance Web Sites" guides through previous research on layer 7 latencies, which covers behavior of web application and aspects of UX rendering by web browser. The list of valid techniques has been discussed and the major outcome is that only 20% of time is spent to retrieve the main HTML document, the other 80% is consumed to download and render JavaScripts, Style Sheets, images, etc.

Here, we are contributing to the discussion about the importance of latency analysis within mobile client-server communication based on the HTTP protocol, which is widely used by REST API, AJAX, etc. Problems and possible solutions discussed in this and following posts allows mobile application developers to decrease human-perceived latency at W3C widgets, native rich applications or mobile web sites.


## Latency definition 

Latency is the elapsed time between A and B where A and B are something you care about.

> Time interval taken to process request
>       by Software Engineering Institute

> Latency is a time delay experienced in the system
>       by wikipedia.org

In practice latency is a random variable therefore it is characterized by a distribution function and its moments (mean, variance, deviation, etc). Latency target values are always given for the mean delay and 95% percentiles.

Another latency concern is its variation, which is sometimes incorrectly referred to as jitter. The variation is the difference in end-to-end delay between selected requests in a flow as RFC 3393 explains.


## Latency is a quality metric

End-to-end latency is one of user-oriented characteristics used for quality assessment of distributed software architecture. The ultimate goal is the ability to quantitatively evaluate and trade-off software quality attributes to ensure competitive end-to-end latency of Internet Services.

Latency belongs to Quality of Service (QoS) requirements that determine the degree of satisfaction of a user of a service. In this respect, it is an end-to-end user oriented metric, defined independently on underlying solutions or technologies. User experience studies and competitors benchmark are good sources to understand end-to-end latencies requirements.

From end-to-end objectives, a partition yields the objectives for each network stage or network element. These parameters are network/technology oriented, are called Grade of Service (GoS), and used to provide a measure of adequacy of a group of components under specified conditions. As an example, GoS may be expressed as probability of blocking, probability of delay, delay time, etc.

The following ITU Recommendations assist on latency activities
* E.600 provides a list of traffic engineering terms and definitions.
* E.736 focuses on packet-level performance evaluation, and packet-level control.
* E.492 provides the definition of traffic sampling for the purposes of GoS monitoring.
* E.493 addresses how-to perform end-to-end GoS monitoring.


## End-user expectation about the latency

Research on human-factors made by Shneiderman in the book "Designing the User Interface" indicates that a system fails to meet user needs if a transaction cannot be accomplished within 3 - 4 seconds. A typical time of a single client to server request is [10 - 20% of total transaction time](http://www.nytimes.com/2009/07/24/business/24trading.html). Therefore interactive response is perceived as bad when remote server feedback takes longer than 500ms.

### Classification on consumer traffic

**Conversational (less than 1 sec)**
* demand low end-to-end delay and the traffic is symmetric (voice, telephony)
* 4 - 25 kbps for voice, 32 - 384 kbps for video telephony
* mean 150, 95% percentile 400 ms

**Interactive (~ 1 sec)**
* interaction between human or machine and remote equipment 
* gaming about 1KB, 95% percentile less the 250 ms
* web is primary down-link traffic, 95% percentile less than 3 - 4 sec

**Streaming (< 10 sec)**
* data is processed as a steady continuous stream (video, audio)
* audio 32 - 128 kbps, initially 95% percentile less than 10 sec. Lately affected by industry (Spotify), less than 1 sec

**Background (> 10 sec)**
* non real-time data traffic (email, synchronization)
* email server access 95% percentile less than 4 sec

### Latency business impact

**Goldman Sachs** makes profits off a 500 ms trading advantage.

**Shopzilla** said that 5 second speed up resulted in 25% in use engagement, 10% increase in revenue and led to 50% reduction on infrastructure demand.

**Google and Bing** [claims](http://radar.oreilly.com/2009/06/bing-and-google-agree-slow-pag.html) that delays under 500 ms impact business metrics; 
* 200ms delay increases time to click by 500ms
* 500ms delay increases time to click by 1200ms
* 1000ms delay increases time to click by 1900ms

**Amazon** every 100ms of latency cost them 1% in sales.

