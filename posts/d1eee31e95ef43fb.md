---
title: 'AWS re:Invent 2025 - AWS Support: New Tiers Making Customer Operations Effortless (SPS325)'
published: true
description: 'In this video, George He, Senior Product Manager at AWS, introduces the redesigned AWS Support with three new tiers: Business Support Plus for scaling startups with AI-powered 24/7 assistance and 30-minute response times, Enterprise Support for organizations at scale featuring Technical Account Managers and 15-minute response times, and Unified Operations for mission-critical operations with Domain Specialist Engineers and 5-minute critical incident response. He demonstrates how AI-powered intelligence, proactive detection, and human experts help customers navigate Gen AI deployments, from initial architecture decisions through production outages to global-scale operations, fundamentally transforming support from reactive to strategic partnership.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/0.jpg'
series: ''
canonical_url: null
id: 3088045
date: '2025-12-06T03:39:54Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - AWS Support: New Tiers Making Customer Operations Effortless (SPS325)**

> In this video, George He, Senior Product Manager at AWS, introduces the redesigned AWS Support with three new tiers: Business Support Plus for scaling startups with AI-powered 24/7 assistance and 30-minute response times, Enterprise Support for organizations at scale featuring Technical Account Managers and 15-minute response times, and Unified Operations for mission-critical operations with Domain Specialist Engineers and 5-minute critical incident response. He demonstrates how AI-powered intelligence, proactive detection, and human experts help customers navigate Gen AI deployments, from initial architecture decisions through production outages to global-scale operations, fundamentally transforming support from reactive to strategic partnership.

{% youtube https://www.youtube.com/watch?v=H7SSM4_dZok %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/0.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=0)

### The Challenge of Deploying at the Speed of Innovation

 Hello, good afternoon. I want to start off by getting to know some of the audience here. If you could raise your hand and let me know if you've deployed an application or something into production in the last 6 months to 1 year, please raise your hand. Yes, that sounds about right. And then keep your hand up if that application and deployment went exactly as you planned on day one. Yes, that's about what I expected. So we all understand that the world is evolving faster than ever before, and that makes launching new products, and especially in AI, which I know we're all talking about today, increasingly difficult. My name is George He. I'm a Senior Product Manager at Amazon Web Services, and over the next 15 minutes, I'm going to show you how we've completely redesigned AWS Support to help you move faster at the speed of innovation.

[![Thumbnail 60](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/60.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=60)

 Let me show you what that means. As I said, we know the world is moving faster than ever, so I've taken a bunch of screenshots from just yesterday of all the great announcements that AWS has made across Nova, Bedrock, CloudWatch, security, GuardDuty, and S3. There's a new advancement from AWS, but all over the world, from the world of AI and technology and cloud, there seems to be a new advancement every few weeks. Each one seems to change the game, and each one potentially makes your current deployment obsolete. So how do you keep up and how do you choose? How do you know that when you go and deploy a specific configuration, you're not making a potentially million-dollar architectural mistake?

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/110.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=110)

 Let me tell you a story, and I hope that it sounds familiar. I'm going to start from the bottom here, and I promise I'll fill it up as we go. Let's say that you're starting out and you're aiming to deploy something into production. It goes like this: week one, your CEO comes to you and says, "Hey, we need AI." I'm sure that's a common story these days. By week two, you're exploring and deep diving, but you're drowning in options. There's Claude, there's Bedrock agents, there's SageMaker. There are tons of almost infinite options for configurations. So how do you configure it correctly?

By week three, maybe you are deploying it, maybe you're working through it, you're testing it, but you're burning potentially through compute costs because you're testing configurations and working things out. Here, you want to move fast, but you're dealing with decision paralysis. You're dealing with tons of decisions on your own. And then you start scaling.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/170.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=170)

 Let's say it's month three, and I'm hoping that you're working very fast here. You launch, it works, and users love it. But two weeks later, it's 2:00 a.m. and your production just went down, and your phone is ringing. It's now 3:00 a.m., and things are off the hook. By this stage, you are potentially spending more time firefighting and putting out operational problems than you are actually building. Here, you want to build and innovate fast, but now you're stuck dealing with operations. And then as you go, you start becoming mission critical.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/210.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=210)

 That means that you're going global, you have multiple regions, you have regulatory compliance issues to deal with, security audits to deal with, your team is getting stretched thin because of all of these different pieces that have to come together, drift is happening, potentially your training is falling behind. So now you're dealing with the reality of being a mission-critical service that's operating at a global scale. And that, of course, makes you wonder: are you ready for what tomorrow potentially holds?

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/240.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=240)

 So like I said, I hope this sounds familiar because this is a story that we at AWS Support have seen hundreds of times across hundreds of customers.

