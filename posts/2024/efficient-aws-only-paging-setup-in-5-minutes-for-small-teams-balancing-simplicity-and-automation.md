# Efficient AWS only Paging Setup in 5 minutes for Small Teams: Balancing Simplicity and Automation

When developing and operating a customer-facing app, it's essential to maintain visibility into traffic to ensure both a smooth user experience and effective resource allocation. Without proper monitoring, sudden traffic spikes—whether from genuine interest or malicious actors—can quickly lead to what's known as a "Denial of Wallet" attack, where unexpected costs accrue due to excessive usage. To safeguard against this, it’s crucial to set up automated paging and alerts that notify the team of unusual traffic patterns, enabling timely responses and budget control. With AWS's robust but complex setup tools, even small teams require a step-by-step playbook to effectively monitor their app’s usage and stay within marketing and operational budgets, ensuring that promotional efforts drive engagement without compromising financial stability.

Setting up an effective paging system for a small, pizza-sized team comes with distinct constraints, especially given our commitment to building entirely within the AWS ecosystem, including AWS Bedrock for experimental features accessed indirectly by our users. To maintain simplicity and reduce setup time, we’re aiming to minimize third-party solutions for paging, keeping our team focused on core development tasks without extra complexity. Additionally, it’s essential that our responders distinguish between critical alerts and personal messaging, ensuring that signals aren’t missed due to phone silence or focus profiles, so we can respond swiftly to issues that matter most.

Our solution for setting up an effective paging system leverages AWS-native tools to keep things simple and efficient for our small team. We use AWS CloudWatch Alarms to monitor key metrics and trigger alerts based on defined thresholds. When an alarm is activated, it routes through AWS Simple Notification Service (SNS), which sends an SMS directly to the mobile phone of the designated responder. Using AWS SNS in sandbox mode provides a straightforward, cost-effective way to ensure timely notifications without needing complex or third-party integrations, making this setup both manageable and responsive to our team’s needs.

**DESIGN ALARMS**. Start with identifying the essential AWS services to monitor based on the app’s core functionality. For each chosen service, we create targeted alarms that can be set up manually or, for efficiency, using Infrastructure as Code to maintain consistency and scalability. A key step in the design process is determining if specific metrics should be constrained by dimensions, such as region or instance type, to ensure the alarms are fine-tuned to our app’s needs. This approach keeps our monitoring relevant, minimizing noise while focusing on critical areas that require immediate attention.

For example, we need to monitor AWS Bedrock utilization closely and receive a page whenever it exceeds a defined threshold. The alarm is defined as
* Namespace `AWS/Bedrock`
* Metric name `Invocations`
* ModelId `meta.llama3-1-70b-instruct-v1:0` (can be skipped if overall usage in a point of interest) 
* Statistic `Sum`
* Period `5 minutes`

**CONFIG PAGING**. Start by setting up an SNS topic, which serves as the channel for your alerts.  In the AWS SNS console, create a new topic and name it according to your needs (e.g., "AlertNotifications"). Next, add your mobile phone number to the SNS sandbox; note that in sandbox mode, you can only send messages to verified phone numbers, so you’ll need to verify each phone by adding the number and following the verification steps provided by AWS.

Once verified, create a subscription that links your phone number to the SNS topic. Choose "SMS" as the protocol and enter your verified mobile number. Now, when an alarm or message is published to this SNS topic, the configured mobile phone will receive an SMS. This setup allows your team to test SMS-based alerts in a controlled environment without incurring costs from accidental or excessive messaging, ideal for a small team refining their paging setup.

Finally, bind the SNS topic with Alarm state trigger `In alarm` and `OK` so that you’ll be paged. The success setup would lead you to the message in the inbox:

```
NOTICE
ALARM: “High Utilisation AWS/Bedrock” in US East (N. Virginia)
```

**CRITICAL ALERT**. Most professional paging solutions utilize critical alerts, ensuring that notifications are delivered even when your phone is muted or a focus mode is activated. However, AWS SNS messages are sent using SMS Sender ID "NOTICE",  which may not be recognized as an emergency contact. While it is possible to set up Origination IDs to customize the sender information, this process is not straightforward and can add complexity to the setup. Therefore, finding a practical solution for ensuring critical notifications stand out requires careful consideration.

The Shortcuts app on the iPhone is a valuable tool for enhancing our paging system. We’ve set up automations that play music whenever an ALARM message is received, ensuring that critical alerts capture our attention even in busy environments.

```
Shortcuts > Automations

When I Get a Message Containing ‘ALARM’ 
Play Musing => artist “System of A Down” composition “Toxicity”
```

The band System of a Down's composition "Toxicity" is a powerful and ironic piece in the setup that showcases a unique blend. Enjoy it here: https://www.youtube.com/watch?v=iywaBOMvYLIC

**AFTERWARDS**. Using Infrastructure as Code (IaC) is an ideal automation goal, but is it worth the effort? We found that any automation of account-level observability, such as monitoring for "Denial of Wallet" attacks, typically isn’t necessary unless you’re operating a SaaS business where billing spikes are critical to manage. Given that setting up such alerts manually only takes about five minutes and is rarely needed, IaC may be overkill for smaller-scale, infrequent configurations. The baseline we've discussed provides a strong foundation, giving you a head start for scaling once the initial seeding phase is complete.

![Is it worth of time!](https://imgs.xkcd.com/comics/is_it_worth_the_time.png)

**REFERENCES**. 
* [Amazon CloudWatch template snippets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-cloudwatch.html)
* [Set custom text tone for named/alphanumeric sender ID](https://forums.macrumors.com/threads/set-custom-text-tone-for-named-alphanumeric-sender-id.2365376/)
* [Creating an outbound calling solution during a pandemic using Amazon Connect](https://aws.amazon.com/blogs/publicsector/creating-outbound-calling-solution-pandemic-using-amazon-connect/)
