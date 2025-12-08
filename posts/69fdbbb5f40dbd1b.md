---
title: 'AWS re:Invent 2025 - AWS Security Hub: Unifying & simplifying security operations at scale (SEC228)'
published: true
description: 'In this video, AWS announces the general availability of the evolved Security Hub, which unifies threat detection, vulnerability management, and cloud security posture management into a single platform. Key features include AI-powered exposure findings that correlate signals from GuardDuty, Inspector, Macie, and partner tools to provide contextualized risk prioritization with near real-time attack path visualization. The service introduces simplified enablement across AWS Organizations, standardized findings using the Open Cybersecurity Schema Framework (OCSF), and unified resource-based pricing with a 30-day free trial. Awesome (SmugMug/Flickr) shares their implementation experience, demonstrating automated workflows that route prioritized findings directly to Jira and appropriate engineering teams, significantly reducing alert fatigue and research time while improving security posture visibility across their 19-year AWS infrastructure.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/0.jpg'
series: ''
canonical_url: null
id: 3087217
date: '2025-12-05T19:02:32Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - AWS Security Hub: Unifying & simplifying security operations at scale (SEC228)**

> In this video, AWS announces the general availability of the evolved Security Hub, which unifies threat detection, vulnerability management, and cloud security posture management into a single platform. Key features include AI-powered exposure findings that correlate signals from GuardDuty, Inspector, Macie, and partner tools to provide contextualized risk prioritization with near real-time attack path visualization. The service introduces simplified enablement across AWS Organizations, standardized findings using the Open Cybersecurity Schema Framework (OCSF), and unified resource-based pricing with a 30-day free trial. Awesome (SmugMug/Flickr) shares their implementation experience, demonstrating automated workflows that route prioritized findings directly to Jira and appropriate engineering teams, significantly reducing alert fatigue and research time while improving security posture visibility across their 19-year AWS infrastructure.

{% youtube https://www.youtube.com/watch?v=mYyBQYIeJzk %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/0.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=0)

### Introduction to AWS Security Hub Evolution at re:Invent 2025

 Welcome everyone to Reinvent 2025. Hopefully everyone is having an exciting time. It was a great day yesterday with the keynote and many exciting announcements. We're going to continue with those announcements and excitement by highlighting one of the key features from the keynote, which is the evolution of AWS Security Hub. As you know, in the AI and generative AI or agentic era, security still remains our top priority. At AWS, security is our top priority. We believe that everything starts with security, and many of our customers believe that as well. To enable that same theme, we have been working very hard, working backwards from our customers' requests and requirements. I will highlight some of those key improvements and innovations in our Security Hub service.

My name is Himanshu, and I lead the team of our worldwide security specialists across all of our security portfolio. We are subject matter experts who help customers with their security outcomes. I am joined with two other speakers today: Scott Ward, who is a principal solutions architect on the services team, and one of our customers who will be here to talk about how they have evolved their journey with Security Hub and how it has been helping them with their security operations. This is SEC228, where we will talk about unification, simplification, as well as automated response and remediations in Security Hub. How many of you are familiar with Security Hub or use Security Hub today? This should be an exciting session for all of you.

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/130.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=130)

This is something that we actually announced in preview in June at Reinforce, and since then we have made quite a few improvements and enhancements. I wanted to level set what we have been hearing from our customers and why we are making these improvements. We do believe that security teams face  quite a few increasing complexity when it comes to the changing landscape and the evolution of technology that we are facing right now. We are in an exciting time where business and application teams want to innovate with artificial intelligence and generative AI, and you are seeing a lot more modernization and transformation of workloads in the cloud.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/150.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=150)

 But with that, what is more important is to not have fragmented visibility across all these teams and resources. One of the first things when I talk to customers about artificial intelligence and generative AI is security. Security is first and foremost. The first thing that they want to know is where all my resources are, how they are configured, and what security policies do they have. Having that unified visibility is very key for our security teams.

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/180.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=180)

 With the visibility, what is also important is context. We can give as many detections and findings as possible, and I have talked to customers and they often tell us that detecting something did not make anybody secure. If the detection does not have the right context, the time that it takes to go take an action, which is really where the meaningful response comes in, that is where really the security impact or outcomes become meaningful. So we wanted to make sure that we are addressing customers' need of adding the right context when it comes to security.

[![Thumbnail 220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/220.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=220)

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/240.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=240)

Additionally, if there are disconnected security  signals, which often can happen because many of our customers use multiple different security tools for multiple different use cases, and if these signals are disconnected and you are not able to connect them, then you are not able to take those response actions quickly either. And then finally, to  automate that response and to reduce that mean time to response, there needs to be some normalization and automation with our tools as well as other tools that you might be using from our partners or a wide array of our own security partners that you leverage.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/270.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=270)

