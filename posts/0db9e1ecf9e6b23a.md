---
title: 'AWS re:Invent 2025 - AIOps Revolution: How iHeart slashed incident response time by 60% with Bedrock'
published: true
description: 'In this video, AWS Solutions Architect Sudipta Ghosh and iHeartMedia''s Harish Nagaraj and Serkan Haytac present their AI-driven incident response transformation. iHeartMedia, serving 250 million monthly users across 850+ radio stations and handling 5-7 billion requests monthly, built a multi-agent system using AWS Bedrock Agent Core and LangGraph to automate incident triage and remediation. The solution reduced response time from manual investigation across multiple dashboards to 30-60 seconds through specialized agents for Kubernetes, logs, and monitoring. Key implementation includes the "agent as tools" pattern to manage context windows efficiently, Bedrock memory for learning from past incidents, and knowledge bases for runbooks. The demo showed automatic root cause analysis and remediation approval via Slack integration with PagerDuty, eliminating the "seven circles of on-call hell" and freeing engineers from 3 AM troubleshooting marathons.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/0.jpg'
series: ''
canonical_url: null
id: 3088760
date: '2025-12-06T11:28:47Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - AIOps Revolution: How iHeart slashed incident response time by 60% with Bedrock**

> In this video, AWS Solutions Architect Sudipta Ghosh and iHeartMedia's Harish Nagaraj and Serkan Haytac present their AI-driven incident response transformation. iHeartMedia, serving 250 million monthly users across 850+ radio stations and handling 5-7 billion requests monthly, built a multi-agent system using AWS Bedrock Agent Core and LangGraph to automate incident triage and remediation. The solution reduced response time from manual investigation across multiple dashboards to 30-60 seconds through specialized agents for Kubernetes, logs, and monitoring. Key implementation includes the "agent as tools" pattern to manage context windows efficiently, Bedrock memory for learning from past incidents, and knowledge bases for runbooks. The demo showed automatic root cause analysis and remediation approval via Slack integration with PagerDuty, eliminating the "seven circles of on-call hell" and freeing engineers from 3 AM troubleshooting marathons.

{% youtube https://www.youtube.com/watch?v=qvhmFAvG_QI %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/0.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=0)

### Introduction: iHeartMedia's AI-Driven Operations Transformation

 Good afternoon and welcome to the session. Today, I'm really excited to share an incredible transformation story that's reshaping how modern enterprises handle IT incidents. We will be exploring iHeartMedia's agentic journey, which demonstrates the transformative power of AI-driven operations.

Hello, myself Sudipta Ghosh. I'm a Senior Solutions Architect at AWS, and today with me I have our esteemed guests from iHeartMedia. Before we dive deep, I would like them to introduce themselves.

Hello everybody. My name is Harish Nagaraj. I'm a VP of Cloud Engineering for iHeartMedia. Excited to be here presenting our story.

Serkan Haytac, Principal Engineer at iHeartMedia. Same, love to see you. I love seeing people here, and I'm looking forward to the presentation. Thank you.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/80.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=80)

As we move along today, we'll learn about the challenges that not only impact the media industry but broadly any large digital organization. We'll also understand the solution approaches for those challenges, and then of course we'll dive deep into the iHeartMedia story  and understand the benefits and the results that they've achieved from this agentic journey. We'll also share some practical implementation guidance that might help you. As you can imagine, it's an evolving journey, so obviously we'll be sharing some of the future things that we'll be doing with iHeartMedia, as well as sharing some critical lessons learned that might save you a few weeks of development time in your own agentic journey. We will hold the questions until the end of the session. We'll be here, so if anybody would like to continue or have any specific questions, we'll be happy to address those after our session, based on the time that permits.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/150.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=150)

### The On-Call Engineer's Nightmare: Seven Circles of Incident Response Hell

Okay, so let me dive deep into the problem that we're trying to solve here. In this particular case, just picture this: you are an on-call engineer, it's 3 a.m. in the morning, and suddenly your pager device starts buzzing, your phone starts ringing. Essentially, the panic sets in, right? Probably your better half is giving you that look, while in the meantime you are looking at something on multiple screens and trying to figure out what is exactly wrong  and why this pager suddenly rang. This is what a modern digital system looks like in a distributed environment, where finding a particular root cause is something like finding a needle in a haystack. Only the difference is the haystack is on fire and somebody keeps on adding more hay to it.

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/180.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=180)

