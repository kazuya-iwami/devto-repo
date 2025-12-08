---
title: 'AWS re:Invent 2025 - Reimagining incident response with MCP on AWS (DEV340)'
published: true
description: 'In this video, Darshit Pandya, Senior Principal Engineer at Serko, introduces the concept of MTTC (mean time to context) to address context paralysis during incident response. He demonstrates how traditional monitoring creates chaos with overwhelming alerts but no clear story. The solution uses Model Context Protocol (MCP) with Amazon Bedrock to provide intelligent incident analysis. Three specialized MCP servers are presented: Auth Flow Context for authentication intelligence, Service Dependency Mapping for cascade failure prevention using Claude Haiku 3, and Recovery Coordination for human-in-the-loop remediation using Claude Sonnet 3.5. The architecture integrates CloudWatch, DynamoDB, Lambda, and EventBridge to deliver actionable intelligence rather than raw metrics. The key philosophy emphasizes engineering clarity over automating chaos, with AI organizing information while humans make final decisions.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Reimagining incident response with MCP on AWS (DEV340)**

> In this video, Darshit Pandya, Senior Principal Engineer at Serko, introduces the concept of MTTC (mean time to context) to address context paralysis during incident response. He demonstrates how traditional monitoring creates chaos with overwhelming alerts but no clear story. The solution uses Model Context Protocol (MCP) with Amazon Bedrock to provide intelligent incident analysis. Three specialized MCP servers are presented: Auth Flow Context for authentication intelligence, Service Dependency Mapping for cascade failure prevention using Claude Haiku 3, and Recovery Coordination for human-in-the-loop remediation using Claude Sonnet 3.5. The architecture integrates CloudWatch, DynamoDB, Lambda, and EventBridge to deliver actionable intelligence rather than raw metrics. The key philosophy emphasizes engineering clarity over automating chaos, with AI organizing information while humans make final decisions.

{% youtube https://www.youtube.com/watch?v=VSDEuFu1sro %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/0.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=0)

### Context Paralysis: Why Traditional Incident Response Creates Chaos in the Cloud

 All right, all right. How are you all? Hope you all had a great time at the reunion so far. Raise your hand if you've ever been paged at midnight or at 3:00 a.m. or after office hours. Quite a few, then you are in the right room. You know, the most challenging part in incident response isn't missing the data, it's missing the context. To achieve better MTTR, which means mean time to resolution, I'm coining a new term. MTTC is very important. MTTC means mean time to context. But before we dive in, let me show you why this matters.

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/50.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=50)

 Picture this, it's almost midnight and your authentication service is going down. Your pager goes off, Slack alerts are lighting up. Within one minute, you are staring at a wall of alerts with tens of CloudWatch tabs open on your laptop. And this is where the ride begins of chasing the symptoms of your incident. Everything looks critical. Nothing tells you what actually started it. We have data, but no clarity. You can see we have tons of different alerts telling all the details, but not telling the complete connected story. This is the chaos we all know, and this is the context paralysis. Sounds familiar, right?

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/110.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=110)

 But before we dive further into this, let me introduce myself. I'm Darshit Pandya, Senior Principal Engineer for Platform at Serko. I'm from Auckland, New Zealand. Originally from India, but for the last decade, Kiwiland has been my home. I'm an AWS Community Builder since 2021 in Serverless. Also, I'm an organizer of Serverless Meetup Auckland, Platform Engineering New Zealand Meetup, and co-organizing ANZ Serverless Days and AWS Community Days New Zealand. Feel free to scan this QR code and connect with me.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/150.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=150)

 So here is what we are going to cover today. These are the four key areas. First, the context paralysis, why traditional approaches create chaos during complex incidents. Second, from chaos to platform intelligence, a better intelligent approach using Model Context Protocol and AWS services. Third, from chaos to clarity, the MCP blueprint, a technical implementation which you all can use as well. And last, beyond MTTR, building the calm into the cloud, the philosophy that changed everything. By the end, you will have a blueprint of this implementation as well. Let's start with the problem.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/200.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=200)

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/210.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=210)

 So what exactly does this context paralysis mean? Let me show you.  Here is what chaos looks like. Alerts slam Slack. Within one minute, tons of alerts are flooding up. CloudWatch errors, RDS throttling, DynamoDB going down, your Lambda timeouts. Many things are keeping a story within minutes of the incident as well. The data isn't missing here. It's overflowing. Now, the guessing begins in the war room. Is this Lambda choking the DynamoDB, or is DynamoDB slowing down the Lambda? Is this the last deployment causing the issue, or is this a cascading problem? Or is this an upstream problem? Something is not right, but in between all of these things, a single statement cuts through the noise. Is this fixed yet? All the stakeholders are asking the same questions. Customers are waiting for that answer. And in this time, everyone is staring at the signals, but no one can see the story. That's context paralysis. Too much data, no shared map. Let me show you what this looks like in the architecture.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/290.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=290)

 This is an e-commerce service architecture, standard, robust, and serverless pattern. But during the incident, it becomes a chain of cascading symptoms.

