---
title: 'AWS re:Invent 2025 - From AI Promise to KPI Impact: Connected Agents & Orchestration (API204)'
published: true
description: 'In this video, Rahul Dureja from Workato explains how orchestration transforms AI agents from promise to real impact. He demonstrates that while AI agents excel at front-end tasks like writing emails, they lack the ability to act on enterprise applications where critical data resides. The solution is orchestration, which enables agents to interact with systems like Salesforce, SAP, and Zendesk securely. Dureja introduces Workato One and Workato''s Enterprise MCP, which leverages 12,000+ connectors to let AI agents perform actions across enterprise applications while maintaining governance and security. He emphasizes that unlike vendor-specific MCP servers that create tool proliferation, Workato''s unified Enterprise MCP provides a single layer for all integrations, avoiding the fragmentation issues from twenty years ago. The presentation showcases how existing orchestrations can be reused through MCP, enabling AI agents to execute complex business processes without rebuilding infrastructure.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/20.jpg'
series: ''
canonical_url: null
id: 3086202
date: '2025-12-05T11:04:05Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - From AI Promise to KPI Impact: Connected Agents & Orchestration (API204)**

> In this video, Rahul Dureja from Workato explains how orchestration transforms AI agents from promise to real impact. He demonstrates that while AI agents excel at front-end tasks like writing emails, they lack the ability to act on enterprise applications where critical data resides. The solution is orchestration, which enables agents to interact with systems like Salesforce, SAP, and Zendesk securely. Dureja introduces Workato One and Workato's Enterprise MCP, which leverages 12,000+ connectors to let AI agents perform actions across enterprise applications while maintaining governance and security. He emphasizes that unlike vendor-specific MCP servers that create tool proliferation, Workato's unified Enterprise MCP provides a single layer for all integrations, avoiding the fragmentation issues from twenty years ago. The presentation showcases how existing orchestrations can be reused through MCP, enabling AI agents to execute complex business processes without rebuilding infrastructure.

{% youtube https://www.youtube.com/watch?v=wysqbV6_v6c %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/20.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=20)

### The Missing Link: Why AI Agents Need Orchestration to Deliver Real Impact

Hello everyone, I'm Rahul Dureja from Workato, and today we'll be talking about how to get your AI agents from the promise that they have to actual real impact.  Today we'll be covering the fact that AI is definitely powerful, but there's something lacking. We'll talk about how orchestration is that silent force that really makes your AI agents smart, and then we'll close out with Workato's point of view on how we can make your AI agents a lot smarter.

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/30.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=30)

 I bring this up everywhere. AI has been the term for the past few years, and everybody has their own interpretation. Everybody thinks about AI in very different ways, but if you take a closer look, AI is essentially this layering of orchestration with agentic capabilities. You have agents that can really help you perform front-end tasks well where they can take actions for you, such as writing emails for you. But orchestration gives them that superpower where they essentially allow you to work on your enterprise applications and perform actions like creating a new order or adding a new employee to your company.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/80.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=80)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/90.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=90)

 Let's talk about where we are in the AI journey right now. We know that AI is everywhere. There are companies who have onboarded AI agents, and you see the clouds, the Bedrocks, the Geminis everywhere.  But what you see is that they're being primarily used for a lot of front-end activities. People are using their AI agents to help them write emails and find information. But what they're not really doing is acting upon your enterprise applications. The main power and the main data lies in your enterprise applications, the Salesforces, the SAPs, the Zendesks that you have. That's where your main data lies, but your AI is not really able to act upon those applications.

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/110.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=110)

 So what's really missing? Your AI agents are smart and they have the ability to perform actions on your behalf, but why aren't they able to do so? The answer to that is orchestration. If you really try to look at any AI agent or any enterprise company looking to adopt AI agents, four main things are of prime importance. They essentially want governed real-time access to their data. They want process orchestration. They essentially want a way to define roles and escalation paths. And they essentially want a continuous life cycle. All of that are the powers that an orchestration engine can bring to you.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/140.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=140)

 Combining AI with orchestration gives you the combined power where your AI agents can essentially act upon your data. They can interact with your enterprise applications and perform actions on your behalf. So if you have to create a new sales order or onboard a new employee, your agent is now empowered to do this with the ability to orchestrate.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/190.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=190)

