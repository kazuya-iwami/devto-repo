---
title: 'AWS re:Invent 2025 - Secure, Easy and Performant VPC Resource Connectivity with Tailscale (CDN201)'
published: true
description: 'In this video, Lee from Tailscale presents how their WireGuard-based solution simplifies VPC resource connectivity compared to traditional VPNs. He addresses common pain points like VPC peering complexity, poor developer experience, and high costs. Tailscale uses identity-based mesh networking instead of hub-and-spoke topology, requiring no firewall changes or open ports. Key use cases include developer access to private AWS resources, CI/CD pipelines via GitHub Actions, cross-cloud connectivity, and EKS control plane security. Real-world results show Cribl scaled 25x with no dedicated IT, Hugging Face saved 240 hours annually, and customers experienced 4x latency reduction and 22x throughput increase. The presentation includes a free personal tier offer and three-month starter tier promotion.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/0.jpg'
series: ''
canonical_url: null
id: 3088452
date: '2025-12-06T08:30:26Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Secure, Easy and Performant VPC Resource Connectivity with Tailscale (CDN201)**

> In this video, Lee from Tailscale presents how their WireGuard-based solution simplifies VPC resource connectivity compared to traditional VPNs. He addresses common pain points like VPC peering complexity, poor developer experience, and high costs. Tailscale uses identity-based mesh networking instead of hub-and-spoke topology, requiring no firewall changes or open ports. Key use cases include developer access to private AWS resources, CI/CD pipelines via GitHub Actions, cross-cloud connectivity, and EKS control plane security. Real-world results show Cribl scaled 25x with no dedicated IT, Hugging Face saved 240 hours annually, and customers experienced 4x latency reduction and 22x throughput increase. The presentation includes a free personal tier offer and three-month starter tier promotion.

{% youtube https://www.youtube.com/watch?v=mY5meE9mroY %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/0.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=0)

### The Pain Points of Traditional VPC Connectivity and VPN Solutions

 All right, everybody here in Theater 3. Happening now: Secure, Easy, and Performant VPC Resource Connectivity with Tailscale. Take it away, Lee. Thank you very much. As just mentioned, I'm here to talk to you about Tailscale, secure, easy, and performant VPC resource connectivity with Tailscale.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/40.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=40)

Now, two words of warning. This is a sponsored presentation, and I do work in sales, so if you feel compelled to part ways with your hard-earned money as a result of this presentation, that means it's gone well. I think I can make a pretty compelling argument for why you should do that anyway, but let's talk a little bit about the agenda.  The pain of connecting to private VPC resources. There are many, many ways to solve this problem. You likely have this problem already, but you've probably experienced some pain at some point here. I'm going to talk about how Tailscale does things a little bit differently.

I'm going to talk about the power of WireGuard, which underpins Tailscale's connectivity solution. I'm going to talk about some use cases. I'm going to talk about how you can do it right and how you can do it wrong, and then you'll see that it says demo on here. This is Las Vegas. I was out too late last night, so we're not getting a demo today.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/70.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=70)

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/80.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=80)

 So what does private VPC resource access look like today? Well, what we have found, or what I have found prior to being a Solutions Engineer—I was a System Administrator, DevOps Engineer, Platform  Engineer, whatever the title is this week—what we generally found is that actually connecting to things that are inside your VPC, that are in a private subnet, that are not exposed to the public internet, is fraught with complexity. If you want to connect two VPCs together, you're immediately in VPC peering pain or you have to deal with transit gateways.

You have limited visibility and control. Sure, you can send all these things to CloudWatch and CloudTrail, but does anybody ever actually look at those things? The costs of doing these things are immediately painful. AWS announced regional NAT gateways, that's a great change, but every single byte or every single packet that goes through those gets charged, and that's an expensive operation.

And then finally, and most importantly, I'd imagine everybody here who's had any technology-based background has used a VPN at some point in their lives, and they've probably had to reboot their laptop because the VPN has gone wonky or caused you some level of frustration. These are things that we hear about at Tailscale all day. Every single time we talk to a new company, they have an existing connectivity solution and they say the developer experience is really, really poor. It's impacting my productivity. It's making things really, really painful.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/150.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=150)

 The complexity can be kind of described a little bit like this. This is a nice visualization of a pretty typical VPN or connectivity topology. You might have some sort of VPN hub, and then you might have a gateway in another region or a gateway in another VPC. It's very limited to the network model that you have originally ascribed to. If you have a problem at some point in this path, if a developer is trying to get into an RDS database or a developer is trying to get to something that's only an internal resource and they can't achieve that even though they're on the VPN, they don't have any idea how to solve that problem.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/200.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=200)

