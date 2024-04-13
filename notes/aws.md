# AWS

- [AWS](#aws)
  - [AWS CDK](#aws-cdk)
  - [AWS CloudFront](#aws-cloudfront)
  - [AWS Cognito](#aws-cognito)
  - [AWS KMS](#aws-kms)
  - [AWS IAM](#aws-iam)
  - [AWS DynamoDB](#aws-dynamodb)
  - [AWS S3](#aws-s3)
  - [AWS RDS \& AWS Aurora](#aws-rds--aws-aurora)
  - [AWS Lambda](#aws-lambda)


## AWS CDK

[Deploying your CDK app to different stages and environments](https://taimos.de/blog/deploying-your-cdk-app-to-different-stages-and-environments) define all your stages for your workload within the same CDK app and configure the differences using custom stack properties, deploy all stages from the same branch and pipeline execution by synthesizing once and using the cloud assembly to run the same artifacts and with the same settings in all stages.


## AWS CloudFront

[A/B Testing with Lambda@Edge](https://medium.com/buildit/a-b-testing-on-aws-cloudfront-with-lambda-edge-a22dd82e9d12) Lambda@Edge allows running Lambda functions at Edge Locations of the CloudFront CDN. It means you may add "intelligence" in the CDN, without having to forward the request to a backend and losing benefits of content caching and geographical proximity with the client. Lambda@Edge give an ability for ability server side A/B testing.

[A Green/Blue deployment to AWS](https://serverfault.com/questions/714742/blue-green-deployments-with-cloudfront) CloudFront requires the CNAME in the distribution config to be unique across your entire account. So controlling blue/green deployment traffic via DNS to different distributions will not work. Use wildcard certificate as suggested by the discussion. My own conclusion - management of blue/green deployments via DNS and CloudFront is not feasible.  

[Secure Your Static Website with AWS CloudFront and Lambda](https://vthub.medium.com/lambda-edge-and-jwt-authentication-to-protect-sensitive-components-of-your-reactjs-app-901e0c10fd35) One of the possible applications of Lambda@Edge is pre-processing and post-processing of the requests that flow through CloudFront. Therefore Lambda@Edge can be used to authorize the user to access a resource behind CloudFront. This article covers an approach on how to protect sensitive parts of your Single Page Application written using ReactJS by leveraging both frontend and backend Authorization, AWS Cognito, Lambda@Edge and CloudFront.

[Authorization@Edge – How to Use Lambda@Edge and JSON Web Tokens to Enhance Web Application Security](https://aws.amazon.com/blogs/networking-and-content-delivery/authorizationedge-how-to-use-lambdaedge-and-json-web-tokens-to-enhance-web-application-security/) Authorization, the function of specifying access rights to resources is often required to help protect restricted content in web applications. This post will show you how to implement a serverless authorization of viewers using Amazon CloudFront, Lambda@Edge and Amazon Cognito without modifying your origin resources.


## AWS Cognito

[Understanding Amazon Cognito user pool OAuth 2.0 grants](https://aws.amazon.com/blogs/mobile/understanding-amazon-cognito-user-pool-oauth-2-0-grants/) AWS Cognito is simplest replacement of OAuth2 Authorization Server, which is configurable using IaC principles. In addition to using the Amazon Cognito-specific user APIs to authenticate users, Amazon Cognito user pools also support the OAuth 2.0 authorization framework for authenticating users. The article explains supported flows and Cognito nuances on using them.

[Server to Server Auth with Amazon Cognito](https://lobster1234.github.io/2018/05/31/server-to-server-auth-with-amazon-cognito/) 

AWS Cognito is usable for server-to-server, machine-to-machine authentication. This is one of the most common scenarios in a microservices world, where services need to talk to other services securely, and using an established standard such as OAuth2. This is also known as client_credentials Grant, or 2-legged OAuth. Amazon Cognito provides a simple and cost effective option to implement Client Credentials Grant OAuth2 flow.

[Adding Advanced Security to a User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-advanced-security.html) Advanced security features include (1) compromised credentials detection - Amazon Cognito compiles data from public leaks of user names and passwords, (2) adaptive authentication - Amazon Cognito can review location and device information from your users' sign-in requests and apply an automatic response to secure the user accounts in your user pool against suspicious activity and (3) Access token customization of scopes and other claims in access tokens.

## AWS KMS

[AWS Key Management Service Best Practices](https://d0.awsstatic.com/whitepapers/aws-kms-best-practices.pdf) AWS Key Management Service (AWS KMS) supports many security features that you can implement to enhance the protection of your encryption keys, including key policies and IAM policies, an encryption context option for cryptographic operations on symmetric encryption keys, an extensive set of condition keys to refine your key policies and IAM policies, and grant constraints to limit grants.

## AWS IAM

[Permissions boundaries for IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) A permissions boundary is an advanced application of a managed policy to set the maximum permissions that an identity-based policy can grant to an IAM entity. Use the policy to harden the boundaries within your system.

## AWS DynamoDB

[How to Model Any Relational Data in DynamoDB to Maximize Performance](https://edward-huang.com/best-practice/database/2021/04/13/how-to-model-any-relational-data-in-dynamodb-to-maximize-performance/) Designing a Database application is the first thing we usually do when we want to start working on an application. DynamoDB is popular because it was designed for enormous, high-velocity use cases, such as the Amazon shopping cart. Thus, it can’t tolerate the inconsistency and slowing performance of joins as a dataset scales. Although DynamoDB is performant, designing a data model in DynamoDB is tricky. For instance, we cannot think about how to normalize the data to avoid anomalies because DynamoDB is a NoSQL database. In SQL, designing models are based on database normalization. We design our data according to these laws, first normal form, second normal form, and third normal form. However, understanding access pattern is key when designing a data model in DynamoDB. For instance, we design our partition key and sort key based on how the user usually seeks operation.

[The million dollar engineering problem](https://segment.com/blog/the-million-dollar-eng-problem/) DynamoDB is Amazon’s hosted version of a NoSQL database that acts as a combination K/V and document store. It has support for secondary indexes to do multiple queries and scans efficiently, and abstracts away the underlying partitioning and replication schemes. The Dynamo pricing model works in terms of throughput. As a user, you pay for a certain capacity on a given table (in terms of reads and writes per second), and Dynamo will throttle any reads or writes that go over your capacity. At face value, it feels like a fairly straightforward model: the more you pay, the more throughput you get. However, correctly provisioning the throughput required is a bit more nuanced, and requires understanding what’s going on under the hood. Badly designed schema would cause hot partitions at DynamoDB, throttling and extra costs.   

[Best practices for managing many-to-many relationships](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-adjacency-graphs.html) Adjacency lists are a design pattern that is useful for modeling many-to-many relationships in Amazon DynamoDB. More generally, they provide a way to represent graph data (nodes and edges) in DynamoDB. How to implement Adjacency list design pattern with DynamoDB.

[The Ten Rules for Data Modeling with DynamoDB](https://www.trek10.com/blog/the-ten-rules-for-data-modeling-with-dynamodb) Modeling your data in DynamoDB is significantly different than modeling in a traditional relational database. And if you try to model your DynamoDB table like your relational database, you'll be in a world of hurt: (1) Understand the basics of single-table design with DynamoDB, (2) Know your access patterns before you start, (3) Model first, code last, (4) Get comfortable with denormalization, (5) Ensure uniqueness with your primary keys, (6) Avoid hot keys, hot partitions, (7) Handle additional access patterns with secondary indexes, (8) Build aggregates into your data model, (9) Use ISO-8601 format for timestamps, and (10) Use On-Demand pricing to start.

[Data Modeling for DynamoDB Single Table Design](https://www.sensedeep.com/blog/posts/2021/dynamodb-singletable-design.html) best practices have evolved around DynamoDB single-table design patterns where one database table serves the entire application and holds multiple different application entities.


## AWS S3

[IAM Policies and Bucket Policies and ACLs! Oh, My! (Controlling Access to S3 Resources)](https://aws.amazon.com/blogs/security/iam-policies-and-bucket-policies-and-acls-oh-my-controlling-access-to-s3-resources/) the distinction between IAM policies, S3 bucket policies, S3 ACLs, and when to use each. They’re all part of the AWS access control toolbox, but they differ in how they’re used.


## AWS RDS & AWS Aurora

[Deep Dive on Amazon Aurora](https://av.tib.eu/media/49124) In this session we will dive deep into the unique features and changes that make up including understanding the architectural differences that contribute to improved scalability, availability and durability. Some of the items that we will cover are the elimination of checkpointing, removal of the log buffer and the use of a 4/6 quorum to improved durability and availability while reducing jitter. Other areas we will go over are improvements in vacuum and shared buffer cache as well some of our new features like Fast Clones and Performance Insight. To finish off the session we will walk through the techniques used to migrate to Aurora PostgreSQL.

[Upgrading the PostgreSQL DB engine for Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.PostgreSQL.html) playbook on best practices of updating AWS RDS.

[12 Golden Signals To Discover Anomalies And Performance Issues on Your AWS RDS Fleet](https://engineering.zalando.com/posts/2024/02/twelve-golden-signals-to-discover-anomalies-and-performance-issues-on-aws-rds.html) Database per service pattern in the microservices world brings an overhead on operating database instances, observing its health status and anomalies. Standardisation on methodology and tooling is a key factor for the success at the scale. We have incorporated learning from past incidents, anomalies and empirical observations into a methodology of observing the health status using 12 golden signals. The most simple way to adopt these methodology within your engineering environment is an open source utility [rds-health](https://github.com/zalando/rds-health) recently released by us.


## AWS Lambda

[Introducing AWS Lambda Extensions](https://aws.amazon.com/blogs/compute/introducing-aws-lambda-extensions-in-preview/) AWS Lambda is announcing a preview of Lambda Extensions, a new way to easily integrate Lambda with your favorite monitoring, observability, security, and governance tools. In this post I explain how Lambda extensions work, how you can begin using them, and the extensions from AWS Lambda Ready Partners that are available today.

[Caching data and configuration settings with AWS Lambda extensions](https://aws.amazon.com/blogs/compute/caching-data-and-configuration-settings-with-aws-lambda-extensions/) Post shows how to build a flexible in-memory AWS Lambda caching layer using Lambda extensions. Lambda functions use REST API calls to access the data and configuration from the cache. This can reduce latency and cost when consuming data from AWS services such as Amazon DynamoDB, AWS Systems Manager Parameter Store, and AWS Secrets Manager. See [demo of the extension](https://github.com/aws-samples/aws-lambda-extensions/tree/main/cache-extension-demo).

