---
title: 'AWS re:Invent 2025 - Actionable controls for improving governance and compliance (COP347)'
published: true
description: 'In this video, Rodolfo Brenes and Arnaud Coulondre demonstrate how to bridge the gap between abstract compliance requirements and actionable AWS controls. They introduce AWS Control Tower''s Control Catalog with over 1000 pre-mapped controls across 17 frameworks including PCI DSS and CIS, eliminating manual spreadsheet maintenance. The session covers three control types—preventive (SCPs, RCPs), detective (AWS Config), and proactive (CloudFormation Guard)—and shows how to discover, filter, and deploy controls at scale. They demonstrate service approval approaches, custom rule creation using Guard DSL and Amazon Q Developer CLI, and continuous monitoring through Security Hub''s CSPM capabilities with exposure findings and attack path visualization. The presentation emphasizes a risk-based approach starting from business objectives and leveraging AWS Artifact, CloudTrail Lake, and Config for comprehensive audit and compliance management.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/0.jpg'
series: ''
canonical_url: null
id: 3085597
date: '2025-12-05T06:55:20Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Actionable controls for improving governance and compliance (COP347)**

> In this video, Rodolfo Brenes and Arnaud Coulondre demonstrate how to bridge the gap between abstract compliance requirements and actionable AWS controls. They introduce AWS Control Tower's Control Catalog with over 1000 pre-mapped controls across 17 frameworks including PCI DSS and CIS, eliminating manual spreadsheet maintenance. The session covers three control types—preventive (SCPs, RCPs), detective (AWS Config), and proactive (CloudFormation Guard)—and shows how to discover, filter, and deploy controls at scale. They demonstrate service approval approaches, custom rule creation using Guard DSL and Amazon Q Developer CLI, and continuous monitoring through Security Hub's CSPM capabilities with exposure findings and attack path visualization. The presentation emphasizes a risk-based approach starting from business objectives and leveraging AWS Artifact, CloudTrail Lake, and Config for comprehensive audit and compliance management.

{% youtube https://www.youtube.com/watch?v=2bJ9F0tr29s %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/0.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=0)

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/40.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=40)

### Introduction: The Challenge of Translating Compliance Requirements into Actionable Controls

 Good morning. Welcome to Reinvent 2025. I'm very excited to be here with all of you today and to kick off this week with this session. This is COP 347, Actionable Controls for Improving Governance and Compliance, and let's get right into it. If we think of cybersecurity as a building, controls are its hidden architecture—those bolts and beams that actually hold every wall. But the challenge with compliance is that every regulatory compliance program starts with a legal abstract, like "organizations shall ensure  that controls are in place for encryption." So what does that mean? How do we bridge that gap? How do we move from those regulatory statements and abstract legal paragraphs to actually implementing controls?

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/90.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=90)

Thankfully, that's the goal for us today. We're going to go from implementation types to how you discover and map controls, and how you can assess and monitor those controls using AWS managed services. Hopefully by the end of this session, you will feel comfortable enough discussing strategies for how to implement those controls and what different services can help you achieve that in AWS. This is the agenda for today. We're going to go through the lifecycle of controls, and at the end we're going to have, of course, some key takeaways.  Let's start with a question: To whom and why are controls important?

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/100.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=100)

 Many of you might already be familiar with this, but we're going to have many stakeholders in the whole process. We're going to have our business personas. Those folks are looking for rapid market adoption, seizing new opportunities from the business side of the house, and keeping minimal barriers for innovation. We also have our developers. Our developers want autonomy in their work. They don't want to be asking for permissions every single time they're trying to do something new. They want to have that flexibility and minimal barriers for last-minute changes that they need to make because of some compliance concern or security issue. They want to be prepared from the start.

First of all, I would like to introduce myself. My name is Rodolfo Brenes. I'm a principal solutions architect for cloud governance, and today with me is Arnaud Coulondre. He's a principal product manager for the AWS Control Tower team, and we're very excited to have you here. Now, as I was mentioning, we also have our security personas, and this is one of the key ones. They want proactive controls instead of reactive controls. They want to make sure that they are following the security posture and guidelines of our organization. And not to say the least, we also have our risk managers. They want to make sure that the organization is following and adhering to the risk appetite of our company.

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/230.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=230)

All of these play together, and the key is striking the right balance between all these personas. But the problem is that something people sometimes forget is that without senior sponsorship or leadership participation in this, it's very complex. We need to make sure that we have that before we even start because we can have all these multiple personas with competitive needs, and if we don't have that, it's very hard to make sure that everyone works together and we are looking toward one single goal. That's very important as well. Now, as we just saw, we have many stakeholders in AWS. We see cloud governance as a  key core component of every part of the organization. We see this in four main pillars, and the whole idea is to provide you the proper foundation that goes from AWS accounts to the guardrails that you put in place to all those controls and how you can actually automate that. We do that focusing on four key domains.

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/260.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=260)

The first one is security, of course, as we just mentioned. This is looking to make sure that we are following the security program of that organization.  Then we have our operations team. They're also concerned about governance because that will help them make sure that they're performing as expected. What is expected? Are they resilient enough? Security is not just part of cloud governance. Then we have our compliance teams. Our compliance objectives are basically making sure that you are also being secure, you're operating as expected, but you're aligning to your internal and external policies. And then, last one, we have cost. This is very important. We also need governance for cost because we want to make sure that we are operating successfully while being cost effective. So what are the things that we can do here as well?

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/320.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=320)