Let's try to understand a little more context and have some additional context on that particular problem.  While it is very important to eliminate the toil for the support engineers and amplify human productivity, we need to understand what we are dealing with at iHeartMedia. iHeartMedia is not just any other media company. It has 850 plus AM and FM stations, and iHeartRadio reaches almost a quarter of a billion people. It is one of the largest podcast platforms in the world, and the uptime expectation is 24/7. Radio cannot be off at any point in time.

When anything in this kind of infrastructure breaks, we're not really talking about impacting a few people. We are really talking about potentially millions of commuters losing their morning show, or a major advertiser's campaign going dark. Interruptions in live streams can have a lasting impact for an artist, and in terms of revenue, you're potentially losing tens of thousands of dollars every minute. To deal with this kind of scale, the operations have to evolve. They have to evolve to a more intelligent system that is capable of self-diagnosis and remediation. Obviously, the repetitive manual tasks that the team is doing are eating up important engineering effort that could have been better spent in terms of innovation. The goal is essentially to amplify the productivity and not just add more people to the whole process.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/280.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=280)

Let me take you through the steps, the current steps before we had this  kind of agentic solution. What I call the seven circles of on-call hell. It starts with the first, obviously, the alarm storm. As I said, your pager device starts ringing, your phone starts buzzing. Essentially, you are just getting panicked, and then starts the login marathon. You need to log into the VPN, you need to log into the AWS console, a number of monitoring dashboards, and potentially SSH into production systems.

Every such step takes precious minutes in that particular incident situation. Once that's done, the information hunt eventually starts. You are looking through the metrics, going through the logs that are never ending, and then trying to figure out what really went wrong.

In the meantime, the tribal knowledge dependency comes into the picture. The obvious questions arise: when was the last time this service was touched, who has worked on this, and is there an operating procedure already listed where we can find a solution for that? Eventually, you get into a manual diagnosis where you are running certain commands and checking service dependencies, checking service health, and things like that. But remember, all these things are taking time and the clock is still ticking, which is very critical at that moment.

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/390.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=390)

Eventually, when the manual investigation happens, you move to the solution hunt and run certain fixes, probably rolling back to the previous stable system and eventually solving the problem. But with that also comes the documentation debt in the sense that the problem got solved, but the knowledge is remaining inside somebody's head, and most likely the next time it happens, we'll start all over again. All this has business impact, and I'm not really talking about the obvious downtime, which is extremely painful enough, but the real hidden cost. 

It causes burnout engineers, silos, and inconsistent responses. It seems like when everything is urgent, nothing is really urgent. It's what I call the alert fatigue syndrome. Now, traditional monitoring systems have the challenge that they generate a lot of noise, and processing through this noise is expensive. They don't generate the signal, and that's why we need an automated system that can help us to pinpoint those root causes in time and also give us recommendations.

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/430.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=430)

So, let's imagine a scenario in a similar incident.  Instead of panicking and going through all the steps I just described in the next couple of minutes, the on-call engineer probably just asks an agent or types in a chatbot what is wrong with my infrastructure or a particular cluster. And while they do that, instantly, the agent system goes through the logs, correlates events and systems, understands what the root cause is, identifies that, and showcases what the recommended options are, all within 30 to 60 seconds. I'm not really talking about science fiction here. That is how the modern AIops should look like for a large retail organization, and that's what we built at iHeartMedia.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/480.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=480)

### Building the Solution: Multi-Agent Architecture with Amazon Bedrock Agent Core

So, let's talk a little bit on the technology side, how we have built it.  This is a reference architecture, as you can see. It's a multi-agent system that was built, and broadly speaking, on the left-hand side, I call it the trigger layer. Essentially, that is where Slack or PagerDuty or those kinds of services are integrated that understand what the human or any system for that matter is trying to get out of that system. The central portion is basically the orchestration platform, which is the brain of this particular platform.

The purpose of this orchestration is essentially fourfold: intent recognition, which is understanding what the human or the system wants; context assembly, getting relevant contextual data; task delegation, in the sense of understanding what the specialized subagent is fit for that particular job; and from all this analysis, generating a response. So those are the important activities of this orchestration. In order to successfully work this agentic system, we also have another layer that is called the data layer, and that has all the necessary data with respect to the server logs or even the knowledge base that has the previous operations knowledge, so that the system can actually recommend based on that knowledge.

So that's an overall architecture, but let me dive a little deeper on how that particular orchestration system works or the multi-agent system works. We have not really built one single monolithic AI agent that is trying to solve everything. Instead, what we did is create specialized subagents who are specifically trained for a particular type of activity, and they're working together based on the incident and providing contextual solutions.

So the first one is the orchestrator agent or SRE agent we might be referring to in our discussion today. Its purpose is basically to get that particular incident question and then identify the specific subagents, and then finally delegate those tasks to the subagents. And eventually, when the responses from the subagents come, it synthesizes the responses and sends back the response to the user.

