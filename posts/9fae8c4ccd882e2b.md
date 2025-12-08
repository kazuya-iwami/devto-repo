---
title: 'AWS re:Invent 2025 - Intelligent security: Protection at scale from development to production-INV214'
published: true
description: 'In this video, AWS CISO Amy and colleagues discuss scaling security teams in the AI era through three key strategies: embedding security expertise throughout development workflows using primitives and automated tools, adapting to evolving threats by measuring the right metrics like fix speed rather than finding volume, and partnering closely with business to solve actual problems. They showcase AWS internal systems like Blackfoot handling 312 trillion flows daily, MadPot engaging 550 million malicious activities, and the new AWS Security Agent that transforms security intentions into mechanisms. Eli Lilly''s Andrea Abell shares how threat modeling third-party integrations with AWS partnership creates a positive flywheel, demonstrating practical supply chain risk management at scale.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/0.jpg'
series: ''
canonical_url: null
id: 3088252
date: '2025-12-06T06:45:41Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Intelligent security: Protection at scale from development to production-INV214**

> In this video, AWS CISO Amy and colleagues discuss scaling security teams in the AI era through three key strategies: embedding security expertise throughout development workflows using primitives and automated tools, adapting to evolving threats by measuring the right metrics like fix speed rather than finding volume, and partnering closely with business to solve actual problems. They showcase AWS internal systems like Blackfoot handling 312 trillion flows daily, MadPot engaging 550 million malicious activities, and the new AWS Security Agent that transforms security intentions into mechanisms. Eli Lilly's Andrea Abell shares how threat modeling third-party integrations with AWS partnership creates a positive flywheel, demonstrating practical supply chain risk management at scale.

{% youtube https://www.youtube.com/watch?v=q3ZRbCTnB3U %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/0.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=0)

[![Thumbnail 10](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/10.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=10)

### Introduction: Scaling Security Teams in the Age of AI

  Good morning. Thanks for joining us this morning, everyone. I'm super excited to be here. I'm Amy. I'm the CISO for AWS, and I'll be joined by Neha Rungta, one of my colleagues, and Andrea Abell from Eli Lilly, and we're going to talk about scaling your security team. I'm super excited about this because securing systems at scale and adapting to the changes around us is a super important topic, especially right now.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/40.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=40)

 I don't know if you've heard, but there's been some technological changes around AI in the last couple of years. I'm going to mix it up in this innovation talk. I want to talk a little bit about how we do this inside at AWS. I want to talk a little bit about how we do this for you, our customers, and then I want to share a great customer story that really inspired me.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/70.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=70)

So it's not just AI, but AI is definitely a great example of how the game we play as security teams is changing.  Traditional ways that we've achieved strong security for our customers have often relied on deterministic systems, on us understanding that if we do this, then that happens or doesn't happen, and that's not enough with non-deterministic systems. So we can't know for sure that if we do this in terms of defense, this exactly is what's going to happen in terms of protection. We don't yet fully know how adding Gen AI to the mix is going to reshape things, right? This is still a field that's changing super fast, but we are starting to learn about some problems that AI is helpful to solve both for us and for threat actors.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/120.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=120)

### The Evolving Threat Landscape: How Gen AI Changes the Security Game

And so I want to talk through those a little bit before we get into three things that I want you to encourage your security teams to do in order to scale to the changes ahead. So first, on the threat actor side, the thing that I think it's most important to think about is that  Gen AI makes it really inexpensive to generate meaningful targeted variations for threat actors. So the economics of making attacks targeted to a particular phishing target or brand have gotten much cheaper, and we're seeing broader deployment and more convincing lures because of that.

We're also observing attacks starting to target the AI tools that builders are depending on as Gen AI coding becomes more prevalent. Prompt injection isn't really new at this point, but we're definitely seeing an evolution and iteration of its use, like prompts buried inside content that the attacker knows that the developer's tool chain is going to interpret. And attackers understand that AI agents are acting more autonomously, and they're planning for that in their threats. And so this is changing what it looks like to be a security team today.

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/180.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=180)

Our traditional approaches aren't enough, but it's definitely not all doom and gloom.  In fact, I'm pretty optimistic because these same opportunities and advancements are there for us as defenders too, particularly around increasing the speed at which we can assemble and process the information we need as a security team, as defenders and responders. So we're often in the game of having to look across a vast amount of information and identifying what my colleague CJ likes to call the needle in the needle stack, and Gen AI, it turns out, is super good at helping us solve that problem very quickly.

And as I'm sure your teams have been, my response teams have been super eager to experiment with this stuff, and internally we've brought our deeper log dive processes down from around four hours to around eleven minutes on average so far, and we're continuing to see gains in that space. So this has really been an advantage for us. For example, when internally, when a credential compromise is detected, we can automatically retrieve the CloudTrail data. We can use AI to discover all affected entities and any possible signs of any lateral movement, and we can really prepare for the human responder a package that's sort of complete with entity graphs and actionable starting recommendations, and this really enables responders to kind of rapidly assess and contain a threat versus something that's more manual.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/270.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=270)

We're exploring making investments like this across our work program and it's really important for us to do so.  We're eager to engage here because we know that we're going to need to move faster. Between the speed of development increasing and the speed of threat actor advancement, we've got to be quick here, and this is going to include both the basics that haven't changed, but might become more important as stuff starts happening at machine scale like credential management, observability, visibility, response workflows, and the things that are changing and have changed because non-deterministic systems are just different.

