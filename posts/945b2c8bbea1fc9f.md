---
title: 'AWS re:Invent 2025 - How well do you know EC2? (CMP356)'
published: true
description: 'In this video, Chad Schmutzer discusses EC2 optimization strategies to close the "optimization gap" where customers typically use only 5-10 instance types instead of leveraging over 1000 available configurations. He explains instance naming conventions, the six EC2 categories, and emphasizes migrating to Nitro-based instances (5th generation+) for 20-30% price performance improvements. Key tools covered include AWS Compute Optimizer for right-sizing recommendations, Attribute-Based Instance Selection (ABIS) for Auto Scaling groups, and purchase optimization through combining Savings Plans, capacity reservations, Spot Instances, and On-Demand. He stresses monitoring workload metrics via CloudWatch, matching workloads to appropriate instance families, and using multiple processor architectures including Graviton for optimal cost and performance outcomes.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - How well do you know EC2? (CMP356)**

> In this video, Chad Schmutzer discusses EC2 optimization strategies to close the "optimization gap" where customers typically use only 5-10 instance types instead of leveraging over 1000 available configurations. He explains instance naming conventions, the six EC2 categories, and emphasizes migrating to Nitro-based instances (5th generation+) for 20-30% price performance improvements. Key tools covered include AWS Compute Optimizer for right-sizing recommendations, Attribute-Based Instance Selection (ABIS) for Auto Scaling groups, and purchase optimization through combining Savings Plans, capacity reservations, Spot Instances, and On-Demand. He stresses monitoring workload metrics via CloudWatch, matching workloads to appropriate instance families, and using multiple processor architectures including Graviton for optimal cost and performance outcomes.

{% youtube https://www.youtube.com/watch?v=sc_3hfucTnY %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/0.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=0)

### Understanding the EC2 Optimization Gap: Moving Beyond Default Configurations

 All right, hi everyone, thanks for coming. This is CMP 356, How Well Do You Know EC2? My name is Chad Schmutzer. I'm a Solutions Architect. I focus on helping customers run at scale on our compute platform, primarily EC2. We've got 20 minutes here in a lightning talk format to talk about how well you know EC2. I'll do my best to cover as much as I can in this 20 minutes, but by all means I'll hang around after and answer all the questions I possibly can if I did not cover what you came here to talk about.

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/50.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=50)

I would like to talk about what I call the optimization gap.  What I see is that customers are aware of the breadness of EC2 but not the full breadness. Just to cover our baseline, we have over 1000 EC2 instance types generally available. We have six different categories within those instance types. We have multiple processor architectures with measurable different price performance characteristics, and we have purchase options offering 50 to 90% off of those.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/80.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=80)

 Now where do customers typically land? We typically see customers and organizations that are focusing on really 5 to 10 different instance types repeatedly. So instead of looking across the broad platform, they pick 5 or 10 and they just use those for the most part. They stick to default configurations. They don't revisit whether they can optimize over time. What happens is they leave significant cost and performance opportunities unaddressed.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/100.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=100)

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/120.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=120)

 The session objectives here are to understand EC2's breadth and optimization opportunities, to talk about learning workload to instance matching methodologies, and to talk about implementing purchase model optimization strategies.  I'll be talking about different instance types here, so it really makes sense for me to just sort of baseline how we name our instance types and what each of the attributes mean. First on the left we have instance family. This is going to be something like C, M, or R. We're going to have the generation of that family of that particular instance type. Then we have the attributes. In this case, the G stands for Graviton, AWS's processor, and the N stands for advanced networking capabilities on that particular instance. And then we have the extra large or the size, so this means it carries a certain amount of vCPUs and memory. Makes sense so far?

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/160.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=160)

 At the very first start, I highly recommend you can run this from your CLI, from your console, whatever, where you have authentication, just do a quick describe instances and check to see how much variation do you have in your environment. Are you more than 5 to 10? We're going to get a unique set of instance types checking here. What I would recommend is that you simply start with this baseline and then work forward to compare your workload requirements in subsequent slides as we go through this. We want to talk about how unique are you from an EC2 perspective when we try to broaden that and broaden your flexibility as you utilize EC2.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/200.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=200)

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/210.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=210)

 I mentioned there's six categories across 1000 different configurations. At a very high level, we have the general purpose M and T families, we have the compute optimized, and we have the memory optimized. These have different applications for different types of workloads and requirements.  We have the storage optimized I, D, and H, accelerated computing P, G, Trainium and Inferentia families, and then finally an HPC family for those of you that are running high performance computing workloads. Again, each of these at a very high level are designed to address different types of workloads, but what we often see is that customers are able to use multiple instance types and families to satisfy the requirements for their particular workload.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/240.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=240)

 One thing that I really see customers missing is categorizing their workloads and using something like CloudWatch or whatnot to track the utilization metrics for their particular workloads. We're not talking about just the standard CPU and memory usage, but also make sure you're looking at things like disk IOPS counts, network throughput, and preferably tracking your own workload metrics. Many customers will miss this step where you really want to make sure that you're sending your particular workload metrics to something like CloudWatch or your preferable platform of monitoring to make sure you understand the characteristics of your workload. Once you have a good understanding of those characteristics, for example, if you're monitoring your CloudWatch metrics for the last 14 days, start to walk down a decision criteria platform and think about, do I have a lot of variable CPU? Should I really be thinking about burstable instance types or Flex instance types? Do I have sustained CPU? Do I need to think about the C family more? Memory pressure or high IOPS? Think about the I family, and ML and AI workloads, of course, P, G, and Trainium, of course.