From a specialized sub-agent standpoint, we have multiple sub-agents. In this case, for example, there are sub-agents with respect to monitoring solutions that can go through certain monitoring information. For example, if there are performance anomalies, latency issues, things like that, it eventually returns structured performance metrics to the orchestrator agent. At the same time, there are parallel log agents and Kubernetes agents, sub-engines running. Now log agents are more optimized to run through millions of lines of code and give a fast response. The Kubernetes agent is giving you pod-level details, like how the pod health is going on, if there is an issue with respect to service mesh and things like that. And of course, we also have a knowledge-based agent in the sense that it has the previous operational history, so that based on the incident, it can provide potentially the recommended steps to bring the resolutions.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/670.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=670)

So, those are the multiple sub-agents, and we'll dive deep very shortly and we'll show you how it works. So, that's the overall orchestration flow that was built as a part of this agentic solution.  Now, obviously, as you can imagine, and as shown in the diagrams, one of the critical components of this is Bedrock Agent Core. So, we have used Bedrock Agent Core to leverage and deploy all these agents or the multi-agent systems that we have talked about. And specifically, I want to call out the Bedrock runtime, which is providing developers a secure way to run and deploy your agents at a large scale.

Now, obviously, we have also used Bedrock memory, which is a critical component and gateway to interact with other APIs. From a runtime standpoint, it is important because it allows you to run multiple types of agents, and it also has the longest-running instances that we allow, like up to eight hours, which is very much important for cases where you are actually doing a multi-agent orchestration. At the same time, runtime also offers complete session isolation, which is extremely important from a security standpoint. So, these are the key features that are being leveraged as a part of that.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/750.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=750)

I'm sure as a part of this re:Invent you have gone through probably some of the Agent Core sessions, but I just want to call out because that's played a very key role in implementing this particular solution. And then the question is how it actually helped us in terms of Agent Core. Obviously, we were exploring that,  the solution, and at that point in time, we really wanted something that would help us to build that session, build that particular solution quickly. And for that, we don't have to get into a situation to manage the infrastructure or operational overhead. So, Bedrock Agent Core provides us that runtime that is managed, and we don't have to manage the infrastructure.

At the same time, it provides the flexibility in terms of the type of agents we want to run or the type of model that you want to access, which is very critical to have the right agent development. And of course, the third, the most important is the security. So, it is built on the same security principles like any other AWS services, and that provides a seamless integration with AWS. So, those are the key differentiators that helped us to leverage Agent Core and build a quick prototype and eventually iterate on top of it.

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/810.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=810)

Another question comes to how you even build an agent. And for that, this particular case, we have used Strands Agent as a framework to build this agent.  It's not that Bedrock Agent Core supports only Strands. There are other frameworks also like QA or other standard frameworks. It's also supported. In our case, we have taken Strands AI because it is, first of all, open source and it gives developer tools to quickly and easily develop and also integrate with AWS services and have MCP support. So these are some of the capabilities that we leveraged to make a scalable solution. So that's the reason for getting Strands. But of course, in your case, probably you can leverage some other frameworks.

### iHeartMedia's Scale and Business: The Power of Audio at 250 Million Users

So I think now we have understood the problem we are trying to solve and the solution approach and the architecture, how we are trying to solve it. So probably it's a good segue for you guys to understand a little more about iHeartMedia, the scale, the business, operations that they're running, so that you get the overall context when we start running through the demonstration. So, at this point in time, I'm going to invite Harish to help us with that iHeart story.

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/880.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=880)

Thank you Sudipta for setting the stage here. From the audience, I want to know how many of you  are doing return to office on a regular basis. All right, we got some. So picture this. It's a Monday morning, you're driving to office, there's a lot of traffic and you're stuck. Basically your mind is racing, there's so many things going on you want to do, but traffic isn't moving anywhere. So what do you do at that time? You just tap on play.

When you press play, you are now listening to your favorite podcaster. Now you're immersed in their world and you're no longer on a journey. At this point, what exactly happened here? This is the power of audio. Audio is a very powerful medium where you don't need to completely dedicate yourself. You can run things in the background and it will keep you engaged. That's the power of audio here.

Why it is important is iHeartMedia excels in delivering audio to the masses over the last two decades or so. Going a little bit deeper into what iHeartMedia does, iHeartMedia is an iconic mass media entertainment company based out of San Antonio, Texas. We have subsidiaries based in Asia Pacific, Europe, and many other areas across the globe. Some of the things that iHeartMedia really excels in and is a leader in is broadcast radio. About 900 stations, radio FM AM stations across the United States. If you turn on radio, it's likely, more than likely that you're listening to an iHeart or an iHeart partner radio station. That's the level of reach that iHeart has.