So in this hour, I want to share with you how we're scaling security at speed. I want to dive deeper into some of the ways to help you scale too, and then we'll make it real with a story that I found pretty compelling from Eli Lilly.

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/330.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=330)

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/370.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=370)

### Embedding Security Throughout Development: Building Primitives Like s2n-tls

Okay, so this talk is going to focus around three main things I want you to remember when you leave this room to encourage your security teams to do.  I want you to think about changing where and how your security team is carrying out our work versus the way it's traditionally practiced so that we can scale. In particular, I'm going to talk about embedding security team expertise and activity throughout your product development workflows. I'm going to talk about how I would like you to change how you adapt to the changes around us, making adjustments so that you can keep a close eye on the risk you want to prevent or manage. And then I'm going to talk about working side by side with the business you support to keep your security team work grounded in the problems your business specifically has  rather than approaching security as a one-size-fits-all generic approach to something that we need to do in business.

Let's get started. So if you aren't already, you have to embed security expertise throughout your business's development and operational workflows. I'm going to go on a bit of a rant. I don't like the phrase shift left, although that's probably what half of this room is thinking, because shift left implies that products get out the door in a linear way, right? We're on a timeline and you're going backwards on it. You're shifting earlier in this linear frame, and that is super not how products get developed anymore, and that is definitely not how products are going to be developed as generative AI changes what development looks like. This is a chaotic iterative mess. And so instead of thinking about shifting your security team left, I want you to think about embedding security expertise through all the different parts of that product development process that we need to secure.

I want to talk a little bit about what we do internally and how we've helped your teams and are continuing to do so. At AWS, we've been iterating on this theme since the beginning because of the scale we have to operate at. Our security builders on my team are builders themselves because we can't secure the size of AWS without that being true, and we create and invest in three different kinds of things. First, whenever we notice that there's tricky work happening that we can help with, we invest in primitives to just take that work away for the builder teams. Let's do it right once. Let's make it easy for you with a library. Second, we embed security guidance and tooling across the lifecycle from what we call internally secure through issue management and active defense and response and compliance and assurance to make sure that whenever a builder needs a bit of information to do something the safe way, it's there for them.

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/500.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=500)

And third, we  adapt our own builder experience as a security team to make it scalable for us to do what we need to do, and we use our own internal tooling and the external tooling that we developed to make sure that the unique perspective we gain from our vantage point helps keep our builders and you as customers safe. And as often as we can, we share these advancements externally to make sure that your job is easier.

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/530.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=530)

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/540.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=540)

So if we think about primitives,  if I go through those three things that we do, these are foundational level building blocks for developers that take tricky or  sorry, can we go back one slide? Thanks. That take tricky security-sensitive work and do it well by default. The primitives are optimized for developer ease of use. We make lots of these internally, and when we know they will work for your environment, we make them for you too. And I wanted to talk about kind of an oldie but a goodie as an example of this, as it's changed over time.

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/580.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=580)

Now we can go to the next slide. So s2n-tls is an example of this kind of primitive that we knew would be useful for you that we released and has evolved and grown over the years.  This is an open-source implementation of the TLS protocol. It's designed to be small, fast, and with simplicity as a priority. We looked at, I think it was OpenSSL at the time, noticed that that was 500,000 lines of code or more, and thought, how are we going to make sure that's secure? And so we looked at what was really necessary. We eliminated a bunch of extra features that people didn't ordinarily use, and we took that down to 6,000 lines of code.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/630.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=630)

We released it to the community in a way that we knew would be secure and that we could understand its security implementations, and that was in 2015. In 2022, we released a similar version called s2n-quic as an open source implementation that was again following the same principles of the QUIC Transport protocol. Then later on that year, we also added support for post-quantum key exchange,  and the key exchange adds support for three of the post-quantum key encapsulation mechanisms from the NIST PQC project.

We knew that proper implementation of any of these protocols needed to be carefully done, carefully built, thoroughly tested, and extensively validated to make sure that everyone could rely on these foundational building blocks in the long run. We knew that you had that problem too, and so we released it. This is just a great example that you can play with and use on your own, as many of you do, of a security primitive. So that's the first kind of work we do: to be builders ourselves and embed what we know throughout the product's development life cycle.

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/680.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=680)

### Automating Security Testing and Compliance with AI Agents

In addition to primitives like this, we embed across the whole workflow.  We first mentioned this example during re:Inforce this year, and the system has continued to improve, and so I wanted to give a little update on it. AWS runs on APIs. We need to test them thoroughly as a security team to make sure that they're going to keep everyone safe as well as any functional testing that needs to happen. Recently we're helping eliminate testing toil from builder teams via an agent-powered solution.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/710.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=710)

When a change is proposed to an API, an agent analyzes that API  and then it creates all of the required resources to properly test each endpoint. And then that's not just the simple test. It creates a bunch of tests to cover the entire landscape of that API for a wide variety of normal and edge cases, and this approach makes sure that we've got consistency in our security testing and assurance without requiring the builder to do a lot of that fiddly work every time.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/750.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=750)

I'll go through a few more examples. Ensuring the right thing is happening from a compliance and assurance perspective as smoothly and as scaled as possible, that's another thing that builders have had to do  throughout the product development cycle in the past. This used to be pretty manual. I'm going to be vocally self-critical to use an internal Amazon little phrase. It turns out we were asking builders a bunch of times in a row things about the controls or implementations of their system, and it also turns out when we looked at this that we know an awful lot about how our products are built.

