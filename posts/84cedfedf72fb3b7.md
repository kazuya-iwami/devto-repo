---
title: 'AWS re:Invent 2025 - State of the Edge:  Delivery of the web with CloudFront, WAF, & Shield (NET211)'
published: true
description: 'In this video, AWS Edge Specialists and Atlassian''s Cloud Network Engineer discuss the evolution of edge services in the AI era. Nishit Sawhney presents AWS''s investments in CloudFront, Shield, and WAF, highlighting performance enhancements like QUIC HTTP/3, HTTPS DNS records, and TLS 1.3 to origin. Etienne Münnich covers security features including Mutual TLS, anti-DDoS managed rules, and AI bot controls. Ambrose Phey shares Atlassian''s journey migrating Jira and Confluence to CloudFront, leveraging Anycast static IPs for enterprise firewall requirements, implementing region steering with CloudFront Functions to eliminate cross-region charges, and adopting CloudFront SaaS Manager for multi-tenant distribution management. The session includes practical insights on WAF rule tuning, Network Error Logging implementation, and the new flat-rate pricing plans that eliminate bill shock concerns.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/0.jpg'
series: ''
canonical_url: null
id: 3085312
date: '2025-12-05T04:52:37Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - State of the Edge:  Delivery of the web with CloudFront, WAF, & Shield (NET211)**

> In this video, AWS Edge Specialists and Atlassian's Cloud Network Engineer discuss the evolution of edge services in the AI era. Nishit Sawhney presents AWS's investments in CloudFront, Shield, and WAF, highlighting performance enhancements like QUIC HTTP/3, HTTPS DNS records, and TLS 1.3 to origin. Etienne Münnich covers security features including Mutual TLS, anti-DDoS managed rules, and AI bot controls. Ambrose Phey shares Atlassian's journey migrating Jira and Confluence to CloudFront, leveraging Anycast static IPs for enterprise firewall requirements, implementing region steering with CloudFront Functions to eliminate cross-region charges, and adopting CloudFront SaaS Manager for multi-tenant distribution management. The session includes practical insights on WAF rule tuning, Network Error Logging implementation, and the new flat-rate pricing plans that eliminate bill shock concerns.

{% youtube https://www.youtube.com/watch?v=j8uPcCJQGYo %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/0.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=0)

### Introduction: Speakers and AWS Edge Services Mission

 Hello everyone. My name is Etienne Münnich. I'm a Senior Edge Specialist Solutions Architect based in Sydney, Australia. It's good to be with you here today. I've traveled a long way to be here. I'm here with my colleague Nishit and with my customer Ambrose.

Good morning everyone. My name is Ambrose Phey and I'm a Cloud Network Engineer at Atlassian in our edge services team. Today I've flown in from Australia and I'm excited to share our story, use case, and outcomes of the work we've been doing over the past year with Amazon's Edge products. But for now I'll pass it over to Nishit.

Hello everyone, I'm Nishit Sawhney. I'm the Director and General Manager for Network Services at Amazon. These include Amazon CloudFront, our perimeter protection services, and Application Load Balancing, or ALBs. So you're in session NET211, the State of the Edge. We're excited to share some insights today, and we're glad to bring you along on the journey that we've been involved with. We're excited to make sure that you take away some of the learnings that Ambrose and his team have worked through, and we hope you enjoy it today. This is a 200 level talk. We expect you to know something about CloudFront, WAF, and Shield. However, if you don't, don't stress—we'll take you along for the ride. I'm going to hand over to Etienne.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/100.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=100)

 All right, so let's start by defining what an Edge service is and what is at the AWS Edge. Well, an Edge service is a distributed computing service that brings network, storage, and compute capabilities physically closer to your users, thereby reducing latency for your users. Now, at AWS there are several Edge services that run either partially or fully at the Edge. For the purposes of this session, we're going to focus on Amazon CloudFront, Shield, and WAF.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/140.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=140)

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/150.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=150)

 Just as a refresher, CloudFront is a global content delivery network. Shield and WAF are security services that protect your applications against Layer 3, Layer 4, and Layer 7 attacks.  Our mission really is to protect your applications and make them fast, secure, and resilient. If you think about it, our mission really is to make the internet fun and engaging for billions of users across the world. This is what drives me and my team at AWS.

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/180.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=180)

### Evolution of the Internet and the Emergence of the Agentic Web Era

 Now, we've evolved our services over the last 17 years as per the evolution of the Internet. The internet has gone through massive shifts, each one redefining how users interact, what applications you build, and what infrastructure services like ours need to deliver. Let's start with how it all began. The web was simple static web. A browser made a request for a file, the server responded—simple GET requests. Security was simple, merely IP blocks. Caching rules were pretty simple.