Another big platform is the digital audio streaming platform where iHeart reaches masses. In the digital audio platform, the major component is podcasting. Podcasting has taken over over the last five to six years, and there is tremendous demand for on-demand podcasting. Another thing that iHeart is really popular in culture for is there are a lot of live events that iHeart is very famous for like Jingle Ball, iHeartMedia music festival. In addition to that, it's also a social platform and audio advertising. By this time you can start looking at what's the scale that we're dealing with at iHeart.

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1030.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1030)

Now one of the key factors that are shaping the business as it stands today in 2025, as I mentioned earlier,  the digital platform is growing. The demand for on-demand audio is ever growing, so that's the number one growing sector in the audio world. You're no longer going to listen to audio in one particular device. You can listen to audio from many different streams and devices right now. So that is our scale and multi-platform reach that we have.

When you're dealing with such a scale, what's the first thing that you've got to take care of? It's streamlining both the operations, both technology operations and business operations equally. That is one of our key factors that is shaping our business. If you look at the overall technology trends, there is a lot of cross-platform integration going on and also there is user-generated content. Above all, the most important topic here is the AI-driven development and operations that we can optimize to better serve the needs.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1100.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1100)

Now let's look at what is iHeartMedia technology today. Even though iHeart is an audio company, we are  a very technologically focused company, and we have to serve an ever-growing demand. Most of it is on AWS. We are a completely AWS platform native solution, and we are always looking at how we can improvise on the cutting edge technology and how we can improve the efficiency. What is it that sets us apart for the next ten years? Another important factor is we also integrate with many ecosystems such as TikTok that also brings in a lot of traffic. So this is iHeartMedia Tech at a high level.

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1140.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1140)

### Digital Platform Architecture: Managing 70+ AWS Services and 100+ Microservices

From cloud engineering and site reliability  engineering, we support three major verticals in iHeart. One is broadcast, which basically is the radio that you listen to, the station that you listen to on the radio. Now again, you might be receiving the signals, RF signals from a tower, but what is powering behind it is all in AWS. That particular broadcast vertical is completely built on serverless architecture, multi-region, and automatic failover. It's a very complicated beast in itself. So that is the broadcast vertical that we support.

The second one is ad and sales tech, which is very important. That is actually the revenue generator for iHeart. If you look at the overall radio broadcast audio market, iHeart garners about 40% of the ad spend that happens in the audio world. Last but not the least is the digital. When I say digital, it's the iHeartRadio platform. We'll dig much deeper into it.

[![Thumbnail 1230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1230.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1230)

Our specific use case here is centered around digital. If you look at it, we get more than 250 million users a month that are accessing the platform, and 90% of the US population is reached with digital. Now, as I mentioned earlier, iHeart Digital is where you can listen to radio, podcasts, your favorite playlists,  everything on iHeart.com, iOS, Android, and many other options available.

And if you look at the traffic, we get about 5 billion to 7 billion requests per month, and we also host a lot of radio stations and local sites across the country, about 3,000 plus radio stations. As I mentioned earlier, iHeart is a leader in the podcasting platform, so we get about 150 million podcast downloads a month. So we are among the top 10 podcast platforms. And if you look at the peak time, we get about 60,000 to 70,000 hits per second. That's the scale of traffic that we get on digital.

[![Thumbnail 1290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1290.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1290)

In addition to that, as I mentioned earlier, you're not receiving audio from one particular platform. You're looking at it coming from Alexa, Roku, Sonos. There are so many integrations that are already built in with iHeartRadio.  Now, let's look at the architecture. This is a 50,000 foot view of the architecture. If you look at it, we use about 70 plus AWS services across the board, and there is a primary and secondary DNS failover setup that is in place, and we have multi-level caching. We use CDN, external CDN, internal caching. It's a very complicated architecture.

We also have multiple Kubernetes clusters, one for backend, one for frontend, and many other Kubernetes clusters across the board. Along with that, we use many services in AWS including some of the foundational services like S3, RDS, VPC, and EC2. In addition to that, we also use SageMaker, EMR, AWS Transform, and many other cutting edge features as well. Now we also have a huge catalog of items, so there's a whole pipeline that takes care of ingesting the catalogs and making it audio ready to be served to the millions. So that's another aspect.

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1380.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1380)

So if you look at the architecture, it's pretty complicated. It's about 100 plus microservices every which way you look at it, and it's completely running in the AWS platform. Now, when you think of this scale of architecture, this is what you're going to look at. You don't know what happens when one thing breaks.  It's hard to figure out what is the root cause and what exactly is a symptom. That's often the case when there are outages.

It's very easy if it's a small subsystem where a user can log in and find the root cause and figure out and come up with how to remediate it, but in our case we have nested distributed systems that talk to each other, so it's really hard to pinpoint where the issue is and how do we resolve it, especially when you are under pressure, when there's an outage going on. That is where AI comes into the picture, where agentic AI comes into the picture here.

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1430.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1430)

