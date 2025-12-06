---
title: 'AWS re:Invent 2025 - Build and optimize edge architecture for resiliency with AI (HMC403)'
published: true
description: 'In this video, Robert Belson and Jesus Federico demonstrate building AI-powered resilience assessment tools for AWS hybrid and edge architectures using Outposts and Local Zones. They progress from creating static Python discovery scripts to implementing agentic solutions with Kiro CLI and Strands Agents. The session showcases integrating MCP servers for AWS documentation and resource discovery, enabling automated infrastructure analysis against best practices. They demonstrate multi-agent patterns including swarm and agent-as-tool approaches, where specialized agents collaborate to assess site-level, compute, network, and data resilience across edge deployments in the Frankfurt region.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/0.jpg'
series: ''
canonical_url: null
id: 3088884
date: '2025-12-06T12:24:49Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Build and optimize edge architecture for resiliency with AI (HMC403)**

> In this video, Robert Belson and Jesus Federico demonstrate building AI-powered resilience assessment tools for AWS hybrid and edge architectures using Outposts and Local Zones. They progress from creating static Python discovery scripts to implementing agentic solutions with Kiro CLI and Strands Agents. The session showcases integrating MCP servers for AWS documentation and resource discovery, enabling automated infrastructure analysis against best practices. They demonstrate multi-agent patterns including swarm and agent-as-tool approaches, where specialized agents collaborate to assess site-level, compute, network, and data resilience across edge deployments in the Frankfurt region.

{% youtube https://www.youtube.com/watch?v=O_fCw4zH88U %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/0.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=0)

### Introduction to Building Edge Architectures with AI: A Live Code Talk at re:Invent

 Las Vegas, welcome to our session Build and Optimize Edge Architectures for Resiliency with AI. It's a 400 level code talk, and if this is your first code talk, it's also my first code talk. We're super excited to take advantage of this format. Previously we've had things like chalk talks where we've had a chance to whiteboard, hey, this is what we think this could look like, or a workshop where you come follow along with me as we build. But now we have an opportunity to code in front of you. This is live. Anything could happen, but we think it's a really exciting format to learn and build together.

Because we have a nice small and intimate room setting, we highly encourage questions. So if you want to see us build or tweak something different, we'd love to hear your feedback. But by way of introduction, I'm Robert Belson. I'm a Senior Solutions Architect based in New York City. I focus on hybrid and edge services and specifically its application with generative and agentic AI. My co-pilot for this afternoon is Jesus Federico. You want to introduce yourself?

Sure. Hi everyone. My name is Jesus Federico. I'm a Principal Solutions Architect with focus on Generative AI. I used to work for the Telco Vertical. Now I'm focused fully on Gen AI. Fun fact, we used to both work together in the Telco Vertical. So we've been working hard on related topics here for well over a year and excited to share not only what we've developed here but what we've been learning from customers and some of their feedback and how it applies to some of these best practices.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/100.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=100)

So before we start coding, since we have an hour together, I think the theory is very important, what we're doing, why, and why we're doing it. And I think it's always helpful to start with a level setting exercise because I find, you know, even as we go, turn back the clock to 2019 re:Invent,  what, six years ago when we first announced a lot of these AWS hybrid and edge services, AWS Outposts, AWS Local Zones, AWS Wavelength Zones, all part of this cloud continuum. We wanted to create a consistent set of AWS services, the same set of APIs, operational consistency, and the same services you know and love exactly where developers and builders need it most.

So just by way of hands, how many of you have had a chance to experiment with a Local Zone or an Outpost or a Wavelength Zone? I think I saw two or three hands. That is super consistent across all the six re:Invents that I've had a chance to build with. We often find that these sessions, folks come who are new to all of these services. So we always figured, you know, even though it's a 400 level chalk talk, we want to start, our code talk rather, we want to start with the basics and recognize that many of you have yet to build across that cloud continuum. Many of you have built on our AWS regions, but if you haven't had a chance to build with some of these other services, the fundamental question we asked was how could we bring these cloud services you know and love closer to where that data is being produced and consumed, or in a city near you, or within the customer premise.

### Understanding AWS Hybrid and Edge Services: Local Zones, Outposts, and Use Cases

So just to define our terms as we go from the left to the right, you take that broad and deep set of services in the region and you can extend it to metro areas where maybe no such AWS region exists today, and that's what we'll call an AWS Local Zone. We can further extend those cloud services you know and love to the customer premise, 42U rack mountable compute that we bring to the customer premise. We call that an AWS Outpost. And we have a number of other services here that service the customer premise, such as Amazon EKS Hybrid Nodes where virtually any physical or virtual machine can connect back to a remote Kubernetes control plane managed by us in EKS Hybrid Nodes, and then of course a number of services to support far edge devices.

We're going to focus today most on Local Zones and Outposts. We see a lot of customers, so Jesus mentioned we both supported telecommunications. It's very common for a telecommunications customer to think about using Local Zones and Outposts to help build out their network. You might have a media and entertainment company using Local Zones to deliver low latency, so to service a new metro to get closer to those customers. Similarly, you might see that for gaming. But it could also be other regulated industries that are using Local Zones or Outposts for data residency requirements. All that data must be processed in location X. And if you bring an Outpost into that premise, into your customer premise, where it happens to be within that sovereign boundary, well, now you've fulfilled that requirement.

So there's a number of different use cases. It could be latency. It could be for migration. It could be for data residency. There's a number of different use cases, but let's jump ahead now that we've defined our terms. Yeah, also, if you're interested in more on how to deploy agentic solutions on the edge, after this session will be another session, 302. It's going to be around deploying agentic solutions on edge. You can just come with us. We're supporting the session, so you can come right along with us. We'll run on over there.

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/300.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=300)

Again, so we've defined our terms of the services that we're going to be working with today, regions, Local Zones, and Outposts.  And we want to talk about resilience. Of course, one of the famous quotes, everything fails all the time. How do we care for resilience?