### Unified Security Operations Through Signal Correlation and Context

So the theme for Security Hub from the very beginning of the evolution is how to change that fragmented security approach into  an integrated and unified security approach. This unified solution now can bring in signals from multiple different security products, whether it is AWS native security services, which are primarily for threat detection such as GuardDuty, sensitive data detection such as Amazon Macie, vulnerability management such as Amazon Inspector, and other identity and access management criteria or control and configuration components that Security Hub was foundationally built on as a cloud security posture management solution.

From that evolution, we are now bringing all these signals from native AWS security services in addition to our partner tools and enriching that information by correlating all these signals in a unified approach. Correlation is very important, and correlation with context is what is key. With correlation and real-time analysis, you and the security teams can now view a full range of where and how a threat is emerging, what path it is taking, and what all resources are impacted by that.

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/360.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=360)

This helps streamline response and automated response and reduces the mean time to respond so that you know exactly where to take action. Security Hub as of yesterday  from our keynote is now generally available with all these key features of unifying that security signal, correlating all that signal, and bringing in a wide range of user experience improvements that customers have been asking for. We will go into some of those details later in the session, but first and foremost I wanted to highlight some key elements.

Imagine your security teams are using some of those signal tools that I just mentioned. AWS has a wide breadth and depth of native AWS security services along with our partner tools where some of the use cases that we do not address, our partner tools address. But imagine you have an EC2 instance and you know it is having a brute force attack. GuardDuty, which is our centralized threat detection service, harnesses a lot of the threat intelligence that is gathered by the scale of AWS and Amazon infrastructure. We automatically manage that threat intelligence as well as use a machine learning model to identify any abnormal or suspicious activity.

That service will right away alert with a finding that there is a potential brute force attack happening from an IP address or a URL or a domain which is malicious or known to be suspicious. But that threat alone cannot really provide the security team much information as to why that happened. Now potentially why that happened is because there was a vulnerability that got exploited. Inspector, which is our vulnerability management service, will also provide a signal and a finding which will tell the security teams that there is a potential vulnerability on that EC2 instance. That vulnerability is still exploitable, and that instance is also reachable through the network and has an open port.

Our misconfiguration detection service, which is what Security Hub CSPM provides that value, or the cloud security posture management service provides that value using control checks and continuous checks, will also highlight that there was a misconfiguration or a security group that was not configured properly. These three findings earlier resided in Security Hub, but there was no correlation and there was no context. So what we have done now is unified that and provided customers with one enriched attack path that you can look at and then go into the trends and analysis to identify one risk resource and highlight the severity of that risk by looking at that same EC2 instance and all the signals associated with that threat vector or that incident.

This is just an example of how some of the native services are adding value, but the same concept applies for all of the other entities such as network exposures, anything that is related to network configurations, or resource control policies or security groups, as well as sensitive data. If you are having an exfiltration event with an S3 bucket which is already shared with your business partners or you intentionally made it open to the public internet for resource sharing, that is a much lower risk with context compared to a bucket that might have sensitive data, which is something very critical and also comes up all the time now, especially in artificial intelligence and generative AI use cases where you want to fine-tune some of your foundational models using that sensitive data to provide your business teams better outcomes.

That is how some of these correlated events are now adding more value within the unified experience of Security Hub. Security Hub since preview in the last six months has further improved that correlation and analysis by using artificial intelligence and machine learning within the service.

### Key Improvements: Real-Time Analysis, Partner Integration, and Centralized Management

Now you can get real-time trend analysis within the Security Hub dashboard that shows you how these incidents are trending. It also provides you real-time risk analysis for your resources where you can identify the severity of where you want to take actions. More importantly, we've integrated the service with some of our own security partners downstream because we understand that most of the automation and remediation happens where security teams need to create automated workflows or send these events to a security incident event monitoring tool or a security orchestration tool.

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/660.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=660)

These are the three areas where we further improved the service during that preview period, which is now generally available. The key benefits of all these improvements allow security teams to have a unified operations view with an enriched and much better user experience within the AWS console.  You can use your existing Security Hub service console, and from there you'll have an option to opt into the new feature set. Using a 30 day free trial, which has been our priority to give customers that free trial to leverage the new features, you can now enable this new service right away.

Additionally, with that context correlation and risk prioritization, you can define where the highest severity issues are that you need to address with your security events and within your security teams. With the detailed attack path and correlation, you can identify immediately which business teams or application teams are using what resources and where those workflows need to act so that your response and remediation are faster and easier.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/730.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=730)