### Governance Challenges and the Risk-Based Approach Framework

We need to focus on every one of these objectives, but what we're really doing is mitigating risk on every single domain. That's the whole idea. With that in mind, we start seeing some challenges. The first one is striking the right balance between innovation  and controls. How do you keep that right balance of your developers doing what they want to do while making sure that you are following the best practices of your company in terms of security?

How do you keep pace with evolving regulatory environments? This is something that we see with a lot of our customers. Every month, we see regulatory changes. The last year has been really chaotic in terms of regulatory changes. The EU Data Act, for example, and everything related to generative AI are things that keep evolving. It's not just that every single time you may see something new, but it will also vary depending on the region that you are in. Keeping pace with this is a key component.

Meeting agility demands to support business change is another challenge. You are decreasing the time to value. You are developing something new and exciting for your customers, but you really need to get that into production. How do you actually make sure that your mergers and acquisitions are going smoothly and that you are not making too many exceptions in terms of your controls that you want to put in place?

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/400.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=400)

Of course, enabling cost and operational efficiency goes without saying. You want to do all of this, but you want to make sure that you are scaling consistency in a cost-effective manner. So how do you start working on this?  It has to be a risk-based approach. You need to follow a risk management framework. The idea with this is that it is a structured, systematic blueprint that allows you to take an approach that is probably going to be very similar to every single thing that you are doing. It has to be a unified strategy, but it also has to be flexible enough because not every workload is the same. It has to be based on risk. Not everything has the same likelihood of happening. Not every probable cause could cause the same level of impact, so you need to make that selection and make sure that you are categorizing your assets properly.

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/450.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=450)

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/470.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=470)

This takes me to this risk-based approach for controls. Whatever framework that you are following, you are going to find something very similar to this.  It will have more steps, but at the very core, this is where we focus. First, as I mentioned, you need to categorize your assets. Once you do that, you select the controls that are appropriate to the risks that you decided you are willing to take, that the appetite of those risks. You are going to select the controls based on  the types of assets, the type of data that you have stored on those assets, and what type of privacy you need to adhere to. All those things factor in, and then you start selecting controls. This is where we are going to focus today. How do we select the controls and once you select what you need, how do you actually implement those controls in AWS?

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/490.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=490)

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/500.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=500)

 What are the different types of implementations that we have? And then, how do you assess how effective those controls are? How do you make sure that something  that you implemented is actually doing what it is supposed to be doing? How do you actually provide your ATOs, your authorization to operate, to the teams? And finally, once you provide that authorization, this is not something you do today and then just forget about it. You need to be continuously monitoring to make sure that maybe something changed and it is still following your organization guidelines and controls. That is what continuous monitoring is key to. Once you go through this process, you need to go into that continuous monitoring, which sometimes is the trickiest part because it is not just about monitoring, it is also about taking actions. If something is no longer following our guidelines, then we need to make changes to make sure that we can provide that authorization.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/560.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=560)

### AWS Control Tower Control Catalog: Simplifying Control Selection and Implementation

Now, let's move into Arnot, who is going to dive deeper into how we can actually go through the process of selection and implementation of controls. Thank you, Rodolfo. I really like that approach, you know, select, implement, assess, monitor.  I am going to show you today how we can use Control Tower, its Control Catalog, and a couple of new features that we recently launched to help you with the first two steps of that approach, which are the select and implement. Select the control that you need for your different use cases and deploy them at scale in your AWS organization.

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/600.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=600)

But let's start with a scenario that we see all the time. We have a customer that comes to us and says, "Hey, I need to be PCI DSS compliant because I am processing credit card data,  but

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/610.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=610)

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/620.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=620)

I also want to follow CIS best practices because it is something recommended by my security team.  Additionally, in the future, I would like to follow other AWS best practices to cover more governance objectives.  This is a typical starting point: multiple compliance needs, different stakeholders, and various security and governance frameworks. Usually, it is at this moment that organizations realize they need a structured approach to governance and compliance in the cloud.

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/650.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=650)

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/660.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=660)

Now that we have selected frameworks such as PCI DSS and CIS,  this is where it gets interesting. Each framework speaks its own language. For example, PCI has its requirements,  CIS has its benchmark, and on the other side of the spectrum, AWS has many managed controls for different use cases. Typically, what you do is map the framework requirements to some of the control objectives you have, and then figure out what AWS managed controls you can use to achieve those objectives in the context of those frameworks.

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/700.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=700)

If we return to our example, you will have to understand PCI DSS and CIS. Then identify what you need  to do for encryption at rest and encryption in transit. Once that is done, you will map those needs to AWS managed controls. This is a challenging process that takes time and resources.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/720.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=720)

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/750.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=750)

Let's see how customers typically manage this today.  Most organizations tackle these challenges with a spreadsheet. You create a first spreadsheet for your PCI requirements and put in that spreadsheet the mapping with the AWS controls you have and the controls you have to author yourself. This becomes a large spreadsheet with thousands of rows. You then have to do that again for CIS and repeat that process for all the frameworks and all the objectives you want to achieve. 

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/770.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=770)

This approach is obviously manual, time-consuming, and prone to error. We have seen customers spend a lot of time just on that process, sometimes months, maintaining those spreadsheets.  Most importantly, this is not a point-in-time process. Everything is evolving, so every time there is a change—for example, a new version of PCI that you need to support or a new governance objective you want to support—you have to go back to those Excel files, maintain them, and it takes time and a lot of energy.

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/800.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=800)

