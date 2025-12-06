---
title: 'AWS re:Invent 2025 - AWS RTB Fabric: Real-time bidding workloads with your AdTech partners (IND379)'
published: true
description: 'In this video, Sergio Serra from AWS introduces RTB Fabric, a purpose-built service for real-time bidding workloads that addresses cost and latency challenges in AdTech. The presentation covers key industry challenges including partner discovery, integration complexity, and cost optimization. Fernando Gamero demonstrates the architecture featuring RTB gateways (requester/responder), links for connectivity, and modules for traffic management. The service enables DSP-SSP integration in hours versus weeks, delivers sub-10 millisecond latency, and promises up to 80% cost savings. Duc Chau from Yieldmo shares production results showing reduced auction timeouts, increased bid rates, and improved integration efficiency. The session emphasizes RTB Fabric''s ability to create a composable AdTech ecosystem with simplified third-party integrations and volume-based pricing.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - AWS RTB Fabric: Real-time bidding workloads with your AdTech partners (IND379)**

> In this video, Sergio Serra from AWS introduces RTB Fabric, a purpose-built service for real-time bidding workloads that addresses cost and latency challenges in AdTech. The presentation covers key industry challenges including partner discovery, integration complexity, and cost optimization. Fernando Gamero demonstrates the architecture featuring RTB gateways (requester/responder), links for connectivity, and modules for traffic management. The service enables DSP-SSP integration in hours versus weeks, delivers sub-10 millisecond latency, and promises up to 80% cost savings. Duc Chau from Yieldmo shares production results showing reduced auction timeouts, increased bid rates, and improved integration efficiency. The session emphasizes RTB Fabric's ability to create a composable AdTech ecosystem with simplified third-party integrations and volume-based pricing.

{% youtube https://www.youtube.com/watch?v=imnB4Cmob4g %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/0.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=0)

### Introduction: Addressing RTB Workload Challenges with AWS RTB Fabric

 All right, am I audible? Great. How many of you believe that cloud computing is too expensive for RTB workloads, and how many of you believe that the latency that the clouds deliver is not necessarily at the level that RTB workloads need? Hi everybody, my name is Sergio Serra. I'm an AdTech veteran, and I recently joined AWS to lead RTB Fabric, a purpose-built service for RTB workloads that will help address these two challenges and these two questions I just asked, along with many more challenges that are endemic to the AdTech ecosystem.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/80.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=80)

Let's start with taking a look at the agenda today and what we will be covering. First of all, today I'm actually joined in this presentation with Fernando Gamero, our Senior Solutions Architect, and Duc Chau, the Chief Technology Officer of one of our beloved customers, Yieldmo.  Just jumping right into the agenda, I will start with listing down and distilling the key challenges of the AdTech industry, and then Fernando will help dive deeper into the intricacies and architecture of RTB Fabric. Then we will try to understand how to onboard into the service. You will see how easy it is actually. We'll look at the capability of the service in terms of observability, in terms of metrics and telemetry, and then we will go to probably what I believe to be the hottest topic at the moment, which is how we can help you release those cost savings that RTB Fabric is supposed to help drive.

Then we will hand it over to Duc, because at AWS and Amazon in general we are customer obsessed and we really believe that there is no better way to talk about our products than having our customer talking about the experience they've been having with the product itself. And finally I'm going to take it back and just drive some insights and key learnings that we went through today.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/150.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=150)

### Endemic Challenges in the AdTech Ecosystem: From Discovery to Cost Optimization

 With that, let's start with the challenges that are endemic to the AdTech ecosystem today. Starting from the top left, you can see that the very first natural problem is for demand and supply to discover each other. You are a DSP, you want to know what Supply-Side Platforms, you want to know what publishers are available, and if you are an SSP on the other end, you want to know what DSPs are specialized for your potential supply, especially if you are not an omni-channel SSP. You want to go by vertical, you want to focus, for instance, on performance versus brand demand or in-app versus CTV or other channels.

