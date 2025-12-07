---
title: 'AWS re:Invent 2025 - Unlocking possibilities with AWS Compute (INV207)'
published: true
description: 'In this video, AWS Vice President Willem Visser presents EC2''s latest innovations, highlighting that customers launch over 271 million instances daily across 38 regions. He showcases customer success stories including Zoox''s autonomous vehicles processing petabytes of data, Nubank achieving 14% cost reduction with Graviton, and Intercom''s AI agent Fin resolving 67% of customer queries. Key announcements include eighth-generation Intel Xeon instances with 15% better price performance, fifth-generation AMD EPYC-powered instances, Apple M4 Max and M3 Ultra Mac instances, and NVIDIA GB300-powered P6e UltraServers. The presentation covers AWS Nitro System advancements, Graviton4''s 40% better price performance, and Trainium3 UltraServers with 4.4x performance improvements. Jeremy Conley details operational enhancements including 12% faster instance launch times and EBS Time-Based Snapshot Copy. Barry Cooks introduces compute abstractions including Lambda tenant isolation, durable functions, Lambda Managed Instances, ECS Express Mode, and SageMaker''s checkpointless training achieving 95% goodput. The session concludes with Amazon Nova Forge, enabling customers to customize frontier models using intermediate training checkpoints.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/0.jpg'
series: ''
canonical_url: null
id: 3088606
date: '2025-12-06T10:09:51Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Unlocking possibilities with AWS Compute (INV207)**

> In this video, AWS Vice President Willem Visser presents EC2's latest innovations, highlighting that customers launch over 271 million instances daily across 38 regions. He showcases customer success stories including Zoox's autonomous vehicles processing petabytes of data, Nubank achieving 14% cost reduction with Graviton, and Intercom's AI agent Fin resolving 67% of customer queries. Key announcements include eighth-generation Intel Xeon instances with 15% better price performance, fifth-generation AMD EPYC-powered instances, Apple M4 Max and M3 Ultra Mac instances, and NVIDIA GB300-powered P6e UltraServers. The presentation covers AWS Nitro System advancements, Graviton4's 40% better price performance, and Trainium3 UltraServers with 4.4x performance improvements. Jeremy Conley details operational enhancements including 12% faster instance launch times and EBS Time-Based Snapshot Copy. Barry Cooks introduces compute abstractions including Lambda tenant isolation, durable functions, Lambda Managed Instances, ECS Express Mode, and SageMaker's checkpointless training achieving 95% goodput. The session concludes with Amazon Nova Forge, enabling customers to customize frontier models using intermediate training checkpoints.

{% youtube https://www.youtube.com/watch?v=2_Ev5YCO2Ik %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/0.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=0)

[![Thumbnail 10](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/10.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=10)

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/30.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=30)

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/50.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=50)

### Introduction: Technology Advancing Faster Than Strategy

 Please welcome to the stage, Vice President of Amazon EC2  Core Platform at AWS, Willem Visser. Hello everyone. Good to see all of you here.  What an exciting time to be in our industry. I have the privilege of leading EC2, and I spend a lot of time with customers every day. A lot of those customer conversations are actually about what features they're looking for.  I then also get to experience my team looking at those features and deciding how we develop them.

[![Thumbnail 60](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/60.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=60)

 We're living and competing in a world today where technology is advancing faster than strategy can keep up. Artificial intelligence, automation, real-time analytics—these are all no longer future concepts. They are driving measurable results today. What's key here is that organizations that move first, adapt faster, and make data-driven decisions are the ones that are going to win markets and rewrite industries. This is more than just a technological shift. It's a business revolution. Success belongs to those who can turn innovation into execution at speed and scale.

Today I want to share how AWS Compute continues to transform what's possible in cloud computing. I want to show how we develop and apply technology to solve what were previously considered impossible problems. As I've said, I have the privilege of leading Amazon EC2, and I get to see this every day. Every innovation that I'll share today stems from similar customer journeys—real challenges that demanded delivering new solutions.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/140.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=140)

### EC2 Scale and Building Blocks for Innovation

I want to start by talking about scale.  Scale creates possibilities. Our customers every day launch more than 271 million instances on EC2. If you do the math, that gets you to just over 1 billion instances every week. Since 2006, we've launched 287 billion instances on EC2, which is a staggering number. With 38 regions globally and over 1,000 instance types to pick from for every kind of workload, we're delivering compute capabilities from cloud to edge.

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/180.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=180)

 What does this mean for you? You can launch global applications within minutes instead of months. Our managed services automatically pick the right resource for you, while our global presence lets you deploy as close as possible to your customers. You can run anything from microservices up to massive AI models—we love them all. Operating at this scale gives us unique insights into your challenges, all while continually optimizing our own infrastructure across compute, networking, and storage.

Every layer of our stack represents countless conversations with customers who've pushed us to think differently about compute. Innovation requires partnership and collaboration at every layer, from silicon to software, and we're going to go into each of those in this presentation. That brings me perhaps to the most important lesson we've learned of them all. It's not just about building a future. It's about building building blocks—building things that millions of customers can actually build on top of.

[![Thumbnail 250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/250.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=250)

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/270.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=270)

### Customer Stories: From Autonomous Vehicles to AI-Powered Customer Service

 As promised earlier, I want to share remarkable customer stories today. If you can't tell from that image already, the first customer story is very close to my heart. It's Zoox. Now these cars, you might have seen them zooming around the strip already. You can download the app and you can take a ride in them.  In October, I actually had the opportunity to go and ride in one of these. The car really is pretty cool. There's a lot of innovation in the design of the car itself.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/280.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=280)

 The way that you sit is different. You actually face each other. It's like little train compartments, lots of space, and the ride is just absolutely incredible. It's just different than anything else in the market, and I loved it. But what's fascinating is not just the physical design. What's fascinating, of course, for us, is the massive data operation behind these cars. Zoox operates on a unique rhythm.

[![Thumbnail 310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/310.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=310)

During the week, Zoox's instruments and vehicles gather petabytes of information.  Then, over the weekends, they take the data set from the entire week and run it through advanced machine learning models in massive simulation runs. This creates an interesting challenge: how do you efficiently manage your compute resources when your needs dramatically swing from that week of gathering information to the intense weekend run that you have? Zoox manages shared infrastructure where application teams deploy and run their services on EC2. When the weekend simulation run comes around and they require thousands of GPU instances, they leverage AWS machine learning capacity blocks, on-demand capacity reservations, and finally, SageMaker HyperPod with flexible training plans.

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/360.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=360)

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/370.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=370)

