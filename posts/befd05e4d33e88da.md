---
title: 'AWS re:Invent 2025 - See The Future of Cloud Defense: Agentic Cloud Security (AIM288)'
published: true
description: 'In this video, a Sysdig representative explains how agentic AI transforms cloud security workflows by automating vulnerability management. The presentation demonstrates how AI agents reduce thousands of vulnerabilities to a prioritized list by analyzing criticality, memory usage, production environment status, internet exposure, and exploitability. The demo shows Sysdig''s implementation filtering 4,704 vulnerabilities down to actionable items, automatically generating fix recipes with specific commands for developers, and creating Jira tickets with detailed remediation guidance—eliminating manual investigation and prioritization work.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/0.jpg'
series: ''
canonical_url: null
id: 3086224
date: '2025-12-05T11:13:20Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - See The Future of Cloud Defense: Agentic Cloud Security (AIM288)**

> In this video, a Sysdig representative explains how agentic AI transforms cloud security workflows by automating vulnerability management. The presentation demonstrates how AI agents reduce thousands of vulnerabilities to a prioritized list by analyzing criticality, memory usage, production environment status, internet exposure, and exploitability. The demo shows Sysdig's implementation filtering 4,704 vulnerabilities down to actionable items, automatically generating fix recipes with specific commands for developers, and creating Jira tickets with detailed remediation guidance—eliminating manual investigation and prioritization work.

{% youtube https://www.youtube.com/watch?v=PfsXtrUuKn0 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/0.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=0)

### Understanding Agentic AI: From Tool to Teammate in Cloud Security

 Thanks everyone for sitting down. I appreciate that you're here, whether you're taking a break or you really want to know about agentic AI and cloud security. Either way is fine with me. Let me dive right in. I may engage with you along the way, but let's see how that goes. I'm going to talk about what makes AI agentic.

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/30.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=30)

 You probably know this well at this point, especially having been here for a few days at re:Invent. We'll talk about how it can play a role as a new teammate and why that's going to be valuable. Then we'll discuss how this shift can change how we do things in cloud security. I'm going to show you one of the workflows that Sysdig has implemented using AI agents to help relieve some of the toil and pain that exists when securing our clouds and workloads. I'll show you a little bit of that, and then at the end, I'll invite you to come talk to us at our booth.

So what makes AI agentic? I think most of you are clear on this at this point, but let's go over it. We want to move from just being able to ask a question and getting some information back to actually giving AI a job to do for us. That way it can act with autonomy around the clock. I don't need to be there saying how should I do this or what should I do next. It's going to do things for you independently. It's going to learn, adapt, and use things at its disposal to do whatever job you give it.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/80.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=80)

 We're going to talk about the job it can do for you in the world of cloud security. It thinks and collaborates. I like the word collaborate because if you were here for the last presentation, there's a lot of fear that AI is going to take my job. What we really want to do, and I've experienced this in my own role, is use AI to help make what I do more effective and maybe even get some time back in my day. We want to use AI to do things that are relatively complex, things we can allow it to do that I don't have to do any longer. It becomes not just a tool, but like a teammate.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/140.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=140)

 By that I mean, regardless of the use case, think about having something that can do analysis that I'm normally doing, but it can present findings to me in a concise and clear way so I get exactly what's needed. Or maybe it's a recommendation: this is the thing you should do next, and here's how to do it. That's awesome. And then in the future, and I don't think we're there yet, but you may agree or disagree with me, you may actually say this is your job, and when necessary, do it. In the world of cloud security, we're still a little nervous about having AI making changes or tweaking something for fear that something catastrophic will happen. I really believe we're going to get there to where we'll allow AI to do things for us. Maybe it's that I can quickly look and say yes, go do it, but we will get there. What we want is something that helps us, that grows with us, and that helps every step of the way.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/200.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=200)

 Now, when we think about this in the world of security, I want to know if anyone here is a security professional or person. I see a couple. Anyone in dev that has to fix things like vulnerabilities and posture? Exactly. You know what I'm talking about. There's this whole world of data. The bigger we get in our clouds and with our workloads, the more signals come our way. It can become a bit painful because I need to analyze that stuff. Hopefully the tool is doing it for me, but often I just get a dashboard. Then what am I going to do to filter through that? What am I going to do to prioritize the list of things that could impact my business? What am I going to do to notify the right people? How can I get the right recommendations without having to swivel in my chair and ask you what's the best way to fix this thing?

Eventually, getting to a world of action, I on the right-hand side have a much more concise set of insights and information that I can deal with and be more effective at what I do when trying to secure workloads, infrastructure, and all the things in my cloud.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/270.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=270)

 We're seeing it like this, and what typically is the case today is that an alert will happen. When that alert happens, I'm going to do some things. What is the problem? Let me go investigate. Is this real or not real? Who's involved? Maybe I need to go talk to John because this is something that he did. What is the impact, whether it's something that has actually happened or something that could be the impact of the issue? Then I get around to figuring out what I'm going to do to fix it.