Once you realize who the partners are and you decide to peer with them, the next challenge is to go into integration, and the integration, of course, is based on the legal and business bit as well as the integration from the technical standpoint. And speaking about the technical standpoint, we have learned from our customers that a new integration can take anywhere from three to four weeks to actually five months depending on the size of the platform that you're trying to integrate.

Once the integration is done, the other problem of course is how do I make sure this integration delivers the latency we need? How do I make sure I co-locate? I go as close as possible to my partner's workload. Another problem that is very endemic to the industry, and I know this from firsthand in my previous experience, is transparency. We are an industry that moved from second-price to first-price because of the black box when someone, some publisher sends a bid. They don't know the response that is coming back and what processing happened behind the scenes. The same thing actually holds true for the DSP. They submit their bid and they don't know what's going to happen. And for the SSPs, if they are reselling again, they have no visibility into what happened in between.

This leads also to data fragmentation, which is a problem that is endemic to any two-sided marketplace like this one. You have demand that knows perfectly well what are the campaigns, what is the demand for specific advertisers, but they don't know how the supply is going to behave. And the supply side knows very well about their users, about their placement where the ad will be rendered, but they don't understand the demand landscape necessarily.

Finally, we have a theme on models optimization.

You might think that sometimes DSPs do the hardest work when it comes to machine learning and artificial intelligence, which is of course true when you think about finding the right ML model for improving your roles when you talk about budget capping, frequency capping, and budget pacing. But on the SSP side, there is also an equally important problem when it comes to yield optimization, such as dynamic margins, dynamic floors, and ad quality. So ML and model optimization is a problem that is common across the marketplace, both demand and supply.

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/360.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=360)

But the very common denominator among all the tech companies is cost optimization. In fact, the lingua franca when you talk to any tech leader, especially on the technical side, is what is your cost per billion. And so that's exactly why we decided to come up with a new service, which is the first  purpose-built service for real-time auctions called AWS RTB Fabric. And the service really allows you to achieve many benefits for your RTB workload.

### RTB Fabric Overview: Key Benefits and Core Components

Starting from the top, first of all, you can connect no longer in a matter of weeks or months but actually hours. And you can connect with new DSPs and new SSPs, but more importantly, you can now co-locate right away with third-party vendors, so third-party applications, and we will dive a little bit deeper into how this is achieved through RTB Fabric. But the second thing that is probably the most important for many of you is RTB Fabric allows you to really save up to 80% when it comes to typical cloud infrastructure costs and networking costs. And so this is of course a huge benefit that democratizes access to hyperscale for small DSPs and small SSPs too and helps improve the bottom line for the big ones as well.

All of this is also achieved in milliseconds. When you talk about RTB Fabric, keep in mind it is a regionalized service, although we allow communication to the public internet as well, so it's not only for companies that are within the RTB Fabric service, but you can also talk to your DSPs that are outside on the public internet, or your SSPs if you're on the other side of the equation. But when it comes to third-party integrations, that's where we introduced a new, very novel concept which we call modules.

Modules are really applications that run within the network path, so they are ultra-fast and can be built by either AWS, can be built by you, so SSPs or DSPs, or third-party vendors that work on data enrichment, work on identity addressability, and all the business logic that is helpful for RTB workloads. So specifically, these are container images that are deployed within your RTB Fabric gateway, and so by doing so, we are able to move the latency from milliseconds really to microseconds. So in fact, if you look at our P99, we are really talking about 500 to 550 microseconds when it comes to talking to modules.

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/510.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=510)

 I will just cover one last slide before handing it over to Fernando, just going through the key components of RTB Fabric. On the top left, you find the RTB gateway. So keep in mind that we try to abstract away the jargon of ad tech when it comes to bidder, DSP, SSP, publisher, buyer, and seller. So for as much as we are concerned, we have a responder that responds back with a bid response to a bid request, and we have a requester that sends a bid request to another responder. And so you have an RTB gateway as a requester and an RTB gateway as a responder.

If you are acting as a responder, specifically for the second category, so if you are a responder, the beauty of the service is that it also comes bundled with a very tiny layer of load balancing. We do DNS traffic distribution, and by doing so we can be extra lean and extra cheap, and of course by lowering the cost, we can reflect this upside into the price, into a lower price for our customers. And so again, if you are a DSP or if you are a bidder, or if you are even an SSP receiving traffic from publishers, you don't need to use another self-managed or managed load balancer. You can just use RTB Fabric to process all your incoming traffic.

