---
title: 'AWS re:Invent 2025 - Nasdaq: Build resilient infrastructure for global financial services (HMC327)'
published: true
description: 'In this video, Nate Sammons from Nasdaq explains how they build resilient, high-performance systems for financial services using AWS Outposts. He covers their 15-year cloud journey from T+1 backups to hard real-time systems handling ultra-low latency transactions in microseconds. Key strategies include hybrid infrastructure with Outposts in their data centers, failure domain analysis across servers, racks, and sites, static stability to avoid runtime changes, and disconnected operation testing for week-long periods. Nasdaq runs A-B pairs of components across multiple racks and sites, uses bare metal EC2 instances with ultra-low latency network cards, and maintains hot spares to avoid control plane dependencies. Future plans include dynamic deployments, Graviton 4 testing, and EKS local clusters.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/0.jpg'
series: ''
canonical_url: null
id: 3087778
date: '2025-12-05T22:53:50Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Nasdaq: Build resilient infrastructure for global financial services (HMC327)**

> In this video, Nate Sammons from Nasdaq explains how they build resilient, high-performance systems for financial services using AWS Outposts. He covers their 15-year cloud journey from T+1 backups to hard real-time systems handling ultra-low latency transactions in microseconds. Key strategies include hybrid infrastructure with Outposts in their data centers, failure domain analysis across servers, racks, and sites, static stability to avoid runtime changes, and disconnected operation testing for week-long periods. Nasdaq runs A-B pairs of components across multiple racks and sites, uses bare metal EC2 instances with ultra-low latency network cards, and maintains hot spares to avoid control plane dependencies. Future plans include dynamic deployments, Graviton 4 testing, and EKS local clusters.

{% youtube https://www.youtube.com/watch?v=-GuZhm4XGxM %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/0.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=0)

### Nasdaq's Cloud Journey and Disaster Recovery Strategy for Financial Markets

 Welcome everybody. I'm Nate Sammons. I work for Nasdaq in enterprise architecture, and today I'm going to talk about how we build resilient, high performance systems for financial services.

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/20.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=20)

 I'm going to talk a little bit about Nasdaq and our cloud journey, then go through strategies for disaster recovery, talk about our solution in hybrid infrastructure mode, and then talk about a couple of architectural considerations as well as operations and then a future-looking view.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/40.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=40)

 Nasdaq has been at the forefront of innovation for decades now. We were the first electronic exchange, and now we operate over 30 different exchanges of different types of instruments in the Nordics as well as in North America. We are both a system operator—we're the operator of these markets—as well as a provider of software to over 130 marketplaces globally. That includes trading systems, clearing systems, risk management, anti-financial crime, and the whole range of different solutions. We deliver those as traditional software installs as well as managed services and SaaS.

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/90.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=90)

Our journey to the cloud started some 15 years ago now, and we've been working our way towards our most critical systems during that time—matching engines and market systems. Over that time, we've moved a number of systems.  We started out with T+1 backups, progressed towards more and more real-time systems, T+1 reporting, then intraday, and now hard real-time systems with our markets. At the same time, we've gone from less critical systems to far more critical systems—really systemically important for national economies around the world, including the US, and very important workloads.

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/130.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=130)

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/150.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=150)

 We've seen a bunch of challenges. First and foremost, we have systems that provide ultra-low latency transactions. We measure things in microseconds and nanoseconds and millions of messages per second where we can't drop anything and can't lose anything. We also have to do this in very specific physical locations.  I talked about proximity and latency. We have a big business that lets customers bring their own hardware into our data center. We operate an ecosystem, and if we decide to move, we have to move our customers with us. We have to be in specific locations for now.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/170.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=170)

 We also have a high resiliency target, and our customers expect 100 percent uptime all the time, every day, as long as the market is operating. And then of course we have regulatory oversight. We have to do this not just in the United States, but we deliver these systems globally, so we operate in a wide variety of different regulatory jurisdictions around the world.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/190.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=190)

 I think everybody's seen this spectrum a couple of times, talking about different ways you can accomplish disaster recovery, all the way from backup and restore, which is kind of where we started, to multi-site active-active multi-region where everything is hot all the time. I'm not really going to drain this slide, but the main point here is that you as technologists and you as business people need to really agree on where a particular workload lands on this spectrum and understand that you can hit your RPO and RTO targets if you progress your way along this spectrum far enough. It's super important that everybody understands what's going on and when a disaster actually happens, that nobody is surprised at the response. You know ahead of time and you're prepared.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/270.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=270)

### Hybrid Infrastructure with AWS Outposts for Ultra-Low Latency Trading Systems