We asked ourselves, could we use that knowledge to help meet our security, compliance, and assurance goals to scale and strengthen what we can give to builders? And so we've invested a lot over the years in automating a bunch of the assessment preparation, but generative AI has really helped us move away from standalone separate compliance work streams toward initial assessments that don't require builder engagement and pulls together most of the information that we're pretty sure is going to be relevant.

We still perform compliance evaluations. There are still humans involved, but we're leveraging automated tooling, including generative AI, to perform that assessment and verification that used to be human to human at every step. And this makes that flow smoother. It speeds up and accelerates the development process, and it makes sure that customers can know that they can trust and use our stuff.

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/840.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=840)

### Active Defense at Scale: Blackfoot, MadPot, Mithra, and Sonaris

After product launch, our active defense system is designed to protect customers and gather threat intelligence at scale. This is another thing that we've talked about because it's cool, but I don't know that we've discussed sort of how it works in  our software development life cycle and operation flow, so I'm going to go through it a little bit here in that context. Blackfoot is an internal system that sits at our network edge and translates external IP addresses to internal ones.

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/860.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=860)

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/870.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/880.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=880)

MadPot is a large honeypot system that we have that monitors,  that includes monitoring sensors and automated response capabilities. Mithra is another internal system that is a  kind of massive neural network graph model that's focused on helping us understand reputation analysis, and then Sonaris actively defends and  mitigates potential security threats automatically for customers by analyzing network traffic.

The fun thing about working at AWS is that you think you know what the word scale means before you take a job like this, and then you get in and a month later you're like, oh, I really didn't know what that word meant. Blackfoot translates over 312 trillion flows a day. MadPot engages over 550 million malicious activities a day.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/910.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=910)

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/940.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=940)

Mithra  takes care of over 200,000 malicious domains a day, and Sonaris blocks almost 5 billion scans every day for us. The cool thing about all of these working together is that even after launch, while we're operating, that's part of your product development. We guide not only our internal protections, but we're able to  provide additional protection to you through AWS Shield, Route 53 Resolver DNS Firewall, and other services. This powers the managed rule sets and services like WAF and Network Firewall, and it delivers findings to customers in GuardDuty and Inspector.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/990.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=990)

This flow, this embedding of security through our operations, not just helps us scale as a security team, but provides you with the information about threats in the context of your accounts to drive your strong security decisions throughout your operations too. It's a great example of working security through what it looks like to build and operate a product in the long run. A key to all of these efforts is that we can't do this well unless we understand deeply how products are being built  at AWS and we're there at the right time. If we didn't understand how products were being built and operated, none of this stuff would work. These are just a small sample of the systems and tools that we've built internally, not only helping to protect you, but to ensure that we as builders work through that build, deploy, operate lifecycle every day, and we understand what it means for our builders to be doing the same thing.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1020.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1020)

### Scaling with AI: Denied Party Screening and Agent Core

AI is helping us in this area.  It's a positive impact. It's expanding the ways that we can accomplish our tasks because it's good at doing some of the problem assembly and solution that we've previously needed humans to do. But in the grand scheme of things, this is just another example of how we're scaling effectively when a new tool falls into our hands. Gen AI did not enable us to do some new magic type of thing. It is helping us do the thing that we were already doing better, and I think that's really key as you think about how to scale to meet the demands of this new phase that we're edging into. I'll talk through a couple of the ways that we're working what AI is good at into our own processes.

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1070.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1070)

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1090.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1090)

One area where we're internally using AI to help scale is with our denied party screening team.  This team uses agents as well as a bunch of traditional machine learning and other automation to make sure that our transactions adhere to global sanction requirements without delay. Again, we're talking about pretty big scale. Every day we answer this question over 2 billion times. We have always been automating this process, but this year,  using Bedrock Agent Core, we launched AI agents to manage aspects of this workflow that previously required humans. We've got a blog post about this. You can read about it. It's pretty cool, but specifically the ability to fine-tune matching, take actions on particular matches, or enable a customer to continue, we're now able to do that better thanks to our work with Agent Core, and we're seeing wonderful results from it.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1120.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1120)

[![Thumbnail 1160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1160.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1160)

We've got 96% overall accuracy with 96% precision and  100% recall when we're comparing against our historical decisions. These are kind of helping us outperform the humans and automate decision making for over 60% of that massive transaction volume. We've really moved beyond trying to figure out what this can do and leveraging it to generate business value widely at scale. We're here to help you do the same, and to dig into that a little bit more, I want to invite Neha Rungta to the stage to talk about one of the ways that we're going to help you do that, an announcement that Matt shared yesterday that I'm particularly excited about. Please welcome Neha to the stage. 

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1200.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1200)

### AWS Security Agent: Turning Security Intentions into Mechanisms

Thank you, Amy. I am Neha and I am thrilled to be here with you to share what we've been building and using internally to scale proactive security. The AWS Security Agent transforms security by helping teams use the right security primitives at the right time in the right context. But to understand how that happens, let's dig into a construct we use a lot at Amazon,  intentions versus mechanisms. Intentions are the things you want to happen.