And then finally, as I mentioned, we have advanced traffic management modules. These are the modules that are built by AWS and come bundled with the price.

Specifically, we have an OpenRTB filter. So if you are receiving supply from a requester, you can decide to filter requests based on any selector of the OpenRTB path. The good thing is that you can decide to filter those requests before they even hit your fleet, saving a bunch of CPU cycles. The other module that is offered bundled into the service by RTB Fabric, so included in the price, is a rate limiter that allows you to set a specific limit of QPS you want to achieve from each one of the links you have. So each one of the SSPs requesters that are sending requests to you. And the last one is error masking, which really allows you during deployment, if you are terminating some instance or recycling instances, you don't really want to have spikes in latency that your SSPs will see or returning HTTP status codes because we know SSPs very often use circuit breakers, so you don't want to have fluctuation in the incoming supply.

We allow you to instead wrap all those errors into a generic 204, which is really a no bid, and so the temporary downtime can be covered by a no bid, and then whenever you are back ready, you will not see any fluctuation in traffic. With that, I think it's time to dive a little bit deeper into the architecture of RTB Fabric, and I would like to hand it over to Fernando.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/720.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=720)

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/730.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=730)

### Understanding RTB Fabric Architecture: Gateways, Links, and Modules

Thanks Sergio. Hello everybody, I'm Fernando. As you mentioned, I'm a Solutions Architect within AWS. I've been working quite closely with the team, helping to look into the service and get it onboarded with customers and that sort of thing. Now that we've covered the  basics, let's dive deep on what the RTB Fabric architecture looks like, covering those components in a bit more detail, but first.  There's a bit of hands up. How many of you are dealing with programmatic advertisement, onboarded SSPs, DSPs? Cool. So we have a fair few of you.

So for those less familiar with AdTech programmatic, let's cover a bit the standard setup that we have here. So on the left-hand side, we can see the publishers, which basically are our digital advertisement in the ad time and space. They are always on the lookout for best partners, best deals, cost-effective platforms where they can actually get these ads from. And they are looking for affordable subscriptions and those advertisers or that chain of ad campaigns that is giving them the best back for the space that they are putting out there. On the other hand, we have the ad agencies, and these are the companies that are managing the launch and the distribution of those campaigns.

And in the middle, we have this fabric. We have a supply side, we have a demand side. And the idea is to be able to connect us to in a very easy way, and that's what we are building the RTB Fabric for. SSPs will help you to connect with the publishers, and DSPs connect the ad agencies, and the idea is to build this ecosystem in an easy way. So that's where the RTB Fabric comes to play, and we can see that as leveraging this ecosystem within AWS to help that onboarding.

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/840.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=840)

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/860.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=860)

So  we covered the first component is the gateways. As Sergio mentioned, the gateways is the core networking feature that we have in RTB Fabric. You will see that there are two types of gateways. We have the requester, which connects to the supply, and we have the responder, which is connecting to the demand side. Now, the connectivity  between both gateways that sit in each of the different partners is done through what we call the link. A link is just basically a network path that we create between the two gateways.

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/880.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=880)

And then on top of that,  we apply the modules. As Sergio mentioned, this is a management component that can configurably be attached to the links, and they run in line, so they are not adding extra latency to this, and they help you to do filtering, they help you to optimize and even control the RTB traffic.

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/920.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=920)

Now because this implies that you are sitting within AWS in the same region, it means that you would need to have those partners co-located. But we also have the capability to allow  both the supply and the demand to communicate to external partners, and this we do with what we call the external links. We'll cover those more in detail, but let's go deep into each of these components and how they look like.

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/940.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=940)

### Deep Dive into Gateway Components and Security Features