So the rest of this talk, we're really going to talk about the far end of the spectrum on the right side here for critical systems that have to be live all the time and can't go down. So the rest of this is all about those types of systems.  First we're going to talk about hybrid compute with Outposts that lets us place workloads in our data center but still manage them with the EC2 API and use Amazon infrastructure.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/280.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=280)

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/290.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=290)

 We're going to talk about an architectural consideration of failure domains, which lets us model how the system is going to respond to various types of issues.  Then we'll talk about static stability, which is another architectural consideration around how you plan ahead for some of these issues. And then we'll talk about disconnected operation, which is a little bit more on the runtime operation side and how we protect ourselves from any sort of issues.

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/300.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=300)

 If we talk about Outposts, this is a pretty simple diagram. On the left side, we've got the region. Obviously, that's where the control plane runs and all of identity and related infrastructure.

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/330.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=330)

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/340.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=340)

We have a direct connect circuit—actually multiple physical circuits in all of these cases—that leads to our data center. There are racks of hardware with EC2 servers. We hook up the EC2 network to our management network, which lets us use hardware as a managed service where we can essentially subscribe to these services and hardware shows up in our data center.  

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/370.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=370)

To meet our needs, we were able to work with Amazon to add ultra-low latency network interface cards to each of these EC2 servers, which is now generally available. You can see the BMN SF2E and other series there—those are bare metal servers. We have bare metal on the EC2 side, but the network in this case is also bare metal. We have a fully physically separate data plane for our market systems. 

Critically, for equal access and to meet one of our regulatory requirements, we use equal-length cables and multicast. We use hardware multicast on these systems, and the bare metal nature of the EC2 instances combined with the network side lets us perform a bunch of performance tuning on the network. We use kernel bypass and all sorts of other tricks. This is the basic physical hardware setup that we are using.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/410.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=410)

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/420.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=420)

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/430.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=430)

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/450.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=450)

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/460.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=460)

### Building Resilience Through Failure Domains, Static Stability, and Disconnected Operation

When we move towards failure domains, this is a way to think about the different bounds of your system and which items have common shared fate with each other. First, let's talk about servers. We have EC2 servers with our software running on them. We run multiple servers, and we also run hot spares in the rack.    These are pre-provisioned EC2 instances that are up and running but not yet running our workload. They are provisioned and ready to go whenever we need them. We run A-B pairs of every software component, so we have a backup for every single thing, and we do not run those A-B pairs on the same instance to avoid shared fate and potential issues.  

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/500.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=500)

At the rack level, if we expand the scope of these failure domains, the AWS Outposts rack could lose power to the entire rack. Obviously, we take great pains to prevent that, but it is possible. We run multiple racks, and for every A-B software component, instead of running the A pair on the first server and first rack and the B copy on another server in that rack, we spread those workloads across racks. This ensures that if an entire rack goes out, we do not lose the A and B copy of any particular component. 

At the physical site level, we run multiple sites. We have a primary site in New Jersey and our secondary site for these markets in Chicago for the US side of things. Critically, we run a separate Outposts instance in both of those locations, and each of those Outposts is owned by different AWS accounts and paired with a different region. The account side of things relates more to cybersecurity blast radius perspective, but it all rolls in here. At the region level, for anything we are running in the region, we run multiple regions. In most cases for all of these critical systems, we are hot-hot across multiple regions, though I am really talking about the Outposts side of things here.

Looking at this setup, I have talked a lot about physical infrastructure—servers, buildings, racks, and this sort of thing. You can do the same exercise logically with your software, and you should. You should understand all of the component boundaries, how they interact, and what happens if one piece goes down and how you respond to that. For each of these items we are talking about how we build and design it, but off of each level in this failure domain hierarchy, you need to have operational runbooks as well. What happens when a server fails? What happens if a rack goes down? All of those procedures need to be covered well ahead of time and worked out with all of your operations staff. Nobody should be surprised at what is happening during a disaster. You should be surprised that a disaster happens, but all of the procedures should be predetermined.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/620.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=620)

All of the responses to that disaster should be very well choreographed and basically second nature to everybody involved. So if we move on to the next architectural  concern, static stability. You may have seen this with a number of Amazon services that are statically stable. In this case, we have a simplified diagram here with the region on the left with a couple of services that we need, and our data center on the right with outposts and a service link in between.

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/660.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=660)

What if somebody runs a trencher through a bunch of optical cables in a farm somewhere and cuts that line? We cannot have the market going down just because somebody ran over a line with a tractor. That is a really bad situation to be in. So obviously we have multiple cables and all that sort of thing, but static stability is all about not changing the system during the day. We are  not flexing up and down on any of these things, again just for these critical components.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/690.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=690)

But it really means that in some cases there might be an issue with the EC2 control plane, for instance, and you cannot provision new servers. That is why we have all of those hot spares in the rack already ready to go. We do not need any kind of connectivity to start using them. We do not need to talk to the control plane; we just start doing it. Obviously under regular circumstances, we would go through the normal channels, but  in disasters, we do not always have those options.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/710.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=710)

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/720.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=720)

We are then very careful about what services we actually deploy onto the outpost. In region we use all kinds of services. On the outpost, we are very careful because we want to be able to run in this sort of configuration where we are able to keep everything running on that outpost and not really deal with any issues.  We use local boot on all of those instances, so they have local disc assigned to each machine. We are not using EBS in this case just because of shared fate. 