So again, don't just standardize on a couple of instance types. Try to pick the few that work for you, but then be flexible across many.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/320.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=320)

### The AWS Nitro Platform: Modernizing Instance Types for Better Price Performance

All right, so why is it important that we do this?  You may have heard other sessions talk about AWS Nitro, but what this really is is the fundamental platform that's powering our EC2 instances and your application. The operating system and the hypervisor all run on top of the Nitro platform. At a bare minimum, this is the Nitro hypervisor that's running and powering your virtual machines.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/350.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=350)

So why is the Nitro platform important and why do we like to talk about it? Well,  it really represents AWS's most significant infrastructure innovation. If you're running any instance type that's pre-fifth generation, M4, C4, R4, etc., you really want to think about moving to a Nitro-powered instance type. So anything from fifth generation and on, and why is this important? From an architecture perspective, Nitro cards have dedicated hardware for your VPC, EBS, and instance storage operations. Nitro security chips are powering the hardware root of trust with firmware protection. And finally, the Nitro hypervisor is a lightweight hypervisor enabling near bare metal performance, right?

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/400.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=400)

And what are the benefits that are coming to you? So we have performance, we have reduced virtualization overhead, we have security with hardware-based isolation and encrypted memory. Innovation, accelerated instance type development and deployment, and then efficiency, improved price performance through architectural optimization.  So you really want to make sure that you take a moment to identify any instances that are running in your environment that are not Nitro-based. I have a couple of CLI commands that you can run here if you'd like to take them away. Run them in your environment.

What I highly recommend is that you run them, discover any old generation types, and send them into AWS Systems Manager for an automated inventory. So what this will do is if you enable Systems Manager on your individual instances, you can use it to have an automated system to track inventory of those. And then you can track to see how you're doing from a flexibility perspective, but also making sure that you're modernizing from anything that's fourth generation or older.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/440.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=440)

 And so what do we do from a modernization perspective once we've found any non-Nitro-based instance types? Well, pretty simple. If you're running something like an M4, you want to think about modernizing to the seventh or eighth generation M instance types with a variety of architectures powering those. Same thing with compute optimized. If you're on a C4 or earlier, think about C7I, C8G, or C8A. Memory optimized R4, you want to look at R7Is, R7As, R8Gs, etc. And finally for burstable, look at the T3s or the T4Gs powered by Graviton. And if you do this, you can have an expected outcome of 20 to 30% price performance improvement simply through your instance type modernization, right sizing, and using flexible instance types.

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/490.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=490)

 Okay, you've probably seen this slide as well many times, but just to hammer this home, a broad set of processors and architectures to choose from. Intel Xeon Scalable processors, AMD EPYC, AWS's Graviton processors, and Apple Silicon. So lots of choices, lots of different architectures to choose from. You want to find the types that work for you and preferably as many as possible that will work for you.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/520.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=520)

