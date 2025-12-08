---
title: 'AWS re:Invent 2025 - 5-Star Customer Service: Duolingo''s Path to Compute Savings (CMP349)'
published: true
description: 'In this video, AWS and Duolingo engineers discuss how Duolingo achieved significant cost savings and performance improvements by migrating to AWS Graviton processors. The session covers Duolingo''s journey from managed services (ElastiCache, RDS) to microservices migration, achieving 75% Graviton adoption for managed services and 30% for microservices. Key results include 20% task count reduction, 10% faster deployments, and over 10% EC2 cost savings by expanding their instance pool with EBS-backed instances. The presentation also highlights how their Technical Account Manager partnership enabled successful high-profile events like the Super Bowl ad campaign, ECS to EKS migration planning, and optimization initiatives across S3 Storage Lens, CloudFront, and DynamoDB that delivered 50% cost reduction. Duolingo targets 50% microservices on Graviton by 2025.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/0.jpg'
series: ''
canonical_url: null
id: 3089009
date: '2025-12-06T13:45:58Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - 5-Star Customer Service: Duolingo's Path to Compute Savings (CMP349)**

> In this video, AWS and Duolingo engineers discuss how Duolingo achieved significant cost savings and performance improvements by migrating to AWS Graviton processors. The session covers Duolingo's journey from managed services (ElastiCache, RDS) to microservices migration, achieving 75% Graviton adoption for managed services and 30% for microservices. Key results include 20% task count reduction, 10% faster deployments, and over 10% EC2 cost savings by expanding their instance pool with EBS-backed instances. The presentation also highlights how their Technical Account Manager partnership enabled successful high-profile events like the Super Bowl ad campaign, ECS to EKS migration planning, and optimization initiatives across S3 Storage Lens, CloudFront, and DynamoDB that delivered 50% cost reduction. Duolingo targets 50% microservices on Graviton by 2025.

{% youtube https://www.youtube.com/watch?v=acRBzghbhE4 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/0.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=0)

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/20.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=20)

### Setting the Stage: Duolingo's Cloud Cost Challenge

 Before we get started, I just want to get a sense of who's joining us in the room today. So by a quick show of hands, how many folks out there have ever looked at their cloud bill and thought there has to be something better? A few hands up for that. Thank you. How many of you maybe worked with a technical account manager, solutions architect, a real business partner, doesn't  have to be at Amazon, who's really made a difference in your business? All right, a few hands up for that. That's great, thank you. How many people have heard of Graviton processors but have not yet had a chance to implement them in your business? All right, a few hands up. All right, keep these things in mind because I think the presentation today is going to resonate with many of you.

So here's the story. It's 2024. Somewhere in Pittsburgh, a Duolingo engineer is looking at their quarterly cloud spend report. You know the feeling, the numbers are growing faster than you expect. But your features are growing, your users are expanding, and it feels like if you start cutting costs, you might be cutting corners. Now Duolingo isn't just any company. They're the app that's taught over 500 million people a new language through that persistent green owl that we all know and love. They process billions of learning events, serve personalized content to users around the world, while maintaining a user experience that keeps us coming back day after day.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/100.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=100)

All right, one more question. How many people have a streak going in their Duolingo app right now? All right, lots of hands. Love it. That's great. But here's the thing about success, it scales. When you're Duolingo, scaling your infrastructure, costs scale too. So the question becomes, how do you maintain this growth trajectory while optimizing for efficiency?  That's what we're going to talk about today.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/120.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=120)

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/140.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=140)

It started with a conversation between Duolingo's technical account manager, who's someone who knows that Amazon's founding principle isn't just about offering lower prices, but about finding innovative ways to deliver exceptional value. During this session, we'll hear directly from both sides of that conversation.  We'll start off with an introduction to AWS Graviton processors and learn about the methodology that took Duolingo from cost savings analysis to proof of concept to enterprise-wide implementation. You'll also discover how AWS Graviton processors  became not just a cost saving measure for Duolingo, but a competitive advantage. And most importantly, you'll learn how the right partnership can take what feels like a constraint, your cloud budget, and turn it into an opportunity for innovation.

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/160.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=160)

### Why Amazon Builds Its Own Chips: The Nitro System Foundation

So whether you raised your hands to any of those questions or you're simply curious about how two organizations can partner together, this is your story. I'm joined by Brent Everman and Jean-Paul Bonny. Let's dive in.  So I get this question a lot. Why do we build our own chips at Amazon? There's definitely choices in the market that we could use. The idea at Amazon is we work backwards from the customer experience and specialization was key. We spent a lot of time operating and building cloud infrastructure and running cloud applications. So we were able to take that specialization and build that into our hardware.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/210.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=210)

Speed and innovation are also key. By having hardware and software engineers under the same roof, we're really able to deliver for customers at a higher pace. And then security. Security is the highest priority at Amazon, and the Graviton processors actually came from building security chips, which we'll talk about here in a minute, and we turned them into the general purpose processors that we use today. So those chips came in what we call the Nitro System. How many people are familiar with the Nitro System?  A few hands up for that. That's great.

It's a combination of hardware and software. The chips provide a simple hardware root of trust. They measure the contents of the flash devices so we can be sure what's running when the firmware is running is what we expect. It monitors all the hardware interfaces and blocks unauthorized modifications. We designed the Nitro System to provide confidentiality between customers as well as confidentiality between customers and AWS. There's no mechanism or system or person that can log in to an EC2 Nitro-based server to access customer data. There's no root user, no interactive access, just a set of narrow and authenticated APIs that we use to manage the systems. I encourage you to take a look for more details on that on our website. We have some third parties that have validated that.

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/260.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=260)