We also use that local gateway for break glass access. In normal cases, if you are SSHing in, you are coming in through that service link and down back to the EC2 instance. But that local gateway lets you just SSH directly in. You need to make sure that you are not hairpinning your traffic to the region or anything, but it lets you do all local control. So the whole plan here is that we are able to manage those systems locally.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/760.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=760)

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/780.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=780)

We also did a deep dive with AWS service teams on all of their internal dependencies. For the things that we are running on the outpost, we need to know everything. We have also done a ton of testing obviously on this.  Then if we talk about disconnected operation, this is where we want to be able to survive those issues where somebody cuts that network line or if the region is having an issue or whatever. This is really about testing all these failure modes. 

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/800.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=800)

We disconnect our outpost for a week or longer just to see what happens basically. In some cases, services are re-upping SSL searches or they are checking with IAM for various things. You need to be aware of all of those operations that are happening so that you can either avoid them or just be aware and understand the failure modes  of those systems as well during your outage. Again, make sure that that local gateway is accessible. That is our break glass access into those machines and we need to make sure that it is active, even if the region is down or even if the service link is having an issue.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/820.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=820)

And again, really this is all about if there is an issue, we can cut these systems off from essentially the rest of the world and make sure that everything is still running locally. The market is going to be fine. We will do the closing cross. We will do all of those things. Everything will be good.  That is really what we are after: no surprises. Everybody knows what is going on. All these systems are very well tested.

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/860.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=860)

### Operational Principles and Future Innovations in Market Infrastructure

There is a bunch of patterns here that you can use with your own systems, whether they are on outpost or whether you are deploying in region with more traditional deployments. So again, the review: we have outposts in our data center. We are analyzing the whole system with this  fault domain concept so that we understand which components live together and have their shared fate on the same server, or same rack or same building or same region or so forth. It is very careful planning there.

We test for static stability and we design for static stability. That makes sure that we can continue running even if there are issues. We do not have to scale the system up or do any of those things. And then again, disconnected operation is just where we are able to keep going even if those lines are cut. That is really the only item here that is specific to outpost. You do not think about disconnected operation if you are in a part of a region, but for these systems,

if we cut over to DR, that is a significant event. It's a reportable event to the regulators. All of our customers are upset, and we have to explain ourselves. We go to great lengths to avoid cutting over to DR. We have a fully functioning DR system and we can definitely run there. We've tested it, but we go to great lengths to not do that. That's probably the biggest difference between a regular workload and something we're running on these ultra-low latency outposts—that extra step in case of more issues.

In general, we run multi-region and multi-AZ in the primary, at least if not in both the primary and the DR. We run hot everywhere. It's not rocket science, but it's a lot of really diligent work and being very careful about dependencies, about failure modes, and about understanding how your system is going to respond to any sort of issue, whether that's man-made or a flood or whatever.

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/970.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=970)

Let's talk about where we're going in the future.  We want to do a little more dynamic deployments. I had talked about how we lay out A and B components on different racks and that sort of thing. We actually plan all of these deployments on outpost down to the individual CPU cores on every single machine. We do this for both resiliency purposes—to make sure that if a server goes down, we don't take the A and B copy of something out—but we also do it so that we're not overloading the PCI bus on a particular machine. We do a lot of performance balancing as well as resiliency balancing.

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/1030.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=1030)

We want to get to where we can do that with a lot more rules-based setups, where we do affinity and anti-affinity patterns and those sorts of things. But we're not quite there yet with the performance side. That's one thing that we're really interested in doing. As I said, we're not scaling these systems up and down during the day, but as we move markets into regular public cloud, that is something we want to be able to do.  These are all stepping stones on our way to being able to run those markets in public cloud.

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/1050.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=1050)

The other thing we're experimenting with is Graviton. We've had really good luck with Graviton 4. We're working very tightly with the Amazon team on the next generation of those CPUs and testing them in outpost and ultra-low latency network cards for Graviton and a number of other advancements there.  Then there's accelerated compute. I didn't really talk about that in this session, but we use FPGA in a number of different systems. We're experimenting with GPUs for real-time risk analysis in markets and that sort of thing. We want to be able to bring more of those components into the outpost.

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/1090.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=1090)

The other angle on that is that we're selling these systems externally to third parties. We want to be able to offer them a more compelling offering with more functionality and more services. Custom silicon, FPGAs, and GPUs are all very much part of our future vision.  And then of course, more services. I talked about how we're very careful about what services we run on that outpost. EKS local clusters is really the next one that we're looking at, and that's toward the dynamic deployment as well as just an easier way to manage things.

[![Thumbnail 1130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/63d8de8db90b3ccd/1130.jpg)](https://www.youtube.com/watch?v=-GuZhm4XGxM&t=1130)

We're very careful about adding more services. We do those deep dives with the service teams and understand failure modes, but we want to continue to up our game on service availability and on the functionality that those services are able to offer in an outpost in these sort of constrained, very critical systems. That's kind of where we're going and where we've been.  Thank you. Fill out the form and have a good re:Invent.


----

; This article is entirely auto-generated using Amazon Bedrock.