That's immediately an IT ticket or it's somebody else who's going to have to solve that problem for them. We hear all the time how painful and frustrating that is, and the topology here is an artifact of that.  So a lot of these VPNs were built on technology that was designed for a day when your work machine was under your desk and was not a portable machine. It had a cable plugged into it, so you might be familiar with VPN technology with encryption that is TLS-based and so it uses certificate-based encryption.

A lot of the pain points that you see in the complexity that we just talked about are because of that lack of design for modern networks. We have Wi-Fi now. We have laptops. We have cell phones. You're on the move all the time. People are working from coffee shops. They're working from their homes. They're working from the office. Things are moving around a lot more.

### WireGuard Technology and Tailscale's Identity-Based Mesh Network Approach

And so in the mid-2010s, a gentleman called Jason Donenfeld felt so empowered to fix this problem that he designed a new connectivity solution called WireGuard. If you're familiar with it, it was built from the ground up for modern networks. It has fewer bottlenecks, which means you can connect all of the devices in a network together. It's way more lightweight and predictable, so it's a much smaller codebase. It was written from the ground up. It has an entirely new cryptography stack underneath it, and it is really, really good in situations when you're moving around a lot.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/290.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=290)

It self-heals the connectivity path. So I can close my laptop and open it again and my VPN is not going to go all sideways and force me to reboot the machine. That was one of the reasons that WireGuard was designed.  And we at Tailscale have built our products on top of that technology. So we are the easiest way for you to get all of the benefits of this new-built cryptography and this new-built connectivity solution without having to do things like key management, without having to do identity, without having to figure out how you're going to build a client that works.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/320.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=320)

Tailscale is a very straightforward way to get all of those benefits. As we said, we built it on Wireguard. We embedded identity directly into the product, so we are SSO  driven. We don't charge extra for SSO. It's all based right from the start. It's all required for you to actually sign up for the product. No usernames and passwords, none of that kind of legacy security stuff.

It works on any operating system, any device, in any location. So one of our engineers recently compiled Tailscale for an old operating system. We work on your cell phone, we work on your laptop, we work on vehicle entertainment systems. We have Tailscale deployed in submarines, we have Tailscale deployed in farms, there's Tailscale about to get launched into space. We will work basically anywhere.

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/380.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=380)

One of my favorite pieces of Tailscale is that we actually handle navigating out of your network for you. So you don't need to open any ports, you don't need to open any security groups. It will connect these two devices together via the magic of our NAT traversal. And so when we layer the identity on top of that, instead of the other topology that we looked  at earlier, you end up with something that looks a little bit like this. Every device is aware of every other device in the network. Every device can communicate with every other device in the network.

We have embedded the identity of that device into the client, and so then we decide what you're actually allowed to communicate with based on who you are and who you've logged in as. And so we build this huge mesh of nodes, and then we build an ACL system on top of that, which will allow you to be able to determine, you know, can this person access this RDS database? Is this person able to access this EC2 instance? And so rather than thinking about this from a network level segmentation, we think about it from an identity-based segmentation.

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/430.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=430)

### Real-World Use Cases: From CI/CD Pipelines to Edge IoT and Performance Benefits

What sort of things does this open up for you? Well, I've used the term VPN quite a lot already in the six or seven minutes of this presentation, but what we are finding from our customers is that  this can solve a whole bunch of things that traditional VPNs or traditional connectivity solutions cannot solve for you. So we've talked a little bit about a standard kind of Business VPN use case. You have employees that are on the road, working remotely. They need to get access to internal applications that you need to secure, and we can solve that problem very straightforwardly.

We can also solve some more AWS and what we call machine to machine based use cases. So if you look at the concept of CI/CD, if you're using GitHub and GitHub's managed runners, you might want to run a database migration. But the GitHub managed runner is outside of your security boundary. Because Tailscale is so lightweight and small, you can drop Tailscale into a GitHub action workflow. We have a GitHub action workflow that will do setup Tailscale and connect you to CI/CD pipeline, and you can now run those RDS migrations or talk to your EKS cluster that has an internal API really easily with just a single step in a workflow. It's a really straightforward way of enabling connectivity to things that you couldn't previously do.

