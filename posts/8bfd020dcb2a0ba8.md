---
title: 'AWS re:Invent 2025 - How Samsung Uses AI Agents to Optimize Infrastructure at Scale (AIM238)'
published: true
description: 'In this video, Kyotack Taylor Kim from Samsung Electronics explains how Samsung uses Cast AI to optimize infrastructure at scale. He discusses managing over 1,000 EKS clusters and tens of thousands of VMs across multi-cloud environments for services like Bixby, Galaxy AI, and Samsung Wallet. Key challenges include managing mixed CPU and GPU workloads, scalability issues, and integration complexity. Samsung implemented an AI Agent solution with Cast AI at its core, achieving over 30% cost savings through bin packing optimization alone. The solution provides AI-driven automation for real-time resource optimization, reduces operational overhead, and enables the use of Spot Instances without AWS commitment. Samsung''s future outlook focuses on fully autonomous cloud operations with minimal human intervention and unified optimization across diverse multi-cloud environments.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8bfd020dcb2a0ba8/0.jpg'
series: ''
canonical_url: null
id: 3086297
date: '2025-12-05T11:29:34Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - How Samsung Uses AI Agents to Optimize Infrastructure at Scale (AIM238)**

> In this video, Kyotack Taylor Kim from Samsung Electronics explains how Samsung uses Cast AI to optimize infrastructure at scale. He discusses managing over 1,000 EKS clusters and tens of thousands of VMs across multi-cloud environments for services like Bixby, Galaxy AI, and Samsung Wallet. Key challenges include managing mixed CPU and GPU workloads, scalability issues, and integration complexity. Samsung implemented an AI Agent solution with Cast AI at its core, achieving over 30% cost savings through bin packing optimization alone. The solution provides AI-driven automation for real-time resource optimization, reduces operational overhead, and enables the use of Spot Instances without AWS commitment. Samsung's future outlook focuses on fully autonomous cloud operations with minimal human intervention and unified optimization across diverse multi-cloud environments.

