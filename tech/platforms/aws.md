---
layout: default
title: AWS
parent: Platforms
grand_parent: Technology Radar
nav_order: 1
description: |
  Amazon Web Services
---

# AWS

## CloudFront

#### Assess

[A/B Testing with Lambda@Edge](https://medium.com/buildit/a-b-testing-on-aws-cloudfront-with-lambda-edge-a22dd82e9d12) - Imagine you have a static website or a Single Page Application served through the CDN. You want to experiment two versions with actual users. 

#### Adopt

[A Green/Blue deployment to AWS](https://serverfault.com/questions/714742/blue-green-deployments-with-cloudfront) - CloudFront requires the CNAME in the distribution config to be unique across your entire account. So controlling blue/green via DNS to different distributions will not work. There is a hack rolling around that would use wild cards but that makes no guarantee that the correct files are served. Controlling blue/green via DNS and CloudFront is not feasible.


## Cognito

#### Adopt

[Understanding Amazon Cognito user pool OAuth 2.0 grants](https://aws.amazon.com/blogs/mobile/understanding-amazon-cognito-user-pool-oauth-2-0-grants/) AWS Cognito is simplest replacement of OAuth2 Authorization Server, which is configurable using IaC principles. In addition to using the Amazon Cognito-specific user APIs to authenticate users, Amazon Cognito user pools also support the OAuth 2.0 authorization framework for authenticating users. The article explains supported flows and Cognito nuances on using them.

[Server to Server Auth with Amazon Cognito](https://lobster1234.github.io/2018/05/31/server-to-server-auth-with-amazon-cognito/) Step-by-Step guide Client Credentials Grant OAuth2 flow implementation with AWS Cognito