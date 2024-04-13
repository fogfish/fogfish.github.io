# Thought on Infrastructure as a Code

There are variety of definitions of Infrastructure as a Code (IaC):

> Quotation from Wikipedia: “The process of managing and provisioning computer data centers through machine-readable definition files, rather than physical hardware configuration or interactive configuration tools.”

> Some other people say: “No one should log in to a new machine and configure it from documentation.”

> The way I think about it: “IaC principle implies that you have ability to tear up/down entire setup at any point of time. If you cannot make it then you have some code, duct tape and WD-40.”

All these considerations strictly imply everything is pure code developed with general purpose programming language(s). Any infrastructure changes must be defined in the code. Nothing should be done manually. It makes management of the operation environment similar to application or any code. This unification emerges to the best practice of [DevOps](https://medium.com/it-dead-inside/devops-is-a-matter-of-attitude-as-much-as-aptitude-339a59b942b9), “where operations and development engineers participating together in the entire service lifecycle, from design to through the development to the production support”.

The direct cause of DevOps is the Service model, which is about development, delivery and operating the software. Modern software companies lead internal organization development towards service teams. A team of 5+ engineers is fully responsible for supporting the entire lifecycle for a few microservices. Usually the scope of the service team is wider than just coding. Teams are building a solution as a collection of stateless microservices, backing services and admin processes (here I am using [12 factor](https://12factor.net/) terminology). The usage of standard tools like AWS CDK, CloudFormation or Terraform is a solid approach to specify the execution environment for their solutions.

It is not enough to script “cloud” API. Scripts are heavily impacted by [the software erosion](https://en.wikipedia.org/wiki/Software_rot) - degradation over time that eventually makes them obsolete. A script does not decay by itself but rather suffers from bad engineering practices, lack of updates with respect to the environment changes.

## History of IaC

IaC is not a new concept at all. It has been developed for past decades and passed a few important milestones: 

**Manual** IaC is an ancient style used before DevOps epoch ([The first DevOpsDays Conference](https://devops.com/the-origins-of-devops-whats-in-a-name/) was held in 2009). No one simply uses manual style to configure infrastructure anymore. Any manual ops leads to configuration drift and automation fear cycle: Any changes outside of automation cycle leads to inconsistent servers, inconsistency leads to fear that automation breaks servers.

**Imperative** IaC consists of commands for the computer to perform (Bash, Python and other scripting languages). Scripts only focus on how the infrastructure changes to meet objectives. The largest problem with imperative style is fault tolerance and error handling. It is required to enumerate in the code ways to apply corrections changes in fragments with excessive rules about cause-effect sequence. Outage until convergence is reached is what you get with imperative style. Chef, Puppet and Ansible are great examples of imperative tools. 

**Declarative** IaC describes what is the infrastructure without explicit definition of commands to be performed. Developers use generic programming languages to express target configurations that are eventually materialized by IaC engine. I’d like to make an emphasis on Amazon Web Service offering. The first [public release](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/ReleaseHistory.html) of AWS CloudFormation was made in 2011. Recently, they have made significant improvement with [AWS CDK](https://github.com/aws/aws-cdk). It was released in 2019, the same year it has been acknowledged as fastly growing open source product.

## AWS CDK

Leverage IaC to **idempotence**. It does not matter how many times code is executed, it always leads to the exact same result. Same input, same infrastructure. 

Strictly type safe language (TypeScript) is used to declare what is the target infrastructure. The development is held using type-safe constructs, each construct corresponds to AWS concept. A familiar tools and techniques speed-up development and static type checks at compile time reduces the possibility of errors. AWS CDK transpile the TypeScript code to AWS Cloud Formation that orchestrates provisioning of cloud components.

## Why to use IaC

IaC delivers few measurable benefits to any service business: cost reduction, faster execution and mitigation of risks. 

**Reduce Cost** associated with the overhead required to execute [toil activity](https://landing.google.com/sre/workbook/chapters/eliminating-toil/). Engineers refocus their effort towards feature development and other business valuable activities. By coding the entire operation environment, the team accomplishes automated and repeatable processes.

**Increase Speed** of iteration cycles. It shortens deployment time but also provides feature visibility to other teams and stakeholders. Usage of IaC unlocks an opportunity to release software at any point in time. For example, [Instagram](https://instagram-engineering.com/continuous-deployment-at-instagram-1e18548f01d1) deploys backend code 30 - 50 times a day.

**Mitigate Risks** of violating robustness/reliability, security or compliance. Humans are one of the factors of misconfigurations, lengthy downtime and any pitfalls during service maintenance. 

IaC also resolves a few non-functional requirements, they simplify the ops part.

**Version control** attributes and records history of infrastructure changes that provides point in time recovery in a matter of minutes. Additionally, you expand normal software development practice to your infrastructure such as branching, tagging, reviewing, change approval, team collaboration and detailed audit trail for changes.

Often infrastructure components are untestable, or not immediately testable. Defects are discovered at runtime with a cost of downtime. The usage of general purpose programming languages improves **testability**. Usual practice of unit and integration testing is directly applicable to your infrastructure. Infrastructure assets are reused across test/live environments. 

IaC is the primary tool to achieve **immutable** deployments that resolves aspects of availability and fault tolerance due to regression in software quality or configurations. 

**Disposability** is an amazing property of software systems. Often, engineers use it with negative connotation. We do not need to spend resources to maintain the infrastructure when it is disposable. Ad-hoc provision (elasticity) is a positive side-effect of disposability, you spawn the infrastructure on-demand. 

IaC ensures **reusability** of infrastructure patterns. Once the infrastructure has been developed, it can be instantiated and reused countless times.

## Immutable Deployment (Green/Blue) with AWS CDK

Continuous CI/CD defines team philosophy and commitment to ensuring that your code/service is always in a release-ready state. It is also an implementation of pipelines to deploy every commit to feature sandbox environment with following promotion to production. Continuous deployment comes with the possibility of continuous downtime. Live upgrades causes a risk of occasional outage. Continuous deployment goes hand-in-hand with a need for clustering, failover, and other high-availability infrastructure. 

The immutable deployment is the solution for availability and fault tolerance due to software regression. Immutable deployment is the ability to apply changes by rebuilding service. 

Never change your infrastructure and application configuration at running systems. Deploy a new copy as a parallel stack. IaC is a tool to achieve this. 

As a result, you’ll get a “time machine” to rollback defective software in a matter of minutes. Another advantage is the ability to split traffic between parallel deployments for quality assurance. Finally, you’ll get rid of the staging environment for read-only services.


## Afterword

IaC imposes a higher cost at the beginning. The learning curve is smooth but knowledge about cloud technologies is required. For example, a basic understanding about AWS Compute/Storage Services and AWS Cloud Formation is required before jumping into AWS CDK development. The time investment is worth it. IaC delivers measurable benefits: cost reduction, faster execution and mitigation of risks.
