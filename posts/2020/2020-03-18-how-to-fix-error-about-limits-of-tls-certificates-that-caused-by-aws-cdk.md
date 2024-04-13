# How To Fix Error About Limits of TLS Certificates That Caused by AWS CDK

Continuous delivery is a team philosophy and commitment to ensuring that your application is always in a release-ready state. It is also an implementation of pipelines to deploy every commit to the feature sandbox environment with the following promotion to production. Tools like AWS CDK enables development opportunities:

* Increase Speed of iteration cycles. It shortens deployment time but also provides early visibility to other teams and stakeholders. Usage of IaC unlocks an opportunity to release software at any point in time.

* Disposability is an amazing property of software systems. Often, engineers use it with negative connotation. We do not need to spend resources to maintain the infrastructure when it is disposable. Ad-hoc provision (elasticity) is a positive side-effect of disposability, you spawn the infrastructure on-demand.

* The immutable deployment is the solution for availability and fault tolerance due to software regression. Immutable deployment is the ability to apply changes by rebuilding an entire service from scratch. Never change your infrastructure and application configuration at running systems. Deploy a new copy as a parallel stack. IaC is a tool to achieve this.

## Continuous Delivery Workflow

I advocate deployment automation workflows for reasons listed above. My development workflow does not differ at all from well-known forking or branching models. It just emphasizes continuous deployment as a key feature along the workflow. It supports integration testing and helps to eliminate all related issues at earlier phases of feature delivery process:

* The master branch of your project is always the latest deployable snapshot of a software asset. AWS CDK and IaC code automates the master snapshot deployments every time when a new feature is merged.

* The feature integration into master is implemented through pull request. IaC executes automated pull request deployment to the sandbox environment every time a new change is proposed by developers (each commit). The deployments happen after quality checks are successfully completed. The sandbox environment gives you the possibility to execute integration tests, share intermediate work with colleges and other stakeholders.

* The merge of pull request triggers the deployment of master branch into the latest environment. Use this environment for features validation before delivery to the live.

* The delivery of the latest environment to live is automated using git tags. A provisioning a new tag caused a new immutable deployment of your application to the live environment, which makes it compliant with green/blue deployment schemas.


## You have reached your limit

I've employed the depicted workflow with few serverless applications. I've made sure that all required resources are spawned for every new instance of application. Unfortunately, the deployment of my application start failing after a few deploy/dispose cycles:

```
Error: you have reached your limit of 20 certificates in the last year.
```

Exactly same issue has been experienced by other developers: [#5889](https://github.com/aws/aws-cdk/issues/5889), [#142](https://github.com/aws-quickstart/quickstart-redhat-openshift/issues/142). The root cause of the error is an automation on TLS Certificate. The following CDK code requests a new certificate from AWS Certificate Manager.

```ts
import * as acm from '@aws-cdk/aws-certificatemanager'

const cert = new acm.DnsValidatedCertificate(this, 'Cert', { domainName, hostedZone })
```

AWS exposes [limits](https://docs.aws.amazon.com/acm/latest/userguide/acm-limits.html) on the number of requests per AWS account. The hard limit is about 2000 requests per year. AWS CDK consumes certificate request quota on every deployment. It does not release it when the stack is destroyed. Automated certificates provision is not really an option for teams if they do few deployments per day.

## Solution on the issue

It would be amazing to bind the request limit with the number of active certificates so that we can really provision a certificate per application on demand. It remains to be seen if AWS will do anything about this issue. Meanwhile, I've adopted a technique of wildcard certificates for my applications. The strategy is simple - create the certificate for your domains `*.example.com` and then re-use this certificate across multiple instances of applications. You have to ensure that these wildcard certificates are suitable for development, live and sandbox environments. The opportunity is unlimited, you are able to create the certificate manually or orchestrate its deployment using cross-stack references. Let's have a quick walk through second option:

```ts
import * as cdk from '@aws-cdk/core'
import * as dns from '@aws-cdk/aws-route53'
import * as acm from '@aws-cdk/aws-certificatemanager'

//
// Common stack creates a certificate and exports its.
// The stack is deployed only once during life-cycle of application.
//
class StackA extends cdk.Stack {
  readonly tlsCertificate: string

  constructor(node: cdk.App, id: string, props: cdk.StackProps) {
    super(node, id, props)
 
    const hostedZone = dns.HostedZone.fromLookup(this, 'HostedZone',{ domainName })
    const cert = new acm.DnsValidatedCertificate(this, 'Cert', { domainName, hostedZone })
    this.tlsCertificate = cert.certificateArn
  }
}

//
// Application stack deploys application resource, which depends on
// existing tlsCertificate. The application stack can be destroyed/deployed
// unlimited amount of time. It would not cause re-create of the certificate.
//
class StackB extends cdk.Stack {
  constructor(node: cdk.App, id: string, props: cdk.StackProps, tlsCertificate: string) {
    super(node, id, props)
    // Use Certificate
  }
}

const app = new cdk.App()
const stack = {
  env: {
    account: process.env.CDK_DEFAULT_ACCOUNT,
    region: process.env.CDK_DEFAULT_REGION,
  }
}

const common = new StackA(app, 'common', stack)
new StackB(app, 'application', stack, common.tlsCertificate)
app.synth()
```

Here, you can find a fully functional [application](https://github.com/fogfish/aws-cdk-pure/tree/master/example/reusable-tls-certificate). It demonstrates the re-use of certificates in actions.

```
cdk deploy -c domain=example.com -c subdomain=myapp application
```

The usage of cross-stack references allows you to deploy a TLS certificate only once using AWS CDK IaC principles. You are not limited with redeployment cycles of actual application. You can safely enforce continuous deployment workflow for your application without any issue with AWS Certificate Manager quotas.