### Defining Resilience: High Availability, Disaster Recovery, and Multi-Dimensional Best Practices

We define resilience as you see here as the ability of a workload to recover from infrastructure or service disruptions. You can think about the mental model for resilience in three different ways, and again this has nothing specifically to do with hybrid and edge services. It's far broader for any region design you're going to think about.

High availability is the resistance to common failures through design for a primary site. In the case of hybrid and edge, what we're going to show you and one of the key tenets is if you're using an AWS Outpost, it's very likely that you're going to want to have multiple compute racks to have high availability. Similarly, for Local Zones, let's say you're deploying in the Northeast for low latency. You're going to want to use two Local Zones either in that same city if they're available or nearby cities too, just like you would in the region with two Availability Zones, US East 1A and 1B for instance.

But then you can also think about disaster recovery. How do you think about backup and manage recovery objectives? We're going to show you how you can, because there's a lot to grasp here and each of these concepts of high availability and disaster recovery have implications to compute like Amazon EC2, storage and RDS databases, networking, and beyond. The cognitive load to have one individual be an expert in all of these design primitives for all of the services of AWS as it applies to hybrid and edge, that's a lot. But what if we could use agentic AI and generative AI more broadly to leverage that expertise and design systems that help make prescriptive recommendations to create that resiliency? That's exactly what we're going to do.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/420.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=420)

If we do a good job today in the spirit of a code talk, if the code that we purport to build delivers what we hope, we hope it covers these areas.  We talked about the mental model of resiliency as a whole. I tried to define as I think about resilience for hybrid and edge environments just starting from the simplest of primitives and extracting out from there. When I think about resilience, we probably want to start with site level resilience. There's a physical discrete location where that compute rack in an AWS Outpost or that Local Zone lives, and I better hope that we have some geodiversity.

I would hope that the networking is resilient, that there are different paths to connect point A to point B if that transfer is important to that application workflow. Compute resilience is also going to be important. If I have, let's just say a fundamental unit of compute, if my application requires three units of compute, whatever that might be, an instance type, a load balancer, I better have more than three units of compute in that discrete environment, and of course you can extrapolate from there.

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/500.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=500)

So site level resilience, compute resilience, network resilience, data is important too. When we start to think about high availability and disaster recovery, I want to have data resilience, so things like clustering and replication. Then more broadly, operational resilience, so not any individual primitive, but how everything works together as you start to think about auto scaling, health checking, and monitoring. If we do a good job today,  these prescriptive recommendations for resiliency developed by an AI agent should be able to come up with recommendations that match those buckets.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/550.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=550)

### Leveraging AI Agents and MCP Servers for Automated Resilience Recommendations

Here's how we think we're going to do it. You could of course implement this differently, but here was our thought. Instead of, or to complement the experts you may already have in each of those domains, what if those experts could be represented as MCP servers? Those MCP servers were then tools provided to an agent complemented by the logical reasoning that an LLM in Amazon Bedrock might have. It could be an Anthropic model, it could be an open weights model, totally up to you. There was this agentic loop I will talk about more that you have a goal and you're going to keep using those tools  in your tool belt.

In this case, all the documentation for best practices that we at AWS have plus specific knowledge of how hybrid and edge works, so what changes in the world of Outposts and Local Zones that we'll talk about. I gather a bunch of data. Have I achieved that goal? Yes, no? Oh, not yet. I need more information to achieve that goal. I'll continue that loop until I'm confident that I've achieved that user's result.

If we just quickly go back to that previous slide for a second, we're going to use all the resiliency primitives that we talked about before. It's not going to just operate in isolation here. It's going to look specifically at the API, so it's not just knowledge of what are general best practices. I want to know my specific environment, what's real, what isn't, how can I make improvements. I'm going to use an MCP server that talks to my AWS various AWS APIs to liaise with VPC, EC2, EBS, Auto Scaling, and the list goes on.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/620.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=620)

Because now I can look at my entire environment, reconcile it with what those best practices exist, and suggest tailored recommendations to me. So let's jump ahead. We're going to start talking about what we are going to build here today  from static to something more advanced. Yes, you want to talk a little bit more about what this looks like?

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/700.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=700)

Sure. So basically, our idea is to start with some configuration static file. Normally how we would do this before is just gather information around our environment, see the resources that we have in terms of Local Zones on Outposts, and then create a kind of first report. It's like kind of a static report if you think about it, having all the resources and saying, hey, now I need to build best practices for my infrastructure. Then you are going to put some MCP servers, some tools to automatically request and fetch all these resources, and based on the knowledge of the AWS MCP documentation part, we combine that to create a new report that is more comprehensive, right? And we will be iterating on the agent that we are going to be building. We are going to be using Strands Agents. For those who don't know Strands, we will talk a little bit around it, and we will talk around several ways how we can use multi-pattern strategy with Strands using, for example, agent as tool or swarm or workflows. 

So our goal today, again, this is kind of a high-level step we're going to be creating. We're going to be creating a static Python file for discovery, then we're going to be creating a report based on this information. Then using Kiro CLI, we are going to be integrating some MCPs. The first one is a tool to get information around the resources automatically. We'll also be using MCP to get all the documentation of AWS best practices around resilience, and we will be switching this from static, from Kiro CLI that is prompting into a Strands Agent, and we'll be building a couple of Strands Agents. And maybe quickly just a show of hands, how many of you over the course of the week have had a chance either to be hands-on or attend a session covering Strands or Kiro? Okay, so good, good. That I was expecting, so we're maybe at 60 to 70% of the room.

So lucky for you today, we're going to apply all of those concepts that you started to see specifically to this hybrid and edge track of how you, it's not just, so there are going to be plenty of other sessions that show you, hey, on an Outpost or Local Zones, those same telecommunications or media and entertainment or gaming companies may be running generative AI models at the edge for latency or data residency reasons. We're flipping that model on its head today and saying, okay, what you could also do is for your own edge infrastructure, use generative AI tools in the cloud to inform how you use those hybrid and edge infrastructure services in the first place. Let's move on.

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/800.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=800)

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/830.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=830)