You look at things like Workload Connectivity. I don't know about you, but I've certainly run some of the agentic AI based things that are running amok around my laptop, able to delete random files and do whatever it wants. The Workload Connectivity and the Securing AI tenants of our platform allow you to actually secure those things and stop those things running rampant and doing things that you really don't want them to do.

Workload Connectivity is really interesting because previously the way you would solve this, I know there were some announcements today about cross-cloud connectivity for like Google Cloud and AWS. We have large organizations right now solving that exact problem with Tailscale. They just drop two Tailscale agents, one in Google Cloud, one in AWS. So they drop something in their AWS VPC and something in their data center or in their office, and that immediately connects those two things together. There's no configuration, no firewall changes, no setup. It just works straight away.

The other two I think are Edge and IoT. Because Tailscale is so small and lightweight, if you are deploying things like kiosks to different stores, or if you are deploying robots and all that kind of stuff, you can actually install Tailscale directly on those things, which will help you with things like getting telemetry off them or debugging problems in those environments. You know, it costs millions of dollars for organizations to send technicians out to fix things that are in the field. You can install Tailscale on those devices and fix them remotely without having to get people on the road. It can save you millions of dollars. We have customers saving millions of dollars on Edge and IoT.

And then finally, our go to market motion is that you are going to love our product so much that you want to take it to work. And so I'll talk a little bit later in the presentation about our pricing structure.

Our pricing structure has a default setting where you use Tailscale for your home lab. We offer a very generous personal tier. So if you have video cameras at home or you want to check the contents on your smart fridge and you want to do all that remotely, Tailscale is a free and easy way to solve that problem.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/630.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=630)

So we talked about some of the real world problems here. Developer access to resources in AWS is the straightforward one. You have EC2 instances,  you want to debug why that EKS pod didn't start properly, so you want to get to the EKS worker node, you want to get into a database to fix a production issue, you want to get access to anything that is only confined to the VPC. Tailscale can solve that quickly and easily and straightforwardly. If you want to connect two AWS regions together or two AWS organizations together, or perhaps you want to enable your customers to be able to communicate back to your infrastructure in a secure way, Tailscale can solve that problem for you.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/690.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=690)

We can help you with secure automation problems. We can help you with things like GitHub Actions or Terraform Cloud or Jenkins runners that need to actually communicate from outside your security boundary to inside your security boundary. And then as I already talked about, the hybrid and cross-cloud connectivity problem. I know there were some announcements today as I already said, but we've been solving that problem for nearly five years. And I think we know what we're doing. I think we've done a pretty good job of that.  That's the same slide twice, apologies for that.

So what does doing it wrong look like? Well, if you have too many layers of indirection, I already talked about this earlier in the presentation. If you do too many layers of indirection, if you have too many hops through an infrastructure in order to make these things work, the debugging and the user experience is really, really painful. So each time you build some sort of hub and spoke connectivity model, each one of those instances is a point of failure that can cause you problems in really critical situations. In a mesh-based approach, there is no single point of failure. No single instance can break any of your connectivity workflows.

We see a lot of organizations doing what we call security by IP address. So you might be using things like IP whitelisting. You might be doing things like only allowing access from certain subnets to other subnets. I've seen in my career hundreds of security groups that only allow access on port and protocol on specific IP addresses. The problem that you run into with that is what if the IP addresses change? What if things move around and you have to migrate these things? That causes real world connectivity problems, and so we focused on doing this via identity rather than via any networking level. We want to know who you are, we want to know the origin of the request, and then we allow access based on that identity rather than the IP address.

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/810.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=810)

And then finally, I've talked about this a lot, but I really think it's worth driving home. The developer experience of doing these things right is just an experience that just works. It's just way better. So what does that look like for organizations that have actually adopted Tailscale? Well, Cribl has grown their organization. You can see their huge booth over there in the corner. They've been using Tailscale since the very inception of the company. They've grown  by 25x in terms of their employee count, and they've had no dedicated IT resources to actually manage connectivity. And they attest a huge portion of that to Tailscale.