So this is what a Nitro System looks like today. When we started, we had customer instances running  with a Xen hypervisor. For those who have used AWS for a while, Xen was where we started. And for those of you who run data centers, 20% is pretty normal as an overhead for a hypervisor. Xen was great. It did a lot and took all these functionalities, handling connection, VPC networking, connections out to EBS and so on, but it used those resources on the host CPU. So we wanted to start offloading that to hardware-based devices.

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/300.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=300)

We moved IO functionality to Nitro cards and also moved management and monitoring and the security functions. Then we were able to replace Xen with a lightweight hypervisor. What did that do for us? That allows us to sell almost 100% of that CPU to customers, allowing us to keep our prices low. 

So I talked about the pace of innovation. You can see the growth of the EC2 instances over the years. We can think of Nitro as a Lego building block that lives underneath all of AWS servers, and it was able to significantly accelerate our engineering pace. In the first 11 years, it took us 11 years to grow from 1 to 70 instances. Then with Nitro, once we added that, we were able to go to 950 instances in just 8 years. So these 950 instances provide our customers with the flexibility to choose the optimal instance type for their specific use case.

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/340.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=340)

### Decoding EC2 Instance Naming and Understanding Compute Options

 So with all those instances, how do you choose? I'll talk through three of our most popular series: C, M, and R. These are compute optimized, general purpose, and memory optimized. What does that mean exactly? It's really a memory to vCPU ratio, which you can see on the screen. It goes up as the instance family changes, but what I'm about to talk through as we decode this will work for all of our instance families. You'll be able to see how this works.

So we're looking at a C8GN.2xlarge. What does that mean? C tells me the series. I know this is a compute series instance, which means that for every vCPU I have 2 gigs of RAM. It's the 8th generation, so you can see where it sits in the lineage and the history of our instance families. This instance has two options: GN. The G tells us this is a Graviton instance, which is what we're going to talk about today. N tells me there's some specialized networking for this particular instance, a little different than standard.

Now, it's a 2xlarge. That tells me how many vCPUs: medium, large, extra large, 2xlarge. We're doubling each time, so this is 8 vCPUs. If we look at this, this is a compute-based instance with 8 vCPUs. That tells me I have 16 gigs of RAM based on the ratio, and the networking feature is up to 50 gigs of additional networking, and it's based on Graviton. Like I said, this will work across our instance families.

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/430.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=430)

### The Evolution of AWS Graviton Processors

So a little bit of history of Graviton. We started in 2018.  We work very closely with ISVs. We know that customers are using third-party applications. Maybe you have a security agent or an observability agent that you need to run in your environment with your own applications. So we leverage this heavily to get those ready for customers.

In 2019, we launched Graviton 2 with performance increases, and this is where Graviton starts to become more widely adopted. Lots of customers are using it, also using it heavily at Amazon. Graviton 3 launches in 2021. Again, compute performance improves, but our customers are telling us they need better floating point, so we improve that for customers as well. Having that software and hardware teams in-house allows us to do that more rapidly.

Then in 2023 we launched Graviton 4: 30% better performance. But now our customers are saying, I want to run larger workloads, maybe a database, something that needs more cores. So we improved the number of cores, improved the memory bandwidth from Graviton 3, and got that out into our customers' hands.

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/490.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=490)

### Graviton Adoption Across AWS Services and Sustainability Benefits

 How can you use Graviton today across EC2? General purpose, compute, and memory optimized, we talked about, right? You can see the different options available that are out there. Even burstable instances, our T series, are available. Maybe you need something more specialized. Accelerated computing: you need a Graviton processor and an Nvidia GPU. Maybe you're doing something that's storage intensive, so you need some fast SSDs, local NVMe storage locally, things like that. The I series will help you with that. Then we can get into HPC optimized if that's something you need. We can help with that part as well with the HPC7G.

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/530.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=530)

Some other ways to look at adopting Graviton. You'll hear some of this from Duolingo today.  Managed services are a great place to get started. We've done the heavy lifting and got the engines running on Graviton, and you just bring your data. Maybe you've got a database that you're using RDS. Maybe you're using ElastiCache. We take care of getting that running for you, and you get the price performance benefit of using Graviton across those managed services.

Analytics is another great place to start. I talk to customers all the time. They start with EMR. They're already using it in some form. They're able to change that over to Graviton and get that price performance benefit very quickly and very easily in their environments. For compute, Duolingo is going to talk today about EKS and ECS, but you can also consume Graviton under Fargate and Lambda as well. Then maybe you're using Amazon SageMaker for machine learning workloads. You can consume Graviton instances under SageMaker as well.

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/580.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=580)

 All right, one more question. How many folks maybe have a sustainability goal for their organizations? A few hands up for that. Great. So this is one of the things about Graviton: 60% less energy to compute the same workloads. We work with a lot of customers. Sometimes you're looking for that compelling event for customers. We have found sustainability is key. They want to be able to show that they're more sustainable, and Graviton is a way to do that. My team works with customers to help them understand their power consumption today.

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/610.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=610)

### Amazon's Internal Success with Graviton on Prime Day



[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/660.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=660)

So last thing before I sort of hand it over to Brent here, but this is how we're using it at Amazon. Prime Day through the last few years. In 2022, we were able to leverage Graviton to minimize the scale of our compute workloads for Prime Day. So our business was growing. Our Prime Day workload for 2022 was only 7% larger, right? That's the idea. We're able to maintain the growth while the retail business was growing. In 2023, over 2,600 services were powered by Graviton processors. In 2024, over 250,000 AWS Graviton chips were used to power Prime Day, and this past October, about 40%, more than 40% actually of the EC2 used by Amazon.com on Prime Day was powered by Graviton. 

So we're taking advantage of it and getting that scalability and that flexibility for our organization and making the same available to customers. So with that, I'm going to hand it over to Brent because he's going to talk about how Technical Account Managers work with customers.

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/700.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=700)