They are a best effort, trying to do the right thing. But intentions always require effort. They need implementation, they need constant auditing, and they can fail. Mechanisms, on the other hand, are different. They are intentions that have been codified and baked in. They work without somebody having to remember, without somebody having to enforce and track. An example is that the lines on a freeway are intentions, but the concrete barriers are mechanisms.

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1260.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1260)

AWS, as Amy has been talking about, has a lot of great mechanisms, but we still have some intentions. Take, for example, Block Public Access. Our automated reasoning team built this policy analysis engine  which you can use to block public access to a resource. When you turn on BPA for your S3 bucket, you can be sure that your bucket will not be public today or ever be public in the future. To make this possible, different services integrate with BPA: S3, DynamoDB, EFS. A lot of these services integrate with BPA capability.

Now what happens if you're a new service? You're building something new. How would you know that BPA is required? The trick here is not that all services or features need this, only those that are critical and have cross-account sharing need it. But when they need it, it is important that they have it. The security knowledge exists, it's there, but it's scattered across many knowledge bases. Teams have to sift through irrelevant bits of requirements to find out what applies to them. It's tedious, it's inconsistent, and like an intention, it fails. What we need is a mechanism to deliver the right knowledge to the right people at the right time.

[![Thumbnail 1350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1350.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1350)

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1380.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1380)

So how are we doing it? Yesterday, Matt's keynote introduced Frontier Agents, a sophisticated class of AI agents that are autonomous, scalable,  and can work long periods of time without requiring human intervention. There are three of these agents: the Kiro Autonomous Agent, the DevOps Agent, and the AWS Security Agent. The Security Agent is context aware. It's aware of your organization's security requirements. It checks the documents you write to find security issues. It checks the code you write, the code you implement,  the code that your coding assistants implement. The Security Agent does on-demand context-aware penetration testing to provide a complete proactive security solution.

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1410.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1410)

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1420.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1420)

Stepping back, what the Security Agent is doing is turning security intentions into mechanisms, from lines on the road to concrete barriers. So  we've been using this very successfully for the past few months, and it's been our way to turn security intentions into mechanisms. Let's look at an example.  We added the Block Public Access as a security requirement for the relevant teams in the Security Agent. When you create a design doc, it becomes a mechanism.

[![Thumbnail 1460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1460.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1460)

Let's look at a design doc for a fictional service. The Security Agent analyzes the requirements against all of the requirements in the document in under a minute. It uses the relevant context. Does this requirement matter? Which of the security requirements are applicable, which are not? And here you can see eleven are not applicable because in general, for a given service, you won't have everything that is  applicable to a given service. You'll have five that are compliant. Great, I don't need to do anything about it. Two that need more information because you may have underspecified something in your document, and the agent says, I don't know enough to say whether you're compliant or not. And one that is not compliant.

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1480.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1480)

So let's look closer.  This fictional service fails the Block Public Access requirement. The amazing part is the team finds out about this in the design phase before a single line of code is written. They know exactly what needs to be done. Nobody had to put in additional effort to remember to make it happen. It now happens on every design review, every time, in the right context. Let's look at another example.

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1510.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1510)

We have an internal security primitive called Daffodil.  It's internal to us because it's a formally verified IAM library that helps the other service teams integrate with IAM correctly. It's proven correct, mathematically proven correct. But the proof doesn't do the teams any good if they don't know about Daffodil. You may have the best thing to use, but if you don't know it exists, how are you going to use it? The other challenge is that IAM integration is so standard that teams will rarely document it in their design, so design review alone wouldn't catch this gap.

[![Thumbnail 1550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1550.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1550)

So the security agent also analyzes code reviews.  It uses the same security requirements we've defined, and here's an example. Within our internal code system, there's a proposed commit where a new service is launching, but the team forgot to integrate with Daffodil. So the security agent flags it and immediately provides remediation guidance. Now, the agent integrates with GitHub, so for all of you, you can have the same capabilities in your GitHub pull requests. Again, an intention to remember now has a mechanism that doesn't forget. You only need to define your requirements once, and they can be applied to your design reviews and your code reviews equally.

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1620.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1620)

Now this was about us and our internal requirements, but you will have your own internal security requirements. They will be specific to your company, your industry, your compliance, and the markets you operate in. You may have new rules on how MCP servers are built and integrated in your company. These guidelines change monthly.  They may even change on a weekly basis, but now you can encode these as security requirements and turn them into mechanisms that apply to all your design and code reviews. When your guidelines change, you can update the requirements. There is no more lag time between what you want to happen and what actually happens.

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1650.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1650)

[![Thumbnail 1660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1660.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1660)

We've loved this velocity change at AWS over the past few months, and we are very eager for you to experience it as well. So go to AWS Security Agent today  and turn your security intentions into your security mechanisms. Thank you very much, and back to Amy.  Thank you so much, Neha. I'm pretty excited about this. I'm super excited to see where it goes and in particular how we can all customize it to our particular different business needs.

### Adapting to Change: Responding to APT29 and Supply Chain Attacks

So let's assume, thanks to our collaborations and work that you're doing on your own, that you're well on your way to embedding security expertise throughout your business and engineering operations rather than trying to bolt it on at the end. You're not done. Sorry. The second thing that you're going to need your security team to do is to be adaptive to the changing realities both outside and inside of your organization, because a bunch of stuff is changing right now.

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1710.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1710)