Hugging Face estimated that they save 240 hours annually just on provisioning new users when they are onboarded into the organization. Corelight, 1000 hours saved annually with fewer VPN connectivity issues. 90% reduction in internal support requests from Instacart. I was part of the sales team that was rolling out Instacart, and the level of happiness that they got just from switching from one product to another, just in terms of IT tickets and people picking up the phone saying, hey, I've got a meeting in 10 minutes and the VPN's not working. It made a huge difference to their business.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/900.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=900)

And then in terms of actual real technology performance, because we use WireGuard as our underlying encryption, we've seen we can guarantee a 4x reduction in latency because we're going directly between those resources and a 22x increase in throughput. We can often find that with Tailscale we can saturate the NIC inside these EC2 instances to, you know, up to the 10 gig NICs basically. So you can get fully encrypted connectivity with full throughput for most EC2 instances.  So here are some diagrams that show you what that looks like in the real world.

### Implementation Examples in AWS Environments and Getting Started with Tailscale

You can see we have an EC2 instance in a private subnet and the NAT gateway in a public subnet. You'll notice I haven't opened any ports in this particular example. I don't open any security groups. I only need outbound connectivity. I do not need any inbound connectivity. Tailscale can connect me to my laptop, I can run SSH commands, I can run debugging things, all those kinds of things. If you're regularly SSHing into things or using something like AWS SSM, there are a lot of things that are fraught with peril there. The fact that you don't have to open any firewall ports is going to make your security team very happy, I suspect.

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/940.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=940)

 We have a Kubernetes operator so we can run within EKS. We can help you move your EKS control plane. Obviously the default setting is public access for your EKS control plane. If there's a Kubernetes zero day tomorrow, a lot of EKS clusters are going to be in a really bad way. They're going to be publicly accessible. You probably don't want to put your MySQL database on the public internet. I don't want to put my EKS control plane on the public internet. That's a very powerful API, and Tailscale will enable you to actually move that into a private subnet so you no longer need to worry about that being on the public internet. I get the same level of throughput and connectivity using the Tailscale Kubernetes operator.

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/980.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=980)

 I can also use Tailscale to connect EKS nodes using EKS hybrid nodes. If you have remote GPUs that you want to connect from another cloud and connect them back to your EKS control plane, Tailscale can solve that problem with you requiring no dedicated hardware, no capex in terms of spend. You don't need IPsec tunnels anymore. All you need to do is install Tailscale on the node that you want to connect and everything will just work. There's a great blog post on the AWS blog about how to do that.

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/1010.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=1010)

 If you want to do, as I already mentioned, just connecting to things like Amazon RDS, that's a very common thing. I imagine most people don't want to put their RDS databases on the public internet anymore. How do you actually run things like database migrations? How do you do debugging? All you have to do is run a Tailscale EC2 instance. You create something called a subnet router that advertises the address of the RDS database, and you can use that to proxy requests through there. We see that as a very common use case.

[![Thumbnail 1040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/1040.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=1040)

 Finally, if you want to connect two specific VPCs together, obviously you can use transit gateways, which are very expensive. You can use VPC peering, but once it gets to more than two connections, it can be quite complicated and difficult to set up. You can actually connect these two VPCs together with just two EC2 instances, or one EC2 instance in each one of the VPCs, and it will connect you together really easily.

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b6cfce26f55cfbfa/1070.jpg)](https://www.youtube.com/watch?v=mY5meE9mroY&t=1070)

 I mentioned at the beginning, I believe we have a plan that's right for everybody. One of the things that you could do right after this presentation is go to Tailscale.com and sign up for our personal tier, which is free forever, up to three users and 100 devices. I think that's a very generous tier. Again, our entire model here is we want you to love Tailscale so much that you take it to work and replace that thing that's been annoying you for years and years and years. If you head over to our booth, it's booth 1818 over in the corner over there somewhere. We actually have a code as well that will give you three months of our starter tier for free as well.

That was just about timed perfectly. I've got about one minute left. I really appreciate everybody taking the time to join me today. I appreciate everybody listening to me, and I hope everybody has a great rest of the conference. Thank you very much. I will be over at the booth for the next 10 to 15 minutes. If anybody has any questions, I can probably take some questions here as well, but thank you everybody.


----

; This article is entirely auto-generated using Amazon Bedrock.