### The Role of Technical Account Managers in Customer Success

All right, thank you Seth and how many people here know what Enterprise Support is, a show of hands. All right, a few people and so for those of you who don't know what it is, Enterprise Support is one of our support offerings from AWS, and with it comes access to a Technical Account Manager. And so how do Technical Account Managers or better known as TAMs help enable customers? And so this is the diagram that we use  when we talk to customers who are looking to enroll in Enterprise Support. What is the value of a TAM?

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/760.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=760)

So first and foremost, TAMs are focused on helping customers achieve their business outcomes, whether that be user growth, reducing operational overhead, or leveraging Gen AI. TAMs also help educate customers about how to leverage AWS services and there's a big difference between building a POC on AWS and then operating that in a mission critical production environment and TAMs leverage their expertise and best practices to help customers do that. And then we also help elevate customers' voice with service teams in our leadership, especially when there are blockers with adopting or operationalizing a service or a workload on AWS. And lastly, we help customers evolve by proactively learning about their initiatives, discussing findings, and diving deep to drive technical solutions. 

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/770.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=770)

### Planning and Executing Duolingo's Super Bowl Campaign

And so now we're going to talk about how as a TAM, I'm the TAM for Duolingo, helped drive some of their strategic outcomes.  So the first thing we're going to talk about is high profile events. TAMs help customers plan and execute these high profile events on AWS. We leverage a structured approach called AWS Countdown to plan and execute these events on AWS. And the example that we have with Duolingo is the Super Bowl ad they did in 2024.

So how many of you here remember that ad or even received the notification that we have a screenshot of on the screen? All right, a couple people, cool, so you're going to learn more about this ad and the impact that it had for Duolingo. And so Duolingo came to us and they had never done anything at this scale before. They were looking to send 4 million notifications within 5 seconds of the ad airing during the Super Bowl. And they've never done anything of this scale and came to AWS and myself were like how do we do this? How do we get to this scale? They built out an architecture and so I started engaging with them using that structured countdown approach to plan and execute this event on AWS.

And so we started by reviewing their architecture, highlighting risks and gaps. I was also basically a part of their engineering team attending their weekly engineering team meetings where we discussed status, risks, and issues for the project. I also helped them secure enough EC2 capacity for the event. As you might imagine, sending that many notifications in 5 seconds took a lot of EC2 capacity to do that. And then also help them outline fallback plans should there be potential issues and lastly I worked alongside of them to conduct test events and I cannot stress the importance of those test events enough.

We conducted those test events over Halloween and Thanksgiving and each time we learned something new that we took back to improve the plans and the architecture for the actual event. And so during the event, you know, I joined the War Room Bridge with the Duolingo team just like I was a part of the team, and I remember getting that nervous feeling because we had months of preparation on the line, and I remember, you know, they had built a nice

UI where they had a button to send out the notifications. I remember hitting that button and those notifications going out and the relief that followed. They did successfully send out those 4 million notifications within 5 seconds of the ad airing.

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/930.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=930)

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/940.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=940)

From a business perspective, this was really about re-engaging those lapsed users. If you use Duolingo at all, you get notifications every day  if you don't do your lesson. They were looking to engage those users who hadn't done their lesson in a while  and this was driving towards their business outcome of user growth. From a Duolingo perspective, this was a very successful event that we put months of planning and preparation into.

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/970.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=970)

If you had seen it or if you're on social media at the time, you probably heard about it because it was a viral hit. If anyone didn't see the ad, it was basically Duolingo's mascot blowing up, as you can see on the screen over here. That was even discussed on the Late Show with Stephen Colbert as well. 

### Migrating to a New AWS Organization for Compliance Certification

Now moving on to another topic. For those of you who aren't very familiar with Duolingo, they also have the Duolingo English Test, or DET. This helps to validate a learner's proficiency in English. Duolingo was a born-in-the-cloud startup, and like many born-in-the-cloud startups, they built everything in a single AWS account. Everything was running in a single AWS account, which eventually became the management account of the organization.

This structure was preventing them from achieving some compliance certifications that they needed to expand DET and grow the product. Many of the organizations and universities that they work with wanted these compliance certifications. I started working closely with the Duolingo team to look through what are the options to help meet some of these compliance certifications. We determined that migrating to a brand new AWS organization and then breaking out that DET workload into its own AWS account was the best path to help achieve that.

I started working with the Duolingo team and creating a step-by-step migration plan to migrate to this new AWS organization and help them execute it by their side. One of the things we saw an opportunity for improvement around was security. In their previous structure, due to having all those workloads running in the management account, they weren't able to implement some security best practices. We took this as the opportunity to implement the security best practices like Service Control Policies, or SCPs, if you're familiar with those.

We were able to execute this migration to the new AWS organization with no impact to their production workloads or downtime at all. The next step then was to migrate the DET workload over to its own AWS account. We took a very similar approach where we created that step-by-step migration plan. I worked very closely with them to migrate that to its own AWS account, and we were able to do that with minimal impact to production for the DET product.

What did this all result in? Due to these efforts as well as other efforts they had internally, they were able to achieve these compliance certifications, which then opened up the door for growth for the DET product.

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/1150.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=1150)

### Supporting Duolingo's Migration from ECS to EKS

How many of you here have been a part of a migration from on-premises to AWS? Show of hands. A lot of you may have horror stories from those migrations, and that's oftentimes what you think about in terms of migrations in AWS from an AWS perspective. 

But we also support migrations like this one from ECS, or Elastic Container Service, to EKS, which is Elastic Kubernetes Service. Duolingo, as I mentioned, is a born-in-the-cloud startup, and they started by running everything in ECS because ECS is super simple to use. You build a container, you throw it at ECS, and ECS does most of the heavy lifting for you.