Now let's see how Control Tower and its control catalog can help you with the first two steps of our risk-based approach: select and implement.  Before we go there, let me ask you a question. How many of you here today are familiar with Control Tower? Quite a lot of people. I am happy about that. How many of you are familiar with its control catalog? Not so many. That is great because we are going to dive into the control catalog, and I hope that by the end of this session you will be able to see how it can help you with your governance journey.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/890.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=890)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/900.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=900)

For those who are familiar with Control Tower, you may know that previously, to have access to AWS managed controls in Control Tower, you had to go through the setup and deployment of a full Control Tower landing zone. This was a process that was somewhat heavy and time-consuming. We heard from some customers that they already have their multi-account environment set up the way they want and do not need Control Tower for that, but they really would like to use Control Tower for the controls. We have recently launched a new feature called the control-on feature, which allows you  to directly have access to the control catalog and all of its controls in Control Tower. It is pretty easy and super simple. You go to Control Tower,  you click on the control catalog, and then you get access to all the controls that we have.

There is no onboarding setup required, and you will be able to search and discover the control that you need without any onboarding. If you find a control that you want to deploy, we will push you through an onboarding experience that is really simple. It basically just requires trusted access to your AWS organization, and if you deploy AWS Config rules, we will set up a Config recorder for you. So it is fast and easy to access the catalog right now.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/950.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=950)

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/970.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=970)

Now let's dive into the catalog features.  First of all, we are talking about a collection of over 1000 AWS managed controls for many different use cases, spanning security, compliance, operations, and cost management. These controls are accessible via the console, API, and CLI.  If you need to find a control or search the catalog, you can go to the console or use the list control API. If you want to get details on a control, you can use the get control API. If you want to know what control is mapped to compliance frameworks, you can use the list control mapping APIs. All those APIs and console experience give you a really easy way to search the catalog.

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1000.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1000)

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1020.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1020)

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1030.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1030)

 Each control in the catalog has a global ARN, so you can think of it as a unique identifier that will work across your entire AWS organization. Every control, and this is important, is deployable through AWS Control Tower.  This means you get a single pane of glass to manage all your controls regardless of their origin or purpose.  We have also enriched all the controls in the catalog with consistent metadata, so you will get a domain for each control, a command control, which is a category for the control, and the objective that the control will achieve. This makes it much easier for you to understand what each control does and why you might need it.

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1060.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1060)

Perhaps something most important for compliance-focused teams is that all the controls in the catalog  have been pre-mapped to 17 compliance frameworks, so we chose the most popular ones like PCI, NIST, ESO, and others. All the controls in the catalog cover many different use cases such as security, operations, and cost management, but we also have all the different types of controls that we have in AWS. In AWS, we have preventive controls, proactive controls, and detective controls, and all those types of control are represented in the catalog.

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1110.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1110)

