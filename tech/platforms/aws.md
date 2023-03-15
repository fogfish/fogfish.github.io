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

[The million dollar engineering problem](https://segment.com/blog/the-million-dollar-eng-problem/) how to identify and fix a problem of hot partitions at DynamoDB. 

[Best practices for managing many-to-many relationships](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-adjacency-graphs.html) Adjacency lists are a design pattern that is useful for modeling many-to-many relationships in Amazon DynamoDB. More generally, they provide a way to represent graph data (nodes and edges) in DynamoDB.

[The Ten Rules for Data Modeling with DynamoDB](https://www.trek10.com/blog/the-ten-rules-for-data-modeling-with-dynamodb) Modeling your data in DynamoDB is significantly different than modeling in a traditional relational database. And if you try to model your DynamoDB table like your relational database, you'll be in a world of hurt...

[Data Modeling for DynamoDB Single Table Design](https://www.sensedeep.com/blog/posts/2021/dynamodb-singletable-design.html) best practices have evolved around DynamoDB single-table design patterns where one database table serves the entire application and holds multiple different application entities.


## S3

[IAM Policies and Bucket Policies and ACLs! Oh, My! (Controlling Access to S3 Resources)](https://aws.amazon.com/blogs/security/iam-policies-and-bucket-policies-and-acls-oh-my-controlling-access-to-s3-resources/) the distinction between IAM policies, S3 bucket policies, S3 ACLs, and when to use each. They’re all part of the AWS access control toolbox, but they differ in how they’re used.

## RDS / Aurora

[Is Aurora PostgreSQL really faster and cheaper than RDS PostgreSQL – Benchmarking](https://www.migops.com/blog/2021/11/26/is-aurora-postgresql-really-faster-and-cheaper-than-rds-postgresql-benchmarking/) why there is a huge CPU utilization on their Aurora instances. Because, this did force our customers to upgrade their Aurora Instance types to resolve performance issues. Some customers have also seen Aurora IOPS being the major reason for heavy bills on Aurora PostgreSQL. Some of our customers also got surprised looking at some wait events on Aurora PostgreSQL that are never seen on PostgreSQL documentation.

[Deep Dive on Amazon Aurora](https://av.tib.eu/media/49124) In this session we will dive deep into the unique features and changes that make up including understanding the architectural differences that contribute to improved scalability, availability and durability. Some of the items that we will cover are the elimination of checkpointing, removal of the log buffer and the use of a 4/6 quorum to improved durability and availability while reducing jitter. Other areas we will go over are improvements in vacuum and shared buffer cache as well some of our new features like Fast Clones and Performance Insight. To finish off the session we will walk through the techniques used to migrate to Aurora PostgreSQL.

[Upgrading the PostgreSQL DB engine for Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.PostgreSQL.html) playbook on best practices of updating AWS RDS.

## Lambda

[Introducing AWS Lambda Extensions](https://aws.amazon.com/blogs/compute/introducing-aws-lambda-extensions-in-preview/) AWS Lambda is announcing a preview of Lambda Extensions, a new way to easily integrate Lambda with your favorite monitoring, observability, security, and governance tools. In this post I explain how Lambda extensions work, how you can begin using them, and the extensions from AWS Lambda Ready Partners that are available today.

[Caching data and configuration settings with AWS Lambda extensions](https://aws.amazon.com/blogs/compute/caching-data-and-configuration-settings-with-aws-lambda-extensions/) Post shows how to build a flexible in-memory AWS Lambda caching layer using Lambda extensions. Lambda functions use REST API calls to access the data and configuration from the cache. This can reduce latency and cost when consuming data from AWS services such as Amazon DynamoDB, AWS Systems Manager Parameter Store, and AWS Secrets Manager. See [demo of the extension](https://github.com/aws-samples/aws-lambda-extensions/tree/main/cache-extension-demo).