First, we get an alert from Cognito. Immediately it triggers 4XX or 5XX errors for Amazon API Gateway. This causes Lambda functions error rates to spike, cold starts start increasing, the system crashes, and finally Lambda is in a retry loop hammering the RDS instance. The problem is these alerts are separate. Whenever we are receiving the alerts from our microservices or authentication Lambda, we are receiving alerts, but separately. In the war room situation when you have an incident under pressure, the human brain struggles to reconstruct this picture or this sequence to identify the root cause, and humans miss obvious patterns when they are stressed. What if AI could organize this chaos rather than adding on top of that? That's exactly what we will see next.

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/390.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=390)

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/400.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=400)

 So how do we move from chaos to platform intelligence? The solution isn't another dashboard, not another alert, or not another metric. On the left,  you have the chaos, and on the right, there are the actions. That's the traditional approach everyone is using until now. But when you add MCP server, the Model Context Protocol, it sits in the middle, providing the context. It correlates signals, applies AI analysis with Amazon Bedrock, and presents you with meaningful actionable intelligence instead of raw data.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/440.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=440)

I have built three specialized MCP servers. First, Auth Flow Context, which analyzes your incident.  Second, Service Dependency, which is going to isolate your incident. And third is going to provide you recovery coordination to execute as well. Let me show you each one in detail.

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/460.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=460)

### Building Platform Intelligence: The MCP Blueprint for Calm and Contextual Incident Response

Auth Flow Context is your authentication intelligence  engine. It has three core tools: correlate your auth events, get authentication health to see what cascading impact is happening, and detect the bottlenecks in your incident. This is pulling the data from Amazon CloudWatch, DynamoDB, and EventBridge. It's talking to Amazon Bedrock with Claude Sonnet 3.5. We are using this Claude Sonnet 3.5 for complex pattern recognition. This is the difference between proactive and reactive response.

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/500.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=500)

Next, Service Dependency Mapping. Service Dependency is your cascade failure prevention  system. Four key capabilities are there: it maps your service dependencies, predicts the cascade, suggests isolation, and creates dependency visuals. Here we are using Claude Haiku 3, because it's required to generate faster responses which can show you where is the problem in your entire system, thus containing it before any problem spreads in the entire complex architecture.

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/540.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=540)

And finally, Recovery Coordination. So Recovery Coordination  is going to keep humans in the loop as well. It's not going to provide direct resolution or execute steps automatically. It suggests the remediation actions and executes low-risk action items. It will categorize low, medium, and high-risk action items as well. While medium and high-risk actions will require human approval, so humans can check what suggestion is coming out. Shall I execute? Is it going to increase my bill? Is it going to increase my instance? So many things it can provide you information as well. And for this one, we are using Claude Sonnet 3.5 for remediation action generation, risk assessment, human approval, and recovery process prediction.

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/590.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=590)

Now let me show you how all these three MCP servers work together. So this is from chaos to clarity, the MCP  blueprint. This is the complete serverless architecture, MCP intelligence blueprint. Claude Desktop sends a request to the MCP server, and the appropriate MCP server calls AWS clients like CloudWatch, DynamoDB, AWS Lambda, and EventBridge.