So let's start with the requester gateway. The requester gateway,  in reality we try to make the least amount of changes to your applications. So you still have your SSP applications still receiving traffic from your publishers, but instead of having to leverage traditionally a VPC endpoint or a gateway which can be very costly and very expensive, the idea is that instead of doing that, we are going to onboard through the RTB Fabric requester gateway. That's going to help us to create a network interface. That network interface is created in your own account and allows that communication between the SSP application and the gateway in milliseconds, providing really fast connectivity with very low latency, which is what we want to achieve.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/990.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=990)

On the other hand, when we are looking at the responder gateway,  exactly the same topology applies here where we have a DSP application. Traditionally you will have a public endpoint like an application load balancer, or even if you are doing something internally or something like Cloud Map to lower that cost associated to the load balancer, you still have to maintain that. So we don't want to make changes to that necessarily. The idea again is the same. We onboard a responder gateway, we create a network interface in your account, and then we enable that traffic to go through, but we still take advantage of your load balancers in this case.

[![Thumbnail 1040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1040.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1040)

Right, so once you have these gateways, we're talking about  how we create that connectivity between them, and this is where the links come to play. These are the core connectivity mechanism that allows you to establish the connections between that supply and the demand partners in the ecosystem. We talk about internal links, standard links, and they basically help you to connect when both partners are in AWS, onboarded to RTB Fabric, and in the same region. The links need to be initiated by one of the partners, and the other partner needs to accept it, so there is no automatic discovery. You need to exchange those IDs to be able to do so. But once they are created, they will provide a private and secure connection between the two partners. It all happens within the RTB Fabric service. The traffic will flow through this link in a single millisecond because, as Sergio mentioned, we are taking advantage of this DNS-based balancing capability. And they are also very scalable. They will support millions of queries per second as a throughput, so you can really scale it up. And on top of that, we will attach the modules, but we'll cover that next.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1120.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1120)

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1150.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1150)

 Remember that we mentioned external links. So let me introduce you to the concept of the external outbound links. This is when we have the need for a requester gateway sitting in RTB Fabric to talk to a DSP that lives outside a different region, still in AWS potentially, or in AWS but not onboarded to the RTB Fabric, or even altogether outside AWS. And similarly we have the external inbound links,  and that will help you to get connectivity from your supply partners that are not in AWS, connecting through the responder gateway and talking to your DSP application.

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1170.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1170)

Right, so let's move on now. Let's cover the modules. As we mentioned, the modules are configurable  traffic management components, and they will help you to bring your own applications, potentially bring your partner applications securely in a compute environment that is then used by the real-time bidding. They run in line, so we are trying to minimize the amount of throughput or latency that is added by them, and they help you to define whatever logic you want to initially filter, optimize, and control that RTB traffic.

[![Thumbnail 1220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1220.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1220)

These modules support containerized applications, and eventually you can bring even your machine learning models or your generative AI agents to do additional enhancement of those transactions. As mentioned earlier,  modules are attached to the links. In reality, they are not standalone services. They operate in line within the link infrastructure between the requesters and the responders, but they are attached to each of the sites. So each partner can create its own set of modules or attach its own set of modules.

[![Thumbnail 1250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1250.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1250)

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1260.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1260)

As mentioned earlier, at launch we released three built-in modules,  and these are the rate limiter, the OpenRTB filter, and the error masking modules that are going to help you protect those workloads.  If you remember when I was describing the RTB Fabric responder gateway, we were talking about still having that load balancer there between the gateway and the DSP application. But that's still going to incur some cost on the load balancer.

[![Thumbnail 1310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1310.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1310)

Now, if you're running your DSP application on an Amazon EKS cluster, for example, or you are deploying into EC2 instances but leveraging EC2 Auto Scaling groups, we can take advantage of that and allow you to completely remove the need for that load balancer. That's what we call the managed endpoints. So the idea behind the managed endpoints is that  instead of having to specify the domain where the traffic is going to flow to for that load balancer, we have the capability to query either through the EKS endpoint or the Auto Scaling group what are the IP addresses of the nodes or the instances that are part of your cluster. Then we record that in a private hosted zone in Route 53, and we use that. The responder gateway uses that to balance the traffic across all your nodes, effectively removing that need for having that load balancer, simplifying your architecture, reducing your cost, and still maintaining that single-digit millisecond latency.

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1360.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1360)

Security is always paramount at AWS.  It's a top priority for us. Hence, we had to make sure that the RTB Fabric was built to ensure that the traffic is private and encrypted. When you create a requester gateway, for example, you're always going to get an HTTPS endpoint. All the traffic that happens between the requester and the responder gateway is secure and private. But that doesn't mean that your responder applications also need to be listening on HTTPS.

We support you to have the last mile, call it the last mile, to be unencrypted to support HTTP. Now, if you still want to do that end-to-end encryption, that's totally supported. But you need to maintain your fleet of certificates on the nodes because now we are not terminating on a load balancer, especially when you're using a managed endpoint, so you have to have those TLS certificates on the node. And if you're using a private CA, and this is quite important, we need that CA certificate because the RTB Fabric gateway is going to do a proper TLS validation, so it's quite important to keep that in mind.

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1440.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1440)