[![Thumbnail 1130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1130.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1130)

### Understanding Control Types: Preventive, Detective, and Proactive Controls in AWS

Now let's dive into those different types of controls.  In AWS, we have three types of controls: preventive, proactive, and detective. Each of those types has its own primitive.  Starting with preventive controls, you can think of these controls as the first line of defense. They stop noncompliant actions from happening before anything is deployed. For preventive controls, you have three primitives. The first one I will talk about is the Service Control Policy, or SCP. These are controls at the AWS organization level. You can imagine them as a protective boundary around your entire organization. For example, an SCP can prevent anyone in your organization from launching resources in unauthorized regions or block the creation of S3 buckets. The beauty here is that even if someone in your organization has admin rights on an account, they will not be able to violate those policies.

The second type of preventive controls that we have on the platform are Resource Control Policies, or RCPs. These policies work at the resource level, and they allow you to define permissions and restrictions for specific AWS resources. For instance, you can ensure that all EBS volumes must be encrypted or that EC2 instances can only use approved AMIs.

Resource Control Policies are powerful because they help maintain consistent resource configuration without requiring any IAM policy changes. The last primitive for preventive controls are the declarative policies. These policies operate at the service level within an AWS organization, and they allow you to define and enforce desired configuration across your entire organization. What makes these policies particularly useful is that they will always work as new service features are released, so you put it once, you forget, and it will always work.

[![Thumbnail 1280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1280.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1280)

[![Thumbnail 1290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1290.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1290)

Unlike SCPs which control API access, declarative policies focus on maintaining consistent service configuration at scale across your organization. With preventive controls, once they are implemented,  you are always compliant. Then we have the detective controls.  These are like the AWS Config rule-based controls. They do not prevent actions from happening, but they alert you when something does not align with your policies. For example, AWS Config rules will check for noncompliant resources, or a Security Hub control will monitor for security best practices. With those controls, you get a status of compliant or noncompliant, and if it is not compliant, then you can act on it and remediate the issue.

[![Thumbnail 1320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1320.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1320)

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1360.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1360)

Finally,  we have the proactive controls.  These are controls that validate resources before they are deployed. They are based on CloudFormation Guard and examine resources during the build and deploy phases to ensure that you meet your security and governance requirements. For example, a proactive control can validate that an EC2 instance configuration includes required tags and security groups before the instance is created. With those controls in your pipeline, you are going to make sure that everything that is deployed is aligned with your policies.

So now you could ask, what type of control should I use? The recommendation that we have is that you should use all three types together: preventive and proactive to stop non-compliant actions, and detective controls to catch anything that could slip through. If you do that, you will really create a comprehensive governance strategy for your AWS environment. With Control Catalog, and we will see that a little bit later in the demo, we are going to help you establish that preventive and detective strategy, kind of like belt and suspenders. But for now, back to Rodolfo for another important governance use case, which is AWS service approval.

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1430.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1430)

[![Thumbnail 1460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1460.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1460)

### Service Approval Strategies: From Blanket Approval to Hybrid Two-Stage Approaches

Alright, so as we just saw, there are a lot of different types of controls, and with Control Catalog, we can align those  using the mapping that is aligned to 17 frameworks. But what happens when we get our developers and all our business owners saying, hey, I want to leverage this particular AWS service, right? I want to get that into production. That is in our approval process, because we need controls for that particular service to allow that to be in production. It has to align to our strategy that we saw and discussed at the beginning. We see our customers working with mainly three ways  to address this.

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1480.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1480)

[![Thumbnail 1490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1490.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1490)

The first one is a blanket approval approach. This is something that we are going to dive deeper into in a second, but this is the first thing that we see our customers trying to do. The second one is basically like an enhancement on the first one, so this is a contextual service approval.  We are going to also discuss that in a second. The last one is a hybrid two-stage approval for every service.  Let us take a look at how this actually looks like. Let us go from one business scenario, and this is the blanket approval approach. We have our product team saying, hey, I have a new feature that is going to work with NoSQL data. I looked at DynamoDB. It looks like something pretty cool. I would like to use that.

[![Thumbnail 1520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1520.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1520)

Let's start working on this for the data storage functionality. This is a new feature that we're going to launch, and we received this request from the product team to the central security team. In this particular case, we get the request and say that  we are using a blanket approval approach right now. We checked and DynamoDB is not on the allow list, so we need to start working on that. Let's review the service and see how it fits in the context of your new workload. However, because we're using blanket approval, that means if we approve it, it's going to be allowed for any other team that wants to use it.

[![Thumbnail 1570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1570.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1570)

As we discussed at the beginning, not every workload is the same, so that becomes complex. You are evaluating the new service in the context of this feature, but then how do you know that it's going to fit every future use case? That becomes a pretty complex task to follow, and you could arguably say that it's impossible. What happens if something new comes out? It is hard to align. However, we see customers trying to do this at the very beginning, and then they say, let's  take a look at something different. This is when we start seeing contextual service approval.

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1620.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1620)

We have the same request. Yes, we want to use DynamoDB, but in this particular case, we ask the product team to do a threat model exercise to see what are the different scenarios that could affect this and how we are aligning to our strategy. We discuss this service in the context of this workload. That means if it gets approved, it will be approved only for this context. If it gets disapproved, it will get blocked, but not for everyone. It will just get denied for this particular context. It's much better. The good thing with this is that it allows you to keep evolving. Based on this review and how it differs  from the baseline, I will allow this for this use case, but in the future, they can use a similar threat model. You just add whatever you think you will need in the new feature in a different context. This is the most common approach that we see.

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1640.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1640)

The third one is when we look at highly regulated industries that need to align with, for example, the compliance program. For this one, I'm almost 100% sure that many of you are already familiar with this. This is just a quick review of the shared responsibility model of AWS.  Keeping this in mind, AWS takes care of the security of the cloud, the one at the bottom in orange. This is basically making sure that the hardware is secure. We don't just ask you to take our word for it. We actually work with multiple compliance programs across the globe. We have third-party auditors that come and conduct audits twice a year in our environments. There are ways for you to download those reports through AWS Artifact, and we're going to discuss that later. We give you evidence that this is compliant.

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1710.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1710)

[![Thumbnail 1720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1720.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1720)

[![Thumbnail 1730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1730.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1730)

If you are trying to follow PCI DSS standards, for example, you can also make sure that you are leveraging services from AWS that are aligning to this program. This actually helps you relieve that part of responsibility that is on your side, the security in the cloud, such as access controls and data encryption. All those things that you are putting in place in our services, you are making sure that you can align to that. This is the hybrid two-stage approach.  We get the same request from the product team and we say, OK, this is going to be a workload that is handling payment card data.  This needs to align to PCI DSS. AWS is already giving me proof that this is aligned to PCI DSS, so DynamoDB is pre-approved.  Then we just need to review that context part that we just saw, but only in the context of my shared responsibility, making sure that I'm also applying the controls that I need to do this.

[![Thumbnail 1750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1750.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1750)

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1780.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1780)

This is a key part, and this is what we normally recommend our customers to take. But then once you decide all this and you say, OK, I need all these controls, that's when you say, OK, which controls and how can I do that?  This is when I'm going to go back to Arnold to see how he actually leverages the controls catalog to not just align our controls to frameworks but also to specific services, depending on what I decided that I need. So back to you Arnold. Thanks Rodolfo. So let's go back to our previous scenario. You need to be PCI DSS compliant and you also want to follow CIS best practices.  Now let's say that you need to ensure encryption at rest in your organization and you want to start protecting your Amazon DynamoDB databases. So enough words and let's see how Control Catalog can help you with that.

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1850.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1850)

### Live Demo: Deploying Encryption Controls for DynamoDB Using Control Catalog

When you arrive in AWS Control Tower, you don't need to set up a landing zone. You can directly go to the control catalog. You can see that we have a bunch of controls for many different use cases. You can see every control with its name, the service of the control, so we are supporting a bunch of different services. The common control that the control applies to. And to come back to the previous discussion, we can see that we have detective, proactive, and preventive controls in the catalog, as well as the type of implementation like AWS Config rules, CloudFormation hooks, or Service Control Policies. 

[![Thumbnail 1870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1870.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1870)

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1880.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1880)

[![Thumbnail 1890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1890.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1890)

You can also browse the catalog by different dimensions. You can look at specific control objectives, for example, backup and recovery, logging, and encryption at rest where we have 78 controls to help you with that.  As mentioned earlier, you can also take a service-specific approach and look at all the controls that we have, for example, for RDS or S3.  You might want to take a framework-based approach so you can see all the frameworks that we are supporting here with all the controls that are related to that framework. 

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1900.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1900)

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1920.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1920)

Now if I go back to the catalog and let's go into the details of one control. For example, let's say that we want to have an S3 bucket notification enabled.  This is the detail of a control. You can understand from the description of a control what it's going to do. You can see the domain, which is log monitoring and accountability, the objective of the control, for example, the different categories the control is mapped to, and the different frameworks the control is mapped to. 

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1950.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1950)

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1960.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1960)

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/1980.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=1980)