Outside your organization, risk is constantly changing,  and you should be deeply aware of it. Our cyber threat intelligence team, ACTI, recently published the details of their work helping to disrupt a watering hole campaign from APT29, or Midnight Blizzard, at the end of the summer. We identified that the actor had compromised legitimate websites and injected JavaScript that redirected only around ten percent of visitors to these actor-controlled domains like findcloudflare.com or other domains that mimicked legitimate verification pages in order to appear legitimate. The ultimate target wasn't AWS. It was a Microsoft device code authentication flow, but we were able to look at what was happening.

Our analysis of the code revealed some evasion techniques that made them harder to detect, including using randomization to only direct that small percentage of visitors, employing Base64 encoding to hide the malicious code, setting cookies to prevent repeat redirects of the same visitor, and pivoting to new infrastructure when it was blocked. There were no compromises of AWS systems or any impact observed on our services or infrastructure, but we saw this was going on. We're committed to protecting the security of the internet, and we actively hunt for and disrupt sophisticated threat actors like this. So we worked to quickly isolate the affected instances, track and disrupt operations even when they were moving, and to share relevant information with our partners. This is crucial. This is super interesting work. It's a great story, and you can read about it on the AWS Security Blog.

But I wanted to pull it out because it's just one example of the types of increasing evolution and scale of attacks that we will all have to defend against, and we will all have to figure out how to defend against together.

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1840.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1840)

Another example was when the Amazon Inspector team contributed research to three different open source package supply chain attacks this fall. All of these attacks targeted packages in open source registries. The Shihuludworm that flashed up a little bit ago used typical developer workflows to harvest credentials,  and when it found NPM credentials of a particular kind, it used those to self-propagate. Detection is of course more challenging when the adversary is using legitimate credentials and doing normal stuff. We collaborated with security researchers and deployed a new detection rule that we paired with AI to identify suspicious package patterns in the NPM registry. The combination of that was that we were able to use indicators like circular dependency or presence or absence of a particular configuration file to get a better idea of when we thought this package was suspicious.

We identified over 150,000 packages based on that that were linked to a particular campaign, and we worked with OpenSSF to coordinate our response and disclosure and detection. This is a little bit further into the Gen AI evolution from a threat actor side. We think this is just going to continue, and so teams need to be adaptive to the risks that we're trying to prevent and properly defend against them.

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/1920.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=1920)

Outside is not the only place that's changing. Probably some of you have noticed that different stuff is happening with development workflows right now too.  As developers change what they do by using Gen AI to build more of the code, more of the tests, and we move to more of a specification mode with Kiro or something else, you need to keep your eye on the risks that you're trying to prevent so that you can be ready to adapt how you act as a security team to meet the risk. I'm actually really excited about something in this particular change of developer workflows because I think it will open up a new opportunity for security teams.

Gen AI augmented code development is just one more example of distance being inserted between the developer, the human, and the running code in production. We've seen several steps apart from these two over the course of my career anyway. When I started, we were working in pretty low-level languages, I'll just say. Then when CICD became more of a thing, and then when test-driven development became more of a thing, we were increasing the distance between the human and the machine running the code. Gen AI is going to do that like, whoa.

This is a real opportunity for security teams because today, if your tests are a little off but a human is kind of involved with their high judgment, it's probably okay. You can catch when something is going off the rails. In the future, if the tests are a little off and the human is nowhere near any of this, things could potentially go really wrong really fast, not just from a security perspective, but from an ops perspective, right? You might have availability risk.

[![Thumbnail 2030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2030.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2030)

As we move into this new world, our developers that we support are going to need the tests to be great. They're going to need a robust safety net in this new reality, and that's going to open something up for the security team that wasn't necessarily true before,  which is that we can use that same robust safety net to fix the things that we know need fixing without input, coordination, or any dependency on the developer. We can just solve it for them. The technology is really giving us an opportunity to change the contract we have with our developers as a result, and this is going to let us adapt much more quickly than we could in terms of response when we discover something new that needs to be changed.

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2070.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2070)

I'm really pumped about this, and I think there are lots more examples in the coming months and years that we'll find if the way we're approaching this problem is to focus on the risk.  Things are still changing quickly, but this focus on the outcome rather than the process to get there is another example of muscle that we've built inside AWS over time. When we realize we need to adapt, which we do because we notice things are changing and we know where we want to go, we start with sort of two-way door learning and experimentation that's human to human. We're bringing our best innovation together with the builders that we support, and then we figure out how to automate something or create an initial experiment, and then we can iterate and scale.

The key to this kind of adaptation, your key to succeed when you're doing it with your security team, is to think very carefully about what you're measuring. You're going to be good at this because you're a security person and to think very carefully about what could go wrong with that system of measurement. What are the perverse incentives? What happens if someone tries to game the numbers? Be really attentive to what you're looking at as a security team, and then you'll be able to adapt as things change.

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2140.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2140)

### Measuring What Matters: Agent Architecture and Security Metrics

I'll talk briefly about this before we  move on to the last thing that your security organization needs to do. We began experimenting with agents in trying to create more general purpose helpers. Our mental model at the beginning was these are non-deterministic, they can do stuff that's generative, so let's think about them as humans. Because we were measuring the security quality of what they were trying to do along with the speed and cost of what it takes to achieve that, we were able to notice that this was not actually going to be a successful approach for us writ large. We realized that a collection of more specific agents outperforms something more general, both in terms of what it can find, but crucially in terms of how much iteration and human engagement was needed along the process.

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2190.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2190)

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2220.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2220)