So why is it important to modernize? Well, as you can imagine,  every generation improves over the previous. So for example, seventh generation M7Is, C7Is, R7Is achieve up to 15% better price performance. So you'll often hear us talking about price performance. It's something that you want to think about measuring in your particular environment. Don't just simply look at the bottom price of that particular instance type. Think about what it means for your particular workload. So if a modern instance type is providing better performance, perhaps you can use a smaller size or you can use less of them to power your overall workload. And in order to measure that, you really have to track price performance by looking at both the overall cost and what the metrics are for your particular workload.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/560.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=560)

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/580.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=580)

### Leveraging AWS Compute Optimizer and Auto Scaling for Workload-Driven Right-Sizing

 Okay, so AWS Compute Optimizer. So one way to think about how to make sure that you're optimizing for your particular workload, and we talked about a path to optimization, is a service called AWS Compute Optimizer.  It's a free service that provides recommendations for all supported resources. So what it can do is it can look across your EC2 footprint and it can help give you recommendations for right sizing, for how that instance is performing for the work that it's doing. It's very easy to set up. I'll show you in the next slide how to enable it, and it provides continuous monitoring so that you can reference Compute Optimizer over time to see how are the improvements that you've made in your environment doing. Are you heading in the right direction? Is there a recommendation?

Is there a different instance type you should be considering or multiple instance types? But it's really a great free service for you to use in your environment that can provide continuous reporting.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/620.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=620)

So how do we enable Compute Optimizer? It's a  one-time setup. You simply run a command: AWS Compute Optimizer update enrollment status to active. What happens next? Once you do that, it collects 14 trailing days of CloudWatch metrics automatically. It analyzes your CPU, memory, network, and disk patterns for all instances. It recommends optimal instance types, including 7th and 8th generation, and it identifies Graviton migration opportunities automatically. You can opt in to say, hey, I'm able to use Graviton, please provide those recommendations as well. And what you can do is you can install the CloudWatch agent and send your memory metrics for your instances to Compute Optimizer so that it can look at the memory utilization running in your environment.

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/660.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=660)

 Okay, so what happens after 14 days? So you can now go back and query Compute Optimizer, say, hey, give me a report and your recommendations for my particular environment, put those in a CSV format so that I can take them back and I can parse those and query them in my environment, whatever the environment that might be for me to check out the optimizations. But ultimately it lets you get recommendations to help you choose optimal configurations for overprovisioned resources and potentially underprovisioned, right? So, but again, using these customized recommendations for your specific workload needs, and it really enables CloudWatch agent to unlock additional savings opportunities.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/710.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=710)

Okay, so what are some different ways to right-size your environment? Well, some customers,  what they do is once they've been able to spend some time right-sizing, understanding which instance sizes are best for their workload, they're able to reduce their overall fleet size. This is one great way to optimize. A second way is to consider either reducing your instance size or changing your instance type, right? So these are three different patterns we commonly see customers utilize. Sometimes they do two at the same time. They're able to reduce fleet size and reduce an instance size, for example, or change a type all at the same time. And in any case, obviously using fewer resources means lower cost, and it helps lower your overall carbon footprint as well for those of you that are after a sustainability advantage by reducing your overall footprint.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/750.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=750)

 Okay, so what are some right-sizing best practices? So we really want to think about targeting utilization ranges depending on the CPU architecture that you've chosen. Perhaps 40 to 70% utilization is optimal for anything that's overprovisioned. If you've got less than or greater than 20% idle resources, consider downsizing. If you're underprovisioned, give yourself, you know, if you have less than 30% headroom, consider sizing up.

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/780.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=780)

All right, another great way to do this is  to use dynamic scaling with EC2 Auto Scaling. So what you can do is you can feed your workload requirements to an EC2 Auto Scaling configuration and then let it figure it out. You can have it scale up and scale down dynamically based on real-time information that your application is sending into CloudWatch. You can scale across multiple instance types. Consider using Spot Instances as well inside of your Auto Scaling group. Anyways, lots of options when you consider using EC2 Auto Scaling.

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/810.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=810)

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/820.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=820)