Just a few weeks ago,  I met with Nubank in Seattle. This is a company that in 2013 set out to take on a really big challenge in Brazil.  This was a market at the time that was dominated by essentially five banks, Nubank obviously not being one of them. Their mission seemed impossible: build a digital bank first in this market dominated by these five banks, and specifically in this market where financial literacy was low and millions were unbanked. Today, it's a massive success story. Nubank serves more than 130 million customers across Brazil, Mexico, and Colombia. Ninety percent of their workloads run on AWS, and thirty percent of that compute workload runs on Graviton. The fact that they're running Graviton had them achieve a fourteen percent reduction in cloud costs and a twenty-one percent reduction in carbon emissions. Nubank also uses Amazon EKS with Karpenter to run sixty percent of their EC2 capacity.

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/450.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=450)

At Amazon itself, we're also tackling seemingly impossible challenges. Amazon Kuiper aims to deliver worldwide internet access through over 3,000 satellites. Before launching a single satellite, their simulations include entire constellations  using EC2 Graviton instances and using FPGA-enabled F2 instances. These simulations model satellites moving at over 17,000 miles per hour while dodging space debris, requiring complex communications all the while, and ensuring reliable internet service delivery.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/480.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=480)

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/490.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=490)

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/520.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=520)

Consider  Intercom's journey into revolutionary customer service. Intercom serves over 25,000 global customers, and they've developed Fin,  their AI agent which has transformed the customer service landscape. What once seemed impossible, having an AI handle complex customer queries across multiple channels with human-like understanding, is now a reality, with Fin successfully resolving sixty-seven percent of customer queries and doing over a million a week. Their technical architecture is equally impressive. They run on AWS Graviton, and you see a little bit of a pattern here. They process 50,000 asynchronous jobs per second.  Their migration to Graviton4 for Elasticsearch achieved a fifty-six percent latency reduction in search queries while cutting their EC2 costs by twenty-two percent.

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/540.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=540)

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/570.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=570)

### Strategic Partnerships: Intel and Mercedes-Benz SAP Migration

Partnerships are an important component to AWS's success because they allow us to accelerate innovation  and quickly deliver the latest compute technologies to our customers. Today, I want to take you through the journey of our strategic partnerships that have fundamentally transformed cloud computing. I want to start with our oldest. Intel has been there since the beginning of AWS, and we still work closely today to solve challenging customer problems. Let me share a very powerful example of one such interaction.  Mercedes-Benz recently completed an extraordinary migration of their ERP system. They moved to SAP RISE on AWS using our U7inh platform. This platform we developed with Intel specifically for memory-intensive workloads like SAP HANA. This wasn't a simple migration. We're talking about a 32 terabyte system supporting over 40,000 users across 17 different systems. What would have seemed impossible before and what would have taken multiple years was accomplished in just nine months.

Together with Intel, we've redefined what's possible in cloud computing through innovation, but also very importantly through custom processors.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/620.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=620)

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/660.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=660)

### Eighth Generation Intel Xeon Instances: Performance and Flexibility

 When we say custom processors at AWS, we truly mean custom, designed specifically for your cloud workloads. We introduced our latest eighth generation of Amazon EC2 instances powered by custom Intel Xeon processors, which delivers the highest Intel Xeon 6 performance, not just in the cloud, but anywhere. What does this mean for you? If you're using the latest Intel Xeon 6-based instances, you'll see 15% better price performance, first of all, and you can process data 2.5 times faster with our enhanced memory throughput.  And for those demanding workloads, you're going to look at 20% higher performance, directly adding to your bottom line.

These improvements come to life across our new R8i, C8i, and M8i instance families. C8i instance families deliver up to 60% faster NGINX performance. R8i instances deliver industry-leading SAP performance workloads, and M8i instances deliver accelerated PostgreSQL performance by up to 30%. Each of these families offer our standard 13 different additional sizes, including bare metal and 96 XLarge options for your most demanding applications.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/730.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=730)

We're also expanding our Flex instance options. Now, as a reminder, these are our instances designed to specifically help customers save money when their applications do not consistently need all the available CPU resources. Building on the success of our C7i Flex and M7i Flex instances, we've now added R8i Flex to join the C8i Flex and M8i Flex family.  These instances offer the most common sizes from large to 16 XLarge. This means whether you're running compute-optimized workloads like web servers and caches and general-purpose applications like microservices and virtual desktops, now memory-intensive workloads are added and you can save an additional 5% on your compute cost.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/760.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=760)

Our customers continually ask for more options,  and we're now launching a preview of X8i instances powered by custom Intel Xeon 6 processors. These instances deliver up to 6 terabytes of memory, 1.5 times more capacity, and 3.4 times more memory bandwidth compared to our previous generation, making them ideal for in-memory databases and mission-critical SAP workloads, delivering 46% higher SAPS performance. That's not our only new Intel instance this week. We're also introducing new C8iNE, also powered by Intel Xeon 6 processors and our latest Nitro v6 card. C8iNE instances deliver up to 2.5 times higher packet performance per vCPU and 2 times higher network bandwidth versus C6iN instances, making them ideal for security appliances and firewalls.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/820.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=820)

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/840.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=840)

### AMD EPYC Innovations: From C8a to High-Frequency Computing

Our partnership with AMD  represents another breakthrough in cloud computing. AWS was the first major cloud provider to launch AMD EPYC instances, and since that introduction in 2018, we've launched every generation of their processors. Earlier this year,  we introduced the fifth-generation AMD EPYC processors into our portfolio and we're now launching the latest instance, C8a. C8a instances deliver up to 30% higher performance compared to C7a instances with memory bandwidth and networking capabilities that transform what's possible for your compute-intensive workloads. These instances deliver up to 384 gigabytes of memory backed by up to 75 gigabits of network bandwidth and 60 gigabits of EBS bandwidth. C8a instances are perfect for everything from batch processing, distributed analytics, to high-performance computing and highly scalable multiplayer gaming.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/900.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=900)

Next, we're launching a tongue twister, the AMD X-based instance X8aedz, right? That's a lot of letters. I apologize for the letters, but that's important.  So these are powered by fifth-generation AMD EPYC processors.