Along came the transactional and e-commerce web. That's where APIs became prevalent with PUT and POST requests. We and CloudFront launched dynamic content acceleration for handling these kinds of requests. Security became more complicated. DDoS attacks at Layer 3, Layer 4, and Layer 7 became prevalent. Then came the next big shift, which was social and mobile. If you think about it, that really democratized the internet, bringing millions and millions of users and scale on the network. Security then became even more complicated with bot activity. These bots acted as humans and tried to conduct or steal information from your websites. Latency also became even more complicated.

Then the next step was really social media and short-form video live streams. Here, latency attachments became emotional. It wasn't just a number. A 100 millisecond latency difference could really be the differentiator between user engagement and abandonment on a platform like TikTok or Reels.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/370.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=370)

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/380.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=380)

Along the right came the Internet of Things. Here, we saw many more devices and many more requests with different traffic profiles on the internet. But what came along with that was opportunities for those devices to be compromised by malicious actors, and that created massive pools of devices and endpoints on the internet that would launch DDoS attacks and other web attacks against applications. We evolved our services along these evolutions, and one such thing that we did was ensure security was no longer reactive—it had to become proactive. So we launched threat intelligence built based on a global set of sensors, which we call a honeypot network. That honeypot network allows us to collect intelligence from the entire web and feed that intelligence into our services at AWS to protect your applications. What really happened here was the perceived latency tolerance for  our users went down. The security and resiliency expectations have increased exponentially. 

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/400.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=400)

But we are now entering the next era, which is the agentic web, or the AI web. I'm going to ask a question: How many of you have observed requests on your web applications that are originating from AI bots or chatbots? Quite a few.  This is the data that we see on websites hosted on CloudFront and AWS WAF. On a daily basis, billions of requests are originating from AI bots like ChatGPT, Claude, Perplexity, Nova, and Bedrock. This is not just for certain industries like retail and e-commerce. Every possible industry and customers in those industries are experiencing this.

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/460.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=460)

You might be wondering whether we have a way to solve this with something called robots.txt. Well, that really is the challenge. Robots.txt was built for an entirely different era, when bots were well-behaved and respectful. But the bots have evolved. The bots used to be traditional bots for crawling and search indexing.  They have now increasingly become AI training bots used for training massive amounts of data and models. They crawl and consume. And increasingly, we are seeing agentic AI bots that are conducting transactions and actions on behalf of users. I hear from many customers, especially in the publishing space, asking for help to deal with this bot activity.

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/510.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=510)

First of all, they want to understand what bot activity is on their websites. They want to apply rules that align with their business logic, like allowing certain bots, denying others, or even in some cases, monetizing bot activity in a sophisticated manner.  So really, the agentic web era is defined by three defining trends. The first one is that the traffic profile is changing. We see a lot of burst traffic on our network. We see a whole bunch of duplex connections to chat applications. We see not just large objects for live streams and video, but also tokens that are used for LLMs. Our network needs to evolve for that.

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/600.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=600)

The second big trend is security. Now, customers are asking for security of their AI web applications, but also protection from AI-powered attacks. And lastly, you developers are going through an unprecedented era where the development cycles are so fast. You can now build applications with AI coding assistance or agentic systems like Bedrock Agent Core, or even VIP coding platforms like Lovable, Cursor, and Kiiro. You all do not want to be hindered by infrastructure complexity. Our customers want the infrastructure services to evolve with the pace of these new development cycles. We do not want infrastructure to be in the way. These three trends are what are driving  our investments in our roadmap at Edge Services.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/620.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=620)

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/640.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=640)

### Performance and Resiliency: AWS Network Infrastructure and Protocol Enhancements

I'm going to talk about these three trends and some of the enhancements we've made, how we've helped customers, and where we are going. Let's start with performance and resiliency.  The underpinnings of great performance, resiliency, and security is the massive AWS network.  This comprises AWS regions as well as distributed POPs that belong to Amazon CloudFront. This network has been growing over 50 percent year over year. We've added 750 plus POPs and 1,140 embedded POPs that are deployed deeply inside high-speed networks.

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/680.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=680)

We are now in 200 plus cities and 50 plus countries, connecting to millions of users around the world. This network handles 1 trillion requests on a given day in hundreds and hundreds of terabits per second peak. From a security standpoint,  this network allows us a tremendous amount of data for threat intelligence purposes. We analyze exabytes of data every minute. Just in the last year, we handled 6.2 million DDoS attacks, which is a growth of 150 percent year over year, with the largest one being 25 terabytes per second.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/710.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=710)

On this network, we recently launched  a global Anycast Bring Your Own IP capability. Until now, when you put your application on CloudFront, you would get dynamically assigned IPs to your applications. Last year, we launched the Anycast capability with static IPs, and customers could use that to put their applications allow-listed for enterprise firewalls or even zero-rating agreements with ISPs. With the recent launch, you can now bring your own IP and enhance that experience. This Bring Your Own IP capability comes integrated with VPC IPA as a unified way to bring IPs to AWS. This gives you a global front door, anycast on the AWS network.

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/790.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=790)