[![Thumbnail 220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/220.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=220)

 Now, why does it matter for agents? Orchestration allows you to connect your enterprise applications. It allows application A to speak with application B, which is very important for agents. If agents have to perform something, they need to understand what application they need to connect to, where they extract data from, and where they write data to. You want to implement guardrails.  Security and trust are the number one things that every enterprise looks at. They want to make sure that if they have an AI agent that they're using to perform actions, it should be done in a secure and trustworthy manner. You should be able to use all forms of authentication that are available to essentially interact with enterprise applications, and your AI agent should comply with those.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/240.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=240)

 This should enable collaboration. The way we look at AI agents is as an assistant to an employee. It could help a salesperson bring up deals that have been dormant for a long time. It could help a customer service representative answer questions in a way that solves customer issues a lot quicker. Your AI agent should be able to enable collaboration and should be able to hand over from a human to an agent any time. And finally, leave a trace. You are having your agent interact with your enterprise applications, and you want to make sure that you're able to leave that paper trail of what's happening behind the scenes.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/280.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=280)

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/290.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=290)

 And that's where orchestration comes in.  Orchestration is your silent force. It's the force that's going to enable your agents to be a lot smarter. It's the force that's going to enable your agents to essentially act upon your enterprise applications and make them more meaningful. Now, let's try and look at it from a business process perspective. Every single enterprise customer has multiple business processes.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/320.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=320)

### Enterprise MCP: Bridging AI Agents and Enterprise Applications Through Orchestration

These could be order to cash, procure to pay, or any other process that you have. At the bottom layer is where you have all of your endpoints. This is where your data resides. These are your systems of truth for data,  which could be your SAPs, your Salesforce applications, or your other ERPs. On top of that, you have your connectivity fabric. This is how you essentially interact with these applications.

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/340.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=340)

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/350.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=350)

Moving forward to today, you already have your agentic layer with agents that are running,  and you want your agents to perform actions on your end applications. What's the missing link? It's the orchestration layer.  The orchestration layer is the one that will allow interaction of your AI agents with your underlying applications, which also lets you enable this deep action layer where your agents can really perform actions on your data. They can perform tasks based on what you're trying to do. They can mimic what a human can do, and that's where their real power lies. That's the power that orchestration brings.

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/380.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=380)

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/400.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=400)

Many customers have already built a lot of orchestrations.  The whole idea of orchestration and iPaaS is not new, and there are customers who may have already built hundreds of thousands of integrations with AI in the mix. They don't really want to rebuild everything. They should be able to reuse what they've already built. This is an example  where an AI app they've onboarded can now interact with your pre-built integrations through MCP. MCP is a great example where it can really enable your AI agents to become a lot smarter without having to put a lot of effort because you already have built your integrations. You've made sure that all of your integrations are using the right security protocols, they're interacting with the right applications, and they're using the right accounts. Now you can put your AI agent through MCP and essentially interact with your end applications and perform those actions.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/440.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=440)

This is where I'd like to talk about Workato's approach.  With Workato, our goal was to essentially help enable AI agents to be a lot smarter for them to interact with their end applications and to take actions on your behalf. Workato always started off as an orchestration platform, and we use the power of our orchestration platform to layer agentic capabilities on top of it. With our agentic platform, it uses the underlying power of orchestration to interact with our end applications and it can allow you to have your AI agents interact with those end applications through MCP.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/480.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=480)

This is where I'd like to introduce Workato One.  If you talk to any director or any VP in any enterprise company, these three facets are the most important for their AI adoption journey. They want access to the right knowledge, which means that they want access to their company documents. They want orchestration because they want to make sure that it can actually perform actions on a user's behalf. The number one thing is trust. They want to make sure that all of this happens in a layer of governance and in a layer of security. These are the three things which we kept in mind when designing Workato One.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/520.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=520)

With Workato One, as you can see here,  we started off as an orchestration platform, which is our iPaaS Plus, but then we layered on top our agentic platform which essentially allows you to build your own agents or leverage Workato's Enterprise MCP to go ahead and empower your existing agents to take advantage of your existing orchestrations. You might ask me whether you need to have Workato Orchestrate to use Workato Agentic. The answer to that would be no. You may have built your orchestrations in any language or in any software, and you would be able to use Workato's MCP to enable those orchestrations to be used with your AI agents and interact with your end applications.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/560.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=560)

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/600.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=600)