### Three Simple Steps to Onboard: Creating Gateways, Establishing Links, and Routing Traffic

As we mentioned  earlier, and Sergio covered earlier, it's very time-consuming to onboard, especially when you have different partners that have to spend weeks or months to do that integration. They might have very complex network setups as well, even if they are in AWS, and there is a high operational overhead. So let's cover how you can onboard to RTB Fabric with three very simple steps, which is going to help you create those gateways, create the links, and then just start routing the traffic and start taking benefits immediately for those partners that you are onboarding.

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1480.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1480)

 So let's start from a scenario where we have an SSP that is in AWS and a DSP that is also in AWS, both in the same region, but they're all talking over the internet. We're going to cover how to onboard those ones. And additionally, we're going to cover how to start communicating with external partners as well, and you'll see how easy it is to do so.

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1510.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1510)

Both the DSPs and the  SSPs need to create a gateway within the RTB Fabric. Now this can happen in parallel. There is no sequence here. I don't need to tell the SSP you need to create the gateway before the DSP creates the gateway. But for the presentation, let's cover this in sequence.

[![Thumbnail 1530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1530.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1530)

 So the first thing that we have to do when we have this requester, or the need for the SSP to create the requester gateway, is to identify the VPC and the subnets where the SSP application lives. We want to ensure that the network interfaces are created on those subnets so we can take advantage of that VPC connectivity. We want to create a security group because we always want to attach a security group to the network interfaces that we are creating to allow traffic coming from the SSP application into the fabric. Ultimately what we get is a domain name. This domain name is going to be important because it's the one that we are going to be using to instruct the SSP application to start forwarding traffic to the RTB Fabric.

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1580.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1580)

[![Thumbnail 1590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1590.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1590)

 Similarly, the DSP is going to create their own gateway that's just purely for the responder side.  But here we have a bit more information to provide. We still need that VPC, we still need that subnet information, but we want to know the protocol that the DSP application is listening on, what port it is listening on as well, and also the domain name. Remember we're saying for those managed endpoints we need to know the DNS name for the load balancer, for example, that it needs to point to. So we provide that information. Again, we also need a security group to be created.

[![Thumbnail 1630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1630.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1630)

At this point in time, once both parties have created the gateways,  they're ready to share that information with each other, so they have to share the IDs of the gateways that they've created, and then we can start creating the links. Again, in this case, we are going to get the requester to create the link and the responder to accept the link. One key point to make is you don't need to create a requester or a responder gateway for every link that you create. Gateways support multiple links.

[![Thumbnail 1660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1660.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1660)

 So what we need to create that link is for the responder gateway in this case to share the ID, and the requester then just creates the link using the peer gateway ID as the responder ID. Now, by default, when you create a link, all the traffic is going to be encrypted end to end. Again, we want to have the best security posture here. But if the responder application is listening on HTTP, on link creation we need to be explicit that we want to be able to use HTTP. Again, it's an opt-in, explicit opt-in that you're on purpose saying, hey, the last mile is going to be unencrypted.

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1710.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1710)

[![Thumbnail 1730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1730.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1730)

Once the link is created, the requester,  the SSP, is going to say, hey, this is the link I created, now go and accept it. So the responder goes and accepts the link. At this point we're ready to start sending traffic through the RTB Fabric. So all we need to do  is to change that endpoint that was configured on the SSP application and say now we don't want to use that endpoint that was directly talking to that DSP. Now we go through the RTB Fabric, so we build the URL. The URL is very basic. You take the gateway domain name that we got on the requester, we attach the link ID, and then we continue using the same path that was attached at the end of the request. So that's simple.

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1770.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1770)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1790.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1790)

Once we have traffic flowing, then you can start deciding which modules you want to  attach. Again, each side has the capability to attach modules that fit best to their requirements, and each of them are independent. In this case, for example, we are attaching an RTB Fabric OpenRTB filter  that is helping us to only accept traffic that has the US dollar as the currency specified. But as you can see it's attached on the gateway ID that is associated to the requester gateway.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1820.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1820)

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1830.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1830)