But as Duolingo started to grow in scale, they knew they needed some additional features and the ability to customize their compute platform for their requirements. After thorough analysis on their end, they decided that migrating to EKS was the best path for them for the next evolution of their platform. Myself and the broader AWS account team started working with the Duolingo team from the very beginning on this migration.

We first started off with enablement and education, and some of you may be familiar with immersion days, which is where you can get hands-on with an AWS service. A lot of the Duolingo engineers didn't have any exposure to Kubernetes or EKS prior to this, so we decided to conduct these hands-on immersion days to give them a better understanding of the concepts, how EKS works, and how to deploy their services on EKS.

One of the things we heard during those sessions was that the Duolingo engineers were somewhat hesitant about this migration. They were wondering, is this the right path for us? Is this the right thing to do for us? We spoke with the project lead at Duolingo about how to address these concerns, and we decided that we would present at their internal engineering summit. Duolingo hosts an internal engineering summit every year where they bring all of their engineers together and they present on best practices, new projects they've worked on, and new technologies they've been using. We brought in one of our Kubernetes experts to talk about what is the value of Kubernetes and what it can unlock for the organization.

I remember one of the engineers in the crowd coming up to us after the talk, to myself, the Kubernetes expert, and the project lead, and saying something along the lines of, I was very hesitant about this migration. I wasn't sure if it's the right thing for us, but now I see the value and I want to be one of the first services to migrate to EKS. A lot of these enablement efforts helped instill confidence in the broader Duolingo organization that this was the right move for them.

As Duolingo started building out this new EKS platform, we conducted targeted deep dives. We knew they wanted to use Argo CD and Karpenter, and so we conducted deep dives on those to ensure they were properly set up and configured in their environment for their requirements. AWS Enterprise Support and TAMs can provide proactive guidance on many AWS services, and we knew that with the size of Duolingo's ECS environment there could be potential challenges with scaling with EKS. The EKS team recommends keeping the number of pods and nodes within a cluster below a certain threshold, although some of that has changed with some of the recent announcements.

We wanted to get out ahead of that and ensure that they were not only scaling the nodes and the pods in an optimized manner, but also we dove into other areas like observability and CoreDNS to ensure they were set up in an optimized manner. One of the other things that we worked with them on was a multi-cluster platform. Duolingo wanted to design this multi-cluster platform for a couple of reasons. The first one was, should they encounter these thresholds, they wanted to be able to move workloads between clusters, and they wanted the ability to do maintenance. If they had maintenance or upgrades they needed to do on a cluster, they didn't want to have to have production downtime for those. We worked closely with the Duolingo team to design this multi-cluster platform that has been a really big win for them.

Lastly, on the networking setup side, for those of you who know, IPv6 has been out there a while in the community, but I would say it's not often been used and so there's not a lot of expertise in IPv6. One of the things that the EKS team recommends is using IPv6 because it can help with things like IP exhaustion that can occur with IPv4. I worked closely with the Duolingo team to ensure their networking with IPv6 was set up in an optimized manner, not only from a cost perspective but also a security perspective.

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/1480.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=1480)

What did all these efforts result in? Duolingo has built out their new EKS platform, and so they have started migrating workloads in 2025. They've even migrated some of their most critical services over to EKS, which shows the trust that they have in the platform, and we are continuing to work with them into 2026 to complete this migration. 

### Cloud Optimization: S3, CloudFront, DynamoDB, and EC2 Instance Pool

TAMs and Enterprise Support are expert in cloud optimization. We want to ensure your environment is optimized for the business outcomes that you are trying to achieve. There are three main pillars that we look at from an optimization perspective: cost, security, and resilience.

I'm going to talk about a few examples of how I've worked with the Duolingo team to optimize their environment. The first one is around S3 Storage Lens. I started working closely with the Duolingo team three years ago, and one of the things I noticed is that they didn't have a lot of visibility into their S3 buckets. Costs kept growing month over month, but they weren't sure what was driving that. Was it the increased user growth, or was it something else driving those increased costs?

I started to dive deep into it and recommended a newish service at that time called S3 Storage Lens, which can provide visibility across your S3 buckets at the AWS organization level and can even provide visibility down to the prefix level within a bucket. After setting this up, we started to dive into the data that it provided. This has been used for many different use cases across Duolingo, but I just wanted to highlight a few of the wins that we were able to achieve by leveraging S3 Storage Lens.

The first one was we identified a bucket that was using S3 Standard storage, and we were able to migrate that to use Intelligent Tiering, thus saving $13,000 a month. Next, due to the visibility in the metrics that it provides, we were able to identify an unused one petabyte bucket and delete that bucket, so that was significant cost savings as well. And then lastly, during the data center migration, we were able to leverage the visibility metrics it provided when we had to migrate S3 buckets from the old account over to the new account to ensure that migration was fully complete.

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/1620.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=1620)

Now moving on to CloudFront. One of the things that TAMs can do for their customers is conduct architecture  reviews. When I was conducting the architecture review for Duolingo, I noticed that their mobile app was directly connecting to their Application Load Balancer or ALB. While that's a perfectly acceptable architecture, I knew that with Duolingo's global user base, they could benefit from having CloudFront sit in between there. With all of the global CloudFront edge locations, they could reduce user latency by having them connect to those edge locations and route over the AWS backbone instead of routing over the public internet.

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/1680.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=1680)

I worked closely with the Duolingo team to introduce and implement CloudFront between their mobile app and their Application Load Balancer. While that was a big win and reduced latency for their users, it also opened up additional optimization on the security and the cost front.  We have projects planned in 2026 where we're going to look to improve security by leveraging AWS WAF or Web Application Firewall, where we're looking to block malicious bots at the edge. We're also looking at improving caching within CloudFront, which will reduce hits to the origin and thus optimize on cost as well.

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/1710.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=1710)

