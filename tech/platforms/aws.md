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

## CDK

[Deploying your CDK app to different stages and environments](https://taimos.de/blog/deploying-your-cdk-app-to-different-stages-and-environments) define all your stages for your workload within the same CDK app and configure the differences using custom stack properties, deploy all stages from the same branch and pipeline execution by synthesizing once and using the cloud assembly to run the same artifacts and with the same settings in all stages.


## CloudFront

[A/B Testing with Lambda@Edge](https://medium.com/buildit/a-b-testing-on-aws-cloudfront-with-lambda-edge-a22dd82e9d12) - Imagine you have a static website or a Single Page Application served through the CDN. You want to experiment two versions with actual users. 

[A Green/Blue deployment to AWS](https://serverfault.com/questions/714742/blue-green-deployments-with-cloudfront) - CloudFront requires the CNAME in the distribution config to be unique across your entire account. So controlling blue/green via DNS to different distributions will not work. There is a hack rolling around that would use wild cards but that makes no guarantee that the correct files are served. Controlling blue/green via DNS and CloudFront is not feasible.

[Secure Your Static Website with AWS CloudFront and Lambda](https://vthub.medium.com/lambda-edge-and-jwt-authentication-to-protect-sensitive-components-of-your-reactjs-app-901e0c10fd35) One of the possible applications of Lambda@Edge is pre-processing and post-processing of the requests that flow through CloudFront. Therefore Lambda@Edge can be used to authorize the user to access a resource behind CloudFront. This article covers an approach on how to protect sensitive parts of your Single Page Application written using ReactJS by leveraging both frontend and backend Authorization, AWS Cognito, Lambda@Edge and CloudFront.

[Authorization@Edge – How to Use Lambda@Edge and JSON Web Tokens to Enhance Web Application Security](https://aws.amazon.com/blogs/networking-and-content-delivery/authorizationedge-how-to-use-lambdaedge-and-json-web-tokens-to-enhance-web-application-security/) Authorization, the function of specifying access rights to resources is often required to help protect restricted content in web applications. This post will show you how to implement a serverless authorization of viewers using Amazon CloudFront, Lambda@Edge and Amazon Cognito without modifying your origin resources.


## Cognito

[Understanding Amazon Cognito user pool OAuth 2.0 grants](https://aws.amazon.com/blogs/mobile/understanding-amazon-cognito-user-pool-oauth-2-0-grants/) AWS Cognito is simplest replacement of OAuth2 Authorization Server, which is configurable using IaC principles. In addition to using the Amazon Cognito-specific user APIs to authenticate users, Amazon Cognito user pools also support the OAuth 2.0 authorization framework for authenticating users. The article explains supported flows and Cognito nuances on using them.

[Server to Server Auth with Amazon Cognito](https://lobster1234.github.io/2018/05/31/server-to-server-auth-with-amazon-cognito/) Step-by-Step guide Client Credentials Grant OAuth2 flow implementation with AWS Cognito

[Adding Advanced Security to a User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-advanced-security.html) 

## KMS

[AWS Key Management Service Best Practices](https://d0.awsstatic.com/whitepapers/aws-kms-best-practices.pdf) tells about designing maintainable solution with AWS KMS. Highlights design pattern about keys access controls, aliases and using the service at scale.  

## IAM

[Permissions boundaries for IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) A permissions boundary is an advanced application of a managed policy to set the maximum permissions that an identity-based policy can grant to an IAM entity.

## Dynamo

[How to Model Any Relational Data in DynamoDB to Maximize Performance](https://edward-huang.com/best-practice/database/2021/04/13/how-to-model-any-relational-data-in-dynamodb-to-maximize-performance/) Designing a Database application is the first thing we usually do when we want to start working on an application. DynamoDB is popular because it was designed for enormous, high-velocity use cases, such as the Amazon shopping cart. Thus, it can’t tolerate the inconsistency and slowing performance of joins as a dataset scales. Although DynamoDB is performant, designing a data model in DynamoDB is tricky. For instance, we cannot think about how to normalize the data to avoid anomalies because DynamoDB is a NoSQL database.

[The million dollar engineering problem](https://segment.com/blog/the-million-dollar-eng-problem/) how to identify and fix a problem of hot partitions at DynamoDB

## S3

[IAM Policies and Bucket Policies and ACLs! Oh, My! (Controlling Access to S3 Resources)](https://aws.amazon.com/blogs/security/iam-policies-and-bucket-policies-and-acls-oh-my-controlling-access-to-s3-resources/) the distinction between IAM policies, S3 bucket policies, S3 ACLs, and when to use each. They’re all part of the AWS access control toolbox, but they differ in how they’re used.