### The Agentic AI Journey: From Cost Optimization to Intelligent Incident Triage

So let's look at how the journey started.  You know, the last few years AI has been the buzzword, and we were looking at how can we use agentic AI to support one of another important cause that we have, which is to optimize the AWS cost across our enterprise. We have close to 170 AWS accounts, and it's really hard to even generate a report of what are the costs of this particular account last month versus how do you compare where the growth is, where the costs are optimized or not.

So our initial foray into AI was how can we use agentic AI to make it easy for anybody to obtain that report. So let's say you go to Slack, you use a Slack bot, you put a command. What was the cost of this particular account in November and it shows up right there. We tried that and it really worked well. And that gave, that was more like a stepping stone for us to move forward.

Then we started thinking what's our biggest problem? Cost is definitely a concern. We're going to report and we're going to find optimized ways, but the biggest headache that we have from a site reliability engineering perspective is outages and handling the on-call, handling the SRE activities that happen on a daily basis.

So that's where we started looking at pivoting from cost to looking at SRE implementations, how we can improve ourselves and make our own lives better when you are on call and you have to log into so many portals and go through a lot of things to even find out where the issue is. We want to simplify that first. So that is what we started looking at, and in the process we ended up reducing a lot of time.

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1540.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1540)

Now, going with the AI agentic AI journey, if you don't have a goal, it's very hard. So what we went through was,  first, our goal is to have insights when there is an outage. The initial thing we want is a triage capability. We want to be able to understand which particular subsystem caused the issue, which is the root cause, and how did it percolate across other systems. So that is what we first focused on, getting the insights and getting the intelligent triaging in place. Second, once you know the issue, then the next step is to see how you can remediate it. What is the recommendation, right? So that is the next step we looked at, how we can take this forward. And third, of course this is still in evolution, is how do you go ahead and remediate it in production. There are a lot of gotchas with it. There are pros and cons with it, but that is the way we are trying to transform our journey using AI Ops, specifically agentic AI.

[![Thumbnail 1600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1600.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1600)

Now some of the benefits that we saw immediately as soon as we got this going is we reduced the response  time. The response time in particular is more of a triage response time. Nobody is looking at going into Kubernetes clusters, sifting through the logs and figuring out what the issue is. It just throws you in a Slack bot exactly what the issue is. It's very easy to get the triaging, and then it improves your operational efficiency, and we reduce the toil. At the end of the day, we don't want our on-call engineers to get completely burnt out. That is the goal, right? And then with this we also have a level of consistency and reliability. A machine always acts much better repeatedly than a human, so we are now transferring the responsibility into the machine and it gives you more reliability.

And last but not the least is how do you preserve the knowledge? Let's say there was an outage in October and now there's a similar outage happening in November. Now you know exactly what was done in October to fix it. Now you could use the same knowledge and come up with a plan for November, right? So that's how some of the benefits that we reaped out of it. At this point, I want to call upon Serkan Haytac who will go through on how we implemented this. We've been talking about what iHeartMedia does and what are the challenges, how we solved it, but he's going to dive deep into it. Thank you.

[![Thumbnail 1700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1700.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1700)

### Live Demonstration: From 10 Tabs to One Question in Seconds

Thank you, Harish. As Harish alluded, we make sure millions of people  can listen to their favorite music, podcast, and radio station all over the world, whether on web, mobile, or smart devices. People depend on us to provide this service. So when something breaks at scale, it breaks at scale, and as you all know, production incidents have impeccable timing. Assuming some of you go on regular on-call rotation, what I'm about to tell you might trigger some on-call PTSD, so bear with me.

Now imagine it's 3 a.m. in the morning, your phone goes berserk, you got out of bed with one eye open, you're in front of, you get in front of your computer. You try to VPN, it takes three tries because, you know, 3 a.m. in the morning, your muscle memory doesn't work. Finally you connect, you need to access your kubectl. You need to refresh your AWS credentials. Classic. Then you need to access the logs. You need to open up a New Relic tab. Prometheus, maybe your metrics are in Prometheus, another tab. CDN stats in another tab. You're collecting them like Pokémon, right? By the time you got all your tabs open, you have ten tabs and ten terminals, and you already forgotten about what alarm woke you up in the first place, right? And you haven't still troubleshooted yet.

So today I'm going to show you how we use AI and multi-agent frameworks to specifically identify, troubleshoot, investigate, and mitigate issues. Basically, we are teaching machines to do all that frantic tab opening for us. So next time this alert comes up, I can issue a command, and by the time I get in front of my computer, I know what's broken and how to fix it. Now I'm going to show you exactly how that looks like in a minute. Spoiler alert, it involves way fewer browser tabs and a lot less profanity.

But before I dive into it, let me give you some context about our environment. We have 15 EKS clusters between multiple AWS accounts, multiple VPCs. Some clusters are shared between teams, and some are dedicated to a single team. Across all these clusters, we've got hundreds of microservices running different tech stacks and different observability stacks. Some users are shipping their logs to New Relic but shipping metrics to Prometheus Grafana. Some send both logs and metrics to New Relic, right. And here's the kicker: some of these services are interdependent. Service A in cluster one can call Service B in cluster two. So when something breaks, good luck. You're playing detective across multiple clusters and VPCs and monitoring systems.

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1920.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1920)

All right, enough talk. Let's see some action. Remember that 3 a.m. disaster I pictured earlier,  the 10 tabs and the 5 terminals and the VPN dance? Let me show you an alternative universe where that doesn't happen. Picture the same nightmare scenario: 3 a.m. in the morning, your phone goes off, Slack lights up, some service is down, right? In the old world, the world we just lived in 5 minutes ago, you'd be trying to remember your password and trying to log into VPN, et cetera. But in this world, watch what happens.

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1960.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1960)

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1970.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1970)

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1980.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1980)