The evolution of Security Hub  stays with the same name. We learned from multiple customers that we wanted to create less confusion, so Security Hub now truly becomes a security hub where we're not only bringing in the previously known findings of control evaluations where it was a centralized monitoring tool that evaluated misconfigurations across your resources and also aggregated all the findings from our native AWS security services as well as from our partner tools. Now it's evolved into building on top of that same cloud security posture management into that unified security operations with a simplified experience.

If you're an existing user of cloud security posture management of Security Hub, you will still be able to continue to use that standalone service. In the AWS console, there will be a new option for Security Hub which you can now opt into and right away initiate a 30 day free trial. If you were using the preview version of the service that we earlier launched in June, you will have an easier transition to just click and update to the generally available version with all your configurations and all your settings still there.

One of the key features of Security Hub is that because it's centralized, security teams wanted the ability to manage all of the AWS accounts across the security operations from one place. We've taken a deliberate effort to aggregate, consolidate, and unify all of this across multiple regions, multiple accounts, and also allow you to have that same centralized configuration that we had built earlier within the service.

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/850.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=850)

### Asset Inventory, Exposure Findings, and the Open Cybersecurity Schema Framework

Some of these details will show you in action. With Security Hub, first and foremost, what you can now look at is  the asset inventory. The first thing customers wanted to know is where all do I even have coverage for my security. This is a deliberate effort that we wanted to provide to customers: within that centralized place when you enable the service now, you can see which all of your accounts and resources in those accounts are protected for threat or which ones are protected for vulnerabilities and where all do you have the right risk configurations or the Security Hub misconfiguration detections enabled.

With that, as your teams add more resources and as they add more AWS accounts, you have one view to look at your asset inventory and the coverage of security that Security Hub is providing. From there itself, you can easily go in and opt in additional accounts whether they are production or development and if you're operating with a CI/CD pipeline, you can easily add more resources to that coverage as well.

Once Security Hub starts aggregating these findings, it uses the underlying AI and machine learning to correlate these findings and in real time start providing exposure findings. Exposure findings are an elevated combination of multiple different findings from our native detection tools as well as our partner tools.

These exposure findings are built on traits, and those traits could be related to misconfiguration, sensitive data detection, network reachability, or whether your RDS databases or S3 buckets are exposed to the Internet. This is a faster way to act on those exposures with the risk and prioritization that the service provides. This eases the triage and alerting process. We deliberately work with our partners to ensure consistency across all findings.

One of the key features that Security Hub provides is that it normalizes all the findings in an open standard format. Earlier, you could take any one of our detection tools and export these findings in EventBridge in JSON format, but all of these detection tools could have multiple different formats, especially if you're using partner tools and sending detections from endpoint detection tools or network security tools. They are in multiple different formats, so we wanted to ensure there is one consistent format. We centralized on an open standard called the Open Cybersecurity Schema Framework, or OCSF standard.

The OCSF standard helps ease the time in integrating these findings into downstream products that support that format and allows you to alert and triage easily. In addition to unification and simplification, one of the other key elements that we gathered from our customers was the pricing aspect. AWS is customer obsessed, and we're constantly ensuring that our services provide the most value at the least cost to our customers.

### Simplified and Unified Pricing Model for Security Hub

Over time, many of our own detection tools have been adding new features. GuardDuty, for example, the foundational service was built on detecting threats across APIs such as control APIs as well as CloudTrail API activity and network activity using VPC DNS flow logs. Customers asked us for runtime monitoring, so we added runtime monitoring to GuardDuty. Customers asked for protection across S3, RDS, Lambda, EKS, and ECS, so we continued to add those protection plans.

With that expansion, all these protection plans had a variable component, and what customers were asking for was a simplified pricing package so they could easily predict and forecast what the pricing or cost of security operations looks like. One of the key elements that we launched is the unified pricing and simplified pricing within Security Hub. The unified pricing comes with three key benefits.

First and foremost, it streamlines all of this into a single invoice or a single bill. Rather than having multiple different variable components, if you want full coverage across threat detection, vulnerability management, sensitive data detection, and cloud security posture management with the CSPM functionality of Security Hub, you can aggregate all of that now into a single invoice or bill. The second big feature that customers asked for was to make it more deterministic, which means rather than always having to predict or forecast how many events are monitored per volume or log events, we can make it more deterministic based on resources, for example, which we know how many are covered because it's shown in the Security Hub console to begin with.

So we've now centered on pricing those components based on resources. The third big component was predictability and the ability to forecast. Customers told us they wanted a tool so they could forecast what that pricing looks like three months or six months down the line. We wanted to give customers flexibility and choice without tying them into any multi-month or multi-year contracts, and it is still a pay as you go model.

