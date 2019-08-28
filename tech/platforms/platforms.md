---
layout: default
title: Platforms
parent: Technology Radar
nav_order: 3
description: Links to software platforms and related resources.
---

# Platforms

## AWS

### AWS CloudFront

#### Assess

[A/B Testing with Lambda@Edge](https://medium.com/buildit/a-b-testing-on-aws-cloudfront-with-lambda-edge-a22dd82e9d12) - Imagine you have a static website or a Single Page Application served through the CDN. You want to experiment two versions with actual users. 

#### Adopt

[A Green/Blue deployment to AWS](https://serverfault.com/questions/714742/blue-green-deployments-with-cloudfront) - CloudFront requires the CNAME in the distribution config to be unique across your entire account. So controlling blue/green via DNS to different distributions will not work. There is a hack rolling around that would use wild cards but that makes no guarantee that the correct files are served. Controlling blue/green via DNS and CloudFront is not feasible.



## Elastic

#### Adopt

[Designing the Perfect Elasticsearch](https://thoughts.t37.net/designing-the-perfect-elasticsearch-cluster-the-almost-definitive-guide-e614eabc1a87) - Cluster design is an overlooked part of running ElasticSearch. Official documentation and blog posts focus on the magic of deploying a cluster in a giffy, while the first problem people face when deploying in production is memory management issues, aka garbage collection madness. This guide answers most questions I was asked, and summarizes everything you should know about designing the perfect ElasticSearch cluster.