The gathered real-time data is then sent back to Amazon Bedrock. Amazon Bedrock will pick the associated cloud model and return a proper analysis from your traces, metrics, and logs as well. This isn't theoretical. This is production ready, which you can use in your AWS environment right now as well.

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/650.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=650)

Let me show you these interactions with some real snippets when I was working on this one as well. This is the auth flow context.  When the incident happened, you have seen that the Slack alerts were flooding with the incident. When you are asking to analyze my current incident and provide the context, first the auth flow context will kick in. It will start analyzing and provide you immediate suggestions as well, such as what is the current status and which services are impacted. Because with microservice architecture, you cannot cover everything, but here is the thing: it will tell you which service has the most impact. The auth-service, order-service, and payment-service are the critical services having an impact right now. In between, we can see that Lambda cold start was detected in the auth pipeline, causing 800 millisecond latency. That's predicted to cascade to users and other services as well. That's the dependency, that's the context, not just metrics. That's actionable intelligence.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/720.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=720)

Service dependency immediately maps the blast radius.  So instead of guessing which service might be affected, we have a clear visual cascade path for isolation and remediation. That is a circuit breaker cascade failure path. Here you can see that in different colors with amber, green, or red, it shows where the actual problem is and what is about to start as well. The cascade failure path goes from authentication to user to the order-service. It estimates the impact and suggests isolation with a circuit breaker on the user-service. That's important. This predictive incident response means we are getting ahead of the problem, not chasing it.

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/770.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=770)

Recovery coordination provides the action plan. This is the third MCP server,  which is going to provide you recovery coordination and suggest specific actions with a risk assessment. Low risk actions are enabled where users can just click the execute button and those actions will be executed into production. Medium and high risk actions require human approval. Once the line manager or on-call incident manager is approving that request because of the sensitivity around it, it will also execute in your production environment as well. This is production ready incident intelligence.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/820.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=820)

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/830.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=830)

This brings us to the philosophy behind all this: context matters to achieve the right MTTR, so focus on context.  But this isn't just about faster incident response. It's about beyond MTTR.  This shows the paradigm shift. The traditional approach involves a number of Slack strikes, threshold breaches, Slack alert floods, and human stress. But with MCP and Amazon Bedrock, we have real-time correlation, Amazon Bedrock analysis, MCP informed clear context, and human in the loop. We are not replacing humans, but we are empowering humans. Intelligence amplification through contextual correlation, not metric multiplication.

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/870.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=870)

As Werner Vogels reminds us, everything fails all the time.  And this is the fundamental truth of distributed systems. The question isn't about how to prevent failure. The question is, how calmly does your system help you to understand and respond when this happens. That's what we are building here, not failure prevention, but calm and intelligent response in production.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/900.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=900)

Here are the key lessons. Three key lessons: context  with metrics, that's reality.

AI organizes and humans decide. Humans still have the power to decide if a suggestion is right to execute right now or if they can wait for a while, and this will help you in the post-incident action items as well. The system is designed for calm. AWS Lambda and MCP server are helping you build these capabilities into your production environment. The focus is on intelligent amplification through contextual correlation, not metric multiplication.

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/320aea12ad2fecbf/940.jpg)](https://www.youtube.com/watch?v=VSDEuFu1sro&t=940)

Let me leave you with one  final thought: don't automate your chaos, engineer clarity. This is the most important lesson. Most engineers try to automate their existing chaos with faster alerts, more dashboards, more metrics, and bigger on-call rotations. This is just automating the problem. Instead, engineer the clarity into the system itself. Build the context for agents that understand your architecture. Use AI to amplify human judgment, not to replace it. Create calm interfaces that work consistently under pressure.

Your next incident is inevitable, but your response to that doesn't have to be chaotic. Thank you. I would love to hear from all of you how you're going to solve your incident response, and I would love to connect with all of you as well. Please scan this QR code, and thank you for attending this session.


----

; This article is entirely auto-generated using Amazon Bedrock.