[![Thumbnail 1230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1230.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1230)

And for customers who do want multi-month or multi-year contracts, we still offer pay-as-you-go monthly pricing based on deterministic resource-based pricing, and it remains very flexible and easy to use. This ease and simplification has now extended to the pricing of the service as well. In addition, building with partners and integrating downstream response is very important.  We recognize that many of you in this room, as well as our customers, use existing partner tools where you orchestrate automation and response. We wanted to simplify that integration for our customers right from the Security Hub interface.

This is all based on the foundation of the Open Cybersecurity Schema Framework, so partners can automate workflow events such as Jira from the Atlassian suite or ServiceNow. We also work with groundbreaking partners who are leveraging artificial intelligence and automation, like Tines, or if you're using a security incident and event monitoring tool like Securonics or Splunk, you can now easily automate these actions downstream as well. This is the benefit of the Open Cybersecurity Schema Framework. The industry has been rallying behind this open source schema, and we wanted to make sure we're ahead of that as well, providing the same flexibility and choice for our customers.

[![Thumbnail 1280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1280.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1280)

### OCSF Standardization and Service Evolution Summary

 All the findings in Security Hub are now standardized in OCSF format. All incoming findings from our partners are also standardized in OCSF format, which makes the downstream integrations very easy. If your security teams want to send a workflow ticket to the business application team where the resource needs to be remediated or the security event needs to be remediated, or if you're aggregating these downstream in a security analytics tool like Securonics, you can easily do that without moving data back and forth. You can also easily investigate those findings because we announced another key improvement in the keynote yesterday: our CloudWatch Unified Data Store, which builds upon that normalization of the Open Cybersecurity Schema Framework.

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1370.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1370)

Whether you're using Amazon Security Lake for log aggregation or looking forward to the CloudWatch Unified Data Store in the future, it will make the investigation process much easier with our partner tools that are leveraging the Open Cybersecurity Schema Framework.  To quickly summarize before we dive into details on each of these key attributes, the Security Hub brand and name remain the same. However, the service has evolved to help unify across multiple different capabilities such as automated signal correlation, exposure findings, and attack paths. We've simplified the pricing and standardized the format of the findings in the Open Cybersecurity Schema Framework. This is the evolution of Security Hub.

You can continue using the CSPM function of the service if you're still using that. It will be an easy way to upgrade to the next evolution of the service with a 30-day free trial, which you can leverage to see all the new changes and simplification in the console for yourself. With that, I'm going to hand it over to Scott to dive into the details of some of these features.

### Deep Dive into Exposures: Prioritization Through Trait Correlation and Attack Paths

Thank you. Good afternoon or good morning, everybody. Thanks for your time. I'm Scott Ward, a Principal Solutions Architect on the security services product team. This team owns Security Hub as well as other centralized security services focused on threat detection and vulnerability response. I work with customers to help them understand how our services work and collaborate with our product managers and engineering teams on figuring out what we should be building and making sure that what we build will actually be useful for our customers.

I want to talk about some key features of the new Security Hub experience and the benefits and value they add. Some of these features we've had since the preview, and some we've added as part of general availability. The first one I want to discuss is the exposures that were mentioned before. We've received a lot of feedback from our customers that our other security services give them a lot of information and findings around what's going on in their environments, but it can be overwhelming when trying to figure out what the most important things are among all those findings. Customers have had to invest in finding additional people or building their own tooling to wade through that and figure out what they should be focusing on. With exposures, we're addressing this challenge.

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1510.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1510)

[![Thumbnail 1530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1530.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1530)

What we're working on is ensuring that exposures serve as that prioritization  to help you figure out where you should be spending your time and which tasks you should be giving to your team to work on. In the Security Hub console, you will have exposures available to you. They have their own widgets that help you understand the summary of exposures, along with their own dashboard and experience within the Security Hub console. 

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1540.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1540)

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1560.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1560)

Let me talk a bit more about how exposures actually work and how we view exposure. An exposure is some sort of weakness that exists, whether in a particular software package, in the configuration of your resource, or some other security best practice that we think isn't being followed that would put that particular resource at risk from being accessed, compromised, or taken over by an external entity.  

[![Thumbnail 1570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1570.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1570)

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1580.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1580)

The way these work is you have the Security Hub service centrally, and it receives findings from various sources. We have Inspector for vulnerabilities and network reachability, GuardDuty for threats, Security Hub CSPM for cloud posture, Amazon Macie for sensitive data, and we also get information that powers the resource inventory that was talked about earlier.   We collect that information to help better understand additional configuration about your resources and the relationships those resources have with other resources in your environment. All that information is collected into Security Hub.