With a maximum frequency of 5 gigahertz, the highest CPU frequency in the cloud, and one of the reasons for the many letters. They deliver up to 2 times higher compute performance and 31% better price performance compared to previous generation X2iEDZ instances with up to 8 terabytes of local NVMe SSD storage. These instances are ideal for EDA workloads. For chip designers, this means faster processing in memory-intensive tasks like floor planning or routing and power integrity analysis.

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/960.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=960)

To round out today's AMD CPU-based announcements, I have two final instance types to share with you. First, we're announcing HPC8a instances powered by fifth-generation AMD EPYC  available in 2026. These instances are designed for compute-intensive, high-performance computing workloads, including computational fluid dynamics, weather forecasting, finite element analytics, and drug discovery applications. And then available in preview, M8azn. These general-purpose instances are powered again by fifth-generation AMD EPYC processors and deliver industry-leading 5 gigahertz of clock speed, the highest maximum CPU frequency in the cloud.

M8azn instances deliver up to 2 times higher compute performance and up to 2 times network throughput compared to prior generation M5azn instances. The high compute and network performance makes them ideal for latency-sensitive and compute-intensive workloads such as gaming, financial services, high-frequency trading, and simulation modeling for the automotive, aerospace, energy, and telco industries.

[![Thumbnail 1040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1040.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1040)

Let me put all of these AMD innovations into perspective because there's been quite a lot of them that I went through now. For EDA customers, the X8aedz instances will accelerate chip design  and verification. For e-commerce platforms, using our M8azn instances, this means supporting more concurrent customers during peak shopping periods with the highest single-threaded performance available. Our new R8a, M8a, and C8a instances deliver up to 30% higher performance and up to 19% better price performance compared to their predecessors.

With R8a also offering 45% more memory bandwidth for databases, and M8a showing off that 39% faster Cassandra workloads, and C8a enabling thousands of additional players to concurrently join the same game without experiencing latency issues. And these are the ones that my sons are the most interested in because they'd like the games to be less laggy.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1100.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1100)

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1120.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1120)

### Apple Silicon on AWS: Next Generation Mac Instances

Now shifting gears, AWS was the first and still is the only major cloud provider to offer Apple Mac-based instances.  And today I'm happy to announce the next generation of our Mac instances available on AWS. First, EC2 M4 Max Mac instances powered by Apple M4 Mac Studio are now in preview,  and EC2 M3 Ultra Mac instances powered by the Apple M3 Ultra Mac Studio are coming soon. Now these join our recently launched Mac 4 and Mac 4 Pro instances, completing our portfolio of the latest Apple Silicon options.

These new instances will further help eliminate the need for physical lab environments, reduce capital expenses, and simplify CI/CD pipelines, enabling developers to build and test and also sign your Apple apps using the very latest Apple hardware.

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1180.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1180)

### NVIDIA Partnership: P6 Instances and the Latest GPU Technologies

We've been collaborating with NVIDIA for 15 years. That's a long time. And we were the first to launch NVIDIA GPUs to the cloud. And since then, we've never stopped innovating. And we've spent all of this time learning about operating GPUs at scale. And I think we excel at doing exactly that. Through our first Elastic Fabric Adapter, EFA,  we enabled HPC-grade networking with lower latency and higher throughput than traditional TCP transport. And we think this differentiates us. It allows customers to achieve on-premise cluster performance with that cloud flexibility.

Earlier this year, we released our P6 generation EC2 instance with NVIDIA Blackwell. The P6 GB200 instances deliver up to 2 times faster performance for AI training and inference compared to P5s.

Three weeks ago, for those of you that might have seen it, we launched P6e B300 as well. These instances are ideal for larger scale AI training and deliver two times more networking bandwidth and 1.5 times more GPU memory than previous generations.