In the world of AI, using agents to help us get more insights immediately when something happens, I still get alerted that this thing happened or is happening. Maybe it's a runtime event, or maybe there are vulnerabilities. But what I get immediately is the impact or potential impact, along with a set of suggestions and recommendations so that you can make a quick decision. And then, in the world of gaining trust, eventually you can say go auto-fix that for me. I'm passing all of these steps that take minutes, hours, sometimes days, and I'm getting all that up front. And then if I need to go and verify, I have all the evidence so I can say, okay, why do you think this is important? What are the signals that led to this problem? So we want to shift the workflow from this long process before I start fixing to understanding fixing, and then if I need to, I get more insights and information.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/370.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=370)

### The Vulnerability Management Challenge and Sysdig's AI-Powered Approach

One of the use cases that we're using this for at Sysdig is around vulnerability management. Now, one of the big complaints we hear, especially in the world of containers—anyone doing containers in here? Yeah, most of you. I'm using these things, I scan them, but there are so many findings that I can't stay ahead of it. I'm not sure exactly how to prioritize, or I do know how, but I'm bouncing between a CMDB or a spreadsheet. I've got to gather my own context. Well, what if we could leverage AI to make that better? 

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/400.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=400)

I'm from Sysdig. Anyone heard of Sysdig before? This is the risky question, right? Yeah, okay. We are a cloud security platform. As a cloud security platform, we bring a number of different disciplines and use cases from vulnerability management and posture all the way through what we're well known for as real-time threat detection. This is where we have a lot of good insights, good data, really deep telemetry, and excellent context. All of this stuff can be grabbed and leveraged by AI when trying to solve a problem. 

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/440.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=440)

Let me paint the picture of the problem that we're solving. I heard a lot of head nodding about vulnerabilities, right? Thousands and thousands of them. And then what do I need to do? I need to answer a bunch of questions. Is it really something I need to worry about? Is it running in production, or is this just something we're playing with in the back? Is it a business-critical app? Is this particular vulnerable thing even being used? Is it in memory? Is it loaded? Is it exposed? Is there a fix? Who owns the fix? 

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/480.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=480)

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/500.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=500)

These are all things that someone in your business is figuring out and spending hours and hours on this weekly, daily, monthly, yearly. Then I need to prioritize that list and open tickets for the right people. Then I need to determine how to fix it—not me, but the person I just shipped it off to. They get a Jira ticket and they're like, okay, now what? A bunch of CVEs, which is a bit of a challenge. And of course, I want to be able to report on progress.  This is something that AI can be very good at helping us solve by pulling together a lot of these pieces for us. 

Here's how we're doing it at Sysdig. One is looking into your environment. In this case, I'm thinking about a Kubernetes, EKS, ECS, maybe some Fargate, and so on. I'm looking and identifying, is this a production cluster? Does it look like a business application? Is there data? Is that a task, or is it just some file server that no one really cares about? Getting that insight, then understanding from a runtime perspective, is the vulnerable thing actually in use? Because that's important. I want to fix those things first. Then, is it exposed externally? That's not that hard to do. But think about pulling that insight in and then asking, is there a known exploit? Can I actually even fix it, or do I need to put a mitigating control in? How am I going to fix this?

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/550.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=550)

So on this first one, we're assisting innovation where we're actually looking into your cloud environment and giving you insights by looking for business signals. Why? To put them in buckets to say this application is running everything that all my users are doing. Let's say I'm in the gaming industry. This application is running everything that all my users are betting on, doing all the things. We're in Vegas, that makes sense, right? So identifying that for me versus something that's less important, reasoning about it, and then giving that information back to me. Then we can get from this really big number to this really small number where then, yes, we want a human to be involved and we want a human to take action for us. 

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/580.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=580)

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/590.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=590)

### Live Demo: How Agentic AI Transforms Vulnerability Prioritization and Remediation

Let me show you the implementation that we have at Sysdig, then we'll wrap up. We'll be pretty much done. We're halfway through. I'm going to switch to my demo.  

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/600.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=600)

I'm on my home screen here. Sysdig does a lot of things around vulnerabilities and around runtime events. So runtime is a very important aspect of your cloud security. Things around compliance and so forth, right? 

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/610.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=610)

What I'm going to show you is how we have implemented agentic AI insights around vulnerability management today.  You see this funnel, right? This is something that happens to most of us. We end up with thousands and thousands of vulnerabilities. How can something help me narrow that down? In this case, I'm talking about AI agents for each one of these steps, going and getting the information, pulling it together for me, and then even providing a strategy on how I'm going to fix said vulnerabilities.

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/660.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=660)