[![Thumbnail 1600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1600.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1600)

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1620.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1620)

Security then extracts from those findings what we call traits. These are individual security markers about that particular resource and its security attributes that are ultimately coming from that finding.  We extract the traits from all of those findings as they come in, and then we move into the correlation and analysis part. We look at all of those traits and examine them to see if there are any things present for a particular resource that would demonstrate a misconfiguration or a toxic combination of these traits that puts this resource at greater risk.  Ultimately, we generate an exposure finding.

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1640.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1640)

[![Thumbnail 1660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1660.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1660)

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1680.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1680)

An exposure finding is a net new finding in Security Hub. It has its own finding type, and you can filter and sort based on exposure specifically.  One of the really cool things is that when we launched this in preview, these exposure correlations were happening every six hours in batch mode, so you would need to wait every six hours to see if there were any new exposures or updates to your existing exposures.  With the general availability of Security Hub, this is now happening in near real time. As these findings come in, we extract the traits and constantly perform new correlation, which means you get insight about new exposures in your environment more quickly or updates to existing exposures. 

[![Thumbnail 1720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1720.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1720)

I've talked about traits, and there are five types of traits that we currently use from a categorization perspective. Assumability focuses on AWS resources that have an IAM role or policy tied to them. Misconfigurations are a trait focused on misconfigured resources. Reachability is focused on anything that has an open network path to the Internet.  Sensitive data, just as it sounds, is focused on the types of traits that are indicators that sensitive data has been identified. The vulnerability trait is focused on resources that have some sort of software vulnerability or weakness in that software that could potentially be exploited.

[![Thumbnail 1730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1730.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1730)

[![Thumbnail 1740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1740.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1740)

[![Thumbnail 1750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1750.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1750)

When we generate our exposures, we use a severity scale of critical, high, medium, and low.  There is a methodology around how we determine the severity of any one exposure. We have correlated all that information together and identified that there are some traits across this resource that demonstrate a weakness, and we determine what kind of severity we should assign to you.  We look at a couple of things. We look at the ease of discovery, which is how easily an automated tool could actually find this particular vulnerable or exposed resource. 

[![Thumbnail 1760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1760.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1760)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1790.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1790)

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1800.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1800)

We look at the ease of exploit, which is how simple it would be for a threat actor to actually exploit the exposure that we have identified.  We look at the likelihood of exploit, which is the probability that it will actually be exploited. We use things like the EPSS score, the publicly available score of exploitability for certain vulnerabilities. We also use our internal threat intelligence around how vulnerabilities are being exploited in AWS environments to help us inform the likelihood of that exploit. We look at awareness, which is how publicly known these exposures or items that we are identifying through those traits are.   We focus on impact, which is the potential impact from data loss or loss of service that this particular exposure might demonstrate for your environment. We use all of those data points and that information to determine the severity rating.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1820.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1820)

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1840.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1840)

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1860.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1860)

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/1880.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=1880)

Let's talk about a quick example of an exposure to make sure we level set how this works. Everything I'm going to show here will ultimately result in one exposure.  The first part is to have a vulnerability. You have an EC2 instance with 125 vulnerabilities on it, but only two of those are actually highly exploitable. Our exposure is going to focus on those two highly exploitable ones. Reachability is another factor, so the instance is reachable from the internet.  Sensitive data is also important. In this case, Macy's identified that there is sensitive data in an S3 bucket, and we've been able to determine that this particular resource has a role that allows it to access that bucket where the sensitive data has been identified. A compromise of that particular resource may present a risk to that sensitive data as well.  Additionally, we have an image configuration of an EC2 instance using instance metadata version 1, which is no longer a best practice because we have version two of instance metadata that has many more security features built into it. The combination of all of those traits results in one particular exposure finding. 

As we've evolved from the preview announced in June of Security Hub to going generally available yesterday, we've enhanced how we're using exposures and the types of signals that we use to generate exposures. Some interesting things we're also adding and using in our exposure correlation now include end of life operating systems. From Amazon Inspector, we get findings that you're using operating systems that have reached end of life, which presents a risk because those vendors are no longer going to be offering security patches related to that. We use that as another signal in potentially generating an exposure.

Another signal is malicious software packages. Inspector has a really cool new capability where it can identify if you have installed software packages that have malicious code in them. There have been a lot of trends lately where, from a supply chain perspective, certain repositories like NPM have been compromised and malicious software has been injected into those repositories. We're helping to identify when you have those types of risks present, and we use that as part of exposures.