Let's go into what happens under the hood when a request comes to this network.  Your users are trying to access applications which might be in AWS regions or other clouds or on-premises networks. When a request hits the AWS edge, the edge POP, there's TLS termination and TLS optimization that happen there. We optimize routing to the best POP from a latency point of view, and we get millions of measurements on real-time latency data from web bugs that are deployed across the world. That allows us to route your requests to the best POP, and we handle congestion on the network as part of this routing.

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/830.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=830)

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/850.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=850)

Your dynamic content or APIs go over the privately managed backbone, the AWS backbone, straight to your regions and your applications. For cacheable content,  it goes through a series of tiered caches on the AWS network. These caches are on the AWS edge POPs as well as in regions in AWS. Optionally, customers configure Origin Shield,  which is yet another layer of caching that allows better offload and better performance through persistent connections back to your origins.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/890.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=890)

As we've seen, a lot of our customers want to customize and personalize their delivery. We've added capabilities like CloudFront Functions and Lambda@Edge over the years, along with a global key-value store that allows you to do A/B testing or manipulate request and response capabilities on your requests. This allows you to put compute closer to where your users are and reduce the latency that they experience.  Over the years, we've added a lot of performance enhancements to different parts of the request flow. One such enhancement was QUIC HTTP/3, launched in 2023.

Originally from Google, it allows faster time to first byte and leads to head of line blocking removal. What it does is combine the TLS and HTTP handshake into a single connection. Over 25% of requests on CloudFront today are on QUIC, and this is available by default. Last week, we added another capability for enhancing the performance of QUIC and HTTP/3. We added support for HTTPS DNS records. This is a DNS record type that allows us to send hints about supported HTTP protocols as part of the DNS query itself.

When your client or browser session requests an IP address for your application hosted on CloudFront, it makes a DNS query, and that's where it gets the A or AAAA address of the CloudFront POP. Your browser then establishes a TCP, TLS, and HTTP connection with CloudFront. That connection happens over HTTP/1.1 or HTTP/2. Then CloudFront provides a clue to your browser saying we support HTTP/3 as part of alt-svc headers. That leads to an additional handshake and round trip time when connecting to the optimal protocol.

With the launch of HTTPS DNS records, in the DNS query itself, we will pass support for HTTP/3 as a signal. When you now make the connection to CloudFront, it comes with the optimal protocol selection of QUIC/3 already determined. This has been a huge hit as it leads not just to performance benefits, but also DNS query cost reduction because Alias Records on Route 53 are free of charge.

There are some other enhancements that we made on round trip reduction. The first one is TLS 1.3 to origin. We just launched this capability that allows you to establish TLS 1.3 connections between CloudFront and your supported origins. So if you have origins that support TLS 1.3, this just works. We've seen almost 36% improvement in handshake time, as TLS 1.3 cuts the round trip connections for TLS exchange.

The second one is TCP Fast Open. TCP Fast Open allows data to be sent as part of the TCP SYN exchange. We've added support for this and observed 50% improvement in TCP SYN times. This is available between CloudFront POPs and our internal caching layers and works by default. The best part about these performance enhancements is that they are enabled by default. They just work and they're free. I would recommend that you look at these and see if they apply to you, especially TLS 1.3 and HTTPS DNS records, and take advantage of these with your application delivery.

### Building a Secure Edge: CloudFront Security Features and AI Bot Management

We're now going to switch to the next part, which is building a secure edge. I'm going to invite Etienne to walk you through that. Thank you, Nishit, for all the work that you and your team put together to improve performance for our customers. I'd like to talk a little bit about how we provide security at the edge, what you can do, and what is provided to you by default when you use CloudFront. Nishit described how the request path happens through our infrastructure, and I'm going to talk in a little more detail about what we do at the edge.

Typically at an edge location, we provide a connection from there back to an AWS origin, which may be a load balancer or could be to your on-premises location. With that type of connection, we can provide to AWS origins using VPC origins where we can ingress traffic into a private subnet, thereby reducing the attack surface and reducing the need for public load balancers.

Recently launched is cross-account VPC origin support, which allows you to use a hub and spoke model for routing CloudFront traffic into private subnets across multiple accounts. Origin access control helps protect your access from CloudFront to resources like Lambda URLs, S3 buckets, and media services.