Now moving on to DynamoDB. Duolingo was heavily using DynamoDB,  but costs kept growing month over month, and it was the second highest line item on their bill. When talking with the Duolingo engineering teams, they weren't very confident that they were using DynamoDB correctly and if they were using it in an optimized manner. There was also some hesitancy internally to expand the adoption of DynamoDB, and they sometimes stuck with relational databases like RDS even when they knew DynamoDB was a better fit for the use case.

I saw this as an opportunity to conduct enablement sessions. Similar to what we did for EKS, we conducted those hands-on enablement sessions with the Duolingo engineers to give them a better understanding of DynamoDB concepts, how to optimize for DynamoDB, and how it works under the hood to give them a much better understanding of DynamoDB and how to work with it in an optimized manner. During those sessions, one of the things we heard is that they were sometimes not sure how to model their tables correctly within DynamoDB. They had a lot of experience doing this in a relational world, but not a lot of experience with single table design.

In talking with Duolingo engineering leaders, we decided to conduct another session at their internal engineering summit on best practices for modeling tables with DynamoDB with one of our DynamoDB experts,

and so we conducted that session to a standing room only crowd at their internal engineering summit, which had rave reviews. That kind of concluded our enablement piece for DynamoDB, and next we wanted to focus on optimization. There were a handful of tables that were cost drivers for the organization, and so what we decided to do was conduct targeted table deep dives on those and better understand the use case, their access patterns, and then make recommendations for optimization and help the Duolingo engineers implement those optimizations.

What did this all result in? You can see with the graph up here, within three months from when this program began, they realized a 50% reduction in their DynamoDB cost, and that came from those targeted table deep dives and the optimizations that were implemented as part of it. We also optimized their reserve capacity for their tables that were using provision throughput. Lastly, we converted some tables that were write heavy, read less to infrequent access storage. I'll say even the more telling success of this was, you know, you kind of see how usage increases over the months after that. That wasn't due to Duolingo engineers going back to their old ways. It was actually because Duolingo engineers felt much more confident in using DynamoDB because it helped accelerate their development, and it became the database of choice for Duolingo. I sometimes joke that DynamoDB is Duolingo's favorite AWS service.

[![Thumbnail 1910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/1910.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=1910)

The last one I'm going to touch on is around  EC2 instance pool optimization. As I mentioned earlier, Duolingo runs everything on ECS and now is in the process of migrating to EKS. So all of their containers run on EC2, and optimizing their EC2 instance pool can have huge impacts on not only performance but also cost. Jean-Paul, who's coming up next, is going to dive into a lot more details on this, but I wanted to cover this at a very high level first. I've helped them migrate to and adopt the latest version of Graviton, Graviton 4, helped them migrate additional services from just using Intel over to using Graviton. We've helped eliminate some of the older generation of instances that they had in their instance pool that were less cost performant. Lastly, they had been only using NVMe backed instances, and so I helped them introduce EBS backed instances.

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2000.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2000)

### Duolingo's Journey: Adopting Graviton for Managed Services and Expanding EC2 Instance Pool

All right, and now I'm going to welcome JP on the stage, who's going to dive into more details on this. Thanks, Brent. So I'm going to talk about our Graviton journey in a few steps. First, how we utilize managed services with Graviton at Duolingo. Then I'm going to talk about how we accelerate that Graviton adoption by optimizing, expanding our EC2 instance pool. Finally, moving actual microservices onto Graviton, and then what's next for us in the world of compute. 

So if managed services of Graviton, once we first heard about this, we were really excited to start utilizing these for the immediate cost savings, but also given how much work we'd have to do. I mean, AWS handled all the crazy compatibility issues, so all we'd have to do is just upgrade our instances, right? Well, there's a little more to it than that. We had to come up with migration plans to make sure we could move our managed services over with little to no interruptions for our learners. Seth mentioned a lot of services that have managed services that have Graviton support, but I'm going to focus on ElastiCache and RDS.

[![Thumbnail 2040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2040.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2040)

So for ElastiCache, this is actually a pretty boring story for us, which is what makes it very exciting for me to talk about today.  You never want to have a crazy exciting story, right? That would take too long. So thankfully, because of all the work AWS did with all the compatibility details and all the other issues, we could just focus on upgrading our caches like we would any other time. So we wound up migrating a lot of R5 instances to R6Gs, and let's talk about the different engines that we had to deal with when moving lots of cache.

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2070.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2070)

So let's start with Memcached on the left here.  So essentially we'd have our microservices that run with Memcached should have of course the cache. And in order to mitigate risk, we actually wound up creating a new Graviton cache. So we actually had our services writing data to both caches, and we did this kind of, like I said, to mitigate risk. So while we were still having data being written to both caches, we were slowly shifting, and once we felt comfortable with all the data in our Graviton cache, we started to slowly shift reads over to that new cache away from the old one. And then once everything looked good and nothing was crazy on our graphs, we would then start to spin down connections to the old x86 cache, decommission it, and we could count our cost savings and be on our way.

Now looking at Redis, this is a little bit of a different story since ideally it should be as simple as Memcache, but if you're running a couple of old Redis clusters, they might not have the latest version, you could introduce having some downtime. And we wanted to naturally mitigate that. So some of the ways we did this was looking at our applications, seeing if we had solid default values for these clusters, and also looking at trying to upgrade these when we had lower volumes of traffic.

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2150.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2150)