### Creating Static Infrastructure Discovery with Kiro CLI and Python

So first, let's talk a little bit around our environment. Again, we are going to be using Outposts.  We are using a specific account. It has five Outposts, four of them are active, one is in provisioning. We'll be running this in the Frankfurt region where we have all the resources. The first thing that we are going to be using is Kiro CLI. This Kiro CLI implements behind the scenes what we see as the agentic loop, alright? It has the capacity of reason. It has the capacity to connect to the resources. But the first thing that we're going to be using is Kiro CLI to create this kind of static files, the reports, and then move forward to create the Strands Agent. So  for those that used, how many of you used before Q Developer CLI? Alright, Kiro CLI is basically the evolution. It's the integration of Q Developer CLI. Now it's Kiro CLI. It's part of integrating agentic development tools.

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/860.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=860)

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/870.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/880.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=880)

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/890.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=890)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/900.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=900)

So let's move into our first step. Let me see. Let's create something here. Can  you switch the, yep. Demo. Yeah, so hopefully we are using the internet. It doesn't  fail. So let's start talking with Kiro CLI, and can everyone see? Is the screen large enough? Good. Okay, great. Alright, for those that haven't used this before,  here we have, we are using an agent that is default. What we are seeing here is how the context is being used, so we can check  more here around the context. Basically, it has 3% of being of some of the tools. If you want to see the tools that it has already, it has, for example, one tool that is called AWS. One is the thinking  report, write, read files and so on.

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/920.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=920)

These tools already consume part of our context window because they're available directly in the KIRO CLI. I have some prompts here to show you. I'm going to say that  I'm presenting at re:Invent 2025 and showing live how KIRO CLI works. For all my requests, save all the generated assets and scripts to a specific folder so it knows where to do the file and we have some kind of consistency. Again, we are going to be using a specific account that has some of the Outposts that we already mentioned, so this is the profile that we have.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/950.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=950)

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/960.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=960)

Here, KIRO CLI is validating and creating the folder. It's already there, so I'm ready to set up all the demos.  What are the first things that you want to build? As we explained, let's move forward and create a static file. I have my prompt here, so let's see.  I'm going to be creating a Python file that is called edge_discovery.py that uses boto3, the SDK library for Python, to discover all the edge resources in this particular region. The script must contain and discover things like availability zones, local zones, and Outposts. I want to see which instance types are available on the Outpost, and the script itself needs to manage all this information around error handling and throttling. I want the output as a JSON file.

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1010.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1010)

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1020.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1020)

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1030.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1030)

Let's start doing this and then let's analyze what it creates so we can understand it a little bit. A key point to think about right now is, if we're going to think about resiliency and reliability in a hybrid and edge environment, step one, even before we think about creating an agent, yes we're using an agentic  CLI tool to do so, but we're just trying to take inventory of what exists before we create prescriptive recommendations. That's exactly what you're going to see here. It already created my  file. Let's check the file first. Let's try to run it. If something happens, it can automatically fix it, but let's see if it runs and we can dive deep a little  bit into the code that it generated.

[![Thumbnail 1040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1040.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1040)

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1050.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1050)

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1060.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1060)

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1070.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1070)

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1080.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1080)

All right, so let's see. Well, it's running.  We set it to create a file that is static and it discovered the edge infrastructure. Basically, if we dive in a little bit on the function it created,  it goes to boto3, using the profile that we set and the region, and it fetches all the information around the availability zones. It has all the information around the zone ID,  state, if it's a local zone, whether it's the parent availability zone, or the network border group that is attached. For the Outposts, it's  doing basically the same. It's listing the boto3 SDK functions to list the Outposts, and basically it's getting the information around the availability zones, the instance  types available, and so on.

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1110.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1110)

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1120.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1120)

Also, we want to see from the Outpost perspective there is a concept that is called slotting. The Outpost itself is a rack service, it's a hardware, a 42U hardware, where you have compute nodes and you need to specify what are the shapings or the types of EC2 instances that you want to have available on that configuration. We want to see in this particular Outpost that we have what are the instance types available,  and also if we have some placement groups, what are the placement groups that we have. Let's see the results. It ran, all right.  This is the information that we got. Let's dive deep a little bit on this. It's basically showing the region, the availability zones, and all the information that we just checked in the Python script.

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1140.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1140)

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1150.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1150)

[![Thumbnail 1160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1160.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1160)

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1170.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1170)

Now let's move forward and say that I want to modify this script. I want this script to be stored  in a JSON file. It's going to be a simple modification. Basically, we expect that it will change a little bit the last part. All right, so now  the output will be saved here in the edge_discovery.json file. If we run this, it will generate my file, my JSON file with the  information we already saw. While it's generating, any questions about what we're doing, why we're doing it, or the sequencing here? Again, this is just  creating a static snapshot, a point in time view of just what's out there. Once we have that, of course, then we'll be able to create recommendations.

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1180.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1180)

One very key insight,  especially for those who are seeing Outposts and local zones for the first time, these are connected edge offerings. The local zone in a given metro or an Outpost sitting in your customer premise always has a parent region. It's always connected to an availability zone in the region. That creates, of course, high availability, but it creates that connectedness that allows you to run API operations.

And why that's important is if you see how all those Outposts have an anchor Availability Zone, every Local Zone is anchored to a region. You're starting to see how that's going to create high availability best practices. You're going to want Outposts anchored to different Availability Zones. You're going to want to have Local Zones with different Availability Zone IDs. So those same best practices continue to apply.