Let me go through each one of these things. First, we've already scanned and figured out that there are some critical vulnerabilities. Most of us want to fix those first and foremost. Then, is it loaded in memory? That's another signal that Sysdig has as a platform. The AI goes, okay, yeah, we've got that, we've collected that. Then, what environment is it in? It's automatically identified some of my environments for me.  I haven't had to go out and tag these things. So let's say I'm just worried about production. Now that number went down from 4,704 to just over 1,000, so I still have a lot in production, and that's concerning.

Which ones are actually exposed to the internet that somebody can get to and start to exploit? I'm now down to a much smaller number. Then, are they exploitable? From our CVE databases, is there a known way that the bad guys are actually getting in and messing around with this to try and get into my AWS Cloud or my Kubernetes clusters? And then there may be some that are exceptions. Today, someone has to go in and say, yeah, this is critical, but it's a Windows thing and we use no Windows packages in anything that we do. What if AI could identify that characteristic for me and say that's an exception? If I need to dig into the details and get an explanation of why it's an exception, I can easily get that as well.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/730.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=730)

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/750.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=750)

That's step one, what we call removing the noise. Step two is I'm plotting these things  against a graph that says how important we deem this to be and how pervasive it is. How pervasive is it in my environment? Meaning, is it deployed across a lot of the things I have?  You'll see one outlier here. It's not so high up, but it certainly is deployed in a lot of places. Now I may think, okay, you're telling me that this is probably one to fix. Explain that to me. So already there are some things I can do. One is I can just ask, explain to me why you think this is important.

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/770.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=770)

Now this is part of the generative piece, which is going to give me some insights and information as to why this is deemed one of the more important images.  This is a container image I'm talking about here. I get some information and various things. I can actually do things like highlight just critical ones for me. But I also get something that says fix here. Now remember what I said earlier? We're not quite ready to have AI just go out and start making updates for me. But I can get what I'll call a recipe. I can get a recipe for what to do.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/820.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=820)

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/830.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=830)

Now, instead of me sending over to you a Jira ticket that says, here's your list of CVEs, please fix them, I can say, here's the list of CVEs, here's where they're running. By the way, these are in use, and let me go ahead and click this button. Here's what we recommend that you do.  Now this is a set of recommendations, both from an app level in this case, a container image, as well as a base layer to say, do this.  It's smart enough to understand your environment, number one. AI agents have understood your environment. Number two, it's smart enough not to say, okay, yeah, bump up from version 3 to 4 of Redis or whatever. We don't want to do that because that might introduce some things that break.

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/860.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=860)

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/870.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/880.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=880)

So the benefit of having this is that now I get everything that I need enumerated, listed, even some commands that can be used by the dev who gets this, and I can easily say, okay, let's create a Jira ticket and I'm off and flying.  What's cool about this is that payload or that recipe, let's call it a task,  is that the payload of insights and information gets dropped in the ticket.  Now that is nicely formatted for the person who receives it, and they can go about fixing it. So now instead of having this really long process of filtering, prioritizing, and figuring out how to fix it, AI has brought that to my doorstep. That's what I love about this. We're going to end up helping save a lot of time, a lot of effort, and a lot of trying to ask a friend or search the internet for things.

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/920.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=920)

I can create a ticket, and if I already have one open, the system knows I already have one open, which is beautiful. That's one of the ways we've implemented Agentic AI. For most of us, I'm going to do this the brute force way. 

We also want to track how we're doing. Typically, you want this to go down and to the right. My AI can help us keep track of that too. I can show this to my boss or to whomever and have a nice track record. Or if it's going the wrong way, I'm going to put more time and effort into this.

That's a little bit about how something like a well-guided AI, a set of AI agents—Agentic AI, if you will—can help you get out from under some of this really hard work. We see this being applicable across all of the domains of cloud security, posture management, and threat detection. We're finding ways to help AI do some of the jobs that today I'm having to do, and it's not really that important of work other than I'm spending time filtering through various pieces of information. Let AI do that. I can get down to the fixing.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/befd05e4d33e88da/990.jpg)](https://www.youtube.com/watch?v=PfsXtrUuKn0&t=990)

I'm going to close it here. Nothing too heavy. Hopefully that's okay with you. It's helping to transform things. We're getting help with defending our clouds and making faster  decisions for response, which is key to staying ahead of threats in the cloud.

Now I'm empowering every one of you to have the information in an easier way than is currently the state of the art for most security tools out there. You can come see this and learn more over at our booth and ask us some questions there. With that, I'm going to wrap up and just say thank you for being here. Hopefully you get some insights into this and start looking for these kinds of things in your shop when you need to actually deliver security tooling and security approaches for your business. That's all I have. Thank you everyone for being here. I appreciate it.


----

; This article is entirely auto-generated using Amazon Bedrock.