Here's an example of a mixed  instance group for an Auto Scaling group config. This is where we're being extremely flexible and we're optimizing on price capacity for the overall  capacity deployment. Another great way is to consider using something called Attribute-Based Instance Selection. This is where you tell an Auto Scaling group what your workload needs, how much memory it needs, how many vCPUs, and then let it go figure out which instance types match that requirement, and then it spins them up for you automatically.

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/840.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=840)

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/860.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=860)

 Here's an example of what we call ABIS, or Attribute-Based Instance Selection, configuration for an Auto Scaling group. Think about utilizing this. It's a great way for you to think less about individual instance types, more about what your workload needs. Okay, consider launching across  multiple generations. Of course, let ABIS do the work for you, but again, the more flexible you are, the more options you have to optimize.

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/870.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/880.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=880)

And  here's just some configs that you can use. I want to keep going to make sure I don't run out of time here. One thing that I'll call out is  make sure that you do enable dynamic scaling. So in this case we're enabling both target tracking and predictive scaling for our Auto Scaling group to make sure that it's able to scale up and down for your application requirements and optimize for both cost and performance.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/900.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=900)

### Implementing Purchase Model Strategies: Combining Spot, Savings Plans, and Capacity Reservations

Okay, so purchase options, and we talked  to, I know I had a question in the audience about optimizing across different purchase models. So we offer a combination of usage and purchase models: On-Demand,

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/920.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=920)

on-demand capacity reservations, capacity blocks, and EC2 Spot Instances. Each of these have benefits. What you can do is utilize all of these across your environment  and then layer things on top of them like Spot for interruptible workloads that really taps into our unused capacity but can provide savings of up to 90% off of the on-demand pricing in exchange for allowing your workload to be interruptible.

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/940.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=940)

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/950.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=950)

What workloads are great for Spot? Asynchronous interruptible workloads,  accelerated scale-out operations, and time-shiftable workloads are all great use cases for Spot. You can use it to integrate  with notifications, to checkpoint and use state management for your work. You can build stateless applications and you can understand the interruption behavior for a particular Spot Instance to make sure that you're architecting to use them correctly.

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/970.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=970)

How else do we optimize for cost? Savings Plans, right? So this is a huge thing that a lot of customers  will sometimes overlook in their savings optimization opportunities. And why is this important? You can combine a Savings Plan with a capacity reservation to make sure that you've ensured the capacity, but you've also ensured the savings. We have a lot of different flavors of Savings Plans in one-year and three-year options, but also compute and instance type. So if you're very flexible and you're able to use multiple instance types, I recommend a Compute Savings Plan. If you've really locked into an instance type that is really good at powering your workload, then an instance-level Savings Plan will give you the best possible pricing for that. But again, utilizing capacity reservations with a Savings Plan is really going to allow you to enjoy the best of both worlds from a capacity perspective, but also a savings perspective.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/1020.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=1020)

 And so how do we layer these all together? What we like to talk about is using for the steady-state workloads, the very bottom, you can use capacity reservations and Savings Plans for that known steady-state workload to optimize both from a capacity perspective as well as a cost perspective. For spiky workloads that you need to know you can get the capacity but you're not quite sure what the variation will be, you can use a little tier of on-demand. And at the top you can layer on some Spot for those flexible, fault-tolerant workloads. So customers that can really utilize all three of these categories for their workloads, they will achieve the most efficient utilization of EC2 at scale.

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/1070.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=1070)

So to tie this all together and to close the optimization gap,  you really want to make sure you take a moment to understand the overall EC2 breadth and optimization opportunities. Think about all the different types, don't spend too much time on them, but understand them, right? Then let our services like Compute Optimizer, Auto Scaling, Systems Manager, and CloudWatch help you optimize and do the workload-to-instance-type matching methodology and let it scale up and down for you. And then make sure that you implement the correct purchase model and optimization strategies for your particular workload. So again, think about how to use Savings Plans, how to use your capacity reservations, and minimize your on-demand and tap into Spot for ultimate savings for those flexible workloads.

[![Thumbnail 1130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/945b2c8bbea1fc9f/1130.jpg)](https://www.youtube.com/watch?v=sc_3hfucTnY&t=1130)

So I think we made it in time. I'm at the final slide. We really appreciate your feedback in the mobile app, and here is my contact information on LinkedIn. Please, if you have any questions that I don't get answered here, just send me a message. I'm happy to respond to you, and I'm happy to  contact you and work outside of re:Invent and get you connected with any additional information that you have. So thank you everyone for coming. I really appreciate it and I'm going to hang around to answer any questions I didn't get to. Thanks.


----

; This article is entirely auto-generated using Amazon Bedrock.