On the requester gateway, you can have more than one module attached to each side, and you can also build dependencies between those modules. So if you want the modules to be executed in a particular order, you can specify that as part of the CLI command. Similarly,  the responder can also attach modules to their side. In this case, all we are doing is attaching  a rate limiter, which is basically saying, okay, I'm only going to be accepting five requests per second. When the gateway drops that request because it's exceeding that rate, as Sergio mentioned, we are going to be getting a no bid response, so we don't really drop it. You still, on the requester side, on the supply side, get a response back. It's a no bid response, so cool.

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1860.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1860)

[![Thumbnail 1890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1890.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1890)

So we've seen now how a DSP and an SSP can integrate  when they are in the same region and both in AWS. But we mentioned earlier that we also support partners that are not in AWS or not integrated to the RTB Fabric. So in this case, we want to create an external outbound link so that the SSP that is onboarded now can talk to a DSP that is not in AWS. So we want to create an external outbound link. By doing so, all we need to do is to specify the public  endpoint of that DSP that we want to now send traffic through, and that's as simple as that. We just create that, and ultimately when you are sending the request to the requester, you just use that link ID and it knows that it needs to send that externally back.

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1920.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1920)

[![Thumbnail 1930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1930.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1930)

[![Thumbnail 1940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1940.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1940)

Similarly, we can also leverage the RTB Fabric for traffic coming to DSPs that have been onboarded.  So in this case, what we do is we create an external inbound link. Here we don't really need to provide any information because when we create the external  inbound link, what we are doing is creating a domain name for you to share with your SSP partner so they can send traffic to you.  All right, cool.

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/1960.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=1960)

### Observability, Cost Model, and Performance Metrics of RTB Fabric

So we've covered implementation. We've covered the details of the components. So let's talk a bit about observability. What do you get with RTB Fabric? So first, let's look into logging. We are using CloudWatch log delivery.  But it's quite important to keep in mind that logging is configured at the link, so we get a lot of information from the link. And because of the nature of RTB traffic where the volume is really high, we're expecting in the order of billions of queries per day, for example, it's quite important to be able to configure sampling for errors or filter logs that we create. Now we don't really use CloudWatch Logs directly. We use CloudWatch log delivery, so that means that you'll be able to send your logs either through CloudWatch Logs, Amazon S3, or Kinesis Firehose. But this is not enabled by default. So if you create a link, you're going to need to configure the log delivery if you want those logs in your account.

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2010.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2010)

Now with the metrics, it's slightly different. Metrics are on by default,  and the metrics are also delivered to your account. So you get metrics for each of the generated links, and for those metrics we have what we call dimensions. So you're going to get information about the HTTP status code. So every request that is coming, like a 200, 202, 204, a 500, you get that information on the metrics for all the traffic going through. But you also get the statistics, so you can see P90, P99 traffic going through the RTB Fabric. Those are metrics that we also get delivered to your account, and you also get raw link traffic, so you're going to get information like total requests, latency, and so forth. And because CloudWatch Metrics are integrated natively with your account, you'll be able to build your own dashboards and create your own alarms so you can respond to problems as they occur.

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2080.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2080)

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2100.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2100)