[![Thumbnail 1230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1230.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1230)

When traffic comes to your CloudFront distribution, CloudFront will provide a TLS certificate for clients to connect with. We have just launched mutual TLS support, allowing clients to provide a certificate and authenticate that way as well. On top of that, the team has enabled post-quantum cryptography support, which means that by default when you use CloudFront, you are future-proofed against that type of attack risk. 

[![Thumbnail 1280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1280.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1280)

[![Thumbnail 1310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1310.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1310)

CloudFront provides the ability to protect against Layer 3 and Layer 4 attack vectors by using AWS Shield.  This helps you protect against known offenders and provides a SYN proxy. There is continuous inspection at the edge, protocol validation, and packet validation. There are automated routing policies for particularly large attacks to help mitigate the risk away from your applications. 

[![Thumbnail 1330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1330.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1330)

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1360.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1360)

CloudFront also provides the ability to monitor and check resource application usage at the edge, ensuring that attack surfaces vulnerable to Slowloris and HTTP/2 Rapid Reset are negated.  There are further protections you can have by using AWS WAF. You can elect to turn this on for your CloudFront distribution. Here you can use controls such as rate limiting, employ Amazon managed rules such as anti-DDoS rules, and use Amazon partnered and managed rules as well. You can build your own custom rules and use bot control and fraud control rule sets at the edge. 

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1380.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1380)

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1400.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1400)

Let's talk about bot control and fraud control. Our anti-DDoS rule set allows you to quickly turn on protections to protect your application against volumetric attacks.  This is something you can tune, and we will talk about this in more detail later. With AWS Bot Control, you can choose two different types of modes of operation to protect against common bots that are easily detected through common signatures, and then look at behavior and machine learning capability with targeted bots for sophisticated bots. 

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1420.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1420)

On top of that, we have our account takeover fraud prevention rule sets and our account creation fraud prevention rule sets.  These are particularly helpful when you want to deal with credential stuffing attacks or when you have a particular benefit when someone wants to sign up for an account, but there is a financial benefit or a scraping benefit that someone would want to take advantage of. These provide you the controls at the edge of the network to prevent that risk.

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1440.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1440)

Let's talk about anti-DDoS rules and how these rules reduce the attack surface. When I have helped customers mitigate DDoS attacks, rate controls alone are not sufficient.  You would not want to spend time going through your logs and trying to figure out how to put these protections at the edge with just simple IP controls. What you can do is use the anti-DDoS rule set, which is quite quick to deploy, and within a few minutes you can have an effective control.

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1500.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1500)

What is really neat about this is that you can tune this rule set based on the type of workload you have and your appetite for absorbing a certain amount of traffic. Typically, you would want to make sure that this rule set sits in the top part of your rules so that it is exposed to as much of the traffic signature as possible. This allows you to tune the rule sets and decide which paths you want to protect. You can scope these protections and add exclusion lists. There may be parts of your application that you do not want this control in place for.  There is a rich set of metrics that are emitted into CloudWatch that you can then use to drive alarms and build dashboards to understand the type of DDoS risk you're facing.

[![Thumbnail 1520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1520.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1520)

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1560.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1560)

This helps you adapt more quickly and mitigate volumetric attacks while improving the accuracy of your DDoS mitigations.  The anti-DDoS rules include a challenge capability that allows you to have a conditional action to help verify whether someone is a valid user or not. The granular controls allow you to change your rule sets based on the risk you're facing at any point in time. You may have a lower risk profile during a large event, or you may have less appetite during a large event. 

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1620.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1620)

Pivoting to what we've recently released, which is Mutual TLS, or Mutual Transport Layer Security authentication, a security protocol that extends standard TLS authentication into bidirectional certificate-based authentication. If you haven't yet used this, it simply means that a client provides a certificate to prove its identity. CloudFront has two modes of operation where you can allow both standard traffic with a normal certificate and Mutual TLS, or Mutual TLS only. This allows you to migrate between the two models. We're quite excited to see what our customers do with this capability. 

[![Thumbnail 1730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1730.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1730)

I'm personally quite excited about what we're doing in the space with Agentec, in the agent internet era. This is an additional capability that was recently launched. We have found that businesses are facing an onslaught of traffic via agents. This means you potentially need to have not only control at the edge but also a rich set of metrics and controls at the center. This allows you to ensure that legitimate AI agents can access your distribution through a process of cryptographic control. This allows you to create fine-tuned controls for your AI agents to access specific parts of your application while denying access to others. 

AWS has managed rules that automatically allow verified bots while blocking unverified traffic. The first step is to identify the risk and then decide what you want to mitigate. This is done through categorization or through the simple AI label where you can choose which specific bot you want to block. With authentication, you'll be able to allow your application to be more deeply integrated into your WAF to ensure that those parts can be monetized if you choose to. There's an additional level of capability that is continuously being built on, and we'll keep adding to this capability over time.

[![Thumbnail 1750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1750.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1750)

[![Thumbnail 1760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1760.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1760)

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1770.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1770)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1790.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1790)

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1800.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1800)

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1810.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1810)

### Simplifying Developer Experience: Security Configuration and Pricing Innovation