[![Thumbnail 1250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1250.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1250)

Let's ask even KIRO CLI to create a, based on the information we have, let's create a first static report. Basically we are saying like an HTML report based on AWS best practice for the edge and considering the resources that we already discovered. We haven't told KIRO CLI what those best practices are, so it's got to figure it out. Yeah, it will be using the knowledge that it was  until the model that it's using, that is auto model, and we'll dive deep a little bit more on that, but it's using the knowledge base that the model itself has onto the cut point time that it was trained, right.

All right, so if we want to use, and we will be using, if we want to use the up-to-date documentation, the latest documentation, we need to find a way to fetch that documentation and we'll be using an MCP tool that provides access to the best practice using AWS MCP documentation, all right, for this one. Any questions so far? No, we'll be showing this right now. It's just the knowledge that this large language model that's being used by default by KIRO natively knows, and the only reason it knows anything about your environment is we specifically say, hey, edge_discovery.json, which we happen to know captured that point of time snapshot.

[![Thumbnail 1310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1310.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1310)

[![Thumbnail 1320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1320.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1320)

### Enhancing Reports with Dynamic MCP Servers and AWS Knowledge Integration

All right.  So here we've got a few. We have our first, this is just generated. We are seeing it for the first time as you.  So we have here our information. We are 3 Availability Zones in the eu-central-1. We have 2 Local Zones enabled, we have 3 Wavelength Zones, and we have 5 Outposts. 4 of them are active. One is in provisioning state. And we have here all the details of the Availability Zones, the Local Zones, and the Outposts.

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1340.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1340)

If you see the Outposts itself, as Robert mentioned,  it is tied to a parent AZ because when we request the Outposts we do something that is called the anchor VPC and we create a subnet in specific AZ that we want to put the control plane go through through that AZ. So we have here 4 Outposts, two of them in the 1a, the one that is provisioning doesn't have any information yet, and the other ones are expanding across the rest of the AZs.

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1370.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1370)

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1400.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1400)

If we see the best practice documentation,  it shows a really comprehensive information, like your infrastructure spanning the 3 AZs with Outposts in each zone, providing excellent geographic distribution. There are some recommendations around there is an Outpost that is in provisioning status, check what's going on, and we have multiple instance types available in terms of diversity. We have several offers around instance type in terms of the Outpost rack one, we have C5, M5, R5, and we can also have different  types here.

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1410.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1410)

So again, this is  kind of static information. We just generated the script and now we are providing some information. Any question? Yes. Yeah, you can ask KIRO and say, you can use, for example, if you are using AWS Organizations and you have cross role, you can say, hey, for example, if you are using Control Tower and you have a cross role at corners, you can say, hey, use this. Verify what are my accounts, you can discover on your account and assume the role to perform this information across all your environments spanning in multiple accounts. Right.

Yeah, I have here my profile. So in the profile itself in the AWS CLI configuration, it has the information around the account ID and the profile of the role that I assume. When I use it for the first time, you know, I don't have a profile. Right, if there are multiple accounts, every single model, those are decoupled. One like one account is what you would have access would give you access to KIRO, but those other accounts could simply be to pull resources.

[![Thumbnail 1520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1520.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1520)

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1540.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1540)

The simplest brute force way is that however you've authenticated to have access to KIRO, that remains unchanged. But you could have your AWS configuration file where you typically have your account secrets, and you could have multiple profiles.  When you prompt KIRO CLI, you could basically say, remember we created that edge discovery JSON file, we could basically say iterate through all of those AWS profiles. Then given those credentials, you're now talking to different accounts with different resources. Pull from there and now you have a more complete set of resources reflecting that JSON.  How you segment by account, you would have to decide what's important to you.

[![Thumbnail 1550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1550.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1550)

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1560.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1560)

[![Thumbnail 1570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1570.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1570)

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1580.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1580)

[![Thumbnail 1600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1600.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1600)

Alright, so let's move forward again. Here we are not using the tools.  The tool itself is what KIRO CLI has embedded as default. It has things like to-do and thinking. You can enable some of these tools in the experimental  part. So you see here I have things like to-do list, checkpoint, delegate, knowledge, and thinking. This is part of the KIRO CLI. But now I want to introduce a new MCP.  So let's create a new agent. Let's create one called AM 3. So here's the configuration of the agent.  Think around this, it's like you can have multiple KIRO CLI instances at the same time. Think around it like profiling, so you can have one agent that has access to different tools at different MCPs. In this particular case, I'm going to be adding this knowledge  base MCP. So hopefully it works.

[![Thumbnail 1630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1630.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1630)

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1640.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1640)

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1650.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1650)

We have this public documentation. We will be sharing a little bit of what all the MCPs we have out there so you can consume. This particular MCP is an MCP that provides information around best practices, latest documentation, dynamically. So now we have created it. Let's change the context and then say I'm going to be using this agent.  So if you see right now, and I go to my tools, I now have a new MCP here that is AWS Knowledge MCP server that has this kind of  information and this kind of tools. So what if instead of saying to create a static file, let's make something different. Let's  say that I want to create an HTML report of edge resilience best practices for my infrastructure, but I want you to find the latest documentation and using the AWS tool that it has, find the resources for me. I don't have a file anymore. You are not relying on anything, on any static file. It needs to somehow with the gigantic loop figure out what are the resources that it needs to find based on the best practice documentation, and it creates a report for me.

[![Thumbnail 1690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1690.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1690)

[![Thumbnail 1700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1700.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1700)

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1710.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1710)

[![Thumbnail 1750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1750.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1750)

While the report is creating, can we take a question from the audience? Sure. What's the difference between the  knowledge MCP server you just added and the knowledge tool that's already there?  OK, different. Yeah, so I see there and then I saw you guys acknowledge the second time, I was just curious.  Yeah, I'm not sure. I'll take that offline. This was like KIRO was rebranded one week ago. It used to be QCLI, so it introduced new features like web search and web fetch which wasn't before on QCLI directly. So you can dive deep a little bit. I don't know if, generally speaking, the idea would be that it's the general knowledge that the KIRO CLI must have for just coding in general and any best practices for software, whereas the more specific guidance you can give, the better. And so we've created, you know, to keep updated documentation to have a specific AWS knowledge, but it's likely to your point that there is some overlap. 

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1780.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1780)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1790.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1790)