Success comes when agents are doing one thing well and then you  can assemble them into something larger and magic like Neha talked about. So we're building that into our security approach because we weren't just looking at one thing, we were thinking about the complete picture of what we needed to measure. If an agent does one thing well, it is also, it turns out, easier to reason about in terms of securing the agent itself. If I'm creating an agent that's sort of wild and needs access to everything, well, it needs access to everything.  If I'm creating an agent that does one thing really specifically, really well, I know what permissions it needs. And so not only performance, but security are complementary here, we discovered in our approach, since this kind of agent lends itself to a least privileged permissions model.

If you've got a virtual teammate that's handling everything for your team, you're going to need to give it a bunch of different access. If you have one that's doing on-call, you can give it one kind of permissions. If you have it doing reliability, maybe you've got just read-only sensor permissions. And this allows you to use agents to take autonomous actions with lower risk because the credentials that they're granted are scoped to what they need to do and to know more. Striking this balance of scope and independent action really allows you to put your controls in place to meet your specific risk appetite and needs.

If this agent was trying to do everything, you lose that ability. If you really think carefully about what you're trying to achieve and measure that, you're going to be able to set up a system that scales for yourself. Because we've set up our teams to stay focused on risk, we're able to understand this, but I wanted to give a little bit of advice and perspective to those of you bringing this talk back to your organizations. Agents that serve your purpose are easy to reason about if you've got tight and holistic measurement that keeps you structured and focused on the right things.

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2320.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2320)

I think it can be very common for security teams to measure what it's easy to measure. How many findings have we generated? How many findings have we fixed?  That's not really what matters. What matters is how long it takes you to fix the findings that you've gotten, not just the volumes, but what does that curve look like. How many of them can we get in under three minutes? How many of them can we get? What's our P50 time? How long is the longest tail open? What do we do about that? How many builder actions are involved in taking this stuff and fixing it?

By focusing on that, even though it's harder to measure, you're going to get better scaled results from security over time. If you think about findings and resolving, campaign away, issue all the tickets you want, you're going to get great finding numbers, you're going to get great resolution numbers, and you're going to completely miss the human timelines that are involved there as well as the toil on your builders. If you're measuring everything together like I just talked about, you're going to be able to come up with better ways to automate and scale your practice. And so the takeaway for the second part of the talk is that you need to measure the right things in order to stay agile and adapt, even at scale and even through fast change.

### Partnering with the Business: From TFI to CVE Management and Agent Core

The final thing that I want to talk about, right, you're embedding security throughout the workflows you support, you're acting as builders, you're keeping your eye on the right things.

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2420.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2420)

There's one more bit that you need to take back to your security team to scale it effectively, and that is to work as partners with your business throughout everything that they do. And like before,  I'll talk through a couple of examples including how Gen AI is changing this, and then I'm excited to hear from Andrea.

The thing that you get out of working lockstep with your business is that you're going to solve their actual problems instead of just doing security in some general sense. At AWS our problem is that we operate at a massive scale, and prioritizing our focus and breaking our work down into manageable chunks is huge. It is what we need to do. It is the core thing that I need my security team to be conscious of as they try to solve for this risk. I'll talk through two quick examples of how we've scaled effectively because we understand our business's problems.

Across AWS, work is tracked through tickets, and in a support context, we use a system called Target for Investigation or TFI to identify, classify, and escalate tickets that may require my team to engage. These tags get put on tickets into a follow-up queue so that we can proactively help builders when we might be needed. This runs on all cases for all AWS services, and to the point about scale being our problem, that means AWS support, who annually resolves around 1.5 million cases across 285 plus services across enterprise, business, and developer customers, needs us to be there at that scale and pace.

The way TFI automatically reviews case correspondence in real time started back in 2022 with keywords and regular expressions. It evolved to use machine learning and it's evolving again to use Gen AI. By leveraging our partnership with that business and our deep understanding of their problems, we've been able to scale TFI to raise the security bar for AWS and our customers despite that massive scale and continued growth.

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2550.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2550)

A little closer to home on the security team, our scale combined with the rapid increase in CVE publications is a real challenge for us and for our business teams. They're projected to increase about 22% year over year this year from a little over 40,000 in 2024  to a little under 50,000 now in 2025. If you think about this from the AWS team's perspective, a single CVE might impact tens of millions of assets, and so it's pretty important that we can understand and act on this stuff quickly.

We've been automating this stuff for some time now and we've automated the first assessment pass to handle a newly published and updated vulnerability within five minutes of publication. This is pretty good, but we're also iteratively improving the speed at which we can conduct deeper vulnerability assessments for the cases where a security engineer is really required to understand the risk. There we're using Gen AI, and thanks to our work, security engineers can now complete comprehensive assessments in under 10 minutes compared to a previous P90 time of around 27 hours. We've used that to increase the number of things we can do deep analysis on. We're able to leverage our security engineers to deeply understand more stuff, and in fact, it's helped us extend that deep analysis coverage by 520%, which scales our ability to confidently respond to changes.

Finally, we're able to use our knowledge of the business and compensating controls to ensure that we're addressing the right risks to our infrastructure and customers in the right way. We're training an AI-based assessment engine that continuously monitors all of the CVEs and maintains a contextual posture for us, taking factors like network reachability or instance details or identity or data classification into account in order to characterize that risk in a nuanced way and do what our business needs. This is going to help us further reduce our false positives and properly characterize risk across millions of assets. This knowledge of our business is what's actively helping us to respond and remediate security issues at our scale, at the speed that we need without making our builders very angry all the time.

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2680.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2680)