Malicious files are another signal, and this is actually based on GuardDuty. If you are using GuardDuty's EC2 malware protection or the runtime monitoring feature, and we're getting findings from GuardDuty that malware has been identified on an EC2 instance or that runtime monitoring has found that you've executed a known malware file, we use those as traits and signals as part of our exposure. Think about the exposure example I gave you where we said you've got vulnerabilities that are highly exploitable and an internet reachable instance. Now we're also able to say you've got some malware also being executed on that instance. There could be a relationship here, and it could raise that priority.

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2010.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2010)

We're also bringing in signals around GuardDuty regarding brute force attempts. Once again, think about that exposure. If we're also now able to tell you not only is this exposure present, but we're also seeing that somebody's trying to brute force their way into that particular resource, that's just another signal to help you with prioritization and your remediation approach.  Some benefits from exposures include prioritizing those findings so we're trying to give you the most impactful things to focus on first. There's low operational overhead because you don't have to invest your resources going through and doing this correlation and figuring out what these combinations are. We're doing that for you.

We take an intelligent approach, putting a lot of thought into how we do this and trying to apply AWS best practices wherever we can. We use internal and external threat intelligence, combining various sources of threat intelligence to help us inform how we generate those exposures and what the severity of those exposures is. We also use automated reasoning. For network reachability, we're using automated reasoning to give us a math-based answer on whether this resource is actually accessible from the Internet or not.

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2060.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2060)

Once we've generated that exposure, the other key thing that goes with exposure is we give you an attack path.  We help you understand what path a malicious actor would actually take to interact with and access this resource. That attack path is a visual representation that you can look at or show to the owner of a particular resource and say here's the complete configuration and here's where we might need to take a step to remediate or reduce this particular exposure. It's a very powerful visual element around what this exposure is and what it means from a path perspective.

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2100.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2100)

### Simplified Enablement Across AWS Organizations and Accounts

The next thing I want to talk about is simplified enablement.  As you've probably noticed by now, Security Hub is powered by a lot of different AWS security services.

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2120.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2120)

You can certainly go in and turn on and enable each one of those services and configure them, but it can be a challenge to invest all your time to configure and manage those. We've worked on Security Hub to simplify that enablement process so you can do it all directly from the Security Hub service. Let's imagine you're a  brand new customer using our security services today. You're using AWS Organizations with an organization management account, member accounts, and regions that you're running in.

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2140.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2140)

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2150.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2150)

What we've done with simplified enablement is set it up so across different security services—Security Hub, GuardDuty,  Security Hub CSPM, and Inspector—the steps you would take are straightforward. You would go into your organization management account, your top-level account of your organization. You would go to Security Hub  and configure a policy that allows your delegated administrator to manage the configuration across all member accounts. Once you're done in that account, you probably don't need to go back there again for Security Hub.

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2180.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2180)

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2190.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2190)

You then pick a delegated administrator account as part of that process. Now you have a delegated administrator account for Security Hub. At that point, you can say, "I want to turn on the other security services." Security Hub will go in and assign a delegated  administrator across the rest of those services for you. It will then enable those security services across the member accounts that you've chosen for your organization.  It will enable those services and their features across all of the regions that you've chosen.

[![Thumbnail 2200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2200.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2200)

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2210.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2210)

If you want to centralize and view all of your findings from your delegated administrator account in one region, you can also enable region aggregation  as part of this, which would centralize  all of your security findings in one specific region. Now you have the ability to go into a single account—your delegated administrator account—and a single region to view all of your security findings across all your different security services, accounts, and regions.

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2230.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2230)

I want to call out a couple of things here. If you're already using  GuardDuty, Inspector, Security Hub, and CSPM, leave it on. We will inherit all the configuration you already have to feed findings in and use Security Hub. You don't have to turn anything off or do anything different for that. However, you can also use what's there in Security Hub to continue to manage that configuration and enhance it or roll it out across additional accounts or regions. It's there to work with what you have already, or you can use it to start from scratch if you're not using these services today.

The way we do this is through a configuration catalog in Security Hub that you can visit. There are a couple of different options. You've got Security Hub Essential, which will cover all of the different security services and features I've mentioned. There are also options focused on threat analytics, vulnerability analytics, or posture management, so you can pick which one works best for you.