[![Thumbnail 1240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1240.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1240)

Yesterday, for those of you that watched it, Matt announced the launch of EC2 P6e GB300 UltraServer, which is the first AWS instance to be powered  by NVIDIA's latest GB300 NVL72 system. GB300 offers 1.5 times FP4 flops and GPU memory to improve performance for the biggest models and the most demanding AI workloads.

A couple of weeks ago, we also launched P6 B300, which delivers two times networking bandwidth and 1.5 times GPU memory size and 1.5 times GPU teraflops compared to P6 B200 instances, making them well suited for those of you that are interested in this detail to train and deploy large trillion parameter foundational models and large language models. The recent launch of B300 and GB300 instances demonstrates our commitment to stay at the forefront of GPU computing. These are the latest and greatest generations of GPUs.

[![Thumbnail 1300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1300.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1300)

### Beyond Performance: Operational Excellence in Daily Operations

And so as you've heard today,  AWS continues to deliver breakthrough innovations at every layer of the stack. From deep collaboration with Intel and AMD and Apple and NVIDIA to the incredible customer stories that we've already shared, we're helping customers unlock entirely new possibilities in the cloud. These innovations aren't just about raw performance. They're about enabling better customer experiences and delivering more value at lower cost.

[![Thumbnail 1350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1350.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1350)

But innovation at AWS isn't limited just to these large technological leaps. We're equally passionate about the smaller things, the countless smaller things that help our customers achieve operational excellence in their day to day work. And to tell you a little bit more about how we've turned this focus into real innovation for our customers, I'm excited to welcome Jeremy Conley to the stage. Jeremy. 

### EC2 Operational Excellence: Launch Times, Snapshots, and Instance Health

Hi everyone. Good afternoon. I'm Jeremy Conley, the Principal Technical Customer Success Manager in Amazon EC2. You know, in today's cloud environment, every millisecond of downtime and every minute of operational overhead directly affects our customers. That's why I'm particularly excited to share some breakthrough improvements we've made in EC2 operational excellence, improvements that are transforming how our customers launch, run, and manage their compute workloads.

At AWS we believe that every incremental improvement unlocks new possibilities for our customers. Today, I'll show you how these targeted innovations are creating transformative outcomes across our compute platform. Let's start with launch time improvements.

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1430.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1430)

With our latest Amazon Linux 2023 release, we've achieved a 12% improvement  in instance launch to SSH ready time across our most commonly used instance types. This comes from extensive optimization work, including streamlined random number generation services and boot process improvements. On our C6G medium instances, for example, we've reduced launch times from 9.1 seconds to 8.05 seconds with similar improvements across other compute optimized and general purpose instance families.

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1470.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1470)

And speaking of ready time, time is critical in cloud operations, which is why  I'm particularly excited about our new Amazon EBS Time-Based Snapshot Copy capability. Customers can now copy EBS snapshots within or between AWS Regions and accounts with a predictable completion duration ranging from 15 minutes to 48 hours, and some customers are seeing up to 350% improvements when adding a one terabyte node.

Let me walk you through how this works.

This feature guarantees consistent and predictable snapshot copy performance by reserving dedicated throughput for copy operations based on the completion time you specify. When you initiate a time-based copy, AWS allocates the necessary bandwidth, giving you the guaranteed completion times needed for your most time-sensitive workloads. You can monitor progress using the console or API, and even Amazon EventBridge events that notify you when a copy completes.

[![Thumbnail 1550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1550.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1550)

With guaranteed completion duration, you can address strict RPO requirements, improve disaster recovery runbooks,  development workflows, and testing cycles with confidence, knowing exactly when your snapshot will be available in the destination region or account. Customers such as MongoDB are using this feature to accelerate their cross-region node provisioning for their Atlas multi-region clusters.

[![Thumbnail 1610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1610.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1610)

Speaking of data assurance, I'm excited to share a major feature in how we help customers maintain instance health. Our EBS Status Health Check capability represents a fundamental shift in how we detect and prevent storage-related issues. Previously, customers might only discover a rare EBS-related problem after their application had experienced impact. Today, we can detect potential issues and deliver that in the form of a health check, removing the operational overhead of troubleshooting any data I/O availability concerns. 

Building on our storage innovations, I'm thrilled to announce Amazon EBS Provisioned Rate for Volume Initialization. This feature allows customers to create fully performing EBS volumes within a predictable timeframe by specifying initialization rates between 100 megabytes per second and 300 megabytes per second. We've seen customers use this feature to accelerate their EC2 instance launch, to build more resilient disaster recovery posture, and to achieve faster workload readiness for applications such as gaming and virtual desktops. On average, customers are realizing 40 to 60% improvement in their rate of volume initialization.

[![Thumbnail 1690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1690.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1690)

Understanding instance placement and proximity is another area where we've made advances. Our instance topology feature provides unprecedented visibility into the relative proximity of your EC2 instances, delivering up to 50% faster latency improvements. I'm also excited to announce a new Capacity Reservation Topology API. This capability allows you to determine the placement of your reserved capacity before launching instances,  a critical advancement for workloads where every minute matters. Customers such as Anthropic used to spend up to 60 minutes launching and validating their instance placement and can now optimize infrastructure placement strategies before deployment, reducing the operational overhead.

[![Thumbnail 1720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1720.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1720)

All these improvements work together to create a more reliable, efficient,  and manageable compute environment. But perhaps more importantly, it represents our commitment to continuous operational excellence. We're not just fixing problems, we're preventing them. We're not just improving performance, we're making it more predictable. And we're not just reducing operational overhead, we're innovating on how operations work.

[![Thumbnail 1760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1760.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1760)

These improvements reflect our commitment to operational excellence, but as always, we're just getting started. As Andy Jassy says, there's no compression algorithm for experience.  And it's this accumulated experience that enables us to continuously improve every aspect of our service. Just as we've seen with these operational innovations, this same drive for excellence leads us to dive deeper, all the way to the silicon level.

Now, I'd like to welcome Willem Visser back to the stage. He'll share how our focus on these operational details has led us to develop our own silicon innovations, bringing even more benefits to our customers. Thank you.

### Custom Silicon Innovation: AWS Nitro System, Graviton, and Trainium

Thanks, Jeremy. I managed to make the stage.

I'm told yesterday somebody took a stumble, so at least that's one point better than the person that took a stumble. Jeremy is my right-hand person when it comes to spending lots of time with customers on operational things, and he certainly is the right person to speak to the improvements that we've made.

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1830.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1830)

Next, I want to show you how we're redefining what's possible at the chip level.  You see, silicon innovation isn't just about better performance. It's about where performance meets possibility. And every advancement we make at the silicon level creates a ripple effect across thousands of applications worldwide, as well as those partners that we've just discussed.

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1880.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1880)

Our journey into custom silicon innovation began with a seemingly impossible challenge: reimagining cloud infrastructure from the ground up. We started very small, working with KVM on off-the-shelf hardware for our Nitro System. The initial step led to something transformative, the AWS Nitro System, the foundation today for all of EC2. Traditionally, hypervisors were responsible for everything: protecting physical hardware and BIOS, virtualization,  CPU, storage, and networking, while providing management capabilities. And that, of course, requires resources.

[![Thumbnail 1910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1910.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1910)

The Nitro System completely reimagined this approach by breaking apart these functions and then offloading them onto dedicated hardware and software. And we can now deliver practically all of the service resources to our customers. It's fundamentally changed how we deliver cloud computing, enabling us to innovate faster and reduce costs for our customers,  as well as, of course, providing enhanced security.

The AWS Nitro System is a comprehensive security and performance foundation for EC2. It provides zero operator access, something that we're so confident in that we had it independently verified. The AWS Nitro System delivers hardware-based protection and enables encryption for your VPC traffic and EBS volumes. Those of you that listened to Rob about networking a little bit before. Through the AWS Nitro Hypervisor, customers get bare metal performance, and for demanding workloads, Nitro supports Elastic Fabric Adapter, what I mentioned before, for ultra-low latency RDMA communications.

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/1960.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=1960)

The secure foundation extends to our networking infrastructure. We've built the most secure,  reliable, and scalable network connecting all of EC2. And unlike traditional architectures, we've created one large, fast network that unifies all of our traffic flows. And the scale here is unprecedented. We connect over one million accelerators, as an example, in a single flat network.

But it's not just about scale. It's about reliability. Our network automatically routes around link or switch failures without performance impact, completely transparently. We've built one common network that connects everything from GPUs to Trainium to AMD to Intel to Graviton to the Apple processors that we talked about before.

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2020.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2020)

Our Graviton journey, which began in 2018, rapidly moved through the generations that you see in front of you, starting from 2018 to 2019, up to 2023 when we released Graviton4.  And each generation has marked a significant leap forward. Graviton4, released in 2024, remains one of the most exciting chips that we've ever released. It features 96 cores and 73 billion transistors. I can't even comprehend that number. And so if you take that 73 billion for Graviton4, Graviton 1 had five. So it's just amazing, right? These numbers just become incomprehensible really to wrap your mind around how much we pack into these chips.

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2060.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2060)

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2070.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2070)

So the benefits are compelling.  Up to 40% better price performance while consuming up to 60% less energy. One of the things for Graviton that we're really, really proud of is it uses less energy.  So net potential cost savings of up to 20%. The performance gain often means using fewer instances for your workloads and potentially decreasing your total cost of ownership even further. And we spoke about that carbon footprint earlier in the deck as well.

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2090.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2090)