[![Thumbnail 250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/250.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=250)

### Redesigning AWS Support: Four Tiers from Business Support Plus to Unified Operations

 That's why we wanted to rebuild support from the ground up to meet your needs. That means that we wanted to shift left and be closer to your operations and be closer to the problems that you have and help you achieve exactly what you're looking to achieve. So by doing that, we believe that we will help you build faster and operate smarter. Specifically, here's what we're doing with our new plans. We are, number one, helping you proactively build resilient architectures and plans.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/290.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=290)

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/300.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=300)

That means that we help you to make those key decisions ahead of time, so that you have stability now and down the road. Then, we're using AI to detect  issues before they impact production, meaning that these problems are identified earlier before they actually become a problem. Then,  our AI agents are giving context and collecting context across your accounts, across your workloads.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/320.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=320)

So that we can pass that context on to support experts to help you identify exactly what the key root cause of your issue is. This enables us to deliver faster response times  from our experts and contextually accurate responses and recommendations.

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/340.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=340)

So what does this all come together to look like? We have four support plans. Starting from the very bottom, we have our Basic support plan,  which provides customers access to basic documentation and health views. Then there's the new Business Support Plus, which offers an AI-powered 24/7 assistance agent that can answer all of your questions. When something does go wrong, you can open a case with support engineers and experts, and you can expect a response within 30 minutes. This is the perfect level of support for customers who are headed into production, working on a proof of concept, or looking to deploy their first application and get their questions answered.

Moving up, we redesigned Enterprise Support. Enterprise Support includes everything from Business Support Plus and access to a Technical Account Manager, or TAM. That person will work with you to develop a strategic plan to achieve your goals. It also includes a 15-minute response time for production-critical issues and AWS Security Incident Response at no additional fee, which gives you access to security experts to help triage any security incidents. This is the tier for customers who are operating at scale with real users.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/440.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=440)

Finally, at the very top, we're launching Unified Operations, which is our new highest tier of support. Unified Operations gives you access to a full team of AWS experts who will work with you to provide guidance on everything from billing to security to architecture. It also gives you peace of mind during large deployments and critical events with a Domain Specialist Engineer who can sit with you during these peak events. 

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/470.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=470)

### Real-World Gen AI Deployment: How Each Support Tier Delivers Different Outcomes

I know I just spoke a lot and it may be interesting and abstract, but let me show you what this looks like by walking you through a hypothetical Gen AI deployment journey. These are real problems that customers in aggregate have brought to us. Continuing with my timeline from earlier,  let's say you're two weeks into your Gen AI project. That means you need to make architecture decisions now for your new application. But you're dealing with complex cost structures and differing pricing models between all the different models available to you. You have nearly infinite service configuration options and potentially fragmented documentation between AWS, 30 third-party providers, and even your own codebase. All that time, your CEO or tech lead is asking you more and more, putting pressure on you for a launch date.

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/510.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=510)

With Business Support Plus,  you can simply ask those questions to our context-aware support assistant in the case console. I actually tried this last night. You can ask something like, "Which model should I use for a customer service chatbot that gives me less than 200 milliseconds latency?" Give the support assistant your requirements and some context, and it will provide an instant recommendation in real time. You can follow up with the assistant with additional questions or open a case to our expert engineers, who can help guide you through configuration choices and help make sense of potentially confusing documentation.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/560.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=560)