So if we see, go ahead. I'll focus on this question. So the built-in knowledge, is it basically using an MCP? Right, because those are all tools that it has, and so it might, it need not be MCP. So as you'll see later when you create a Strands agent, you can just have local tools that you'll see with like a Docker. You have just some function like def cool function, and then it just has some logic.  It need not be called through MCP. That's just one way for a protocol for different, for an agent to talk to a tool. So in that case it may not be called via MCP itself. 

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1800.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1800)

So if we see what is happening behind the scenes, when I ask this kind of request, it's going to be running the tool to search the documentation,  like receiving best practices for Availability Zones, Outposts and failover best practices, Local Zone architecture best practices, and so on.

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1810.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1810)

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1820.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1820)

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1830.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1830)

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1840.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1840)

It's collecting all this information and  putting it in context. Now it needs to find all the resources because I want this applied for my account. It will find all the resources using the profile and will run the  CLI using this tool to get all this information and create the HTML file for us. It's going to take a while, so  let's go back into the presentation to discuss some of the next things that we are doing. We can talk a little bit about the  MCP servers we have there meanwhile, and then we'll go back to check on the progress.

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1850.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1850)

[![Thumbnail 1870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1870.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1870)

 Talking a little bit about some of the MCP servers, we added this Knowledge MCP server, but there are other MCP servers that you can use. For example, in this particular case, we didn't add it because it was built into the KIRO  CLI, but you can use a Cloud Control API MCP server to have access to all the resources around your AWS infrastructure. It basically has information around how to access with API and integrate with your environment. In this particular case, we didn't use it because it already has a tool, which is the AWS tool, to get that information. Any questions? All right, so we already integrated this.

[![Thumbnail 1910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/1910.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=1910)

### Building Strands Agents: From Kiro CLI to Python-Based Autonomous Agents

 Let's talk a little bit around Strands. Well, it's still generating and hasn't finished yet, so we can talk a little bit around how we want to move forward. Now we want to move out from creating this kind of assessment and report, but it was kind of interactive, right? We needed to dive deep and type what we have in a prompting style. What if we want to run a kind of agent and we want to build an agent using Python that can run, for example, in my backend system and I can execute it every time that I need, instead of having something that I need to prompt manually? We can create something using a Python file and run an agent that can operate in the backend. That's why we're going to be using Strands Agents.

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2000.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2000)

Strands Agents is an open source SDK for building agents, and you can use Strands with multiple model providers. In this case we are going to be using Bedrock, but you can use other providers, and it has a set of tools that you can integrate and use. Do you want to mention something? Sure, maybe just as we'll see in the code, maybe just a few things when you think about an agent. It's got to have a model, it's got to have a prompt, so the brain of how it's able to logically reason, the prompt to tell it what it's doing, and then a series of tools, the actions it can take to deliver against that goal. We believe that with Strands it's one of the easiest ways,  specifically native to AWS, to marry those three, almost like strands of DNA that are intertwined. The ability to have a really simple way where you can create agents in just a very few lines of code, but also native integrations if you wanted to have a RAG workflow as part of your tools. So if you have company specific data, you had a specific question and you had that RAG workflow and you wanted to expose it as a tool, you could write a bunch of extra code to do that manually, or you could just use the native retrieve library that's already integrated with Bedrock knowledge bases. That's an example of where the AWS integration shines with something like Strands.

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2060.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2060)

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2070.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2070)

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2090.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2090)

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2100.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2100)

So on the other hand now, we are going to be switching to use KIRO. The IDE is the agentic development platform for prototyping. In this particular case, we were using KIRO C line. Now we are going to be using the IDE to create our first agent. Let's move on and let's see if we should have the report ready.  Okay, it created a new report and  generated this report. We can see that it has similar information: three Availability Zones, two Local Zones, four out of five Outposts, with one of them in provisioning state. But it also has more information around, for example, Auto Scaling groups, load balancers, and placement groups. It has information also around multiple deployments  of EC2 instance infrastructure that spans across other resources, for example load balancers, that we didn't have before. 

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2110.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2110)

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2120.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2120)

When we created this, we didn't ask any specific resources to  be statically generated, or we didn't define it statically. It just found it for us. It basically discovered this placement group, Local Zones, and all the stuff. So it generates this for  us based on the documentation that it finds, and it creates the report.

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2130.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2130)

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2150.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2150)

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2160.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2160)

So let's move into  KIRO. This is the development environment. Again, we're going to be using live coding. This is a code talk, so let's hopefully it works. So let's start with KIRO, asking something around  reading the documentation of Strands, and based on the documentation, Strands will create our first agent. 

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2170.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2170)

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2190.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2190)

[![Thumbnail 2200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2200.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2200)

So in order to read the documentation of Strands, we can use a Strands MCP documentation server for showing that. We have this  Strands documentation. Let's figure out how to create our Strands documentation agent. All right, so we can use, for example, this agent that is called a Strands MCP agent.  Basically, you run the UVX command, but in our particular case, you can also use, for example, you can retrieve the documentation of the software that you are building. 

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2220.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2220)

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2240.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2240)

Ask the KIRO environment to read the documentation if you don't have an MCP server for that, and based on the documentation, start building on top of that. And a key point that I think you'll see here is the configuration of this agent, whether it's the KIRO IDE or the KIRO CLI, was almost identical. And you'll notice when you specify those MCP servers, the first time  when we were looking at the AWS Knowledge MCP server, it was more of a URL because it was a remote MCP server. In this case, when we're showing Strands, it was a local MCP server, so the consistency of defining the MCP services, or rather to define these MCP servers, is consistent, but it could be local, it could be remote, it could be a combination of  both. It doesn't matter.

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2250.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2250)

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2270.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2270)

