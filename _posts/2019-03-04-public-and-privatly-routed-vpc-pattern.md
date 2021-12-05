---
layout: post
title:  Public and Privately Routed VPC pattern
description: |
  The routed VPC pattern is an universal and scalable pattern for AWS networking architecture.
tags:
  - coding
  - aws
  - aws vpc
  - infrastructure as a code
  - security
  - scalability
  - networking
  - cloud pattern
---

# Public and Privately Routed VPC pattern

My initial networking setup was extremely simple, I've followed a pattern - [VPC with a Single Public Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario1.html). It served me for awhile until, I've got a new operational requirements:

1. Support deployment of internal back-end HTTP(S) services.
2. Interoperability of serverless components with traditional Docker-based one.
3. Support multiple environments such as dev, live and others.

Essentially, I need to migrate my system to another well-advertise pattern - [VPC with Public and Private Subnets (NAT)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html).

## Background

There are few excellent articles in the Internet about this. I strongly advise to read them. 

[VPC with Public and Private Subnets (NAT)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html) - official AWS documentation clearly defines the architecture and required components. Unfortunately, it misses Infrastructure as a Code. At the end of the article, it provides a walk-though in the AWS Console, which is not advisable in real life.

[AWS NETWORKING, ENVIRONMENTS AND YOU](https://charity.wtf/2016/03/23/aws-networking-environments-and-you/) - excellent overview about possible configuration options to support multiple environment configurations. This article helped me to make mind how to implement support of multiple environments for my case.

[Practical VPC Design](https://medium.com/aws-activate-startup-blog/practical-vpc-design-8412e1a18dcc) - step by step practical guidance how to implement scalable VPC. The article talks about scalable address space allocation between VPCs, Availability Zones and Subnets.

## My solution

My solution is a consolidation of these articles into AWS Cloud Formation template that spawn the networking infrastructure in my account. The networking is designed to support multi-tier applications and splits concerns between Internet facing and privately routed back-end systems.

{: .lh-0 }
```
                +----------------------------------------------------------+
                |VPC                                                       |
                |                                                          |
                |                                                          |
                |                                                          |
                |                        +-----------+                     |
                |              +---------+Network ACL+--------+            |
                |              |         +-----------+        |            |
                |              |                              |            |
                |              |                              |            |
                |     +--------v----------+         +---------v---------+  |
                |     |Public Subnet      |         |Private Subnet     |  |
                |     |                   |         |                   |  |
                |     |     +------+      |         |                   |  |
Elastic IP  <---------+     |NAT GW|<-----|----+    |                   |  |
                |     |     +------+      |    |    |                   |  |
                |     |                   |    |    |                   |  |
                |     +-------------------+    |    +-------------------+  |
                |                              |                           |
                |     +-------------------+    |    +-------------------+  |
                |     |Public             |    |    |Private            |  |
Internet GW <---------+Routing            |    +----+Route              |  |
                |     |Table              |         |Table              |  |
                |     +-------------------+         +-------------------+  |
                |                                                          |
                +----------------------------------------------------------+

```

I'd like to highlight a few best practices that help me design scalable and maintainable networking solution.

1. Existed implementation divides VPC network ranges evenly across two availability zones `eu-west-1a` and `eu-west-1b` in region. Half of VPC capacity is reserved for expansion to other availability zones.

1. Each availability zone allocates its CIDR blocks between two subnets public for Internet facing services and private for back-end. Half of availability zone capacity is reserved for further subnet expansion.

1. VPC environments are isolated each other. I've built a dedicated networking subsystems for development and live environments with explicit restrictions on traffic routing between them. However, my solution supports VPC peering option to temporary link these environments in case of data migration use-cases.

1. VPC configurations do not use overlapping CIDR blocks anywhere.

1. We are using private subnets and availability zone allocation to keep data close to applications.

1. The spin-off of the infrastructure is automated using AWS Cloud Formation service.


The deployment of AWS resources in this design is grouped by unique routing requirement and deployed to appropriates subnets. Nodes and other resources in public subnets are directly routable from the Internet. Resources in private subnets are only routable within VPC boundaries but they do have egress Internet connectivity via NAT Gateway. The model supports a scalability of public/private subnets once new routing requirements arises in the system. DevOps access to AWS resources is provisioned by SSH, access to private subnet requires multi-hop SSH. AWS resources within the VPC network do not have any restrictions to access internal, external or AWS services, configurations are provisioned either using Security Groups of IAM roles.

## Lesson learned

The implementation of this schema is trivial with AWS CloudFourmation. However, I've made a few learning, worth of sharing here.

**Layered stacks** 

I've started with monolith Cloud Formation template that spin off [entire infrastructure](https://github.com/fogfish/ecsd/blob/master/rel/ecs.yaml): networking gears, load balancers, compute resources, etc. The maintainability of it becomes a headache. It might even lead you to situation when you cannot apply updates without a downtime. Therefore, you have to split you solution for few independent layers - VPC networking shall be deployed in its own template.


**Environment identity**

Encode environment identity into CIDR block allocated to VPC.

```yaml
Vpc:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: !Sub 10.${EnvId}.0.0/16
      ...
```

**Layout**

I've used a following CIDR blocks for each VPC deployments.

```
VPC CIDR
   10.0.0.0/16: (Mask: 255.255.0.0 Host: 65534)
      10.0.0.0/18   - AZ A (Mask: 255.255.192.0, Host: 16382)
      10.0.64.0/18  - AZ B (Mask: 255.255.192.0, Host: 16382)
      10.0.128.0/18 - Spare 

AZ CIDR
   10.0.0.0/18: (Mask: 255.255.192.0, Host: 16382)
      10.0.0.0/19
         10.0.0.0/20  - Private (Mask: 255.255.224.0, Host: 4094)
         10.0.16.0/20 - Spare 
      10.0.32.0/19
         10.0.32.0/20 - Public (Mask: 255.255.240.0, Hosts: 4094)
         10.0.48.0/20 - Spare
```

**Export**

Use AWS CloudFormation Output to export VPC configuration. 

```yaml
Output:

  VpcId:
    Value: !Ref Vpc
    Export:
      Name: !Sub ${AWS::StackName}

  CIDR:
    Value: !Sub 10.${EnvId}.0.0/16
    Export:
      Name: !Sub ${AWS::StackName}-cidr

  SubnetPublicA:
    Value: !Ref SubnetPublicA
    Export:
      Name: !Sub ${AWS::StackName}-public-a

  SubnetPublicB:
    Value: !Ref SubnetPublicB
    Export:
      Name: !Sub ${AWS::StackName}-public-b

  SubnetPrivateA:
    Value: !Ref SubnetPrivateA
    Export:
      Name: !Sub ${AWS::StackName}-private-a

  SubnetPrivateB:
    Value: !Ref SubnetPrivateB
    Export:
      Name: !Sub ${AWS::StackName}-private-b
```

It helps you later to reference the variable from other stacks 

```yaml
  EcsLoadBalancer:
    Type: "AWS::ElasticLoadBalancingV2::LoadBalancer"
    Properties:
      ...
      Subnets: 
        - Fn::ImportValue: !Sub ${Env}-vpc-private-a
        - Fn::ImportValue: !Sub ${Env}-vpc-private-b
```

Next thing to investigate: How to implement Public and Privately Routed VPC pattern with [AWS CDK](https://docs.aws.amazon.com/cdk/latest/guide/home.html) using TypeScript.