The service for the control is Amazon S3, the resource that the control is controlling is an S3 bucket. It's a detective control that we have here from AWS Security Hub that is deployable in 27 regions. That's the date of the release, and something interesting I wanted to show you here is that this control is mapped and related to another control which is a proactive control.  This means that the detective control and the proactive control can work together to have both detective and proactive behavior.  You're going to see that this control is really similar to the CloudFormation hook proactive control that we were looking at before, but it's a proactive one. Obviously, if you go to the relationship, you're going to see that the proactive control is related to the detective control.  What is interesting is that you can choose to enable those two controls together to have both detective and proactive protection.

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2000.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2000)

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2020.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2020)

[![Thumbnail 2040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2040.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2040)

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2050.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2050)

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2060.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2060)

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2070.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2070)

So now let's go back to our example with PCI DSS and CIS for encryption at rest for your Amazon DynamoDB.  What is really interesting is that I can filter the catalog by all the dimensions that I want to find the controls that I need. So I'm going to ask to give me the PCI DSS 4.0 controls.  I'm going to do the same for CIS. And you can do that for whatever needs that you have. So CIS 8.0. Now I get the intersection of the controls of the two frameworks.  I want to ensure encryption at rest. So I'm going to do filtering with encrypt data at rest, and I want to start with my Amazon DynamoDB. So I'm going to filter by the service DynamoDB.   Voilà, I have 5 controls that can help me with encryption at rest for my DynamoDB table.  Let's say that I want to use KMS keys for that, so I'm going to choose that detective control, but I see that I have a similar control at the bottom here, which is a proactive one that has exactly the same behavior but with a proactive control and detective control. So I can select those two controls, decide to enable them.

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2110.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2110)

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2120.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2120)

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2130.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2130)

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2140.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2140)

Now I want to protect all my DynamoDB databases in my workload organizational unit.  I choose that workload OU and then I start the enablement of those two controls for all the accounts in that OU.  We can see that the two controls are enabling now.  After a couple of minutes, we're going to have the two controls enabled in my workload OU,  which means that in a few steps, I'm sure that all my DynamoDB tables in my workload OU are encrypted with KMS. Job done. It was quick and easy, and obviously you can repeat that process for all the other AWS services, control objectives, and compliance frameworks you need to support in your organization.

I hope you like the demo. Give it a try. It's easy and smooth. Just go to Control Tower and you can start exploring the control catalog. That's it for the select and implement phases of our risk-based approach. Now back to Rodolfo, who will talk about an important use case: what should I do if I don't have a managed control in the catalog for something that I want to protect.

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2210.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2210)

### Creating Custom Controls with AWS Config, RDK, and Guard DSL

As we just saw, there is a catalog with over 1000 controls available. These are pre-created and easy to deploy, but every organization is different, and we understand that there are specific baselines specific to your systems.  Besides preventative and proactive controls, you can actually deploy detective controls that are outside the catalog. For this, we use AWS Config. AWS Config allows you to have two types of controls, both detective. One is managed controls, which are basically those rules that are already part of the catalog. Then we have custom controls, which you can create to make sure that you are following or aligning to maybe an outlier or something very specific that requires some exceptions.

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2260.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2260)

With custom controls, there are two main ways to do that. One is a Lambda-based approach.  You can create a Lambda and make sure that it has the proper permissions and the proper coding, for example in Python, and how it can align to whatever you are trying to do. You can deploy that from Config. We understand that sometimes this is hard for those folks that are not familiar with developing. So we have an open-source tool called the Rule Development Kit. The RDK is basically an open-source tool that customers can leverage to create a boilerplate of the Lambda and test the control locally and then deploy that centrally. You just need to focus on the logic of whatever you're trying to do. If you want to check one specific version on an EKS cluster, you can do that. You just need to focus on the logic and then you can deploy that.

The other option is Guard DSL. This is a domain-specific language where you just need to focus on the policy itself of the compliance requirement that you have. This leverages Guard DSL from CloudFormation with the same logic, and you can just from the console or from the CLI or from an API say, "Hey, this is the policy that I need to evaluate. Forget about all the things that are happening around it." AWS Config will take care of it and will deploy that custom control on your behalf. This allows you to deploy it in multiple ways. It allows you to be flexible because it aligns to whatever custom requirement you have, and it's also CLI-based. You can actually work with that.

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2350.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2350)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2380.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2380)

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2400.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2400)