So here is my challenge for you today.  Take one initiative. I promise you'll be amazed by this and what you're going to accomplish in just one week with one developer and one workload.

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2120.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2120)

I've had so many examples from customers that took up this challenge and took one developer, put them for one week on this. Absolutely incredible. Just think about it. Less time than it takes you to plan your next sprint, you can take one engineer and put them on this and see what you get. And the potential upside for you is incredible.  So I really encourage you to go and spend that one week of one engineer and see what you can actually gain here.

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2150.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2150)

Building on our security-first approach, we've now made EC2 instance attestation generally available. And this is a game changer for a big subset of our customers who need to validate that only trusted software is running on their EC2 instances, including those with AI chips and GPUs. With a combination of Nitro TPM and attestable Amazon  machine images, customers can now cryptographically verify their instances are actually running trusted configurations on the software. And by integrating with AWS KMS, customers can restrict key operations to instances that pass this attestation test.

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2180.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2180)

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2210.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2210)

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2220.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2220)

Let's talk about Amazon EC2 Trainium 3 UltraServers, powered by our fourth-generation Trainium 3 chips. Now again, there's going to be some numbers here.  It's very hard really to put these numbers into context, but I'm going to try and give them to you so that you can interpret them. These instances deliver up to 4.4% higher performance, 3.9% higher memory bandwidth, and over four times better performance compared to Trainium 2 UltraServers. And what's truly groundbreaking is the scale that we can achieve. Trainium 3 UltraServers represent a significant leap forward in performance,  scaling up to an impressive 144 chips that we packed together in the UltraServers, which is essentially four of these hosts packed together. So truly groundbreaking performance for Trainium 3. 

But numbers and specifications only tell part of the story. The true power of these innovations emerges when you see how they enable entirely new ways of building applications, which is the more interesting part. You've heard me talk extensively about our innovations in silicon, our broad portfolio of EC2 instances, and hopefully you interpreted some of those numbers, and the incredible performance that we're delivering at the chip level. Many of our customers love this level of control, the ability to fine-tune their infrastructure down to the instance type and processor architecture. They need that control.

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2310.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2310)

But we also hear from customers who want to abstract away infrastructure decisions. They want to focus purely on the application code and business logic, letting AWS handle everything else. I like to think of my team as, if you can visualize this, I like to think of my team as the team that creates little Lego blocks. And we decide on the size and we decide on the color, and we decide where we're going to actually go and produce them and which regions we're going to put them down, where you can go and buy them. And our next speaker, Barry Cooks, VP of Compute Abstractions, he and his team is that kid who walks in, looks at all the Lego blocks that we've built, and he snaps them all together, and he's the one that wins the best spaceship award. So let me show you how it is. Barry Cooks, please come onto the stage. Thank you. 

### Compute Abstractions Strategy: Lambda, ECS, EKS, and SageMaker Innovations

How's everybody doing? I'm going to take us in a slightly new direction here, and that is, I'm going to talk about compute abstractions, but I also kind of want to do this in the frame of a little inside baseball. So let's talk a little bit about our strategy internally for our compute abstractions. What are our strategic pillars? What are we trying to do with all of these abstractions? I think there's a great quote attributed to Mark Twain, although I hear there's some French mathematician in the 1600s who also claims the same quote. And that is, I didn't have time to write you a short letter, so I wrote you a long one instead. One of our key goals with our compute abstractions is that we want to be able to give you exactly what you need in a concise, clean way so that you can use that abstraction to meet your goals.

[![Thumbnail 2360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2360.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2360)

And there looks like a lot of different logo madness going on here, and there is.  Because the other thing that's also true around compute abstractions is that we have this inside joke slash criticism, and that is, if there's a good idea, there's at least twelve ways to do it in AWS. And the reality of that quote and that joke, yeah, there is. But realistically, there's just one way. It's the way you chose. All of those different twelve ways, we do that on purpose. We do that because customers tell us, here's how I want to do it, and here's how I want to do it. And we try to meet customers where they are. We don't try to force function them into other things. So that's been a key attribute of how we are trying to build these compute abstractions over time.

There's a lot of detail hidden behind our compute abstractions. So there are three things to take away with respect to our strategy, our strategic pillars. Number one, we do want to make it easy. We want to make it easy for your newer employees to use compute abstractions.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2410.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2410)

 We want to make it easy for AI agents to leverage these compute abstractions. We don't want you to feel like you've stepped into a cul-de-sac. The worst thing we can do for a customer is they make a critical decision to leverage one of our abstractions, and then a year or two later as their workload is growing and changing, they feel like we pushed them into a cul-de-sac and they have to back out and re-implement or rearchitect in a different way.

So we want to avoid those cul-de-sacs. We want to be able to have you come in, pick an abstraction, and grow on that as needed. Then the third one is we want our expertise. We've got 20 years of experience running workloads at global scale all over the place. We want that expertise built into the abstraction so that you can take advantage of the hard lessons we've learned over those 20 years. So that's a super important piece for us.

And I want to say, for you, what does it really boil down to? So our goal for our customers is we want anyone to be able to use these abstractions. We want you to use them for any application, whatever language you've chosen, whether you're doing a big fat old school monolith, whether you're going to use containers, if you want to just have functions. We want that to work across the board. We want you to be able to run it anywhere.

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2490.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2490)

Willem early in his talk showed you the global reach that AWS has. We want you to be able to run your abstraction and your code as close to your customers as you choose. And lastly, we want you to be able to do it at any scale.  And I think this is an important one because we talk to a lot of startups, and I have yet to meet a startup where they think they have a terrible idea and they're going to be out of business in nine months. Most of them want to grow in scale, and we want to be able to take it from small to global scale, and we want to support customers with that.