[![Thumbnail 1990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/1990.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=1990)

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2000.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2000)

An alert is triggered.  I'm literally going to ask the agent, what's the issue with this service?  The agent goes out and hits all the endpoints that we've been talking about. There's no 10 tabs, no 5 terminals,  no VPN dances, and we got the answer back. See that answer? One question, zero additional context needed. The agent  figured it out and everything else. This is what I mean by teaching machines to do all that frantic tab opening for us.  While the agent is gathering information, checking Kubernetes events and the New Relic logs, you could literally be making coffee.

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2020.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2020)

Now, let's see another example. Here we have a PagerDuty alert going off.  One of our cron jobs failed to complete successfully. Normally we have runbooks for this, you know. Some SRE uncle or some developer who picked the short straw goes into the cluster, reruns the job, and that will be that. It's a simple task, but it's annoying. You still need to authenticate to the VPN, know which command to run, switch contexts. But here's how we handle it here right now.

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2060.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2060)

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2070.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2070)

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2090.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2090)

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2100.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2100)

 The alert is triggered. The agent automatically in the background starts looking for the root cause.  We click on it. It gives us the root cause and asks us to remediate, basically asking us, hey, do you want me to rerun this job? We give a thumbs up. The job is rerun, and that will be that.  I wish I could type better, I guess, for the demo, but  the job is rerun.

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2110.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2110)

And the job is wrong. Now here's the most important part. In both cases, I gave it zero context, no service name,  no cluster name, no cluster ID, no namespaces. The agent figured out in both cases which service to investigate, in which cluster, which namespace, and hit all the right data sources automatically.

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2140.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2140)

### How It Works: Slack Bot Integration with Bedrock Memory and Knowledge Bases

So how does it do that? That's what we're going to walk through next. We've got two main components making this magic happen. We have a Slack bot which is built on top of the Slack  Bolt framework. This is basically where humans ask for help. And our multi-agent orchestration layer, which is AWS Bedrock Agents. Now let's talk about how you actually use these things. There's one way, basically direct mention. You tag the bot on a Slack thread underneath like a PagerDuty Slack thread. SRE bot, what is the issue with the service? The second one is automatic invocation for specific alert types that we know the bot can handle. It just shows up, no tagging needed. The alert fires, the bot investigates, and we just approve the remedy, just like the previous cron job failure example that I just gave you guys.

Now when an alert fires or it's summoned directly by tagging the bot, we pass some light metadata. The Slack channel ID, the PagerDuty alert content, the message content, nothing fancy, just the essentials. The flow, that information flows into what we call the SRE agent. That's the orchestrator layer that you see in the middle of the screen. The SRE agent takes one look at that metadata and consults its system prompt. This is basically its instruction manual for what to do when things break and extracts key details to start the investigation, like which cluster, which environment, which namespace, based on the Slack metadata as well as what's in the system prompt.