{% youtube https://www.youtube.com/watch?v=h8wY_GlNR3I %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8bfd020dcb2a0ba8/0.jpg)](https://www.youtube.com/watch?v=h8wY_GlNR3I&t=0)

### Introduction: Samsung Electronics' AI Infrastructure Leadership and Global Operations

 Hello, I'm Kyotack Taylor Kim from Samsung Electronics. Today, I'll introduce myself and our subject. I'm a Lead of Core SRE Part at Samsung Electronics. I operate several projects from Samsung Electronics, including Bixby and Galaxy AI, which are main AI services at Samsung Electronics. I'm also engaged in mission-critical projects such as Samsung Wallet and Samsung Accounts. I will discuss examples from these projects during this session. Today's agenda is how Samsung uses AI agents to optimize infrastructure at scale with Cast AI. I come from Samsung Electronics and I'm not an employee of Cast AI, so I'm simply explaining my experience using Cast AI.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8bfd020dcb2a0ba8/80.jpg)](https://www.youtube.com/watch?v=h8wY_GlNR3I&t=80)

 Let me explain Samsung Electronics in brief. This company was established in 1969 and has grown to more than 260,000 people worldwide. My company produces mobile phones based on the Galaxy brand and also manufactures displays and home appliances. We also have B2B businesses where we make semiconductors, which are an indispensable part of Samsung Electronics operations. Additionally, there are R&D centers established globally at the bottom of that slide. Each R&D center focuses on localization and has specialized features such as AI or SRE.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8bfd020dcb2a0ba8/150.jpg)](https://www.youtube.com/watch?v=h8wY_GlNR3I&t=150)

### Operational Challenges: Managing Massive-Scale Multi-Cloud Kubernetes and Mixed CPU-GPU Workloads

 Here are the challenges my company faces. We need to operate massive-scale Kubernetes operations, and scale and complexity are very challenging for us because we run our applications on top of multi-cloud environments including AWS, Google Cloud, Azure, and Samsung Cloud. This makes operations more complex as we must consider more infrastructure code based on the multi-cloud approach. We also need to think about fine-grained security policies for operators and developers. Additionally, we have more than 50 applications running on top of AWS, with some applications running on AWS only and others running on AWS and other cloud providers. We operate over 1,000 EKS clusters in our system and tens of thousands of virtual machines running our applications.

The second challenge involves CPU and GPU workloads. A critical mix of CPU and GPU workloads is very important to us because we have a separated team for training and inference. The training team wanted to simplify their operational requirements and run their training infrastructure on top of a single Kubernetes cluster. The inference team wanted to have more GPUs at cost-effective prices. We also have traditional backend and frontend development teams that want unified requirements from us. Standard cost-saving limits are also important. Until last year, we only focused on Reserved Instances and savings plans for cost savings.

This approach is very attractive because with only some commitment to AWS, we can get more cost-effective infrastructure. However, from this year we are also considering Spot Instances, which became much more attractive after we started using the Cast AI solution. Spot Instances are very appealing because without any commitment to AWS, we can get more affordable infrastructure by using Cast AI.

### Samsung's AI Agent Solution Architecture with Cast AI Integration

This is the solution that Samsung has implemented through an ongoing project called AI Agent. AI Agent has several separated categories, including infrastructure management, security, change management, and monitoring. Additionally, Cast AI integration manages GPU and CPU workloads. I will touch more about this later. Banking optimization for intelligent workload placement is also important. On the right side at the center of our AI Agent architecture, there is Cast AI. Cast AI controls the AI operations and workload simultaneously and supports AWS EKS.

Here are some key challenges we face. First, there are scalability issues. We need to efficiently manage thousands of diverse workloads across massive Kubernetes clusters, and this is very important because our operators need to support Kubernetes version upgrades. Everybody knows this is very difficult because we spend much time on it. Additionally, there is a performance lag issue. In the case of Samsung Wallet, Korean customers tend not to bring their plastic cards to the market these days. So when Samsung Wallet is impaired, customers cannot buy anything from the market. For the Samsung Wallet project, performance lag or impairment and latency are very critical concerns. Integration complexity is another challenge. As I mentioned earlier, my company is running more than fifty applications on the cloud. In this case, simple architecture is very important. I am thinking about Cast AI because when we put only one component in our infrastructure, we can easily achieve our AI architecture in place.

### Technical Implementation and Quantifiable Impact: Automation, Bin Packing, and Cost Reduction

Here are some technical solutions we are implementing. The first is automation driven by AI, using Samsung's AI Agent to continuously optimize the cloud in real time. As I mentioned earlier, change management, security, monitoring, and other AI Agents are running and cooperating. On top of these AI Agents, there is also a supervisor agent running. When the implementation is complete, I believe this operation system will be very smart. The next solution is Cast AI integration, leveraging intelligent automation to scale fast and automatically right-size workloads in real time. When we implement Cast AI, operational engineers do not need to operate manually anymore. Cast AI automatically controls our infrastructure. The next solution is bin packing optimization. I believe that when someone considers Cast AI first, they easily tend to focus on Spot Instances only. However, I had some proof of concept in my team, and the results were very good. When we focused only on bin packing automation using Cast AI, the cost savings exceeded thirty percent.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8bfd020dcb2a0ba8/620.jpg)](https://www.youtube.com/watch?v=h8wY_GlNR3I&t=620)

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8bfd020dcb2a0ba8/630.jpg)](https://www.youtube.com/watch?v=h8wY_GlNR3I&t=630)

I think we are using Cast AI simultaneously, and the cost saving output will be even better.  I'm going to talk to you about the results and future outlook of Samsung Electronics as well.  Let me discuss the quantifiable impact. Reducing operational overhead is crucial. AI-driven automation streamlines infrastructure management, which is very important because when I installed the Cast AI solutions, we can easily figure out our status and progress by using Cast AI's monitoring. I believe that the monitoring dashboard of Cast AI is quite impressive, and I've also discussed this aspect with the Cast AI team. Reducing manual work for scaling, provisioning, and optimization is also important. Less toil is very important for the operational team.

I have more than 50 silos, and after I implemented Cast AI, the team no longer needs to monitor every morning. This is also very significant. The next benefit is improved application efficiency and real-time optimization. Cast AI continuously right-sizes resources, which means the Cast AI agent is always checking our workload and sometimes scaling the number of nodes or adjusting node sizes. We can eliminate over and under-provisioning, allowing us to use all of our resources at their physical load, and this is a very important aspect.

Significant cost reduction combines AI automation, smart bin packing, and Spot usage. The combination of these three aspects creates much better cost saving outcomes in our company and enables substantial savings across Kubernetes environments. Automation also means that we can focus on other things in our company. We can usually focus on creating other agent jobs and making additional environments for AI operations, which is also significant cost reduction as well.

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8bfd020dcb2a0ba8/800.jpg)](https://www.youtube.com/watch?v=h8wY_GlNR3I&t=800)

### Future Outlook: Toward AI-Driven Autonomy and Unified Multi-Cloud Optimization

Here is the future outlook of my company. We are moving toward AI-driven autonomy and a fully autonomous cloud operation. This is the final goal. From the logs and AI systems, we are getting more information about our infrastructures, and by using those kinds of log systems, we can create more advanced AI autonomy. Minimal human intervention is required. In the case of mission-critical projects, we need to monitor our systems 24/7. This was a very critical aspect for operations because human beings cannot focus on monitoring all the time. When we introduced Cast AI, the team now requires minimal human intervention.  The second aspect is industrial standards. We are defining new benchmarks for efficiency and scalability. Over the last few years, for example, techniques like mixture of experts could be a benchmark and dominant in this area. These kinds of changes have a very big impact on our infrastructures, and Samsung Electronics is more focused on these kinds of changes through adopting new technology in GPU processing.

Next is driving cost-effective AI-powered cloud operations. Cost-effective means we can use fully optimized infrastructures based on the Cast AI product, and we are focused on AI-powered automation operations as well.

Lastly, unified optimization is very important to my team. We are expanding AI automation across CPU and GPU workloads. You can see the GB 200 and GB 300 during this re:Invent, and it is an excellent combination of CPU and GPU. We already have GPU and CPU combination workloads in our systems. Well-organized operations are very important as well.

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8bfd020dcb2a0ba8/1030.jpg)](https://www.youtube.com/watch?v=h8wY_GlNR3I&t=1030)

Finally, delivering consistent optimization across diverse environments. Multi-cloud is always very challenging. Sometimes one public cloud could be impaired. In this case, we need to fail over our workload to other companies. This kind of orchestration is very challenging for the operational team, so we needed to make our architecture simpler. In the middle of our architecture is Cast AI solutions.  This is all of my deck. If you have any questions, just let me know.


----

; This article is entirely auto-generated using Amazon Bedrock.