Now, I'd like to hand back to Nisha to talk about some of the work the team is doing to help improve our developers' lives. This is my favorite part. Let's talk about security.  Many customers tell us that configuring security in this ever-evolving landscape is hard, but it does not need to be hard.  This is an area where we are massively investing in solving this challenge. Earlier this year, we reimagined the developer experience for configuring security rules with CloudFront and ALBs.  Here, you can add WAF capability as part of protection packs. These are tailored rules and rule sets designed for your traffic profile. It comes with improved visibility as well as easier, more intuitive configuration controls.  We're adding more capabilities here to bring smarter recommendations and automated traffic analysis on these rule packs in the future.  Already with these enhancements, we've seen  about an 80% reduction in the initial configuration steps and a dramatic improvement in the success rate for customers configuring their security rules on the front doors.

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1830.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1830)

Now, here's another favorite part of mine.  Developer complexity is not just technical complexity; it is also pricing complexity. When CloudFront was launched back in 2008, we disrupted the CDN pricing industry with pay-as-you-go pricing. Customers could simply use the CDN and pay as their traffic grew, with pricing scaling accordingly.

However, over the years, customers have told us that while they like the pay-as-you-go pricing model, they're worried about bill shocks. They might see their websites getting really popular and viral, leading to sudden increases in bills, or they might get hotlinked or even suffer from DDoS attacks that could lead to exponential request charges. We've solved that problem with what we call flat-rate pricing plans, which was launched two weeks ago. These plans start with a $0 pricing option that includes not just CloudFront, but also security services, Amazon S3, Route 53, CloudWatch, ACM, and a bunch of other services.

These pricing plans come with no overages whatsoever. You pay a fixed monthly price or you start with a free plan and then upgrade to these according to your business growth, but you'll never see overage or unpredictable pricing anymore. There are no long-term commitments either. In just a week or so since launch, tens of thousands of customers are using and taking benefit of these plans to serve their applications. If you haven't looked at this, I would encourage you to take a look.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/1950.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=1950)

Another great simplification is CloudFront SaaS Manager.  Many of our customers host millions of domains or hundreds of thousands of domains in multi-tenant applications. White-coding platforms like Cursor and Lovable also allow customers to build applications with lots of multi-tenant domains and tenants. Until now, you would have to create individual distributions and apply policies such as cache policies or security policies individually on those distributions.

With CloudFront SaaS Manager, it provides a unified way and a dramatically simpler experience in which you can handle multi-tenant applications on the AWS Edge. You can apply template policies that can apply to the entire list of tenants, or you can override certain specific policies because you may have some premium set of customers and other tiers of customers. This is one of the great simplifications that we are evolving. This is the first step, and we're adding more capabilities, such as tenant-specific analytics and reports.

[![Thumbnail 2030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2030.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2030)

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2060.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2060)

### Atlassian's Journey: Migrating Jira and Confluence to CloudFront

To learn about  the real-world applications of the services and enhancements we've discussed, I would like to invite Ambrose from Atlassian. I'm excited to be sharing the ways in which Atlassian has leveraged CloudFront, WAF, and Shield to improve our global security footprint alongside some useful patterns we have found in our widespread adoption journey. To give some background,  Atlassian as a whole aims to unlock the potential in every team. We do so by offering our applications across a variety of collections, all with the goal to help teams organize, communicate, and collaborate around shared work.

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2100.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2100)

Some commonly used applications you might have seen before include Confluence for information sharing, Jira for project and issue tracking, Loom for asynchronous video messaging, Bitbucket for code hosting, and Rovo, our AI-powered suite of applications and agents connected to the Atlassian platform. As of today, Atlassian now serves millions of customers globally across a wide range of industries.  Atlassian has come a really long way from when we first launched in 2002.

Over the years we've released and acquired many products, and we even went public in 2015. In 2016, Atlassian began the journey to move all of its workloads to be fully hosted in the cloud, and as of 2022, we completed this journey. As of today, we handle tens of billions of requests across tens of thousands of EC2 instances distributed across 13 different regions, handling multiple petabytes of data delivered to clients every single day.

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2190.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2190)

But that's not the end of our story. Over the years, the team has continued to improve and develop our cloud posture and architecture, leading to our migration of our largest products, Jira and Confluence, onto CloudFront. Atlassian already has a long running history with CloudFront. We've invested several years and deployed thousands of distributions.  This figure shows a historical high-level view of a request flow and what a page load might look like. Dynamic requests land on a network load balancer towards our ingress proxies before getting routed to a number of different upstream services, and CloudFront accelerates our static, front-end, and single page application delivery across the company. As of today, every single page load on every Atlassian product uses CloudFront in some way.

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2240.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2240)

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2250.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2250)

We were already using a mixture of off-the-shelf and internally developed technologies to mitigate inauthentic and malicious requests.  With that foundation, what more were we looking to achieve? Building on our rich experience with CloudFront, we wanted to leverage it beyond just acting as a basic CDN. We still had not yet adopted this infrastructure for the delivery of our first fragment or primary domains on our major products. Our goals for moving Jira and Confluence onto CloudFront were multifaceted.  We wanted to strengthen our global security footprint against growing sophistication in modern attacks and compliance regimes. We wanted to deploy a layered defense at the edge, leveraging a consistent control plane to help with operational burden. We wanted to identify common and shared logic in our application which we could perform closer to our customers. And finally, we wanted to improve latency and performance for our end users.

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2260.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2260)

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2270.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2270)