But here's where it gets smart. The system prompt has special instructions based on the app and the error type. When it determines what kind of problem we are dealing with, it doesn't just figure it out. It invokes the right specialized agents with the specific context that agent needs. You need Kubernetes events, it calls the Kubernetes agent for Kubernetes events and deployment data. Application logs, it calls the New Relic agent with the service ID and the time window. CDN issue, Fastly agents get involved with the service ID that is specific to the application that is having the issue. Each agent launches with exactly the context it needs, no wasted tokens asking which cluster, no back and forth, just targeted investigations.

Now we also integrated two really powerful capabilities, Bedrock memory and Bedrock knowledge bases. Let me give you a concrete example of how memory works in practice. In our demo earlier, you guys saw a cron job failed to complete successfully, right? The agent picked up, the bot investigated, figured out the remedy, asked for our permission to rerun it, and we gave it a thumbs up and it ran it. Pretty cool, right? But here's where the memory takes it to the next level. Imagine telling the bot, hey, next time you see this error, you get this exact same error, do not ask me, just rerun it automatically. The bot stores that information in the memory, and next time the alert fires, it looks into it, checks the memory, and sees it no longer needs acknowledgement and reruns the job. It's like training your own on-call buddy to not bother you at 3 a.m. in the morning for things that you know they can handle themselves.

Then there's the knowledge base. This is where we store carefully curated institutional knowledge for troubleshooting our apps that you don't want cluttering up your context window.

Runbooks for specific apps, readme files, RCAs, postmortems—agents can query this data on demand and pull only the relevant information when it's needed. Artist service acting up? Pull in the artist service runbook. Seeing an error you've seen before? Grab that RCA from three months ago where you actually fixed that issue. We are not preloading all this stuff into the context. We are retrieving it just in time, only when it's relevant.

### Solving the Context Window Problem: Agent-as-Tools Pattern for Scalable Investigation

Now you might be thinking, okay, this sounds great, but how does it actually handle complex investigation? What happens when it needs to pull events from Kubernetes and logs from New Relic and CDN stats from Fastly all for the same specific incident? Here's the thing most people don't realize when building AI agents: context windows are finite and they fill up fast. First, let me show you or demonstrate to you what the problem is, then I'll show you how we solve it.

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2490.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2490)

So just like the previous example, right, we asked the agent what's the issue with the service.  The prompt, system prompt as well as the tool, accounts for 70K, right? That 70K you see over there I carried forward to the next iteration because our agent decides, okay, I need to get Kubernetes events as well. When the agent calls that tool, that 50K token as well as the response is also added to the agent's context window. That's also coming with us.

Now finally our agent decides, okay, I need to get the logs as well, but here's what happens: we're already over our context window. That's game over. The agent stops. No more tool calls, no more analysis, like a pod getting out of memory killed mid-request. We only made it through three calls, didn't get to check from ECS, query databases, or correlate any other services.

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2570.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2570)

So how do we fix it?  Instead of one giant agent dragging around an ever-growing context blob, we use the agent as tools pattern, a pattern where a coordinator agent delegates specific tasks to specialized agents. And here's the key: each specialized agent gets its own context window. Think of it like microservices, right? Microservices for AI agents. Specifically, you wouldn't build a monolith that handles authentication, subscription, billing, all the streaming in one giant code base, right? Same principle applies here: isolated context, isolated execution context with clean interfaces.

The coordinator agent sits on top with a 200K token window. When it determines it needs Prometheus metrics, it doesn't call a tool that dumps 150K tokens of raw data into its context. Instead, it delegates the task to the Prometheus agent, spawning it as a completely separate agent with its own context. The Prometheus agent does a deep dive, queries Prometheus, crunches metrics, analyzes trends, then returns to the coordinator a summary of 500 tokens. The summary basically states service throughput dropped 80% at 2:47 UTC, correlates with increased error rates at session service.

The coordinator service receives the 500 token summary. The Prometheus agent that spent 160K tokens is gone, thrown away. We don't need to carry those tokens forward. That part of the investigation is done. Same with the Kubernetes agent, right? It finds pods crashing, identifies OOM killed containers, correlates with recent deployments, and returns a summary of 1,000 tokens.

Twelve pods were killed out of memory in the last hour. The memory limit was set to 512 megabytes, but actual usage was spiking at 890 megabytes, which started after deployment at 12:45 UTC. Again, that full 61,000 token context from the Kubernetes agent that we used is thrown away. We don't need that anymore. The coordinator only gets a 1,000 token summary.