[![Thumbnail 2290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2290.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2290)

Once you're in the configuration console, you give it a name and a description, and then we have the security capabilities. In this case, I've chosen the full Security Hub Essential option.  We're listing out all the posture management, vulnerability management, threat analytics, and posture management capabilities that you can do there. By default, we select everything for you. You then have the ability in the account section to choose whether you want to enable this across all of your accounts, or you can go to an organization tree and say you want these specific accounts or specific organizational units enabled. In the region section, you can either choose all regions or specific regions that you want enabled. We give you a lot of flexibility around how you configure this and roll it out, maybe in a proof of concept, and then continue to evolve it into additional parts of your organization.

[![Thumbnail 2340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2340.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2340)

More specifically, for security capabilities, you also have the ability to customize that. If you want to turn on some capabilities but maybe not turn on some of these, you have that ability to manage that and evolve into it over time.  Here's a key thing I'll call out: let's say you're using GuardDuty already and you use runtime monitoring, but you've unchecked runtime monitoring here for whatever reason you don't want it linked in. We don't turn off runtime monitoring in GuardDuty for you. We will honor the configurations you have in your lower environments if it's already on. However, if you weren't using it and you checked it, we would use that as a signal to turn that on in your services. You could also still go to those individual services and manage that configuration separately, but we're giving you a more centralized way to do that.

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2380.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2380)

We also give you visibility around that configuration. We have an account coverage dashboard, the one on the right there, that helps you understand across all of your accounts where Security Hub is enabled, what your coverage is across each of those security services, and then at the individual account level, what your coverage is. We also have a widget that will be available in the summary dashboard that helps you quickly understand your coverage across each of these. 

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2420.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2420)

### Trends and Executive Dashboard for Security Posture Visibility

It helps you identify where you might have a coverage gap, perhaps where you're trying to burn down to achieve full coverage, or where there might be areas where something got turned off or misconfigured, so you have that visibility now. The last thing I want to highlight here is the trends feature. We added new functionality to give customers more insight into what's going on in their environment versus just a point in time. 

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2430.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2430)

[![Thumbnail 2450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2450.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2450)

There's now a new executive tab  that exists in the Security Hub console where we give you an initial trends overview, which is really valuable because you can go in and see counts at a particular point in time of how much you have from threats findings, exposure findings, and number of resources. But then you can see period over period how this has changed day over day, week over week, and month over month. How has that number changed? 

We then give you widgets focused on trends that range from 5 days all the way up to 1 year as far as the time frame that you can investigate. Across threats, across exposures, and across active resources, you have the ability to create a more upper-level view or take a management-type view of how you're doing over time. Are you seeing improvement or not in these particular spaces? These are very powerful capabilities to help you better understand your overall security posture and how you're doing relative to your security goals.

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2530.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2530)

### Customer Experience: Awesome's Journey with Security Hub and Automated Workflows

Thank you for your time on that. I'm going to hand it over to Lee from Awesome, and let's hear about their experience with the Security Hub service. Good morning and welcome. I'm here to walk you through our journey with Security Hub at Awesome. For those of you who might not know our parent name, you likely know our brands: Smug Mug and Flickr. My name is Lee Zahn, and I've been a site reliability engineer at Awesome for the last 4 years as of Monday. Before that, I spent the last 20 years working in financial services. 

I love our mission statement at Awesome: building a better world through the power of photography. At Smug Mug, we focus on helping photographers run their businesses and sharing their customers' most cherished memories. At Flickr, we're all about helping photographers share, learn, and grow their art with the world. A critical pillar of our mission is trust. We have to keep our users' photos safe. Earlier this year, we had the unique opportunity to review the new Security Hub, and I want to share what that experience looked like for us.

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2560.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2560)

We have 19 years of building on AWS.  That's 19 years of experiments that became production, quick fixes that became permanent, and a continuously evolving architecture. We needed a way to identify and prioritize any potential risks. We already had findings coming in through multiple sources and tools: GuardDuty, Security Hub, CSPM, Inspector, Config, CloudTrail, and third-party tools. But we didn't have any correlation between these findings. There was no context.

Without that context, it was hard to prioritize and route findings to appropriate teams. I was spending a lot of time doing the initial research on findings before I could even get anyone else involved. Once a finding from these tools was assigned, it wasn't user-friendly to do the research on what we needed to do to fix that finding. We had a major visibility problem, especially as our teams and responsibilities naturally shift over time.

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2620.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2620)

Why Security Hub?  First and foremost, Security Hub is an AWS service. It naturally integrates with all the other tools that we're already using. But it's not just about gathering data; it's about taking action. We needed the ability to send notifications directly to Jira or Slack. This allows us to prioritize our work. Instead of a fire hose of alerts, we can identify which items are critical for us specifically and route them accordingly.

We weren't able to take advantage of unified enablement in our preview, but I'm really excited about this. I had previously done the work to enable GuardDuty, Inspector, Security Hub, CSPM, and related security tools across our accounts and regions. This was not an easy task. The ability to enable these tools from a single source with bundled pricing sounds excellent, and then let us view all these findings in one central place.