Another thing that the AWS business uniquely needs is the ability to understand the security of an API.  We're basically an API vendor. The way we work through these problems with our business is that we start with the largest opportunity we know how to tackle. We do it coarse-grained, and then we refine from there. This is a ratchet, an iterative cycle that we're using to ever improve the effectiveness of the security that we deliver, while allowing us to avoid the paralysis at the start that I think sometimes trips security teams up.

To understand the security picture for an API, we started by understanding the SDK really well and created a whole bunch of tests based on that. That's awesome, but it's static. So we look at how the API is used. What does the interaction look like? What's it trying to do? We use that to understand the security and refine what we're doing. And increasingly now we're able to automate helpers that can give us a much richer picture of the overall system's context and how they connect to each other.

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2760.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2760)

We're going to do something else next time because we're there with the business, because we understand what they're trying to do, we know where to start, and then we know how to use all of the stuff that I've discussed so far to be ever more effective. What advice do I have for you to help do this in your environment? Start by really understanding what your business is trying to achieve. It will be different than what we're trying to achieve.  That's awesome. That's great.

What's your particular problem and constraints? Look at the overall system you're living in, identify some big opportunities, solve them at a core grain. It's better than where you are today. And then once you deeply understand those two points, you're going to be able to innovate. Ask questions end to end and notice when things around you are changing.

So many of you in this room are responsible for companies that build and deploy software. There are three kinds of cost that happen in that process. You've got a cost to build, you've got a cost to secure, and you've got a cost to deploy it. Gen AI is changing the first and last of those. Stay tight with your business so you understand how you can reduce the cost of the middle too.

[![Thumbnail 2810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2810.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2810)

Noticing these changes in our own business got us asking what  needed to be different as we moved into an agentic world, and I've talked a little bit already about some of the ways we're using AI to help scale our security business. But there's also the securing of agents itself, and so I wanted to take just a minute to talk through how we approached this problem because I think it will be applicable to your environment too.

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2850.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2850)

We started by noticing that we're going to need to securely build, we're going to need to securely configure, we're going to need to securely execute and scale agents and ensure isolation between them. We're going to have to be able to remember past interactions and learn from them for these systems to deliver value in the long term. We need to know what the agent is, what it needs to access,  does it need this table in a fine-grained way, because these are neither exactly like a human nor exactly like a service.

[![Thumbnail 2880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2880.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2880)

And then we need our agents to be able to carry out complicated workflows and discover and connect with custom and common tools. And then finally, we need telemetry, because if you don't know what's going on, you can't secure it. To address these needs, we built Agent Core modular services that you can mix and match with everything you need for getting agents safely into production.  And if you kind of think about the problem and break it out in that way, you'll see in your business how to unlock the value of this stuff in the long term.

[![Thumbnail 2900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2900.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2900)

Whether you're talking about agentic systems, fast API analysis, ticketing, something else, working hand in hand with the business that you support is crucial  for any security team that needs to scale. I was super inspired by a story from Andrea at Lilly about how they did just that around supply chain risk, and I'd love to welcome her to the stage to tell us more. Please give her a big round of applause.

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2920.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2920)

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2940.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2940)

### Eli Lilly's Approach: Threat Modeling the Supply Chain

Thank you. Hello. At Lilly,  we are always working to bring life-changing medicine to our patients faster. And for me and my team, that means doing it securely and navigating a complex supply chain in the process. So, of course, we're also working to navigate AI,  the threats and the immense opportunities that go with it.