[![Thumbnail 2280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2280.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2280)

Now, you might be thinking that if a large portion of our goals were pertaining to security, why not just place WAF on our existing load balancers.  That would be a good question. Firstly, as network load balancers are a layer 3, layer 4 construct, WAF logic simply doesn't belong there.  While we could have attached WAF to our application load balancers,  this would have still left our ingress proxies needing to do a lot of the heavy lifting to mitigate inauthentic and malicious requests. We wanted to push this defense beyond the compute infrastructure we managed.

[![Thumbnail 2290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2290.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2290)

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2310.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2310)

 With that in mind, the journey of getting Jira and Confluence onto CloudFront would pose quite a unique challenge.  It would need to handle orders of magnitude more traffic than any previously existing CloudFront distribution. It would need to accommodate logic to support customer data distributed across 13 different regions. It would need to be able to host millions of custom domains and customer sites. And finally, it would need to support an architecture which aligned with our customers' security requirements. All in all, this would prove to be more involved than just changing where a domain record pointed and required us to use some of CloudFront's newest features.

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2320.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2320)

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2350.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2350)

[![Thumbnail 2360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2360.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2360)

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2370.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2370)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2380.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2380)

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2400.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2400)

### Implementing Security at Scale: Anycast Static IPs and WAF Protection Strategies

 One of the big questions before even moving to CloudFront was to identify how we could differentiate ourselves amongst other CloudFront customers. To build some context, Atlassian has a variety of customers who might operate their workstations or API integrations under strict egress controls.  Others might be doing something similar to this figure, where they leverage split-tunnel VPNs or secure web gateways to ensure that our products are only accessible from their corporate IPs.  Such customers require that their cloud SaaS provider present itself on the internet from a predefined list of IPs not cohabited with any other SaaS software.  This allows them to perform enterprise application allow listing. Typically, when you use CloudFront, your distribution will use a rotating pool of IP addresses to serve traffic from.  As you can imagine, this would pose quite a problem for our customers, as there would be no clear way to differentiate ourselves amongst other CloudFront services sitting behind CloudFront. But this is exactly where CloudFront's Anycast static IP feature came into play.  By leveraging this new feature on our CloudFront distributions, we were able to dedicate a set of IPs across our CloudFront distributions, allowing ourselves to be uniquely identifiable against other CloudFront customers.

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2420.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2420)

This was a massive unblocker in unlocking the transition of Jira and Confluence onto CloudFront. Many customer network policies simply would not have been compatible with CloudFront's traditional shared dynamic IP space.  With the confidence that we had a unique way to present ourselves, our attention turned to protection. Building on our previous experience, we launched Shield Advanced and Layer 7 application protection features. This enables WAF and CloudFront to intelligently catch DDoS attacks. We then combined this with managed rules for anti-DDoS, common web attacks, IP reputation lists, alongside a variety of rate limits.

[![Thumbnail 2450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2450.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2450)

When crafting your managed rules, it can sometimes be overwhelming trying to decide which rules should go in what order.  This figure here is a fantastic example and mental model approach to use. However, you might need to do something more tailored to your environment. What we have found to be effective for us is layering our rules in the following order. First, label any traffic that you might want to apply modifications or exclusions against in different policies in your chain. These might be for trusted IPs, countries, or even specific clients.

Next come your strict blocking rules. These might be your known blocking conditions or managed rules for common web attacks. Neatly behind those come your coarse-grain rate limits and custom rules, and finally your IP reputation lists pushing out silent challenges and CAPTCHAs. I want to take a second to call out the managed anti-DDoS rules set. This is something you might want to put earlier in your rule chain, and we have seen this being incredibly effective at stopping the initial and largest wave of attacks in a matter of seconds.

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2530.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2530)

It pairs really well with Shield in blocking the remaining trail of malicious requests. All of this combined has allowed us to help create a cost-effective protection funnel at the network edge.  As part of all of this, you will likely need to perform some tuning. This typically is quite an iterative process and does require both time and patience. As per this figure, it is recommended to first implement your rules set into count mode, allowing you the opportunity to observe and adjust any logic before promoting it to a block or challenge action.

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2580.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2580)

In our case, some interesting tuning scenarios included Confluence pages with .conf, .md, or Java exceptions in them. Others included scenarios where customers would put bits of source code or program outputs into their pages, searches, or even Jira ticket comments and descriptions. The result of all of this work is we are now blocking hundreds of thousands of requests made by bots and scanners, as well as the occasional 10 and 100 million DDoS attack. 

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2600.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2600)

### Optimizing Performance and Observability: Region Steering and Network Error Logging