To that point, you can go even further simplifying this.  With Quiro CLI, this is a generative AI service that we have in AWS.  This allows you to install Quiro, which is going to be like your assistant. You can ask it questions in plain English and actually help Quiro CLI help you create these rules. If you have AWS CLI, you install Quiro CLI, and if you even install the RDK, you basically have everything you need. You can just go ahead from the console and say, "Hey, Quiro, help me create a Config rule that checks if I'm running a specific EKS version that I don't want to have. Just detect me if something is there." It will leverage the RDK that is already there in that environment, so you basically don't need to learn it. Then it will create that boilerplate, which is the section that is in black and red on the top,  and it will actually take that and embed the logic that you ask it to follow.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2410.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2410)

Once that's done, the assistant will actually help you deploy it if you give it enough permissions.  It will give you the steps to deploy, and even if you want to go further, you can actually deploy that from the console. This is pretty cool, and I've started using it a lot, so I highly recommend testing this. It's definitely a better way to do this. If you have the RDK and want to do this, we have NCP servers for knowledge on AWS that Quiro can connect to, so it gives you that context on AWS and allows you to do further things. This allows you to actually have those customized controls that we just discussed, which you may need already.

[![Thumbnail 2450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2450.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2450)

[![Thumbnail 2470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2470.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2470)

This is an example of Guard DSL. This is also a custom rule that I just created.  As you can see, it's basically just checking the metadata of the resource, saying if it is something different than 1.34, if it is an EKS version beyond that, just let me know and I will take an action. It integrates with CloudWatch log groups, so this means that it will let you know if it is actually following best practices.  You can see at the execution that in this particular case, it's giving me 1.28. It's not what I expected, so that's why it's giving me that it's not compliant.

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2500.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2500)

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2510.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2510)

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2520.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2520)

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2530.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2530)

### Assessment and Monitoring: Leveraging Control Tower, AWS Config, and Security Hub for Continuous Compliance

Moving from this, we go into the final steps: assessment and monitoring. So we just covered how we select, how we implement, and how we build custom controls. Let's take a look at which services—Config, Control Tower, and Security Hub—help me with that final step of assessment and monitoring.  This is the same environment that we just saw with Arnold. This is Identity Center, and these are just the accounts that I have.  If I go to my management account, this is where you will see Control Tower. We just saw how you can see the catalog from here.  When we go to Control Tower, you will see the main dashboard is telling me that I have two organizational units and I have 24 accounts. I have 26 controls deployed: 21 preventive, 4 detective, and 1 proactive.  These are actually the controls that Arnold just deployed. We can see that we have non-compliant resources, and we're going to take a look at that. We have four organizational units, and we have one organizational unit that is non-compliant, and we're going to see what is happening with that.

[![Thumbnail 2540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2540.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2540)

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2550.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2550)

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2560.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2560)

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2570.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2570)

We have enrolled accounts, and we see that there is a workload account named Workload1 that is non-compliant.  We click on that account, and we're going to get the details of that account. We're going to see related non-compliant resources.  This is where we see the DynamoDB table. Remember that we just deployed a detective control that checks if this is being encrypted with KMS keys.  It's telling me that you have a non-compliant resource in here. You can see the enabled controls that are part of that account. You will see that you have your SEPs and you have your detective controls. You will see the regions this account is operated on.  Right now we have four regions, and all the other ones are non-governed. That means that Control Tower is not looking at them, and you can actually avoid workloads from being deployed there if you want.

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2580.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2580)

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2590.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2590)

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2600.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2600)

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2610.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2610)

You will see other Security Hub rules, and we're going to see what those are in a second, but let's just focus on this non-compliant control that we just deployed in DynamoDB.  If you click on the control that we deployed, you're going to see, as we just saw, the domain. It's going to tell me all the mapping and all the things that we did when we actually pushed that into production.  It's going to tell me the data protection. It's going to give me the frameworks. It's going to give me all that data that we just saw from the catalog itself. It's going to tell me where it is deployed, and also from here you can see what Arnold just showed us: the related controls.  Here I can move. This is non-compliant. What else do I have? I see that there is a proactive control, but keep in mind that this proactive control works with infrastructure as code.  If I try to deploy something, I have a CloudFormation template that I'm trying to deploy. There is a rule that was already created in the vacuum. We didn't see any of this when we were deploying the control because you don't have to be worried about that part.

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2620.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2620)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2630.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2630)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2640.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2640)

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2650.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2650)

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2660.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2660)

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2670.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2670)