What does this all mean? Your decisions can now actually be made on day one, saving you time on your deployment and dealing with fewer decisions on your own.  Let's fast forward to month three. You've successfully deployed your Gen AI application. But now it's 2:00 a.m. on a Tuesday and an on-call alarm is going off because your production just went down. If you're dealing with this on your own, you might get a cryptic error message that you have to troubleshoot. You might raise a case to AWS, but you're worried that an engineer may not get back to you in time. All that while, you're losing customer trust, customer confidence, and potentially revenue every minute.

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/600.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=600)

Meanwhile, with Enterprise Support,  we can help you navigate through that difficult outage. Your TAM can work with you before an outage even happens to help set up a failover zone or failover region. That means your customers may never even notice that an outage occurred.

You can raise a case, and within 15 minutes, a support engineer is working with you with all the context of your problem to help you resolve the issue. If it's a security problem, Security Incident Response can alert you and connect you to experts immediately who can help guide you and triage the issue.

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/660.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=660)

After resolution, your TAM can sit down with you and work together to figure out what went wrong and how to build so that problem will never happen again. Throughout this entire process, your customers may never even know that something went wrong. This is not just faster support that we want to deliver here. We want to deliver a fundamentally different outcome to your customers. 

We're going to jump ahead to month 9. Now your application is mission critical and operating at a global scale. That means you have multi-region architecture decisions to make. These can be complex and confusing, and you're going to have to convince your team that you're making the right decision every single time. You're going to have to deal with security and compliance challenges between regions, and most importantly, every decision has high risk with zero tolerance for that risk now.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/710.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=710)

When you do make a deployment or decision, there's an extended war room for your whole team every time you have that critical deployment. Even though you're now operating with zero tolerance for downtime and operating at a global scale, that pressure is starting to put stress on your team.  But instead, with Unified Operations, we have a team of AWS experts who can help guide you through those difficult deployments. Your TAM can help you with global architecture design, which helps take away some of that decision paralysis. Domain Specialist Engineers can join you for key launches and peak events, providing you that guiding hand during critical events.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/760.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=760)

When things do go wrong, you can have critical incident response within 5 minutes. This goes beyond just making support faster. We're creating a strategic partnership and looking to be a part of your operations, not just waiting by the phone for your call.  So how do we actually do this? We really focus on three key pillars. First is AI-powered intelligence. With new agents like Support Assistant, AWS DevOps Agent, and Security Agent, they're now analyzing millions of deployments just like yours, learning patterns and predicting issues. That means AI is now handling the noise, and you still get instant answers to all of your questions.

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/830.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=830)

We're also doing proactive detection, which gives you real-time monitoring, anomaly detection, and predictive alerts. This means we see the problem before it ever becomes an incident and can detect problems faster than ever before. All of this feeds into our human experts, who are the TAMs, the Domain Specialists, and the 24/7 engineers. AI helps bring them the context, and they can bring their judgment and expertise, meaning both faster responses and more accurate responses. 

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/850.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=850)

You don't have to take my word for it. Here are a few real customers with real results. I'm not going to go through every single one of them, but I want you to be assured that these aren't outliers. We have thousands of customers with AWS Support who've transformed their operations with our help. 

Let me recap what I've covered. Here's what you need to know. There are three new tiers, and we've always kept Basic Support. So there are three new tiers for three types of customers. Starting from tier two, we have Business Support Plus for those startups and businesses that are looking to scale. We have Enterprise Support for those organizations that are operating and growing at scale. Finally, we have Unified Operations, which is for those customers who are now operating at mission critical scale. We give you three choices, so you can always have the option to choose based on where you are in your journey.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/890.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=890)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d1eee31e95ef43fb/900.jpg)](https://www.youtube.com/watch?v=H7SSM4_dZok&t=900)

With that, I invite you to come visit the AWS Support booth over in C50, which is that way. Feel free to scan this QR code for any more information.  Thank you, and come visit us to learn more. 


----

; This article is entirely auto-generated using Amazon Bedrock.