[![Thumbnail 2950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/2950.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=2950)

I don't know about you. I don't want to go back to a world without AI. I love it, but when it comes to  defending an enterprise, I'm definitely old school. I live by the principles of the cyber kill chain, which at the most basic level are what Amy just said. You can't defend what you can't see.

So we at Lilly have spent a lot of time making sure we have visibility across our ecosystem because, you know, face it, we have to be able to stop and or detect a cyber event before it impacts the business. And if we're embedding AI in our workflows, that's even more important, but the place where we're still struggling to do that is the supply chain. In fact, I can't think of another place where we determine the security or the security posture of something by answering a questionnaire.

[![Thumbnail 3020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3020.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3020)

[![Thumbnail 3050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3050.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3050)

A friend and I were just talking about this, and we were saying we're actually even approaching the problem wrong. We're using AI and bots to answer the questionnaires faster rather than uplift security.  And so the attackers are proving us wrong time and time again, because the supply chain compromises are killing us. So we've got to flip the equation. I'm not sure we flipped the equation, but we definitely went old school at Eli Lilly and said anytime that we are connecting a third party or vendor software to something we care about, we're going to threat model it. And so that's what we started doing, and I'm going to share a little bit of a story with you, a slightly exaggerated story, but I think it may resonate.  And hopefully maybe entertain you as well.

[![Thumbnail 3060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3060.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3060)

So if you remember, I said that at Eli Lilly,  what we're always trying to do is increase access to our life changing medicines, and it's a noble goal. If you think about it, the business is highly motivated to go out and find novel technologies that are going to accelerate reach and scale. So if you think about it, somebody goes and finds this new technology that is going to bring life changing medicines out faster. And so what that sometimes looks like to me is a kid who says to their parents, if you just get me this one toy for Christmas, I will never ask for anything ever again.

[![Thumbnail 3110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3110.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3110)

And so security, as we're starting to threat model, often says things like the adults or the reasonable person who says, well, kid, you're going to shoot your eye out.  And the threat model comes through and says, well, there are clear text passwords, there are shared services, there is lateral movement potential, excessive admin, and so on. And when we actually start explaining to the business, you can actually see the business start to be deflated. But a good threat model does more than that, right? A good threat model also says the mitigation options and the priorities that come with it. And if we take what we were hearing from Neha earlier, just think about what could happen if we had an agent that could threat model. So hopefully coming soon, maybe.

[![Thumbnail 3160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3160.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3160)

But if we start to think about how threat modeling can then start to be a solution,  we bring the business along and say, okay, we can't take the solution as is. And if it's an internal problem, we'd be already working forward. But that's where the supply chain is really tricky, right? It isn't just the two of us, it's not just the business and security. We've got the vendor, and that's where this year we've been lucky in multiple instances to have the supplier platform actually be on AWS. And so I've actually been able to call our account managers. So shout out to Matt Sater and Dave Reed, who keep picking up the phone when I call, and we've become a three-legged stool who look at the threat models.

[![Thumbnail 3210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3210.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3210)

[![Thumbnail 3230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3230.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3230)

And we actually map out a plan, starting, of course, with the most critical, so we can start to move forward to a mitigation plan and actually start moving forward  to meet what the business needs to do so that we can get those technologies in place. And I think that the challenge that all of us have in the industry is that we know we're not doing the right things when it comes to supply chain risk. I think everyone acknowledges that. I think the great news is with AI we actually have an opportunity in front of us,  and I think with agents and a new approach and the right partners, we can actually start to change that because no one in security wants to prevent joy. That's actually not our goal, but I think what we want to do is that when you fall in love with technology or the latest toy, we want to make sure that you're able to get what you want out of it without the unintended consequences.

[![Thumbnail 3280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3280.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3280)

So in the age of AI, I think we actually can do that. We can take a threat driven approach, we can be sophisticated, we can use technology, and together we can fly. So I want to thank AWS for being great partners, and I really want to thank Amy and Neha for sharing the stage. I've loved working with them.  Thank you so much, Andrea. We love working together with Eli Lilly and working hand in hand to meet these challenges, but the thing that I love most about the work Andrea was just describing here is that it creates a positive flywheel that emerges when you work backward from the business outcomes you're trying to deliver and reduce developer friction along the way.

### Key Takeaways: Embed, Adapt, and Partner for Security at Scale

Security is part of what any business delivers to its customers, and a business framing of your security goals really helps clarify how you can work across your business to deliver. Our active defense team way back when actually started this way and found really eager investment partners from our S3 organization when they were trying to reduce the chance of unintended data discovery by all the mean folks on the Internet. This led to data science, distributed systems engineering, threat intelligence, and software developers all working together cross-disciplinary to leverage AWS's unique scale and solve the business's security need to protect customers. That effort grew to include some of the super fun stuff I talked about earlier in the talk.

[![Thumbnail 3370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3370.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3370)

Scaling your security team through changes like the ones that we're in is not easy, and I know it can be unsettling, but I've tried to lay out three things that will make it much simpler than you might think in order to get started. It requires you to embed your  expertise throughout your business processes and workflows, to keep your eye on the risk and adapt when you need to, and to do it all with a strong culture in your security team of working with your business. I'll leave you with a few quick practical ways to accomplish this in your own world, and then I hope you'll enjoy the rest of your show.

[![Thumbnail 3400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3400.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3400)

Embedding your expertise throughout your business processes and workflows is much easier when you are a builder too, and this doesn't mean you need to go it alone. We are here to help you.  It doesn't mean you need to change everything all at once, of course, but the constructive mindset of figuring out what you can build to solve problems and not being afraid of creating the tools or extensions you need is key to making this work.

[![Thumbnail 3430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3430.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3430)

It's much easier to keep your eye on the risk and adapt as things change when you're actually measuring the right things: security quality, how quickly are you fixing stuff rather than what you are finding,  in tension with what it's actually taking to achieve that goal, and your error rates, particularly false positives. They burn trust.

[![Thumbnail 3450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9fae8c4ccd882e2b/3450.jpg)](https://www.youtube.com/watch?v=q3ZRbCTnB3U&t=3450)

Finally, your business partnerships are going to improve when you understand and work backward from business outcomes, especially framing any security needs that you have  in terms of what your customers expect from you. Understanding from the business where you're putting in friction, whether that's the right place to put in friction. We're not ever going to have none. We don't want people releasing unsafe stuff, but doing it smartly with your business bought in is going to help you scale.

And yes, AI can help. It's making it more challenging in some ways, but I'm super bullish. I'm very optimistic about us as defenders being able to do some pretty awesome stuff with this. It's going to open up new opportunities like the ability to deploy fixes like I talked about earlier. On the whole, I'm going to be optimistic and say that I think it can be helpful as we undertake this journey, but that it's just another tool as we undertake the same journey that we have been on all along.

Thank you so much for joining me today. Please do complete the survey linked in your mobile app. We run on data and continuous improvement at AWS, and we would love to hear how we can do better. I hope you have found this useful and that you have a great rest of your show. Take care.


----

; This article is entirely auto-generated using Amazon Bedrock.