So in order to do that, again, I downloaded the documentation of Strands, all right? So I'm asking to read the documentation.  Also, I created a base template of a simple Strands agent, so it has all the information around my profile. I could use even KIRO to say, hey, you know, create an agent using this profile in order to avoid kind of noise. Since we have a short time, I want to be focused on what we have today and focus on  our configuration.

[![Thumbnail 2280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2280.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2280)

So I created a kind of base agent that says I'm going to be using this profile.  I'm going to be using a structured response in Strands. That means, for example, if you want the output of your agent to be structured, let's suppose that you are returning a person object. You want to have a name, you want to have the age, so you can create a kind of, when you create the agent, you say, hey, use a static output. In this particular case, I'm going to say I'm going to be asking for a markdown report.

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2310.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2310)

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2330.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2330)

[![Thumbnail 2340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2340.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2340)

So these are just placeholders, right? This is our main loop, and we are going to be creating  using our profile. I'm going to be creating a runtime client for Bedrock with 300 seconds, 300 seconds to create the client. Then I'm going to be creating the agent itself and asking the agent for something. These are like the most important parts. Quickly,  folks, if this is your first time dealing with Strands, what, 44 lines of code, really three if you exclude the closing parentheses. When we say you can define an agent in three lines of code, that's what we mean. 

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2370.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2370)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2380.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2380)

You see the system prompt, you see the model, you see the agent instantiation, and that's it. That's it. So I'm going to be asking to, using the Strands documentation from this folder, create an agent with the Strands name of version one using this base, and basically the script should be running using this model. The 300 seconds is there for real timeout, and also we are going to be using some tools similar to the static file that we created to create some information around  get Outposts, get Local Zones, availability zones, and so on. So behind the scenes, it's going to be creating several functions to get information similar to the  Python file that we created at the beginning.

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2390.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2390)

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2400.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2400)

So let's go and ask KIRO to do that for us. Meanwhile, this creates the file, all right? It's reading the documentation.  I have a backup here. We can show how the code is going to look that it is going to be generating. I'm only letting this generate the code, but we can start reading how the code will look  like. Shall we do that? Let's go.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2410.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2410)

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2420.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2420)

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2430.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2430)

[![Thumbnail 2440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2440.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2440)

### Simplifying Agent Code with MCP Integration and Structured Output Models

Now it's running, but let's see  in order to get some of the time. Let's go back to the agent instantiation, show what's the same, and then let's show what's different and what the agent did.  We should still see somewhere in the middle of the file where it instantiated the agent just like it did before. But let's go to the agent part where we define the agent. We have our main part  where we have our config, our Bedrock client, the model itself, and now we are creating an agent.  We are saying that you have some tools. Which tools are these? Basically we asked to create some kind of functions where it can fetch information around Outposts, Local Zones, Availability Zones, EC2, and RDS as I mentioned.

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2460.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2460)

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2490.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2490)

In this particular case, we are creating this agent and the system prompt for the agent is basically  this. You are an AWS Edge Infrastructure Resilience Expert and your role is to analyze the AWS infrastructure deployment and provide comprehensive resilience assessment. Your analysis should cover a lot of points that it needs to cover. This is being generated right now, but I have a backup here just to show you this. When we create this, we ask to create a kind of tools, right? Like EC2 instances which are now running.  Meanwhile it's running, I'll be showing this. If you want to see the tools that we created, we asked to create like get outputs. It's using boto3, the client to list the outputs and list all the resources pretty similar to the Python file that we created at the beginning.

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2510.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2510)

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2530.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2530)

[![Thumbnail 2540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2540.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2540)

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2550.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2550)

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2560.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2560)

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2570.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2570)

 After it runs, it's running right now, so it just created the new one. Let's see not in the backup, let's see the one that it actually created, which should be pretty similar. It's running right now. It's conducting a comprehensive  AWS infrastructure resilience analysis on the EU-Central-1 region, and it's calling the tools. It's calling get output, get Local Zones, get Availability Zones, instances,  all the stuff, and now it's basically outputting. It's creating my output. This is the report that is going to be saved in one second after it finished,  with my markdown file with all the information that I have. Any questions so far?  I think just a key learning objective is no surprise that the individual tools that are being instantiated like get Outposts is going to be using almost identical code to that first program we created that describes the available EC2 instances  that are available or describes the Availability Zones, the Local Zones, and the Outposts that are available anchored to that region.

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2580.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2580)

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2590.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2590)

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2600.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2600)

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2610.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2610)

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2620.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2620)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2630.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2630)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2640.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2640)

We're taking these little primitives, these functions and  showing it could be a standalone Python file. It can be part of a tool that talks to an agent. It can be embedded into some remote MCP server that our  CLI or agent talks to. It's just different ways of using the generative AI toolkit in AWS to get the results you need and often much faster than doing so manually.  Now it's basically the output of all the information that it's providing. Once it's finished, it should generate  a file with the markdown that as we asked, like the big one. I think it should be finishing in a couple of seconds. Hopefully it  works. After this, we are going to be doing something similar to what we did with KIRO CLI. What if we say, hey, just remove all these tools and  let's use a tool that can provide access to my environment? I don't want to define any kind of tool and simplify my agent saying, now you are an expert, you have access to my AWS infrastructure,  you have access to the AWS documentation, do what I need. It's going to be simplifying our code of Strands agent in less than 100 lines.

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2650.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2650)

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2660.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2660)

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2670.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2670)

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2680.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2680)

[![Thumbnail 2690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2690.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2690)

[![Thumbnail 2700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2700.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2700)

 Right. Any questions so far?  I have a question. In terms of cost, how many credits is it costing?  Yeah, so when you are running Strands, you can use the metrics tool, so it generates how many input tokens and output tokens it's consuming. I don't have your particular exercise.  Yeah, it should be printing the metrics here. I think I have  right now. We haven't benchmarked this exercise in the specific number of credits that it would consume in KIRO yet. 

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2720.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2720)

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2730.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2730)