One of my favorite features of AWS Inspector specifically is the software bill of materials. It gives us an overview of all the software in our AMIs and containers. This gives us a great view. Now with Security Hub, we get a resource inventory of everything. I'm able to generate my own reports against this data, and it is essential in researching issues. If there's a fatal or critical risk finding, we want to know right now.

With near real-time risk analytics, we can trust that these findings are making it to us in a timely manner. But only the findings that are impactful. When I originally set up automations with Inspector, I just got flooded with tickets. Adding notifications across each of the AWS security tools is cumbersome, requires maintenance, and a new system to learn each time. I love that I get findings in one place with one set of automations.

[![Thumbnail 2740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2740.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2740)

This is what our workflow looks like. On the left,  you have our inputs. We aren't just looking at one signal; we're ingesting findings from GuardDuty, from Security Hub, CSPM, and Amazon Inspector. In the past, we had to log into each console for each service. Now it flows into Security Hub as our single pane of glass. But this is the key: a single pane of glass is useless if no one is looking at it.

We didn't want our security team to just stare at a dashboard all day, and that's where the automations come in. We realized that if a finding sits in Security Hub, it's just data. When it moves to Jira, it becomes a task. We build automation that evaluates these findings in real time. It filters out the noise, the low priority stuff that usually causes alert fatigue, and identifies the signals that matter to us most.

Crucially, we didn't want to just dump these tickets into a generic security bucket. That's where tickets go to die. Instead, we route them directly to the teams that own the infrastructure. If it's an EC2 software vulnerability, it goes to the software packaging team. If it's an S3 misconfiguration, it goes to the infrastructure team. Security Hub is a self-service portal for teams working on these issues to do research and dig deeper, and that's where it really shines: connecting that loop back around where it's essential to research these issues.

[![Thumbnail 2840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2840.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2840)

This puts the agency back in the hands of the builders. They get a ticket in the tool they use every day, Jira, with all the context they need linked right back to the finding. They don't have to be security experts. They just have to fix the issue assigned to them. This loop turns security from a gatekeeper into a partner. 

This might sound a little repetitive, but the things that we wanted out of Security Hub we're actually getting. So we see Security Hub acting as that single pane of glass, allowing deeper insight into each finding, no matter the source. This is essential for our technical teams, but I was surprised at how well this works for our non-technical teams. We have trends that was just released, and the trends really show what our security posture looks like across our environments.

When security findings are coming in, it's easier to do the research. Not only does unified enablement look to save on our security tooling costs, but engineer time is expensive, even if it's just a simple loss of focus. We're spending less engineer time researching and prioritizing issues and also routing issues to the right teams much faster. I mentioned breaking my Jira board when I was working on automations around Inspector findings. The findings in Security Hub have already been evaluated and allow us to set up a much more effective notifications.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/2910.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=2910)

So the journey continues.  This is my wish list. As we look ahead, we're excited about bundled configuration and pricing for AWS security tools to make cost management easier. We're also looking forward to more notification options, specifically tighter Slack integrations and better Jira grouping to further automate our ticket creation. We're excited to work with the newly announced AWS security agent and the new AWS security incident response teams and would love to see future integrations with these in Security Hub.

I appreciate these new features in Security Hub as we build a foundation that keeps our promise to our customers, keeping their photos and businesses safe and secure. Once again, thank you for your time. You have a lot of interesting choices to make throughout your day, and thank you for coming here and spending time with us. A couple of things I want to highlight as a takeaway: remember, we're unifying your security services, so we're bringing that into one centralized place so you can get visibility, have the maintenance, and then we're doing the work to help you with more prioritization.

[![Thumbnail 3000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/3000.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=3000)

We're helping you understand what are the most important things in your environment and where should you be spending your time. We're providing things that might even wake somebody up in the middle of the night because we've got the right prioritization. We're really working to help you do more with less by bringing all of this together and doing a lot of the work to help you with that prioritization. Here are a few materials you can certainly use to reference in getting started  with Security Hub.

[![Thumbnail 3020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/69fdbbb5f40dbd1b/3020.jpg)](https://www.youtube.com/watch?v=mYyBQYIeJzk&t=3020)

Remember that there is a 30-day free trial with the Security Hub service so you can get in, give it a try, and see how things work for you. We'd love to get your feedback on what you liked and what you didn't like. We definitely use that to help improve how we deliver these sessions.  If you have any questions, Lee and I will hang out down here and we'll take your questions off to the side of the stage. Thank you for your time. Thanks everybody.


----

; This article is entirely auto-generated using Amazon Bedrock.