Another capability that Atlassian provides to its customers is something commonly referred to as data residency. This gives organizations the ability to choose where their application data is hosted, such as whether their data is globally distributed  or stored in a defined geographic location, such as the EU or the US. This is particularly important in regulated industries where customers need to meet a number of data management requirements. Using this figure as an example to help paint a picture, let us say we have a company whose Confluence tenant resides in the EU but they might have employees living in the US.

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2640.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2640)

Previously, requests would land on the nearest ingress proxy to them, so let us say in Virginia, before getting routed cross-region to their backend tenant in the EU. Bringing our attention back to CloudFront, we wanted to find a way to optimize our ability  to route our customer requests to their respective data regions, which could be located in one of our 13 different regions. This is exactly where the origin control helper method for CloudFront Functions came into play.

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2660.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2660)

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2680.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2680)

Leveraging CloudFront Functions and this new helper method,  we were able to implement logic where we take a customer's site name, look them up in a key value store to identify their backend region, and subsequently steer traffic directly to that customer's backend tenant. We were still able to route traffic to the closest possible region where that made sense, such as for loading common front-end components and assets. 

[![Thumbnail 2690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2690.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2690)

In total, across a number of experiences we saw improvements in our time to first byte, but more importantly, for requests that were region-steered, we managed to  completely remove cross-region charges and benefit from a zero rating of data out between AWS Regions and CloudFront, resulting in some very natural cost optimizations. Now, with the mention of cost savings, you are probably wondering, but how much?

[![Thumbnail 2710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2710.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2710)

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2730.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2730)

Well, here are the results. Each color in this graph is representing  an individual region, and in total is representing the stacked total cost over time. As you can see, we've seen significant reductions in our data transfer out fees, thanks to the zero rating between Amazon Regions and CloudFront. We've also seen similar reductions in our cross-region fees thanks to CloudFront doing the work of determining the data residency  region. Additionally, we enjoy zero rated requests to CloudFront that get blocked by WAF opposed to if they were blocked on an ALB.

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2770.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2770)

This region steering feature has been so well received across the company that we've had many requests from teams across Atlassian to help provide similar functionality to their services too. So all this combined now means that we're delivering a more secure experience presented behind unique static IPs and we're leveraging CloudFront to pull data from the correct origin region, all the while saving money doing it.  Before I move on to the next section, I want to share some additional notes that we've learned along the way.

[![Thumbnail 2780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2780.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2780)

[![Thumbnail 2800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2800.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2800)

First, if you're using a network load balancer as your origin, make sure your proxies return  the HTTP connection close header during draining events. This can help prevent some surprising connection terminations on in-flight requests as your proxies scale in. This isn't a problem with application load balancers as they'll add the header for you to tell CloudFront to close the connection gracefully. Next, CloudFront supports the use of stale  if error and stale while revalidate cache control directives. These respectively let you serve stale cache data during transient errors and allow CloudFront to asynchronously update its cache, reducing latency spikes and origin calls.

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2820.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2820)

[![Thumbnail 2840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2840.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2840)

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2850.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2850)

Finally, a low hanging fruit to improve performance:  look to tune your origin keep-alive timeouts where appropriate to benefit most from persistent connections. For us this has helped us yield around 100 to 150 millisecond speed ups. In parallel to all of this work, the team also wanted to find a way to improve the observability we had at the network edge.  CloudFront itself comes out of the box with a range of different metrics and logging. These might range from giving us insights into where  our users are coming from, the types of requests that we're getting, and even the responses that we're sending back. But these are all from the perspective of CloudFront.

[![Thumbnail 2860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2860.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2860)

[![Thumbnail 2890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2890.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2890)

Our team  wanted to enhance the visibility we had into the network and enrich our overall view. This is exactly where Network Error Logging came into play. For a little background, Network Error Logging is a client-side mechanism that is supported by a variety of browsers, particularly Chrome and Chromium. It can generate and send reports about network issues as seen from the client side. As members of the network edge, this data about the availability of our edge as seen by our clients  can be incredibly valuable when diagnosing a variety of network issues, including ones about issues that might never reach our infrastructure.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2910.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2910)

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2920.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2920)

So what did this implementation look like? Believe it or not, it wasn't overly complex. By getting our reverse proxies to add the report-to and NEL headers on all client responses,  we can tell supported browsers where to report errors to. Leveraging CloudFront to act as a globally accessible endpoint, we run a Lambda@Edge function to intercept  and process all requests to be sent towards the Kinesis endpoint before further processing by our observability and monitoring services. This architecture allows us to leverage CloudFront and Lambda@Edge to serverlessly process these network error logs into Kinesis.

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2940.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2940)

This enhanced visibility has already proved useful  when investigating low-grade network failures. One example of this was when we were investigating an intermittent TCP reset issue impacting a percentage of customers across a random country. These errors were occurring before requests even reached our CDN infrastructure. But thanks to our serverless Network Error Logging architecture, we were able to gain insights on failed requests that were not yet visible from our CloudFront metrics or logs. This information has helped us accurately identify start times, error rates, affected clients, geolocations, and the exact TCP error itself.