[![Thumbnail 2740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2740.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2740)

[![Thumbnail 2750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2750.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2750)

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2760.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2760)

I don't have the exact point right away, but we can do the iterator and provide and print the metrics for you. Yeah, so going back to the report, if  you want to see the report, it has pretty extensive documentation. It  should have also some recommendations around buckets, information that we didn't have before, and also some links around documentation and even some code of  how to modify, for example, the load balancer that we identified that is running spanning on multiple Availability Zones and so on. Again, if we want to simplify this  a little bit of the code, because now our version one is more than 500 lines  of code that we may not need all of them.

[![Thumbnail 2810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2810.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2810)

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2820.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2820)

So what if we say that I want to create a new version, alright? It's gonna be version two. Let me show you the prompt here. So I'm gonna be creating a new version of this resilience agent, but based on the first one, but with an additional tool. I want to configure the MCP, the Knowledge MCP server, and also remove all the tools to get resources from AWS. Instead, use the predefined use AWS tool that comes from Strands. So the idea here is just to simplify the code, and similar to what we did in the first Quiro CLI, it's gonna be created in a Python file with a Strands agent that is simple to read and basically it's prompting. So  again, let's see how it should look like while it generates the code for us. Can you read there? 

[![Thumbnail 2830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2830.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2830)

[![Thumbnail 2840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2840.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2840)

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2850.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2850)

[![Thumbnail 2860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2860.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2860)

[![Thumbnail 2870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2870.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2870)

[![Thumbnail 2880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2880.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2880)

[![Thumbnail 2890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2890.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2890)

Okay, so it's pretty similar. We have the  response in a structured format output response, but now we have a system prompt saying that similar, you are an AWS resilience expert, you have real access to real-time AWS documentation, and your role is to analyze the infrastructure  deployment and provide a comprehensive resilience report. Pretty similar. We have our model now. We have this MCP  client that is a streamable HTTP client that has access to the endpoint of the MCP, and we are creating an agent with a model and saying like, now you are gonna be using this  tool, use AWS. This is a tool that is embedded in Strands. You will see it here in the Strands tools. You can find more tools available on  the documentation on GitHub on the Strands tools repo. And also now we're gonna be using the AWS Knowledge MCP that is basically what we defined before.  And instead of having all those tools, get Outposts, get Availability Zones, get EC2 instances, you could define all those tools manually, or you could have your  Strands agent in turn be an MCP client that connects to these remote MCP servers, yeah, saving you a bunch of time.

[![Thumbnail 2900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2900.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2900)

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2910.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2910)

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2920.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2920)

[![Thumbnail 2930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2930.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2930)

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2940.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2940)

[![Thumbnail 2950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2950.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2950)

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2960.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2960)

[![Thumbnail 2970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2970.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2970)

Now, if you see, it's running the version two. It's  using the tools to get all the resources documentation, and also it's using the AWS search documentation itself. Now it's pretty similar to the first, to the second version that we did with Quiro CLI, saying like,  hey, now you have access to the tool, now you have access to the AWS environment, and just figure out and do all the  things that you need in order to get that information for me. So once we run that, we should get a report pretty similar  to the first one, maybe with more details because now it's conducting, okay, it's printing here. It has access to more resources because it can  think instead of saying static, oh, you have access to the Application Load Balancer, you have access to the EC2. It can figure out, oh, I need access to the placement group, I  need access to understand what are the S3 or how is the Network Load Balancers or whatever resources are spanning across our infrastructure. It's generating that. I'm gonna be showing one  that is similar we just created this, but basically the output should be something similar of what we  are running, right? So any questions here?

[![Thumbnail 2980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2980.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2980)

[![Thumbnail 2990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/2990.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=2990)

[![Thumbnail 3000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3000.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3000)

Nope. What's the, so I see in Strands agent, agent,  I'm just curious about the code itself. What's the event loop or the agent loop? Is it, are we just like calling agent dot run and it's just, yeah, so let's say  the code of the agent loop part here. So the agent itself, it's basically you have to select which is the model,  alright?

[![Thumbnail 3020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3020.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3020)

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3030.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3030)

[![Thumbnail 3040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3040.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3040)

[![Thumbnail 3050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3050.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3050)

And you can have different model providers here. You can use OpenAI, you can use Bedrock. In this particular case we are using Bedrock, but you can implement even your own provider. And then you have a set of tools that are available, the system prompt, and then when you want to call it, it's basically  agent, like do this, run this execution for me, and this is the prompt that we are asking the agent. What we are asking  the agent is please conduct a comprehensive AWS infrastructure resilience analysis for this region. It's a variable that is passing, and the report must be generated on the current date, today,  and include the following things. Edge infrastructure report, it generates that for you. So it's kind of you define what is the system from the agent, the tools available,  and then you ask the things.

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3080.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3080)

[![Thumbnail 3090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3090.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3090)

So in order to have a response that is structured, you need to pass this argument that is a structured output model, and it's basically a class that says in description mode what is the model the agent needs to fulfill as a response. Suppose that you have something, an agent that validates information for a person. You may need to have age, name, address, whatever. You may  ask something and then you can ask directly information response, structured response, age, and you have the information there.  Clear? We have eight more minutes. Awesome.

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3100.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3100)

[![Thumbnail 3110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3110.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3110)

### Multi-Agent Patterns in Strands: Swarms, Workflows, and Agent-as-Tool Architecture

So it's going to be generating this. Let's dive deep a little bit on  what we have. While this is generating, let's talk a little bit of how we can create  multi-agent patterns on Strands. Now we saw information around, okay, now we have the code, we have access to some tools, and we can create a new version that has multiple agents. Instead of having isolated tools, what if we can create an agent that is an expert of systems administration, for example, AWS systems admin. It has access to your AWS environment and it can do read operations, write operations, whatever is the scope of the agent, so it has access to a specific tool. Then you can do an AWS documentation expert where it has access to the MCP of the AWS knowledge, and then you can figure out and say, hey, I want a coordinator that I will be sending a request, and using those agents it will figure out and do, perform the action for me. So this one is going to be the third agent that we are going to be implementing if I think we have time.

But then we have other kinds of patterns. We can do something like instead of having agents and pack that and provide that agent as a tool for other agents, we can do a kind of graph where this agent can talk with others in a kind of directed acyclic graph. So we can say which is the flow like, I'm the researcher, I'm going to be passing the information to the writer, and the writer passes the information to the reviewer, and the reviewer prints the output, for example, and it's just a kind of graph information. And then we can create a kind of workflow that in the graph we can go back and we can define the transitions going back and forth, but it's established transitions, stable transitions. And in the kind of workflow, it's basically a structural coordinator of tasks that we can define. First step, do this. Second step, do whatever, like researching, like coding, right? If you want to code an application, maybe we need to figure out first is the architecture itself, send the coder, the tester, and the reviewer, and so on. So we can create a kind of flow.

Third, we have the swarm pattern. In the swarm pattern, it's a kind of multi-agent that there is not a single coordinator. It's basically between them they figure out the task, and you can specify the swarm agent in two different ways. You can define a swarm agent saying like, hey, you have this complex task, create the agents that are experts in each particular area, and each particular area they start talking to each other as they believe they need to do. Or you can specify specific agents. You can create agents like AWS documentation expert, the systems admin expert, the report expert, and then as a whole say like, hey, I have this problem, figure out how to solve it. And they will be passing information like, hey, I need to talk with the expert. The expert saying like, hey, I need to talk with the system admin to get this information, go back to the other documentation expert, and they can retrieve the information and work as a whole, sharing information across the agents.

[![Thumbnail 3320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3320.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3320)

[![Thumbnail 3330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3330.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3330)

[![Thumbnail 3340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3340.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3340)

[![Thumbnail 3350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3350.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3350)

[![Thumbnail 3360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3360.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3360)

[![Thumbnail 3380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3380.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3380)

Let me show you the code for Swarm and how it looks like. I have a couple of examples here.  So, still generating the, well, let me close this in a second.  In the Swarm part, we are saying here that, again, we have a response, but then we are creating Agent One.  You are the Infrastructure Expert. Your name is Infrastructure Discovery, and you are an agent that specializes in discovering and cataloging AWS resources.  So it has access to the tool Use AWS. Second, we have an agent that is the Best Practice Agent. It has access to the AWS Knowledge MCP.  Third, we have an agent that does the Gap Analysis. It doesn't have any kind of tool, but it says based on the specialized agents, comparing our infrastructure versus the best practices, which are the gaps, all right? And it figures out what are the gaps and what we need to do in order to make our infrastructure  very similar or comparable against the AWS best practices.

[![Thumbnail 3390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3390.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3390)

[![Thumbnail 3400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3400.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3400)

And then we have the Recommendations Agent. Again, you are a Recommendation Agent based on the  previous information. Just gather and create an actionable remediation plan for that. And based on that, we create a Swarm. And it says, okay, let's create a Swarm  where we have all these agents. The first that will receive this is the Infrastructure Agent. You can put the Best Practice Agent, whatever you want, and you have some transition and match iteration that you can take. As we don't define how the transition will look like, it starts talking to each other like, hey, I'm the expert of this information. So it's passing information to the other one, and it just creates a kind of mesh loop instead of having interaction between all the agents.

[![Thumbnail 3450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3450.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3450)

[![Thumbnail 3460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3460.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3460)

[![Thumbnail 3470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3470.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3470)

[![Thumbnail 3480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3480.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3480)

[![Thumbnail 3490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3490.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3490)

And the agent as a tool, all right, which is, let me see if this finishes. We can ask our third version to create an agent as tool. I'll be sharing here. But the agent as a tool,  now what we are saying is, all right, now I just want to have an expert that is an  AWS Systems Admin Expert, and we define this as a tool. But if you see the implementation code of this tool, it is again an agent. So we have the same pattern. We have an agent  with a model, we have a tool. But we are exposing this function, which is an agent itself, as a tool that can be consumed by  another agent as a coordinator agent, all right? So we can create a kind of tree where we have a coordinator talking with other agents that are being exposed as a tool. 

[![Thumbnail 3500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3500.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3500)

[![Thumbnail 3510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3510.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3510)

[![Thumbnail 3520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3520.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3520)

[![Thumbnail 3550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3550.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3550)

In this case, we have a Systems Admin Expert that has access to the Use AWS tool. We have a Documentation Expert that has access to the MCP.  And we have, finally, the coordinator that provides information to both tools  and it provides information for you. So that's about all the time we have left here, but just to give you, I guess, a little bit of parting wisdom. We recognize  that we had to make trade-offs in the session of going all in on what those best practices are for resiliency while trying to introduce all these primitives for generative AI and Agentic AI specifically. We thought that to strike a healthy balance, we would show you how to take or how to go from a static resiliency report with a simple Python file to starting to integrate MCP servers, Strands Agents, KIRO CLI, and KIRO IDE, the KIRO's IDE, and to take that one step further. If you've had sessions that you've seen around Agent Core, you could run  that Strands Agent that we created in Agent Core, so you don't have to run it remotely on your laptop. You could have it running in that secure, isolated environment.

[![Thumbnail 3570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba1fdedd6875afab/3570.jpg)](https://www.youtube.com/watch?v=O_fCw4zH88U&t=3570)

So we hope that this painted a picture of what the future of, let's say, your Well-Architected Framework review could look like for your applications using AWS hybrid and edge services  or otherwise. If you're interested in getting a little bit more hands-on with hybrid and edge services, come with us just across the hall here for this next workshop. We hope you had fun here over the past hour. Thanks for hanging out with us, and we'll stick around to answer any questions and look into that credits question you asked. So thanks so much, folks. We appreciate it.


----

; This article is entirely auto-generated using Amazon Bedrock.