So I asked my teams what should we cover off as interesting launches, and they gave me 50 things. And I don't have 50 minutes, so I couldn't even do any kind of justice on launches today. But a huge shout out though to the organization because it has been a very busy year. We've done a ton of things. I'm going to walk you through a few items that I think are super interesting.

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2530.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2530)

A great place to start is Lambda. Lambda is kind of the ultimate of abstractions,  right? It's a developer-friendly system where you don't know anything about what we're doing under the covers. You just run your code. We take care of everything to do with infrastructure, so it's a super high level abstraction. It's super friendly for developers. But Lambda doesn't know everything about what you're doing.

So one of the challenges that we identified this year was, hey, we are running at a scale and we have adversarial tenants, and we want to continue to use Lambda, but we need to protect tenant A from tenant B. So you're running a multi-tenant workload. How can I do this? And the answer is you can. You can do it today. It's great. You just create a copy of every function that you have a tenant for. And if you've got 10,000 tenants, guess what you've got to do? 10,000 copies of the same function. And then you have to operate that, and you've got to go manage that. And that is, frankly, not very fun.

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2580.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2580)

 So what do we want to do? We want to make this much simpler, so we're pretty excited. We're launching Lambda tenant isolation. With this, your developer stays where they want to live. They're in the code. All they have to do is pass us a tenant ID. Tell us you want to be in the tenant isolation mode, pass the tenant ID, and we will take care of the rest under the cover.

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2600.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2600)

 So Lambda will make sure that you get the hard security boundary around your tenants that you want. We will log and provide information about the tenants and where they've run. So this way you stay in the abstraction that you want to be in. You're still working in the world of Lambda. You're just telling us, hey, this is multi-tenant, and here's the ID for this tenant. And then we will manage the infrastructure so that you don't have to worry about it, and we'll give you the logging and reporting on how that's working.

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2630.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2630)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2640.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2640)

Another area in Lambda that we're pretty excited about,  these days, agents are everywhere, so you have these emerging new workload patterns, one of which is I need to call out to an agent, but then I've got to wait for something.  Maybe I'm doing payment processing. I need to go do some fraud checks before I do the next step in my process. And you can create these multiple Lambdas, and you're waiting, and you're building your own retry logic. You're having to deal with the fact that now I've got to go replay some portion of a transaction when it comes back.

You've got long waits that you've got to deal with. You're creating boilerplate code for all of these things. What you really want to do is have a way to orchestrate this and have Lambda help you go down that path. So we're super excited to launch durable functions. With durable functions, you basically can get in there, focus on the code, tell us it's going to be done with durable functions, and set your steps up.

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2680.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2680)

 I even have a cute little wait in there while you hang out for five seconds, just to point out that we're not going to charge you while you're hanging out waiting. You're only charged for the CPU you use. So with this, you're not writing the boilerplate. We're going to make sure it's idempotent. We're going to store off the checkpoint between steps in your process. We're going to restore all of that for you so you don't have to deal with those scenarios. We're going to deal with retry logic. So we're going to create a system where you focus on your problem, and your problem was payment processing.

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2730.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2730)

Or maybe your problem is that you've got a manual process where your VP of IT needs to go approve something in email, and so welcome to the world of waiting four weeks for them to get back from their golf trip, right? Not a problem, we'll wait one year if that's how long you need. So you can wait up to one year with these and then continue execution. So it creates a really nice environment for you to be able to go step through and build a different type of application than  you've traditionally done with Lambda in the past.

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2770.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2770)

Another area, and this one's going to be fun. In the world of Lambda, I talked about no cul-de-sacs, right, we really don't want that. So something to keep in mind, dirty little secret, and it's not actually a secret. Guess where all your serverless functions run? On a server somewhere, right? There are servers under the cover in Lambda. One of the things we've discovered in talking with customers is they have loved the Lambda model, they've grown massively in scale with Lambda, and they've gotten to the point where they want to be able to take advantage of individual tweaks for performance reasons,  for cost reasons, and a number of different things. And Lambda doesn't let you do that, right? It's a very high level abstraction. We're doing everything behind the scenes, you have no visibility into it, and you don't get to touch anything.

One of the things we realized was that's not going to work for everyone, and I don't want you in a cul-de-sac. I don't want you to back out of Lambda and try to implement on some other service only because your workload has changed over time and grown to the point where you have these economies of scale. So we're really excited now to roll out Lambda Managed Instances. Some of you would have seen this about a year ago this week, we launched EKS Auto Mode. EKS Auto Mode takes advantage of the same concept, managed instances. We're going to take care of the instance, we're going to patch, so nobody has to patch, right?

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2820.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2820)

So now with Lambda, you have that same capability. Your developer, unchanged experience. They're still just writing Lambda functions,  they're still focused on the same problem that they've been focused on, which is their business logic. But what this allows you to do at scale is to give us the decisions that you want us to implement with respect to the actual underlying hardware that we use to run those functions. And there's a number of really good benefits for this, that's an example I've got on the slide. Dylan was just talking about it. Maybe you want to go use Graviton and take advantage of the things you can do with Graviton. Now you can tell us, go give me Graviton instances for these, right?

Maybe you have an IO heavy workload. In the past, you couldn't tell us that. There was nothing you could do to get Lambda to recognize your IO patterns. Now you can pick out IO intensive instances, and we will use those, and we'll execute on those. Another thing that's super useful here is that this is multi-concurrent. So if you are going to go write thread-safe code, we will run your functions multi-concurrently. It's a huge benefit. We're seeing customers who've been playing with this getting 40% plus improvements. It can really both save you a lot of money and dramatically improve your performance.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2910.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2910)

So we think this one is going to be super useful because it doesn't disrupt your developer experience, but it does allow large scale customers with large scale workloads to take full advantage of the EC2 concepts and fleet and the different kinds of patterns that we have there. Now, as we've looked at the simplicity and we've said how do we keep things simple, how do we move things forward, we didn't just do things in Lambda this year, we've done a number of things in other spaces. Not surprisingly,  ECS is now also going to roll into managed instances.

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2940.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2940)

So traditionally in ECS you had these two spectrums. I could sit on one side, and on that side I had Fargate, fully abstracting, right, I'm telling you about CPU memory. And on the other side I had ECS EC2. I get to go on and drive and patch and deal with a bunch of issues. Now with managed instances, we're going to meet you in the middle, maybe it's the Goldilocks zone. We're going to let you go and take full control and run those instances. We'll still do all the patching,  all the other components.