That's where I add another layer to my diagram earlier, the Enterprise MCP.  My Enterprise MCP allows my AI agents to essentially interact with my end applications, and now all of my deterministic processes can now be done in a non-deterministic way through the use of my AI agents. That's where the power of MCP lies. That's the power that Workato is essentially talking about, the power of Workato's Enterprise MCP which allows you to interact with these applications. With an Enterprise MCP, the idea was very simple.  The idea was the fact that you should be able to use your existing orchestration layer.

You've already done a lot of work connecting to your applications. You may have ERPs and CRMs that you've already connected to, and you're going to use that to empower your agents. You already have agents performing all of your front-end tasks. The question is how do you go from those agents who are just performing your front-end tasks to actually performing actions on your applications? That's where Workato's Enterprise MCP comes in.

Workato's Enterprise MCP takes into account Workato's 12,000+ connectors that allow you to interact with applications. It uses our preexisting governance model to ensure that you're able to interact with applications in a secure way. You're able to impersonate a user to perform an action on your behalf. So if I am a support person who wants to check the status of a request, the action can be performed on my behalf with Workato's Enterprise MCP.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/670.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=670)

Imagine having your agent—whether it's a cloud-based solution, ChatGPT, or Bedrock—say "summarize my open sales pipeline." It can connect to your Salesforce instances, look at your call records, because you've already built those connectors, capture all of that data, and present to you one unified view of your sales pipeline.  The opportunities with MCP are endless. You can apply MCP in multiple different scenarios. You can take your existing business processes, for which you already have the integrations, expose them via MCP to your AI agents, and your AI agents will help you perform that. What that does is really help adoption at scale.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/750.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=750)

Earlier, if you had to perform a business process, you would have to understand what APIs are available, what services are available, and then somebody would have to stretch that through in a very deterministic manner. Now with MCP, all of that is done through an AI agent. MCP discovers those APIs, discovers those applications, and performs that action for you. That's where the power of MCP lies. 

### Workato's Enterprise MCP Advantage: Unified Control Over Vendor Proliferation

A lot of times I get asked why you would choose Workato's Enterprise MCP over vendor-specific MCP servers when every vendor is talking about MCP servers and many CRM and ERP applications have their own MCP servers. It all goes back to the whole idea of tool proliferation. Back in the days when orchestration started, every single company had their own tool. SAP had their own integration tool, Oracle had their own tool, and all of these tools required you to have a separate level of skill for each tool. They also required separate governance models and a separate set of people managing these tools.

Now with MCP as well, it's the same thing. Every vendor is going to have its own MCP tool with its own security model and its own governance model, which gets us to the same point we were at about twenty years ago. We get to a point where there's again tool proliferation, or in this case, MCP proliferation. This makes your life even harder. But with something like Workato's Enterprise MCP, where you can interact with all of the applications you have through a single unified layer, it allows you to accelerate your value because you're building out an MCP server which is exposed to your agents and follows a security protocol that you have built out.

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/870.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=870)

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/890.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=890)

You do not need to rely on the security protocols from your vendor-specific MCPs. It's powered by Workato One, and our entire connectivity landscape helps you empower your entire MCP adoption.  Without an enterprise MCP, you're not orchestrating. You're actually outsourcing control to every vendor in your stack because you're at the mercy of every vendor that has their own MCP server to adapt to their standards and guidelines. That's where Workato's Enterprise MCP becomes very important. 

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/900.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=900)

Finally, we talk a lot about MCP, but one thing that's core to MCP and to AI, and for AI to be smarter, is the orchestration layer. 

The orchestration layer is critical to your integration strategy. The orchestration layer needs to be powerful and adaptable, allowing you to build your integrations in a manner that is secure, governed, and scalable. That's where Workato's enterprise-grade capabilities come out of the box. You have access to a completely managed offering and can deploy your applications in an always-on model. Workato provides zero downtime upgrades, log and audit streaming, and the ability for your orchestrations to listen to your applications in multiple ways.

You can listen to a change in an object inside of Salesforce, or listen to a new file added to an S3 bucket, and all of that can be done through Workato. The idea is that while AI agents are the future, orchestration still remains essential. You need to partner with a platform that can help make your orchestrations faster, secure, and more enterprise-grade, and that's where Workato comes in.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/abc8ce89cd0d9a08/990.jpg)](https://www.youtube.com/watch?v=wysqbV6_v6c&t=990)

I would recommend all of you to visit our booth at 1085. We have multiple live demonstrations available that will showcase our agentic orchestration platform, demonstrate the power of Workato's Enterprise MCP, and give you a flavor of what Workato can do for your business.  Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