So once we felt comfortable with all the mitigations in place, we then went to the console, changed our instances from those R5s to R6Gs. Our data is intact, we had a little reboot and then things were fine and we had our Graviton cache.  So now RDS is a similar story to ElastiCache, but it can be a little more complex due to the auto-scaling readers, writers, and probably just those crazy applications you can have on RDS. So it can be scary if you especially have to do a failover or a database upgrade for something critical, but we still approach this as we wanted to, of course, minimize downtime and make this easier to do in the future.

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2180.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2180)

So a show of hands, who has a service running on RDS? Anyone? Okay, yeah, a lot of you. So  in this case, this flow chart behind you might look a little familiar, but if not, let's talk about it. So thanks to RDS being a managed Graviton service, it was as simple as just doing a typical RDS upgrade where we would add our new instances to our RDS cluster, and then we'd have to, of course, wait for us to get online data syncing, so we might be waiting a little bit depending on how big your cluster is. And once that's available, we can then start moving those old instances. Everything's pretty good so far.

And then finally we get to the scary part, doing that writer failover. Now sometimes even upgrading a couple of databases, I still get a little scared doing this, but once we're able to do that failover, of course mitigating risk with those applications, our writer will now change. Things are fine, looking good, and then we can deploy our service, clean up our references to the old RDS, and just make sure things are running smoothly again. So we wound up applying this methodology to both our Aurora and Postgres workloads, and after doing this a few times for a couple of upgrades, we thought to ourselves, hey, let's try and minimize the amount of engineering effort required, and we created a Temporal workflow to help other engineers and service owners to do this without having platform support.

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2270.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2270)

And we wound up doing that at least 10 times to mainly accelerate that Graviton managed services adoption and even resizing instances as our traffic grows. So with all that said, we now have over 75% of our managed services running on Graviton just because of all the planning and work that we've done in our org.  And just given how easy Graviton is to use whenever new service owners or teams want to spin up new managed services, Graviton is the default option going forward. So now we're saving a little money with our managed services on Graviton, but we want to step it up to get some bigger cost savings for our workloads.

[![Thumbnail 2290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2290.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2290)

 So of course enter EC2. It's the largest item on our bill, and we use it for a lot of things. So as Brent said at Duolingo, we were running most of our EC2 on NVMe instances, and at the time that was because of all the data we had. But over the years as we've been migrating workloads around, we've been reducing logging, improving observability. We kind of had this thought to ourselves, do we really need all this storage? And then we also have this other goal of wanting to be able to adopt newer EC2 instances quicker.

So instead of waiting for that NVMe specific flag on that decoder ring, we want to just run on a wider variety of instances. So essentially we wanted to take that letter D out of our instance type thing and expand our EC2 pool. So for some context for us, our EC2 setup isn't just simply using stuff on demand. We do use a good amount of the spot market for powering our workloads. And we also work with AWS Technical Account as well as several third parties to analyze our EC2 usage to make sure we're diversifying our workloads across spot, on-demand, reserved instances, savings plans, all the typical EC2 things.

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2380.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2380)

So with all of this said, how do we go about trying to tackle this instance pool instance type challenge while still staying on the spot market and utilizing our cost savings? So first, let's actually just talk about the spot market for a little bit. So as I said, at Duolingo we're actually leveraging it pretty heavily, so we're essentially using spare EC2 capacity  to power most of our microservice workloads. And these come with a very significant benefit where it's up to 90% off the on-demand price.

Now that sounds pretty good to me, but there is a trade-off we have to consider, and that is spot interruptions. So whenever the on-demand pool is seeing issues with their EC2, they will reclaim the spot instances. So you essentially have two minutes to vacate your workloads, and then that machine will get reclaimed.

So how do we at Duolingo deal with this trade-off? We've designed most of our microservices to support something called graceful shutdown and of course auto scaling.

Graceful shutdown essentially means whenever we get that underlying SIGTERM signal from an EC2 instance, our ECS tasks will see that signal and then they will reject incoming requests from the load balancer, and those will get routed to another task behind it. But it'll still process in-flight requests, so ideally our learners aren't going to see any disruption while we're still processing their requests for saying a Shriek or doing a lesson. And while that's also happening, and while that task is shutting down, we're spinning up another task on another machine that's hopefully not getting interrupted.

So that's actually one way we've been able to stay on the spot market while having little to no interruption. And then also another point about being on the spot market is just diversifying your workloads, of course running your EC2 in multiple availability zones, but also having a breadth of instance types. So with that said, let's actually start talking about how we went about adding more instance types to our pool.

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2480.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2480)

So with EBS we had to have a couple considerations.  First off being disk space. I know I mentioned this a couple of times, but how much space do we really need? I mean these are really big NVME drives. The second one being spot interruptions. We don't want to spend our time spinning up new EC2 machines and we want to make sure these aren't going to be reclaimed in case of interruptions. And then finally rolling this out, how do we roll this out and in worst case, how do we roll back if it all kind of goes wrong?

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2510.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2510)

 So first looking at disk space, of course as you can see from the screenshot we have a lot of room in these EC2 instances and we first wanted to kind of get a feel for how our microservices were using these. So naturally I decided to just take an EC2 machine, SSH in, and do a disk screen and look at it. So the first thing, this is a C5D 18X large. There's several workloads utilizing the CPU and memory before I bin packing, but as you can see, look at the purple rectangle, we're not really using that disk space.

There's a lot more room for other things that could be there. So we're only using 4% of that NVME drive and then about 6% for that EBS volume. And then looking at this gave me the confidence to kind of look at the rest of our fleet, and we saw similar patterns for most of our microservices. Now we still had some workloads that still would need that NVME drive, whether they're reading or writing from disk, or they just have a lot of files they're storing.

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2580.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2580)

But thanks to using ECS we were able to utilize task placement constraints where those workloads could specify the type of machine they needed. So in that case they could still deploy, get those NVME machines while most of our other workloads were more compute agnostic and they could just  potentially use other EC2 machines.

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2590.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2590)