The next space I want to touch on, still in the world of ECS. Deployments tend to be hard. People write lots of code to handle their deployments. They're trying to get a scenario laid out where they're testing and safe, right? We want to do things, step it into production. I talked earlier about how we're trying to make things easier. We're also trying to leverage our own expertise. We do a lot of deployments at AWS as you can imagine with our scale, and what we want to be able to do is put some of that expertise into ECS.

[![Thumbnail 2990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/2990.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=2990)

We want to allow you to take full advantage of the things that we've learned over the years. So we're super excited today to roll out ECS native deployments. With native deployments, you can do blue-green, you can do canaries. Our best practices are built into these.  We will auto roll back, right? So you get these capabilities, the same sorts of things that we do day in and day out in our regions as we roll out updates to our services. You can now do built-in as part of ECS. Another space that we've talked about quite a bit.

[![Thumbnail 3010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3010.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3010)

Containers as a concept aren't  terribly complicated. They're fairly straightforward, and lots of developers have wrapped their heads around them. Container orchestration is a complicated set of tooling. We just talked about deployments, for example. So it's not always easy for people to get started who are new to this space. One of the things that we've recognized is it frankly takes a lot of time in terms of learning curve for folks, and we want to see if we can shorten that.

[![Thumbnail 3040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3040.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3040)

And so today we're rolling out a new change in ECS. It's called ECS Express Mode.  And the idea with ECS Express Mode was, can I get it down to one create call to go create my application? And the answer is yes, because we just did it. Can I do this in a way that is easier for me to consume? Yes. So when you would do this in ECS in the past, you had about 300 parameters you would be setting and telling us about in order to roll a new app out. We've got that down to 30. We dropped it by a factor of 10. So now you can go set a relatively small set of parameters.

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3100.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3100)

I like to call this the George Foreman grill, but I'm dating myself slightly with that. You want to set it and forget it. You want us to manage as much of that complexity as possible. And that's what ECS Express Mode is all about. And I think it's been such a busy year for ECS that I wanted to kind of walk you through how we've approached this pattern. What does it look like that we've done? And so if you look at the bottom of this slide, you can see what we've typically managed in the ECS world when you're on ECS EC2. And you can see what you've owned and what you've had to manage  and what you've had to control and think about and wrap your head around to run things in ECS.

[![Thumbnail 3110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3110.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3110)

[![Thumbnail 3120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3120.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3120)

[![Thumbnail 3130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3130.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3130)

As we've rolled forward over the course of this year,  when we add in ECS Managed Instances, we take a lot of burden off of folks' plates. All of a sudden you're letting us take care of a slew of things that have been your problem in the past.  When we rolled in deployments, now we're handling that aspect of things for you. As we roll in and we say, hey, we're going to do ECS Express,  we've rolled another set of things that we're going to go and manage on your behalf. And what this leaves you with is a few security permissions that you're going to own and set, and your actual container app, the thing you actually care about, the thing you're trying to deliver to your customers. And we can take care of these other pieces.

Now, I mentioned before, we don't want cul-de-sacs. And we definitely do not want to create cul-de-sacs with ECS. So do I have to revert to ECS EC2 if I find that some decision we're making in here, let's say on the load balancer, is making you unhappy? Because we are making decisions on your behalf if we're managing it. We want you to be able to peel the cover back. We want you to be able to take on management responsibility for these components. Things like that load balancer, maybe you want to change those settings and you want to own the load balancer piece, but you don't want to revert everything. So we're going to support letting you peel that cover back, letting you go in and take over management capabilities for some of the things that we're managing. It gives you more flexibility and lets you kind of dive in and do things in your own way.

[![Thumbnail 3200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3200.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3200)

Now, ECS is not our only container orchestrator. It is not the only place that we've been trying to focus on building things out that will help our customers and manage more of their components. We have a lot of folks with open source first policies,  a lot of folks who love Kubernetes. And in the Kubernetes space, we have iterated over time. We've been adding more and more management capabilities. That's what our customers have asked us to do. As you go through and you try to scale up and manage dozens or hundreds of clusters across regions, it starts to get pretty complicated. People have lots of tooling that they want to use for these things, and now you're managing tooling and the clusters. And so it can create a pretty challenging environment in terms of your headspace.

So what we've rolled out is Amazon EKS capabilities. Similar to what I showed in the ECS model, we're taking on more management responsibility for you. We're going to try to make things a little easier, especially when operating at scale. So in this case, the first thing that we're going to roll out, Argo CD. Super popular open source framework, most of our customers were using. I didn't meet a single one, including people who were maintainers of Argo CD, who enjoyed managing the thing. So we will manage Argo CD on your behalf, make it significantly easier for you to step in and get your deployments done the way you want without all of the management overhead.

[![Thumbnail 3280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3280.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3280)

A couple of other things that are rolling out immediately in this is also Kro, the Kubernetes Resource Orchestrator that we rolled out as an open source component delivered by our team here at AWS. ACK, another controller for Kubernetes from Amazon.  These will all be managed components inside the stack, so you don't have to go and take on the management responsibility for those. If you remember last year, Karpenter in auto mode became a managed component in the EKS stack. So we continue to kind of build out that framework and get more and more of those capabilities built in. Have feedback, have favorite ones, want something else, let us know. We're super excited about this framework. This is not the last of our capabilities.

We want to continue to add them as customers ask us for new and additional things.

[![Thumbnail 3330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3330.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3330)

Now as we talk about running clusters at scale, one of the focus areas that we had this year was around scaling for AI workloads. So 1.6 million Trainium clusters in a single EKS cluster. That's a lot of Trainium power.  This is one of the ones that's being used by Anthropic. We worked directly with the team in Anthropic, worked with the community in Kubernetes, worked on our own backend components, and we're able to support the scale needs for Claude and future training on Claude. We did the same with the Nova team here at Amazon. And now we're working with customers who are not in the AI space, who also have clusters of huge scale, would like to have uniform single clusters for various reasons, and we're working to unlock those capabilities next.

[![Thumbnail 3370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3370.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3370)

Now as you look at the clusters and you start thinking, man, this is 100,000 nodes, you run into a really interesting problem when you hit these kinds of scales. And that problem is math and statistics. Right, when you start running things at this scale,  something's going to break, I guarantee it. Every single day something's going to break. And that is a huge time sink for customers.

[![Thumbnail 3390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3390.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3390)

So in SageMaker, we have observed this, we've worked closely with a lot of customers who are doing training, and we wanted to get your goodput numbers up.  We're super excited to say that we are now going to do checkpointless training. So, we started this year, we focused on checkpoints, we've put them in memory, we tried to make them faster to load, faster to save. And at some point we said, why do we need checkpoints in the first place? Like why can't we recover your cluster with the right mechanisms built into the core of how SageMaker operates? And that's what you will get with restartless training, checkpointless.

The great thing about this, you don't have to do anything other than turn it on. We will manage this for you. We will automatically handle these things. We're seeing goodput numbers north of 95% from customers who've been using this in anger at this point. So it can dramatically change in a completely hands-off way how you recover from faults. The faults are still happening. But you don't have to worry about them. Your data science team doesn't have to restart a giant cluster, reorganize things, load that snapshot back, right? None of that stuff has to happen. And so it's a huge move forward for us with respect to training on SageMaker.

[![Thumbnail 3470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3470.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3470)

[![Thumbnail 3480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3480.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3480)

Another place that we've struggled with SageMaker has been people build the cluster out. They have multiple things they want to run on that cluster. Invariably, you have conflict, usually business priorities is your conflict, and starting and stopping training is super painful.  It takes too much time, it's hard for people to do. So, what are we going to do about it? We decided to introduce Elastic Training. So with Elastic Training on HyperPod,  based on your priorities, we can allocate additional CPU and additional GPU to the nodes that are needed for the higher priority task, automatically reallocate them back for the lower priority ones in the future. So it allows you to very quickly and flexibly move forward with your training, again, without having to deal with all of the pain points on your side of the responsibility line.

[![Thumbnail 3510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3510.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3510)

[![Thumbnail 3530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3530.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3530)

So the next challenge we've seen with customers is customizing models.  Right? Models are fun, but they're not really that great unless you can get them to do what you need them to do in your shop. And using your data is requiring a lot of skill, a lot of time, a lot of experimentation. And with SageMaker now, we're going to change the game again. With SageMaker AI model customization,  you will get clean, simple workflows, so that non-experts can actually come in and do those training runs, get the customization in place. And this model includes an AI backend that will help you select, like, if you're not super familiar, which model should I even customize in the first place? What if you don't have your own data, you just want some additional data of a particular type? We can now start to help you with each of these core problems. We can generate datasets, we can evaluate your model for success, we can look for bias introduced by the new data that you just rolled into an existing model. And we can help you iterate to get to a successful outcome for your customers.

We thought about that for a while. And kind of like the one on checkpoints, we said, well, so we're going to let you customize models, but one of the challenges with AI models is that you train them and they go, they're kind of like an old dog, right? They get old fast. They don't learn new tricks really well. And the biggest challenge we've seen with training models has been once you get into going and customize a model that's already gone through its full pre-training, you're not going to get the kind of results you might want. What you really wanted was your own model. And you couldn't figure out how to build one. It's too hard.

[![Thumbnail 3620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3620.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3620)

So now, what we've decided to do is Amazon Nova Forge. You can use SageMaker, open the hood, and grab an intermediate checkpoint from our training of a Nova model,  and then modify it and continue the pre-training. That stage in pre-training is the best time to introduce new data to a model. This will allow you to go in and change the data mix. We'll give you full capability to look at exactly what dataset we were using and how we were doing it, and you can change the mix.

You want more scientific data and less data on other aspects of language? Great, change it around. You've got custom physics modeling and stuff that you want to put in the model? You can add that data. It allows you to come out the other side with your very own model, fully customized. It's your way to get to a model that has your proprietary capabilities built into a frontier model. So we're super excited about that. We've got that working, and a number of customers have seen amazing results with this, including Reddit.

[![Thumbnail 3680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3680.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3680)

[![Thumbnail 3690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3690.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3690)

### Closing: Building the Impossible with AWS Compute

We talked about a whole bunch of things here with respect to how we're trying to move the ball forward. I think that one of the things to keep in mind as we go forward is you can get anyone, any application, anywhere, at any scale.  And before I bring Willem back out, because he's going to come back up here for a second, I wanted to highlight that he had that really cool Zoox customer experience demo. I'm super competitive,  so I called a friend.

[![Thumbnail 3720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3720.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3720)

We've been working really closely with Aurora. Aurora does self-driving trucks. They're much bigger than the Zoox cars. I was going for size. These are off and running. They've been working across our systems. They're doing runs in Texas today on the interstates. You'll see them. Give them a wave. They won't wave back. They're expanding across the US. They're going to run coast to coast, California to Florida, in the near future. And with that, Willem, why don't you come on back up here and tell folks about it. 

[![Thumbnail 3740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3740.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3740)

That's right. All right. Like I said, the cool kid that snaps all of my Lego blocks together and then walks away with a spaceship and the prize. So to put this into perspective, every single day,  Aurora processes more than 5 million transactions and about 17 petabytes of information that it needs to process every single day. So this is another example of rapid innovations complementing our longer-term engineering investments. But the real power of these investments comes alive in how our customers are using this.

[![Thumbnail 3790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3790.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3790)

From Intercom pushing the boundaries of AI agents to Amazon Leo building cloud architecture in space, to Zoox processing real-time autonomous vehicle data, to Nu Bank transforming financial services for millions, each of these customers represents how AWS innovates and enables transformation at scale. And so I want to emphasize that everything that we've discussed today began with a customer conversation,  a challenge that needed to be solved, or a possibility waiting to be unlocked.

[![Thumbnail 3840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cfe224df655df9bb/3840.jpg)](https://www.youtube.com/watch?v=2_Ev5YCO2Ik&t=3840)

Whether you're optimizing current workloads like Mercedes-Benz that we spoke about with that SAP workload, or scaling globally with your expanded regional presence and enhanced capacity management, or transforming your business with the latest GPU and custom silicon innovations, AWS Compute provides the foundation you need to succeed. And today we've seen how transformation happens, not just through technology, but through the partnerships that we spoke about, the innovation, and the relentless focus on your needs. I'm excited to see what you'll build with these capabilities. Thank you for your time today, and now let's continue to build what others think is impossible together. Thank you. 


----

; This article is entirely auto-generated using Amazon Bedrock.