Cool. So  we've covered now observability as well as dived deep into the components, dived deep into the onboarding with those three simple steps, but now to complete the picture, let's focus on the cost and performance because this is where it's going to be super important. So  for the RTB Fabric, we've built the price model to align with RTB economics.

We are focusing on high volume of transactions, so this is quite important. As a result, you don't pay for the RTB Fabric gateways as you create them. You're only paying for all the requests that you send, not even the ones that you receive, so there is no upfront commitment. You pay for the number of RTB requests or responses that you send monthly on a price per million basis.

Because there are two types of links covered, internal and external or external and external, the prices are going to vary based on the traffic that you send and based on the type of link that you use. So that's another dimension that you need to take into account. But we will apply a volume-based tiering discount, so the price will differ for your first two trillion transactions and then anything above those two trillion transactions per month to support the needs as you scale up on RTB Fabric.

Now there is a standard size for the RTB transactions included in the transaction price, and with each RTB transaction we include two kilobytes of data for free. When the payload for the response or the request exceeds those two kilobytes, then you're going to be paying for those transactions, that extra payload on a price per gigabyte basis. Now there is also a fee for the no-bid transactions, but that's at a much lower rate than for the bid transactions.

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2210.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2210)

 When it comes to performance, you can really rely on RTB Fabric to deliver. You're going to get consistent single-digit millisecond latency even with millions of queries per second, so that's pretty impressive. Consider a traditional integration where you often see 50 to 100 millisecond latency. RTB Fabric can help you to deliver that sub-10 milliseconds consistently. And also, RTB Fabric is available across multiple regions, so you want to place your workloads close to your partners. We're reaching for global scale here.

So far you've seen the architecture, you've seen the onboarding process, you've seen the benefits as well from performance and cost. But let's hear from a customer that has experienced this firsthand. I'm going to invite Duc Chau to join me on the stage. Yieldmo is one of the first customers that we onboarded, and they've been running production traffic and they've seen measurable improvements across their business. So he's going to share with you some real results, not just the promises of what I've been talking about, but actual production data. So Duc, over to you.

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2310.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2310)

### Yieldmo's Success Story and Key Takeaways: Building the Foundation for AdTech Innovation

Thank you, Fernando. Hi everyone, I'm Duc Chau. Today I'm going to talk about how RTB Fabric has helped us create a win-win-win across our partners.  It might look simple on paper, serving the right ad to the right person at the right time with the right creative, but in practice, it's much more complicated than that. I've been doing AdTech since 1999. I've seen a lot of trends come and go. Some became standards, OpenRTB, Prebid. Some fade away, Flash, a few other things. But the one common thread amongst the ideas that became a standard was that it was built on a solid infrastructure, a solid foundation, and that's what RTB Fabric promises us and that's why I'm excited to see RTB Fabric evolve.

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2350.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2350)

 RTB Fabric allowed our demand partners to see more bids, allowing them to have more opportunities to deliver outcomes for their customers. For us, it improved bid efficiencies in the form of lower timeouts and increased win rates, resulting in higher revenue. For the supply side, they get an increase in yield, ultimately being able to deliver better advertising experiences to their customers, creating this flywheel to drive their customers back to their products and their services.

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2390.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2390)

 When people talk about programmatic or real-time bidding, they like to anchor on what I call above the bid. So let's kind of talk about the concept of above the bid and below the bid.

Above the bid is generally things like how you serve an ad, how you match an ad, security, privacy, and all the various optimization techniques you apply to creative as well as optimization are important. But you can't do any of that without what's under the bid, which is a reliable transport layer. If you have an inefficient transport layer, cost compounds and latency compounds. So you have to have this solid foundation for us to do all the things we want to do above the bid. That's the promise of RTB Fabric.

[![Thumbnail 2440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2440.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2440)

Let's talk about some of the results that we've seen as early customers of RTB Fabric.  We've seen auction timeouts reduce, we've seen bid rates increase. I'm most excited about the last two, a decrease in cost because it's running across one of the largest private networks in the world. And I'm also extremely excited about the integration time. We've seen the process go from weeks to days. The example I like to use is it's very similar to a LinkedIn request or a friend request on Friendster, MySpace, Facebook, you name it. So this process of going back and forth in a very laborious process has now been made very self-service and very asynchronous. That's extremely important because it allows innovation to happen, and it removes the burden of custom integrations.

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2500.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2500)