[![Thumbnail 2690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2690.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2690)

[![Thumbnail 2700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2700.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2700)

Here, if you go to enabled controls, you can see the whole thing.  You can actually filter by, in this case, let's say proactive controls, and this is telling me that this control is enabled. Where is it enabled? What are the frameworks that are related to it?  And again, I can see recent operations. I can see when something changed, when it was enabled, and even though I can actually check if something was disabled.  So this is my main dashboard for those kinds of controls. If we move into the audit account, this is our security services account in this particular case. If we go to AWS Config, remember that in the back end, Config is actually powering detective controls in Control Tower.  Here we're also going to have a dashboard. The inventory is a key component because this is basically the list of assets.  It allows you to have your rules in here. You will see that you have multiple rules in here. There are multiple that are non-compliant, and this is because we also deploy Security Hub rules.   

[![Thumbnail 2710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2710.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2710)

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2720.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2720)

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2730.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2730)

We're going to look at what those are as well.  We can see the resource inventory, and you can filter in here, for example by non-compliant resources in this particular account in the security services account.  In this case, I have an S3 bucket. It's telling me that it has some non-compliance rules even though some checks are being passed.  We're going to take a look at how we can actually examine those. We can see the attributes of this particular resource. This is the whole metadata you can explore if you need it.

[![Thumbnail 2740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2740.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2740)

[![Thumbnail 2750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2750.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2750)

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2760.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2760)

This is actually what has been happening in the backend. All the data that is being gathered for this resource. It gives you the resource timeline as you can see.  I can see when it changed from compliant to non-compliant or vice versa.  I can see if somebody made a change because CloudTrail is also part of Config, so it gives you that historical context of something that is happening.  I can see when that happened and I can actually take a look at that. This is very powerful.

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2770.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2770)

[![Thumbnail 2780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2780.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2780)

[![Thumbnail 2790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2790.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2790)

Now I was showing you one single account. In Config you can have aggregators, which means within an aggregator you can actually run queries.  An aggregator is basically everything put together. If you say show me all non-compliant resources, but I'm using the aggregator for my whole organization, so it's going to run a query.  It's pre-populated, and you can change that if you need it. Then it will give you all the different types of resources and all the rules that are associated to that resource that might be giving you a non-compliant status.  You can actually just take a look at that and make action.

[![Thumbnail 2810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2810.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2810)

But of course, this is one way to do it in Config. Let's move into Security Hub, because it's very important that you have non-compliance and you have those historical changes and you have the resource inventory, but you need also the security context.  So if we go to Security Hub, Security Hub allows you to deploy managed frameworks, not just controls individually, but also deploy, let's say, a whole CIS framework. I can do that. It will aggregate the data so you can quickly take a look at this.

[![Thumbnail 2830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2830.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2830)

[![Thumbnail 2840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2840.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2840)

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2850.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2850)

[![Thumbnail 2860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2860.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2860)

You can say, OK, by accounts, by specific instances I can have my findings, security findings that are related to misconfiguration, for example, on all my regions.  Remember that we saw that we are operating in four regions.  It's giving me all these findings based on the threat, changes over time, resources with the most amount of changes.  So this is like the executive summary. You can actually use this dashboard for that. If you go into the controls, you will have a score of 77% of all the controls that are deployed.  This means that I have 492 findings of those controls in different resources. I can actually filter down to a specific, let's say, requirements and concerns that I have, and I can try to prioritize.

[![Thumbnail 2880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2880.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2880)

[![Thumbnail 2900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2900.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2900)

Let's look at a critical one. I see this is a particular security group that is open.  Something that I deployed a control that is associated to a framework. I see that I have plenty of accounts that have no issues with security groups, but there is maybe one that I would like to focus on. If I filter by compliance status, I see that I want to fail, and this is because there is a security group that is open on that account.  So I can actually start doing things and getting that context related to a misconfiguration that is not just something that is non-compliant, it's also that it is a security risk.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2910.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2910)

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2920.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2920)

So this is why we focus on those objectives at the beginning.  This is the security case. We can see the overall organization deployment. I can see the same as we saw in Control Tower, but this is from a security standpoint of the policies of Security Hub CSPM.  This is the Cloud Security Posture Management part. I can see that I have three frameworks that I deployed using Security Hub controls. All the relationships I can go into the insights. It will generate insights that are pre-populated that you can actually take a look at.

[![Thumbnail 2950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2950.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2950)

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2960.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2960)

You can actually go to findings again. This is another window to focus on findings with context. I see that I have an EKS cluster over there with a critical finding.  If I click on it, it's going to give me the overview. It's going to give me the resource data again. Config is pushing all that data we just saw into Security Hub as well.  So you're going to have the resource history. You're going to have the compliance history from here that actually aggregates to a specific control. You can actually take actions from there. You can change the status of the finding, you can actually export the finding if you need it to a third party tool as well. You can add notes, you can further investigate if you need it, using something like Detective in case you have that enabled. This is very powerful content that you can get from Security Hub CSPM.

[![Thumbnail 2990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/2990.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=2990)

[![Thumbnail 3000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3000.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3000)

Now let's look at Security Hub. This is a new experience that we launched, and this gives you not just the misconfiguration part, but it aggregates data from other services to give you more context.  This is the executive summary. 

[![Thumbnail 3010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3010.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3010)

[![Thumbnail 3020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3020.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3020)

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3030.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3030)

[![Thumbnail 3040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3040.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3040)

[![Thumbnail 3050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3050.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3050)

[![Thumbnail 3060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3060.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3060)

[![Thumbnail 3070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3070.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3070)

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3080.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3080)

[![Thumbnail 3090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3090.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3090)

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3100.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3100)

[![Thumbnail 3110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3110.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3110)

[![Thumbnail 3120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3120.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3120)

[![Thumbnail 3130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3130.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3130)

[![Thumbnail 3140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3140.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3140)

[![Thumbnail 3150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3150.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3150)

[![Thumbnail 3160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3160.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3160)

[![Thumbnail 3170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3170.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3170)

[![Thumbnail 3180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3180.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3180)

[![Thumbnail 3190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3190.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3190)

[![Thumbnail 3200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3200.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3200)

[![Thumbnail 3210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3210.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3210)

This is telling me that I have recurring findings in this particular case over the last 90 days. I have exposure findings. I'm going to explain  what that means. You can actually filter by days. This was last week when I recorded this, so you can actually see the coverage, right? Security  posture management at 100% because we use Control Tower with Security Hub to deploy those controls. We go to the triage section, and we are going to see that we have a threat summary and  an exposure summary, and we have 4 million findings in there. If I actually want to take a look at those to see what that means,  it is giving me something that I have some specific things that I need to be concerned about. But why is that? Why does that mean it is exposed? If I look at those, if I actually see  a potential hijacking to an EC2 instance, it is telling me that you have an EKS cluster as a primary resource. It is being highlighted because this is accessible.  I can see the traits that are actually contributing to this finding. The first one is telling me that it is reachable from the internet. Then it is telling me that it is misconfigured, and this is actually a control. Remember,  we have a control that is looking for EKS versions of clusters. This is telling me that I have a non-supported EKS version that is actually publicly accessible. That is a concern.  I can see the compliance history, and all of that is being enabled by those detective controls that I have in place. Anything that you can actually do from here is explore the finding again. This  is an exposure finding. You can actually change the status of it. You can investigate it. In this particular case, as we saw, we have  three that are found in this demo environment. I have security groups that might be a concern, an EC2 instance again that has a vulnerability and also has an open  security group. It is the same situation. Those detective controls are actually checking the configuration of that security group, and I also have Inspector running in the backend. This is another service that is telling me that there is a known  CVE. I am aggregating not just those controls that allow me to be compliant, but I am giving you the service capabilities to actually assess  how effective those controls are actually being deployed and how they can actually be continuously monitored. It gives me those contextual threats  and, in this particular case, it is telling me that there is a misconfiguration because we are focusing on that control of that configuration use case. But really powerful things you can actually see the traits,  and you can actually feel that. The last piece that I want to show you here is the actual attack path.  This is a potential attack path that is based on those findings. I have an EC2 instance. I have a security group. I have the EC2 instance. What it is finding is telling me here  that I have a vulnerability because this EC2 instance has known CVEs, and I have a security group that is associated with that misconfiguration, one of those controls. It is telling me that I have  that exact concern. All the data of that resource is here so I can explain why it is highlighting it. As an example, there it is.  All right, so let us move into auditing, the last part. We just saw how we can assess and how we can monitor. To wrap up quickly is  how you actually do this with AWS services. The first one is AWS Artifact. As we just saw at the beginning, the shared responsibility model  allows me to actually trust in AWS. How do you actually download those reports? This is through AWS Artifact. This is free. You go into the console, you actually accept, and you can download the reports, let us say, for PCI DSS. Those third-party audit reports that we have, you can automate that download every single time there is a new version. You can download the latest one, and you can actually even accept agreements if you are operating on that landscape, for example.

[![Thumbnail 3240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3240.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3240)

[![Thumbnail 3260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3260.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3260)

[![Thumbnail 3270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3270.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3270)

[![Thumbnail 3280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3280.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3280)

[![Thumbnail 3290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3290.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3290)

[![Thumbnail 3300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3300.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3300)

CloudTrail has a CloudTrail Lake service.  This is a managed data lake for audit events that allows you to have all this aggregated data. In this particular case, you can actually use generative AI capabilities to ask plain language questions. In this case, show me all the mutated events in Config in the last couple of months. It generates a query for you. It gives you, in this particular case, this is a smaller  environment, so I have only four. It is summarizing those findings. It is telling me what changed and what is basically the explanation of the aggregation of those API calls.  It also generates pre-populated dashboards so you can actually look at those in an organizational way. Then you have, of course, AWS Config  as we just saw. It will also have compliance dashboards, and you can export your snapshots into, let us say, QuickSight dashboards that we create using QuickSight and generative AI capabilities. There  is one that you can download here if you want to actually just have something pre-populated from Config. You can actually do that as well. 

[![Thumbnail 3310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3310.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3310)

[![Thumbnail 3320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3320.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3320)

So the key takeaways for this are to remember to start based on your objectives. Work backwards from that to define a strategy. Implement a risk-based  approach. You want to make sure that you are basing everything on your appetite for risk and using a managed framework for it. Leverage as much as you can  manage services that will ease a lot of the pain, and you don't have to learn every single thing. It will give you automation around that and hopefully it will actually address all the concerns that you may have and give you a place to start.

At the start, we remember that we talked about the analogy of how controls are your hidden architecture for your building. When you start a construction, you want to make sure that you have a well-defined plan. It's the same with governance. It's the same with controls. You need to define that from the beginning so you have a clear path moving forward. That's the key.

[![Thumbnail 3360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bfb0cea1fc9141ca/3360.jpg)](https://www.youtube.com/watch?v=2bJ9F0tr29s&t=3360)

We really would like to thank you for your time. I know that this was a long session. These are some additional resources  you want to go and take a closer look into—resources, blogs about controls and Security Hub controls. Feel free to take a look at that. Very importantly, if you want to discuss more, come by the kiosk of governance. We have goodies, we have stickers, and we have experts in there if you want to dive deeper into any of this.

Last but not least, please remember to fill out the survey. Feedback is very important, and have a nice weekend. Have a nice re:Invent. Welcome. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
