---
layout: post
title:  Retain Confidentiality in Open Source Infrastructure.
description: |
  Open Source Infrastructure uses confidential cloud resources. Key Vault addresses confidentiality challenge if you open sources your AWS CDK infrastructure.
---


# Retain Confidentiality in Open Source Infrastructure

Infrastructure as a Code principles implies that your have ability to tear up/down entire setup at any point of time. If you cannot make it then you have some code, duct tape and WD-40. The usage of commodity cloud platforms makes unnecessary to maintain an infrastructure code in proprietary repositories. Anyway, comparable design patterns are used over and over again. The Open Source Infrastructure is next logical evolution, tools like [AWS CDK](https://docs.aws.amazon.com/cdk/latest/guide/home.html) makes infrastructure code sharable, composable and reusable in various use-cases.

Going to Open Source does not mean that you have to disclosure private configurations with entire community. How to leverage confidentiality and openness same time? I am asking this question every time when committing IaaC features to open source repositories. The Key Vault is a right tool to address confidentiality challenge. I've began to use [AWS Secret Manager](https://aws.amazon.com/secrets-manager/) in my projects. Let me elaborate here...

Often, Open Source Infrastructure uses existing cloud resources or config such as Hosted Zone Id, S3 Bucket Names, Internal Domain Names, not mentioned a secret keys. No one benefits if this information is exposed. It would not allow to re-use **high-order** infrastructure component. Its have to be configurable via side-channel. AWS CDK allows to use either stack parameters or environment variables. However, this does not sound as an ultimate solution to retain confidentiality. You advance a problem to next level. Your CI/CD gets a responsibility to parametrize infrastructure deployment with confidential configuration. A circle is closed - how to protect information at CI/CD? CI/CD advertises the usage of encryption methods or key vaults. 

## Key Vault (AWS Secret Manager)

It is possible to use [AWS Secret Manager](https://aws.amazon.com/secrets-manager/) directly from infrastructure code and making it to be a central place to keep sensitive configuration. As a side node, the secret manager uses [AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-secrets-manager.html) inside. 

The secret store becomes a config repository for your infrastructure. You can use aws command line tool to manage config as JSON objects

```bash
cat > config.json << EOF
{
  "confidential": "secret"
}
EOF

aws secretsmanager create-secret \
  --name MyConfig \
  --secret-string file://config.json
```

Once the storage is defined, use aws command line to retrieve and update values.

```bash
aws secretsmanager get-secret-value \
  --secret-id arn:aws:secretsmanager:eu-west-1:000000000000:secret:MyConfig-xxxxxx \
| jq '.SecretString | fromjson' > config.json

aws secretsmanager update-secret \
  --secret-id  arn:aws:secretsmanager:eu-west-1:000000000000:secret:MyConfig-xxxxxx \
  --secret-string file://config.json
```

## Key Vault usage with AWS CDK

Once, the config is defined you are able to use confidential data inside open source infrastructure. Let me show a two examples for you.

The first examples uses class hierarchy of AWS CDK, please see the [official](https://docs.aws.amazon.com/cdk/latest/guide/get_secrets_manager_value.html) guideline about it.

```typescript
import * as secret from '@aws-cdk/aws-secretsmanager'
import * as dns from '@aws-cdk/aws-route53'

// fetch secret
const config = secret.Secret.fromSecretAttributes(stack, 'Secret', {secretArn: 'MyConfig'})
const secret = config.secretValueFromJson('confidential').toString()

// use secret
const zone = dns.HostedZone.fromHostedZoneAttributes(parent, 'HostedZone', {hostedZoneId: secret})
```

## High Order Component: config

Let's shift a focus from category of class hierarchy to category of pure functions. It is based on [purely functional AWS CDK extension](https://github.com/fogfish/aws-cdk-pure). The library `aws-cdk-pure-hoc` implements `config` component to fetch data from AWS Secret manager. The component provides a single function to read `string` values

```typescript
function String(key: string, bucket?: string ): IPure<string>
```

If `bucket` is not defined then `AWS_IAAC_CONFIG` environment variable is used.


Here is a full example of `config` HoC usage 

```typescript
import * as config from 'aws-cdk-pure-hoc/config'

// fetch secret
config.String('confidential', 'MyConfig').flatMap(MyHostedZone)

// use secret
function MyHostedZone(secret: string) { ... }
```

## Key Management Service (AWS KMS)

Sometimes, We shall pass secrets to applications or runtime environment. Secrets must not be visible while they are passing through CI/CD, AWS Cloud Formation, etc. [AWS KMS](https://aws.amazon.com/kms/) helps to retain end-to-end encryption. AWS CDK rocks again here, we can simplify a key management process

Let's create our KMS key

```typescript
import * as iam from '@aws-cdk/aws-iam'
import * as kms from '@aws-cdk/aws-kms'

const key = new kms.Key(this, 'MyKey')
key.addAlias('app/key')

// Allow any one in the account to use a key if valid IAM role exists
key.grantDecrypt(new iam.AccountPrincipal(cdk.Aws.ACCOUNT_ID))

// Give AWS User permissions to use the key
key.grantEncryptDecrypt(
  new iam.ArnPrincipal(`arn:aws:iam::${cdk.Aws.ACCOUNT_ID}:user/${username}`)
)
```

Encrypt/Decrypt your secrets with [aws command line](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) utility.

```bash
export KEY=alias/app/key

aws kms encrypt \
  --key-id $KEY \
  --plaintext "PlainText" \
  --query CiphertextBlob \
  --output text

aws kms decrypt \
  --ciphertext-blob fileb://<(echo "CryptoText" | base64 --decode) \
  --query Plaintext \
  --output text | base64 --decode
```

Finally, you need to grant permission to other IAM roles to utilise the key

```typescript
const key = kms.Key.fromKeyArn(this, 'KMS', 'arn:...')
key.grantDecrypt(/* iam.IRole */)
```

## Afterwords

The twelve-factor application principles advices environment variables to store the config.

> Env vars are easy to change between deploys without changing any code; unlike config files, there is little chance of them being checked into the code repo accidentally; and .. they are a language- and OS-agnostic standard.

The discussed technique gives you a secure approach to retain confidentiality of your configuration. It is an environment variables managed by Key Vault service.