Next were interruption rates on these instances. So thankfully AWS has a pretty good website with the  Spot Instance Advisor where we're able to look at general interruption rates for these machines. So looking at that website, we had an idea that these EBS machines will be interrupted less or about the same as the NVME machines. And also here's a picture of us graphing our own spot data for a C7G 2X large or Graviton 3 processor.

The dark blue is the NVME option and the teal is the EBS, and we can see that the EBS is interrupted a lot less, which means it's more available in the spot market compared to the NVME counterpart. And then the reason it goes down a bit is as we were rolling this out for this one specific instance, we found that they were about the same with the EBS being ever so slightly more available. So seeing this for this one instance type gave us more confidence that this was the right way to expand our pool and even improve reliability.

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2650.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2650)

Finally, how do we go about rolling this all out in production?  I mean, as much as I'd love to just roll this out in production, we focused on doing small changes and using small clusters to test this out. So when I first saw this, we first deployed one microservice with a mix of EBS and NVME instances, and we were able to observe interruption rates and saw that nothing was changing, which is great.

And then after that we worked on moving up to slightly bigger ECS clusters like a dev cluster, a staging, and then a few different versions of production. And again thanks to using task placement constraints we felt pretty confident that workloads that still needed those disks could still have access to them while the rest of our services would utilize whatever is in our pool mainly with the EBS.