Why does this matter to the ecosystem?  We're all in this room because we want to deliver ad experiences that are meaningful to the customers. With a solid foundation, we increase market liquidity, we optimize our costs, and we also futureproof the tech stack for the ad serving ecosystem. And that piece is not a small feat. I think composable AdTech components is where the world's heading, and I think RTB Fabric is the start of that.

So think of the world before AWS. If you had to do something, you would have to stand up a server, update a load balancer, update DNS entries. Now we don't think about it because it just works. RTB Fabric's vision is very similar to that for AdTech. All the technologies that you're building today that require very custom solutions will likely go away, whether you need to write a throttling system, whether you need to instrument additional data on top of the bid stream, or whether you just need to support different versions of OpenRTB. Think of a world where that just becomes easy because you have a solid foundation to do that on. It allows you the bandwidth to think about other stuff, such as new business models, new partners to connect with, new innovations, because it just works.

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2590.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2590)

 Fast is good, but predictable is better. And I'm excited to see the innovations that are going to come out of this process as people start adopting RTB Fabric and all of the infrastructure that it's going to be deployed to the AWS ecosystem. So with that, I'm going to hand it back to Sergio to kind of take us home.

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2630.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2630)

Thanks Luke. This was amazing. I mean, it's always great to really hear the feedback from our customers, and we look forward  to hearing your experience as soon as you start playing around with RTB Fabric and releasing these cost savings and latency efficiencies. I think we learned a lot today, and so just to summarize, we learned first of all that getting into RTB Fabric is extremely easy. It really takes you a bunch of CLI commands, and specifically three, to get on board, to be in the system with your own RTB gateway.

And then with that gateway, you are able to communicate ultra fast within the RTB Fabric ecosystem, but you can also use your RTB gateway to talk to external partners. If you are an SSP, you can send requests to external links, which again are really one-on-one relationships with your own DSPs that sit outside AWS. You can still do that. It's still going to be more cost efficient than public pricing and the majority of private agreements. But if you stay within RTB Fabric, of course the synergies go up because you have much lower latency, much lower cost.

So look at RTB Fabric as the foundational layer for incrementing synergies between AWS and your partners that will be in AWS. So the more partners come to AWS, the more RTB workloads are on AWS, the more efficient you will become as an RTB platform. I think the second interesting thing, actually I would say the third interesting thing that we discussed today, is the concept of modules. We have modules that are built in and bundled into the price as we discussed, super helpful for responders in order to process requests before they even reach their own fleet, but helpful for the requesters, especially when you think about what's next in the future.

We are working with many independent software vendors to deliver their own applications that today RTB platforms integrated with in a very custom way. Everyone has their own API, everyone has their own latency SLAs, co-location is a nightmare. We take all of this away. We just allow you to integrate with these third parties with a button, literally one click. You attach their application into your RTB gateway, which again is a single tenant cluster that is co-located with you, so latency becomes extremely low.

Cost of transferring data doesn't exist anymore with third party applications, and again, you just become more efficient, even from an operational standpoint. So really, I think my personal opinion on RTB Fabric is that it allows you to buy velocity, business velocity. If you want to be fast in integrating with new partners, with taking all the new ad tech trends into account and integrating them into your stack, I think it becomes a no-brainer because this is the platform that allows you to build, react, and win the game.

[![Thumbnail 2860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bd7ff0d5aefbf44b/2860.jpg)](https://www.youtube.com/watch?v=imnB4Cmob4g&t=2860)

With that, I would like to thank everybody for joining this session, and please feel free to take a survey. We love your opinion. We can get better, but more importantly, do check out the RTB Fabric official website and start playing around with the service, start realizing those synergies and those cost efficiencies that we just spoke about. If you have any questions, please do talk to your  account managers and reach out to us. With that, thank you so much and looking forward to new customer feedback.


----

; This article is entirely auto-generated using Amazon Bedrock.