The same idea applies with the New Relic agent. It queries the logs, identifies stack traces, maybe identifies connection pool exhaustion, and returns 22,500 tokens. Now look at the coordinator agent's context window. After all this investigation, we made three deep investigations across three different platforms: Prometheus, Kubernetes, and New Relic, and we barely made a dent on the coordinator's context window. The coordinator can now synthesize these findings or even delegate more agents if needed to further expand or basically come to the conclusion of the investigation.

[![Thumbnail 2800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2800.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2800)

### Future Roadmap and Key Lessons: Crawl, Walk, Run with Quality Context

This is how we can investigate across Kubernetes, New Relic, Prometheus, Fastly, or whatever tools you are using. The coordinator's context window stays manageable because we are not dumping every tool's raw output into it. We are getting summaries from isolated agents. So what's next?  Currently, our agents handle straightforward incidents like service restarts, pod failures, and basic recovery tasks. The next phase involves expanding its capability to execute complex multi-step remediation procedures and incorporate more tools and services.

A key advantage of the current architecture we have is that as we incorporate more tools and services, including the new Model Context Protocols from our vendors, we significantly expand the agents' capabilities from both a troubleshooting and remediation perspective. This allows us to enrich the agent's context with additional data sources such as APM metrics, distributed tracing, and security posture information. The result is a more comprehensive view of our infrastructure, enabling more informed decision making and proactive incident management.

Here's a big one, and the biggest one that I'm excited about. Our next goal is an evaluation system where AI agents create known issues in test clusters, synthetic incidents with predetermined causes and fixes. Then we unleash our agent on these manufactured problems and see if it arrives at the same diagnosis and remediation as we expect. It's like a dojo for your SRE agent, basically.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/2910.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=2910)

Lesson learned. All right, so if there's one thing you should remember from this talk, except from our  stunning presentation, it's that quality context is everything. You know how we always say garbage in, garbage out? Well, it turns out slapping AI on top of your existing chaos doesn't magically fix it. Your agent is only as good as what you feed it. Feed it garbage context, you will get garbage responses.

It's like asking your AI agent to debug production issues, but the only context you're giving is, well, it's broken and a screenshot of a 500 error. Good luck, it's not going to work. Start by crawling. Let your agents do read-only stuff first: incident response, data gathering. Get comfortable with how it thinks, watch it work, fine-tune its system prompts and tools, and build trust.

Then, once you're confident it's not going to suggest deleting the database as a fix, graduate to safe write operations. Let it handle the high toil, soul-crushing tasks like rerunning the cron jobs that I showed you earlier. Finally, run. After you've proven reliability, and I mean actually proven it, not just hoped really, really hard, you can enable high-stakes actions like production rollbacks, the stuff that makes your heart rate skip a bit.

Build evaluation infrastructure from day one, because your agent needs CI/CD too. Prompts drift, models update, and patterns change monthly. Without proper testing, you are basically shipping to production based on "well, it worked on my computer," right? You wouldn't deploy your code without testing, so don't deploy AI without evaluations. Build a proper evaluation environment and test thoroughly before going to production, or prepare to explain to your CTO why your agent suggested deleting the database as a fix.

So three takeaways. Context is everything. AI amplifies whatever you feed it. Crawl, walk, run. Build trust before you hand over the keys to the kingdom. Test like your production depends on it, because it does. We became SREs to solve interesting problems, not to be professional tab openers at 3 a.m. So let's teach machines to open tabs. Let's teach them to investigate, and maybe eventually let's teach them to fix things while we stay asleep. That's the dream, and we are closer than you think. Thank you.

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/3100.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=3100)

Okay, so I think we learned a lot and talked about our many services.  What I want to also call out here is, you know, we have SkillBuilder. Probably most of you know about it, but I just wanted to also ensure that all the new things that we talked about, like Agent Core or any other services or solutions, there are a lot of learning resources available. So please take advantage of that, and there are a lot of free courses available. Do explore those, and that will help, right?

[![Thumbnail 3160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0db9e1ecf9e6b23a/3160.jpg)](https://www.youtube.com/watch?v=qvhmFAvG_QI&t=3160)

Now, before we wrap up today, I just want to leave you with one thought. When we saw this six to eight months back, the iHeart engineers were really struggling with respect to keeping up with the scale and the issues. But today, with this solution, they have the precious bandwidth to look into future enhancements and the next generation of streaming, which is very important. So the AIops revolution is not really coming. It's already here. And with Amazon Bedrock, you have everything that's needed to join that, right? With that said, thank you for coming to the session. I really hope you enjoy the rest of re:Invent, and  see you all at Replay. Thank you so much.


----

; This article is entirely auto-generated using Amazon Bedrock.