[![Thumbnail 2990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/2990.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=2990)

### Scaling Operations with CloudFront SaaS Manager and Key Takeaways

By collaboratively working with Amazon, this allowed us to confidently and accurately identify the root cause of this connection issue pertaining to a network change outside our own infrastructure. Alright, we've talked about WAF,  Shield, and a few different ways we're using CloudFront distributions across a range of our products and services. There's one last product that the CloudFront team has shipped that I would like to share how the team is looking to use, not just to scale our services, but also to scale the operational effectiveness.

[![Thumbnail 3010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3010.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3010)

Everything I've discussed so far has been related to standard distributions.  These distributions are singular and contain all the settings pertaining to origin configs, cache behaviors, and security settings. What this has meant for us is that by deploying these distributions in front of a variety of our products, it has been an involved task between members of our edge team and our service owners, where we provision the distribution for them and then begin this collaborative effort to transition traffic across, tune a variety of security rules, and smooth out the rough edges.

[![Thumbnail 3050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3050.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3050)

[![Thumbnail 3070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3070.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3070)

With that in mind, Atlassian has thousands  of ancillary services which range from supporting features and functionality on our main product to internal tooling. This approach of standard distributions simply did not scale with our DevOps model and philosophy of you build it, you run it.  Operating and managing thousands of distributions at scale in a conjoined way created too much friction. We were hitting service limits, account limits, and it was going to be too operationally painful to issue our parent level configuration for these shared distributions.

[![Thumbnail 3090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3090.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3090)

We really wanted a way to have the  flexibility of enforcing a strict base configuration that the edge team could manage, control, and roll out, while allowing our service owners the ability to own and run their own CloudFront distributions without needing to be CloudFront experts. This is exactly where CloudFront SaaS Manager played a huge role in unblocking this operational challenge. CloudFront SaaS Manager has a construct called multi-tenant distributions, which act as a parent template for child distribution tenants to inherit.

[![Thumbnail 3130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3130.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3130)

[![Thumbnail 3150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3150.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3150)

This effectively gave us a clean demarcation point for what and how our team manages the global configuration  that we expect all distribution templates to adhere to, while giving our service owners the flexibility and ability to load parameters into these defined templates, to define custom origins, domains, and even apply customizations to their specific distribution WAF. In practice, how did this look?  We deployed numbers of distribution tenants based on use case to help improve reliability and reduce blast radius, including the addition of a canary style template to soak initial changes on.

[![Thumbnail 3170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3170.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3170)

[![Thumbnail 3190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3190.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3190)

Next, we integrated a service broker for our service owners to interact with, which provisions a distribution tenant on behalf of them.  This additional broker also helps us enforce final base configuration elements which we want to restrict our service owners from having the flexibility to adjust, such as specific inclusions of WAF rules we want permanently enforced. As a result, this will pave the way for our ancillary services and their service owners  to onboard onto CloudFront and WAF, being able to do so in a self-service and self-managed model, leading to increased speed of CloudFront adoption and allowing our developers to focus on building more features at the edge, opposed to the handholding of individual CloudFront adoptions.

[![Thumbnail 3240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3240.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3240)

[![Thumbnail 3250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3250.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3250)

As I wrap up, I want to reiterate on a couple of key takeaways. First, look to leverage CloudFront's new Anycast static IP feature in scenarios where your customers have specific firewall requirements. In multi-origin and region scenarios, think of how you can leverage the new origin control helper method to steer traffic to different origins.  This can potentially help reduce or even avoid cross-region data transfer fees. When you need a scalable multi-domain solution while keeping configuration and infrastructure simple, consider the use of CloudFront SaaS Manager. And finally,  Ben, a principal engineer in our team, has open sourced the Python Lambda function which prepares CloudFront logs sent to S3 for Kinesis. This can help save a lot of money versus real-time logs at scale.

[![Thumbnail 3290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3290.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3290)

I would like to thank you for taking the time to listen to our story and our journey, and I hope you have all found something useful to take away. I will now pass it back to Etienne. Thank you everyone for listening. We hope that you have enjoyed today's session. I want to highlight that we have a number of sessions that will dive much more deeply into a number of the topics that we have discussed today.  I encourage you to look them up. This is worth taking a photograph of. If it is too small, have a search for any session that starts with NET, and that will get you into the realm of these sessions.

[![Thumbnail 3310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3310.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3310)

[![Thumbnail 3320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/84cedfedf72fb3b7/3320.jpg)](https://www.youtube.com/watch?v=j8uPcCJQGYo&t=3320)

Lastly, I encourage you to go build. It has never been a better time to go build with CloudFront, and especially when you start layering in Shield and WAF.  Thank you again for your time today, and  please provide some feedback for us. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