[![Thumbnail 2700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2700.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2700)

So now after all that said, what were some of the results of this? So we were able to see an over 10% savings  on our EC2 costs, which is huge without changing any application logic. So most service owners didn't really know where their machines were running, and they were still running the same.

Being on the FinOps team, this was a really great number to see since I care about cost efficiency. But more importantly than costs, this also positioned us better to adopt newer instances without waiting for specific options and also helped improve our pool flexibility. So you can say this is one of the few times where we have a cost savings win and a reliability win come together.

[![Thumbnail 2740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2740.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2740)

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2760.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2760)

### Migrating Microservices to Graviton and Achieving Significant Cost Savings

Now we talked about moving to Graviton with managed services and of course the work we did to expand our pool. So let's talk about microservices.  Essentially we started early with ARM and we had to pick a target, finally update our CI/CD, and then how do we actually accelerate that to be more on Graviton. So first, starting early. We at Duolingo were lucky enough to start working  on Graviton early back in 2022. When Apple first moved their laptops over to ARM from Intel, we used this as an opportunity to try and help dogfood this amongst our developers, since who didn't want to get a new laptop to try out a new technology.

Thankfully, thanks to our dogfooding and using this technology, we noticed a lot of different compatibility issues early back in the day. A lot of things when started up, Docker might be defaulting to AMD64 instead of ARM64. Your favorite package might be deprecated or just not support ARM. We ran into a lot of issues, but thanks to just people working together, we were able to help solidify our local development story for this new architecture. We also saw meeting benefits there. First, things were running a little more efficiently and laptop fans were pretty quiet, so that made meetings a lot smoother.

[![Thumbnail 2830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2830.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2830)

This is all great for a local story and eventually trying to get on Graviton, but we first need to pick a service to move to Graviton that would show people that this is something that's worth investing in. So naturally you pick the hardest thing you can, a monolith. So a show of hands, who has a monolith  or has worked with a monolith in their company? I see a couple of hands here. I know I have. I'm talking about it. Our monolith at Duolingo is called Duolingo 2. I don't know what happened to Duolingo 1. That'd probably be another talk.

Like most monoliths, our monolith for Duolingo 2 was written in Python 2. It's pretty critical. It's a lot of traffic. Not many engineers know all the exact ins and outs of it. It's just your typical monolith things. But by picking this as a target, we wanted to prove to our engineering org that this is technology we want to invest in and this could have a big impact for us for cost efficiency.

[![Thumbnail 2890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2890.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2890)

So how do we go about doing this? First you want to build it locally and thankfully we had the investment to do that locally. And then when you run into that, of course you have to update some packages, remove things, really look at some parts of your code and wonder why did we do this. But that's only half the battle is running it locally. How do we actually get it out into production on Graviton?  So naturally, here's another flow chart of the process we wound up taking.

With our monolith, we had a specific configuration for deploying it, so we naturally need to update that to support Graviton. More often than not you might see some build failures and some other packages might need to be removed here and there, but eventually we're able to get it built. And then naturally when you get it built, you want to deploy it, but not to all of production. At Duolingo, we believe in using canaries and multiple environments to slowly phase these changes in.

When we were deploying our Graviton monolith, we wanted to just try it in our canary to see if there were any weird runtime errors or code paths we couldn't cover locally. And when we did, we worked on rolling back and then looking at those logs and then redeploying and then redeveloping locally and kind of going through this loop a couple times. So we started with the canary, then a smaller production environment, and it kind of kept growing until we eventually reached all of production.

[![Thumbnail 2970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/2970.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=2970)

We had some pretty great benefits there. We saw a 10% task count reduction, which meant obviously your code's deploying quicker and we're also spending less money. But we didn't want to stop with just Graviton 2. So we immediately wanted to utilize newer Graviton instances, so we wound up adding Graviton 3 or C7Gs  to our pool on top of those Graviton 2 instances, and we saw something even more magical happen.

When we deployed it again without changing any of our Python 2 code, we saw our task counts dropping by 20%. And at first we were kind of scared, like latency is going down, task counts are going down, what's happening? But I think that just speaks to the efficiency of the Graviton chips from generational leaps. And then with lower task counts we also had shorter deployment times. So instead of taking 2 hours to deploy a monolith, it took a little over 100 minutes.

I don't know about you, but I love getting time back to do other things and not watch a deployment go out. So this was pretty huge for us. And thanks to all the investment we did with ARM and Graviton, we were able to get this advantage. We were able to utilize this newer generational upgrade essentially for free. Which, I said, being a FinOps person, that is some cost efficiency right there.

[![Thumbnail 3050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/3050.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=3050)

Now I want to say we just immediately started moving more things over to Graviton, but unfortunately due to the high demand of Graviton instances on the spot market, the availability wasn't great, so we had to pause our Graviton journey. But not completely. We still were working on improving the tooling to make sure that other service teams could still move over their workloads to Graviton without having to involve more of the platform teams. So we wanted to make sure developers could  still choose an architecture between x86 or ARM, and they can still deploy to one target or another.

So here is a picture of our CI/CD a little bit. We use GitHub for hosting our code, and ideally if a team has chosen ARM or Graviton, they can essentially pick one of two paths. So if a developer does a pull request, we'll then build that image on Jenkins and then we'll run their tests, whether unit tests or integration tests, for that specific architecture to make sure it's working. And then once it's approved, we'll then push it out, we'll build that specific image, and then also using ECS task placement constraints, we'll make sure that the Graviton image goes to our Graviton instances. Because if they crossed wires, that would just be a pretty sad error. So even though it's not the most seamless, we still want to empower developers to choose their journey and make it easy for them to be able to build without having to think too much about it.

[![Thumbnail 3120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/3120.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=3120)

Now after establishing this ecosystem and moving a few workloads over, our FinOps team wanted to move  that cost savings needle even further and accelerate that Graviton adoption. So here is a bar graph of our EC2 spend, anonymized of course, and here's a couple of services that were larger than others. So naturally we were looking at these graphs for a while, and we saw that these same five services were driving up 25% of our EC2 bill. Naturally we're thinking, oh, these would be great targets to move to Graviton for cost optimization.

But even more to it than that, these are also a good mix of different workloads, like Python, Java, Kotlin, and we also thought this would be another great example for other engineers. So if we could show that we're moving more modern services to Graviton, it would hopefully inspire them to move their services, so it'll hopefully have a compounding effect. And then thanks to all the improvements to our tooling we were making while we were waiting for the spot market to catch up, these migrations were pretty easy and pretty seamless. We just worked with service teams to just say, hey, we're going to move this over to Graviton to not interrupt their production pushes, and then we were able to move these over to staging first, and then finally production.

[![Thumbnail 3190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/3190.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=3190)

And some of the results that we got were pretty great  from that. So first we got the 20% savings for those specific things, but as an aggregate we now have over 30% of our microservices running on Graviton. And then we also have this really cool declining vCPU per hour cost graph. So you can probably guess where we started really accelerating that journey from June to July. And it's not that we were using less EC2 instances, but I think we're being more cost efficient about our workloads. So we're able to scale with our business and just be more cost optimized about it going forward.

[![Thumbnail 3230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/3230.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=3230)

So here are some of the results to date  just to sum it up. So we have that 30% on Graviton, but we also have a new goal in 2025 where we want to increase that number to 50%. And we also wound up getting our 10% EC2 savings, coupling that with EBS, and then also the 10% faster deployments from just how efficient the architecture is. And then on the managed services side, we have that 75% number and it's growing, but we also have a nice 10% RDS savings as well as the 25% ElastiCache savings.

[![Thumbnail 3270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/3270.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=3270)

Now what's next for us at Duolingo in terms of compute?  As Brent alluded to, we're doing an ECS to EKS migration, and that's going to unlock some cool things for us, like using Karpenter to interact with the spot market, ephemeral staging servers so you're not going to have multiple teams fighting over one server, and then maybe down the line a cool service mesh. And then the next one would be multi-arch. So we do love Graviton, but sometimes the spot market might also love it too much too, so we might have to fall back to Intel. So this way we can position ourselves to build one image, simplify our CI/CD, and that way most developers can just say, hey, I'm building my thing, and we just deploy it, whether it's x86 or Graviton. And then finally, more Graviton.

You can probably tell that I do love Graviton, and we want to get onto more Graviton for more cost savings and even that power efficiency as well. Now I want to hand it back to Seth to close this out.

[![Thumbnail 3340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/3340.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=3340)

Awesome. Thanks, Jean-Paul. Thanks, Brent. For those of you that want to figure out where to get started, some resources are available. So we have a Graviton technical guide available on GitHub. It's a big repository. We keep it up to date. You can get a lot of technical information and guidance on how to migrate different applications, whatever  programming languages, Java, Ruby, Python, C, C++, whatever you need. Resources are out there so you can get started in your organizations today.

If you want to learn more about enterprise support, you can take a look. Those QR codes will take you out to those links and learn how to work with an AWS Technical Account Manager. I think they're one of the great resources inside our organization. Maybe you're looking at how to measure performance. I think that's a key differentiator, right? Looking at how to see the difference between the performance on these instances. APR is a tool that we've open sourced to help customers gather the right data so they can make the right decisions about their performance for their applications.

[![Thumbnail 3400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f79de5eb0e562fbd/3400.jpg)](https://www.youtube.com/watch?v=acRBzghbhE4&t=3400)

And maybe you need help from partners. AWS ProServe has a Graviton migration accelerator program. We also have systems integrators that can help as well, as well as the software solutions. Some examples are up there. If you come across a software solution that doesn't work on Graviton today, please reach out. We have a team that works with ISVs to make that happen. And then again, certified third parties,  solutions of system integrators that you can work with. You can see some of them on the screen. That's growing every day.

Thank you very much. Thank you everyone for joining us today. We appreciate you taking your time. Enjoy the rest of the week.


----

; This article is entirely auto-generated using Amazon Bedrock.
