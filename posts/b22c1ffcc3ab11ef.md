---
title: 'AWS re:Invent 2025 - Under the hood: Architecting Amazon EKS for scale and performance (CNS429)'
published: true
description: 'In this video, AWS introduces Amazon EKS Ultra-scale Clusters supporting up to 100,000 nodes and 800,000 NVIDIA GPUs in a single cluster. The team explains architectural innovations including a reimagined etcd data store with offloaded consensus to a multi-AZ transaction journal, in-memory database using TEFS, and intelligent key partitioning. They also announce provisioned control plane with three new performance tiers (XL, 2XL, 4XL) offering up to 16GB etcd capacity and predictable high performance. Anthropic''s Nova DasSarma shares how they run Claude model training on EKS ultra-scale clusters, using custom schedulers like Cartographer for workload-level scheduling and achieving 5000 GB/s throughput with S3. Performance improvements include 3X faster pod startup with AWS SOCI parallel pull and optimized networking with CNI enhancements.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/0.jpg'
series: ''
canonical_url: null
id: 3088633
date: '2025-12-06T10:22:42Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Under the hood: Architecting Amazon EKS for scale and performance (CNS429)**

> In this video, AWS introduces Amazon EKS Ultra-scale Clusters supporting up to 100,000 nodes and 800,000 NVIDIA GPUs in a single cluster. The team explains architectural innovations including a reimagined etcd data store with offloaded consensus to a multi-AZ transaction journal, in-memory database using TEFS, and intelligent key partitioning. They also announce provisioned control plane with three new performance tiers (XL, 2XL, 4XL) offering up to 16GB etcd capacity and predictable high performance. Anthropic's Nova DasSarma shares how they run Claude model training on EKS ultra-scale clusters, using custom schedulers like Cartographer for workload-level scheduling and achieving 5000 GB/s throughput with S3. Performance improvements include 3X faster pod startup with AWS SOCI parallel pull and optimized networking with CNI enhancements.

{% youtube https://www.youtube.com/watch?v=eFrSL5efkk0 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/0.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=0)

### Introduction to CNS 429: Amazon EKS Architecture and the Evolution of Kubernetes at Scale

 Good morning, everybody. Welcome to CNS 429, Under the Hood: Architecting Amazon EKS for Scale and Performance. I am Sheetal Joshi, Principal Specialist Solutions Architect for Amazon EKS here at AWS. I've been here coming up on the five-year mark. I'm going to be joined by Raghav in a few minutes here, and completing our lineup is Nova DasSarma from Anthropic, who is a Member of Technical Staff and also Head of Infrastructure at Anthropic. I just envy her job. She makes all these cool things possible, training these large cloud models at Anthropic. We are just excited to have her here on the stage with us today.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/70.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=70)

As you can see here, Kubernetes since its launch has revolutionized how applications are built and deployed.  In a recent survey of Kubernetes, as you can see, 93% of companies are either running it in production, evaluating it, or piloting it. Kubernetes is basically the declarative form that makes it easier for infrastructure management and became the de facto standard for deploying applications in cloud native environments.

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/90.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=90)

 Moving forward to Amazon EKS, while Kubernetes is undoubtedly great, the real challenge lies in managing Kubernetes at scale, managing hundreds of clusters in your production environments. As a CNCF certified and fully managed Kubernetes service, Amazon EKS uplifts all of that operational burden away from you to basically focus on your business applications. Here at Amazon, we run tens of millions of clusters at scale, and Amazon EKS over the years has become the most trusted way to run Kubernetes and enables you to build reliable, scalable, and secure applications. While doing that, it is fully upstream and certified Kubernetes conformant as well.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/140.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=140)

 As you can see, it's a quick journey into our Amazon EKS. We are into eight years of managed Kubernetes on AWS. We launched Amazon EKS here in 2018 during re:Invent. Since then, we have launched multiples of features, starting from managed control plane capability to helping you manage the compute that is required to run your applications, and also launching a lot of the add-ons, whether it is fully managed or the marketplace add-ons that your applications depend on and your cluster depends on as well. In 2022, we launched IPv6 clusters, and moving on, last year we launched Auto Mode and Hybrid Nodes, making it easier to get started on Amazon EKS and also making it easier to get started with your applications wherever your infrastructure is.

A little bit of recap, the one thing that I am most proud of, and the entire EKS team as well, is the launch of the Karpenter project four years ago. It is our version of the cluster autoscaler, and we are really proud to donate it back to CNCF. Karpenter helps you to autoscale and also bin pack your applications efficiently onto the infrastructure. This year, we have been focused on scale and performance along with that, providing the managed capabilities for you to run your applications and also manage your clusters at scale too. So let's just get right into it.

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/230.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=230)

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/240.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=240)

### EKS as the Foundation for Diverse Workloads: From Web Applications to AI/ML Innovation

 Today I'm very thrilled to share how EKS is powering innovations while allowing you to architect your cluster for scale.  As you can see, let's start with the foundational truth. Your mission is to deliver the applications and not worry about the infrastructure unless you really require to, and the versatility of this foundation is evident in the incredible diversity of workloads that our customers deploy to Amazon EKS. As you can see, some customers just take whatever they run in a legacy environment, containerize it, and deploy to EKS. Some run web applications at really large scale, and some are building large complex data pipelines and data frameworks onto EKS as well. This architectural approach allows organizations like Anthropic to also run AI and ML workloads on EKS.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/290.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=290)

 So how are our customers actually accelerating AI and ML with EKS here? Looking at it, these are some of the examples. What you see here is autonomous vehicle development, and for autonomous vehicle development, EKS provides the scalable infrastructure to process these massive amounts of sensor data and complex perception models that need to be deployed.

For robotics development, Amazon EKS orchestrates the training environments that simulate real-world physics scenarios, allowing companies to safely test and refine their robotic control systems. Moving on to generative AI, large language model training on Amazon EKS has revolutionized how these models are developed and built. Leveraging all the fundamental AWS EC2 capabilities, Amazon EKS allows you to build scalable infrastructure where you can host your models and provide inference endpoints. As you already know, there has been a lot of buzz about agents, at least at re:Invent. In the emerging field of agentic AI, Amazon EKS enables teams to run thousands of agents and orchestrate these agents at scale as well.

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/360.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=360)

Moving on, let's take a look  at what the AI/ML workload characteristics are. These are not like traditional web applications. AI/ML workloads depend on vast amounts of data, whether it is structured or unstructured data, and they are trained on vast amounts of data as well. They are compute-intensive and require high-bandwidth, low-latency networking interconnect and parallel read-write storage solutions. At the same time, simplified infrastructure management is at the core for efficient scaling, scheduling, and discovery of this infrastructure, as well as the repair of this infrastructure. Along with that, they depend on a broad set of capabilities, plugins, tools and frameworks, and also the guardrails to meet security and compliance requirements.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/410.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=410)

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/430.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=430)

This is not just us saying this. This is what  Gartner says. As you can see, by 2028, 95% of new AI deployments will use Kubernetes, which is up from less than 30% today. This is evident in how customers are leveraging Kubernetes and Amazon EKS for their AI/ML revolution  as well. This is all great, but why do customers really choose Amazon EKS for AI/ML? Because Amazon EKS is upstream Kubernetes conformant, it already comes with a tooling ecosystem. You can quick start with a vibrant open-source system with all of the tooling integrations.

The second critical advantage of Amazon EKS is the unparalleled customization capabilities that it provides. Organizations have granular control over the infrastructure that they can choose from with all of the underlying AWS EC2 instance types. Third, of course, is extensibility. You get integrations with a wide variety of AWS services from compute to storage to networking, and you can run anywhere. We have launched hybrid nodes, and with the support of hybrid nodes, you can leverage the infrastructure that is on-premises or on your edge to connect to the Amazon EKS control plane and run your AI/ML workloads there too.

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/510.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=510)

At the same time, it is highly scalable. Rapid scale in and out with governance control is possible with Amazon EKS. At the same time, with the help of Karpenter, you can achieve cost optimization out of the box. Karpenter supports right-sizing and efficient management of your compute for performance and scale.  This is great, but all of this innovation depends on building the right foundation as well. What does Amazon EKS provide for you to build that right foundation to begin with?

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/520.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=520)

### Amazon EKS Control Plane Architecture: Deterministic Resiliency and Intelligent Scaling

As you can see,  just as a level set, we're doing a quick recap of the Amazon EKS control plane architecture here. Let me walk you through the core architecture. When you create a cluster, you are connecting to the regional endpoint and you invoke an endpoint to create a cluster. When you do that, we create API server instances across two availability zones, and the etcd data store is spread across three availability zones. Security is built into the foundation of Amazon EKS as well, and the control plane and the etcd components run in a private VPC, providing VPC-level isolation for you. For cluster access, you create an API server endpoint and we provision a network load balancer underneath it, and you can say whether you want a public or private or both for the API endpoint as well.

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/570.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=570)

 How do we actually handle the resiliency here? How do we respond to availability zone failure? Spreading these API server instances and etcd across different availability zones is great, but how do we handle the resiliency? What we do here is deterministic availability zone resiliency. We have implemented what we call deterministic resiliency for the control plane as well as etcd. What that really means is predictable outcomes even during availability zone failure, seamless API experience, and no disruption of Kubernetes controller workflows, and maintained API latencies throughout as well. During an availability zone failure, we take four critical actions.

We immediately pause all cluster updates to preserve capacity, redirect all API traffic to the healthy availability zone, and relocate leader-elected controllers to the healthy AZs. Along with that, we transfer the etcd leadership to maintain database performance and quorum. This depends on the static stability principles. Our highest priority is to aim for static stability, even when there is an AZ failure, so existing clusters continue to run and provide that availability for you to run your applications and to be able to connect to your Kubernetes control plane.

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/680.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=680)

We carefully architect around these dependency failure modes, and we proactively test thoroughly for AZ failure scenarios on our side with all of the packet drop scenarios, health check scenarios, and dependency service disconnects, because your running applications depend on multiple other AWS services and interconnects as well. So moving on, how does Amazon EKS scale its control plane? Let's talk about the core scaling architecture. What happens here is that Amazon EKS acts on multiple signals. We started off with monitoring CPU and memory, and then we added on the node count, etcd database size, and  other API server performance metrics as well.

Our recent breakthrough improvements include parallel scaling of the API server and etcd, and blue-green API server deployments, optimized bootstrap with the preloading of all of the files and with S3 prefetching as well. This actually helps reduce the warm-up times for all of the components. The scaling process is both intelligent and conservative, with global scaling using progressively larger instances and smart cooldown periods, which are reduced to 15 minutes, and automatic QPS and burst rate adjustment for our controller frameworks and the scheduler. This all lets us maintain the promise of 99.95% availability while providing the scaling and reducing the time from 150 minutes to 10 minutes for you.

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/740.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=740)

### Data Plane Flexibility and GPU-Powered Infrastructure: Karpenter, Hybrid Nodes, and Accelerated Compute

So moving on from control plane architecture to the  data plane, what you see here is from the utmost left, which is providing you the utmost flexibility of managing your own infrastructure, which is self-managed node groups, to the utmost right, which is completely managed auto mode node pools, wherein we shift away the operational responsibility from you, and then we manage the instances and the add-ons that are required for your applications to run. As you can see here, we also support the Karpenter node pool, and on the right side, you can bring your own infrastructure from your own premises with the help of hybrid nodes.

Four years ago, as I said, we built Karpenter, and Karpenter is the next generation autoscaler, and we have donated it back to CNCF. Karpenter gives you the flexibility of choosing different instance types from on-demand and spot instances. It also does support GPU, and you can specify the capacity reservations for GPU within the Karpenter specification. Karpenter helps reduce cost by constantly rebalancing your workloads and bin packing those very efficiently as well.

With Amazon EKS hybrid nodes, you can now use your existing infrastructure, as I said, on the edge or on-premises, connecting back to the control plane that is actually running within the region, thus getting the efficiency and scalability out of the box. Here at Amazon EKS, we support all of the EC2 instance types from different categories. As you can see, general purpose, burstable, storage high I/O, and graphic intensive, moving on to different capabilities with a choice of processors to choose from, whether it is AWS, Intel, and high memory footprint instance types and all, also the accelerated compute instance types with the instance storage choices of HDD or NVMe and different sizes, networking, and bare metal options as well.

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/870.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/880.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=880)

With EKS, you have access to all of the silicon innovation that is happening here at AWS, whether it is powered by Nitro or Graviton for the best price performance as well. With all of that, you will continue to have access to the purchase of different  options, whether it is on-demand, savings plans, or spot instances. So what you see here on the screen are the different accelerated compute instance types, whether it is NVIDIA or our  own accelerated frameworks as well, including Trainium and Inferentia. At the top, as you can see, we do support different GPU instance types, G's and P5s, and also the latest versions of the P instances too.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/900.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=900)

This is what I'm really excited to share with you today.  As you can see, our customers use millions of GPU-powered instances with Amazon EKS, and that number has only doubled since last year.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/910.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=910)

 This means Amazon EKS is becoming the de facto choice for most customers to run their AI/ML workloads here at AWS.

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/930.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=930)

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/940.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=940)

### Accelerating Innovation: New Features for Optimizing Compute, Operations, and AI/ML Workflows

So how does Amazon EKS actually accelerate innovation? We talked about building stronger foundations, but how do we accelerate innovation here at AWS?  What you can see here are all of the features that we launched recently with Amazon EKS. As you can see, we make using EKS easier  with the launch of auto mode and hybrid nodes. What you see here are features that actually help to optimize compute.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/950.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=950)

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/980.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=980)

 The first one that you see here is AWS SOCI. With that, we launched lazy loading last year, but basically with lazy loading, our customers have not been able to meet the performance that they really were depending on. So we launched parallel pull. With parallel pull, Amazon EKS accelerates image loading and unpacking  with very memory-intensive and high-performing HTTP range methods.

The other thing that you can see here is that Amazon EKS supports capacity block reservations that you have purchased and accelerated AMIs. The accelerated AMIs actually combine all of the optimized drivers and runtime components for GPU and AI accelerators, significantly reducing the setup time for you. With the launch of Kubernetes DRA, you can actually enable fine-grained sharing and allocation of GPU resources across multiple AI workloads and pods here at AWS. With EKS, you can integrate EC2 Ultra Servers very easily, and on EKS you can run different device plugins, operators, and frameworks.

The last one here is S3 Mountpoint drivers. The S3 Mountpoint drivers enable direct access to your training data and model artifacts stored in S3, treating them as a local file system with enhanced performance. This capability significantly reduces the data loading time for your AI/ML workloads.

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1050.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1050)

 This is all great, but what about operations? Day 2 operations are also critical, right? We do support node health and auto repair. So what does this really mean? EKS continually monitors the health of these nodes through a sophisticated system that integrates with EC2 health checks and Kubernetes node conditions. When an unhealthy node is detected, it repairs that node and replaces it with a healthy node by gracefully shutting down that node and gracefully evicting the pods on that node as well. With Container Insights, you can provide comprehensive observability into your EKS AI/ML workloads as well.

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1090.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1090)

 What you see here is how our customers are architecting all these together. The top part is the AI/ML OSS tooling and integrations. As you can see, customers are building Jupyter notebooks, they are configuring AI/ML workflows and AI/ML job scheduling, and building model registries with memory optimizations, multi-modal management, and dynamic batching included. Kubernetes as a platform, a layer down, what we see is our customers already have EKS as the platform for other applications, and they extend the same platform to build data pipelines and do model development, model training, and inference on the same cluster with all of the infrastructure orchestrations integrating into other AWS services.

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1140.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1140)

 Moving on, this is how it looks like for most customers in production. As you can see, EKS supports multiple model frameworks, and some of our customers do implement MLOps workflows on EKS. They run the analysis, model development, and use different frameworks such as Kubeflow, Ray, and MLflow, doing model training, evaluations, and validations as well. On the right-hand side, as you can see, customers are also doing model deployment and serving, leveraging both NVIDIA GPUs and the AWS Neuron architecture here, and integrating with all of the AWS storage layers and networking services included.

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1200.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1200)

### Introducing Amazon EKS Ultra-Scale Clusters: Supporting 100,000 Nodes and 800,000 GPUs

This is all great, right? But AI/ML workloads have some key challenges even after all of this building of stronger foundations and the features that we talked about. They have some critical characteristics, as we can see here.  These AI/ML workloads, especially training, require massive coordinated compute.

Training AI and ML models, especially at scale, requires massive coordinated compute that needs coordination along thousands of accelerated instances to work as a single coordinated system with low latency and high bandwidth needs. Along with that, the tools and frameworks that we use to orchestrate these AI/ML workloads do not work well across clusters, and it is hard to manage frameworks and mapping across different clusters. This also means managing different clusters increases operational overhead too.

[![Thumbnail 1270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1270.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1270)

So customers are really looking for reduced operational overhead, simplified cluster management, and also shared governance on these clusters, thus allowing them to get to the improved cost efficiency that they're looking for, increasing the resource utilization across different workloads on the same cluster, and sharing the capacity pools that they have already purchased. As an answer to this, I am very proud to introduce you to what we launched here recently:  Amazon EKS Ultra-scale Clusters.

[![Thumbnail 1300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1300.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1300)

So what is Amazon EKS Ultra-scale Clusters? I feel it's a groundbreaking innovation here at AWS that I'm very proud to announce that our Amazon EKS team has done, pushing the boundaries of what's possible with EKS. These clusters support up to 100,000 nodes in a single cluster, enabling you to manage 800,000 NVIDIA GPUs, which translates into  nearly 1.6 million AWS Trainium chips.

Without further ado, I would like to invite Raghav on the stage. Raghav is an engineering leader here at Amazon EKS. He and his team are responsible for operating tens of millions of clusters at scale. Raghav, please take it away.

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1340.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1340)

### Reimagining etcd for Ultra-Scale: Raghav's Deep Dive into Next-Generation Data Store Architecture

Thank you. Thanks for the introduction, Sheetal. Alright, so I think as Sheetal was discussing, Kubernetes has basically become a key enabler for running large-scale AI/ML workloads over the last few years.  We noticed this trend around two or three years ago that larger models were more capable, and we wanted to make sure that we're setting up the right fundamentals to support those workloads.

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1360.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1360)

So I want to start with the data store itself. We'll dig into how did we actually enable ultra-scale  for Kubernetes architecture. It all starts with etcd. So etcd is at the heart of every Kubernetes cluster, as most of you know. It's the distributed key-value store that stores every piece of configuration, so every pod, every config map.

Here's how it works by default. So for every Amazon EKS cluster, we create a three-node etcd cluster that uses the RAFT consensus algorithm to maintain strong consistency across the three nodes. So the architecture you're seeing here shows the key components. You have gRPC and HTTP for client communication. The MVCC layer handles the concurrent reads and writes. You have the BoltDB as a database engine and then the Write Ahead Log for durability.

So over eight years, we've made this architecture incredibly reliable. We scale this cluster on demand in response to your workloads. We run tens of millions of clusters using this architecture today. However, etcd was never designed to support a single Kubernetes cluster with 100,000 nodes, right? So we had to ask ourselves, how do we take this battle-tested infrastructure and basically reimagine it to support 100,000 nodes in a single cluster?

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1430.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1430)

 So this is probably the most important slide I'll go into today, so bear with me. In traditional etcd, the RAFT consensus algorithm is deeply integrated into the database itself. What you see is every write has to go through the RAFT that gets coordinated across the three nodes first. This works great at regular scale, thousands of nodes, maybe tens of thousands of pods, but that becomes an extreme bottleneck when you're looking at 100,000 nodes.

So the first thing that we tackled here was we offloaded consensus to a purpose-built multi-AZ transaction journal within AWS. When etcd is not managing its own consensus and durability, we offload this to a specialized system that's purpose-built to do high-throughput distributed consensus. This removes one of the largest bottlenecks in a Kubernetes architecture today. And this also allowed us to scale etcd horizontally because we're not bound by the quorum requirement anymore, so we don't have to have three nodes for etcd or five or seven. We can scale etcd horizontally, which is a really good property to have.

Next, traditional etcd also stores all the data on the disk using BoltDB. So when you're looking at millions of objects undergoing read and write operations, disk I/O becomes the next natural bottleneck.

We leveraged the fact that we had moved durability to the purpose-built system, and we could move the cluster state from static EBS volumes to an in-memory database using TEFS. This helped us unlock an order of magnitude higher read and write throughput because we're just reading and writing from the memory itself. Also, this change helped us increase the total etcd database size that I'll go into later.

The next thing is that traditional etcd is also a monolithic database. All of the cluster state is stored in a single key-value store, so you can imagine it becomes very hard to scale this architecture horizontally. When we were scaling up these workloads, we identified around four or five keys that take most of the traffic: nodes, pods, leases, and events. We intelligently partitioned those keys into their separate dedicated etcd key-value stores, again allowing us to increase the total read-write throughput for etcd by this change. These are the three core changes that we did that really helped us support the scale and the throughput that we need to support ultra-scale.

[![Thumbnail 1570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1570.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1570)

All right, so let's take a look at the same architecture with the changes I just described.  We created a consensus interface which connects to a multi-AZ transaction journal. You have the MVCC layer that's now writing to the in-memory database, and then finally we have a partitioned key space that can allow us to horizontally scale etcd. But the key thing here is we maintain the same API semantics that Kubernetes expects. We didn't have to change anything in Kubernetes core while we're doing these three changes.

It must be resonating with you that these are not incremental improvements. This is a complete architectural rethink in how Kubernetes can store and retrieve data at scale, and this is what basically made ultra-scale possible. I'll show you some of the results in the next slides.

[![Thumbnail 1610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1610.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1610)

### Ultra-Scale Performance Metrics: Managing Millions of Objects with Unprecedented Throughput and Latency

 In the first slide, what you're looking at is just the scale of the operations. What you're seeing here is three distinct AI/ML workloads from our customer deployments and testing scenarios. I'll start with the first one. We submitted a massive pre-training job using StatefulSets on all the 100,000 nodes. This gave us the confidence that we can sustain long training runs over an extended period of time.

Next, we submitted what I call the mixed mode workload: multiple parallel fine-tuning jobs where each job occupied 10,000 nodes, and then we also served real-time inference using LeaderWorkerSet on the Llama 3.2 model. The mixed mode workloads are slightly different. They're very bursty, so they pose extreme stress on the control plane. I think the key takeaway from this slide is all of these workloads are running in a single ultra-scale cluster, so you're not looking to split these workloads across multiple clusters trying to coordinate them. As Sheetal mentioned, the frameworks out of the box do not think about being able to manage these workloads across multiple clusters. This is the real part of why we're motivated to solve this problem in a single cluster.

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1680.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1680)

All right, so in the next slide,  we're looking at the number of objects of different kinds as we scale from zero to 100,000 nodes and then back down. At peak, we are managing tens of millions of objects: around 8 million pods, 100,000 node objects obviously representing the cluster infrastructure, 6 million lease objects for doing leader election coordination, and then tens of millions of events representing the cluster activity. This is clearly unprecedented scale. Traditional etcd was never designed to handle this volume of objects.

As a result, we also had to push the limits on the total etcd database size. With ultra-scale clusters, we support up to 20 gigabytes for a single cluster, which is 2.5 times what we get in standard EKS today. We all know it's not just about the raw storage capacity. What matters is can we actually serve milliseconds of read and write latency when the working set gets this large? And that's what I'm going to go into next.

[![Thumbnail 1740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1740.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1740)

All right, so this slide talks about the throughput.  On the left, you can see that at our peak we were able to serve around 7,500 read requests per second. The throughput also scales smoothly as we start to add more nodes and more pods to the cluster. You must be wondering why is this important. These are graphs, they're interesting, these are high values. But if you think about it, Kubernetes or most of our customers use a lot of controllers and operators, right? At that scale, they're all looking for changes in the cluster itself to make decisions such as pod placement, monitor pod health, and coordinate workloads. So all of this starts to matter.

In addition to the next-generation data store, I do want to say that we use an upstream change that allowed us to serve consistent read requests from an API server cache instead of going to etcd, and that really saved that additional round trip. On the right, you have the write throughput. Write requests are obviously more expensive because they still require consensus and they require persistence. We were still able to peak at around 8,000 to 9,000 write requests per second.

This was primarily driven by the next-generation data store, the journal backend that I discussed previously. When you're thinking about having millions of objects, 100,000 nodes, there's always a part that is being created. There's always a part whose configuration is being updated. You're doing scaling operations. All of those go through etcd. Traditional etcd would have bottlenecked at a fraction of this throughput.

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1840.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1840)

All right, let's quickly look at the second half of this equation, which is latency, right? Because you can have all the throughput in the world, but if you're taking seconds to serve a request, that will still impact your workloads. So on the left you can see that we were able to serve  read, write, and delete requests between 100 milliseconds to 1 second at P99. We achieve this optimal performance by carefully tuning things such as request timeout, retry strategies, worker parallelism, and throttling rules. It's worth noting that doing all of this work only matters at this specific scale.

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1860.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1860)

 On the right you have list requests. If you're familiar with list requests, they're very special to the Kubernetes architecture because a single list request can return thousands, or in the scale that we're talking about, millions of objects in a single request. We were still able to respond to this request between 5 to 20 seconds, which is way below the 32-second upstream SLO for list. So one of the ways we achieve this that I want to highlight is we made an optimization in how the items in the list requests get encoded. By default, Kubernetes encodes the objects in the list request in a batch, but these batches were getting so big that we moved to incrementally encoding these list requests, significantly reducing the memory consumption that it takes to serve one. As a result, this allows you to serve a lot more requests in parallel.

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/1920.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=1920)

### Optimizing the Data Plane and Networking: Accelerating Container Images, Network Bandwidth, and Node Scale-Up

So all of the work that I've described so far results in a single high-performance, highly responsive Kubernetes cluster. All right, so we just walked through the performance  landscape of the control plane, right? But that's kind of like half of the story. It's an important part, but it's still half of the story. So to enable AI/ML workloads, we knew we had to optimize every layer of the stack. We started with the control plane, then we got to data plane and networking. So let me just walk through a couple of other innovations that we did to maximize application performance and resiliency.

AI/ML workloads run on instance types that have two unique characteristics. They have network bandwidth up to 100 gigabits per second, and they also have EBS volumes that support extremely high IOPS and throughput. And we wanted to make sure we take full use of these characteristics. So when we look at these AI/ML workloads, they tend to have very large container images, often exceeding 5 gigabytes. Pulling these container images can have a pretty large impact on your workload readiness. If you think about it, this really determines how long it takes to start the training or serve that first inference request.

So using the AWS SOCI snapshotter that I think we highlighted previously, we basically parallelized both the container download and unpacking operations. Next, we saw that the AI/ML workloads also download a lot of data at startup, often from S3. So we made a change in our CNI that allows a single pod to connect to all the network cards on this instance, giving it access to the entire network bandwidth that I just described. The combination of these two changes reduces the time it takes from a pod to go from being scheduled to running to having all the data it needs by 3X.

And then finally we have to talk about node scale-up, right? So Karpenter has become the standard in how customers are scaling compute in response to pending workloads. Since we own both the compute scaling and the networking plugin, we made another optimization where we pre-assigned IP prefixes to pods to nodes during that launch instead of doing it reactively through a CNI after the launch. This further reduced the time it takes for a node to launch, become ready for workloads. And if you think about it, if you're scaling from 0 to 100,000 nodes, every second matters. And I'm really happy to announce that all of these features and capabilities have been available in EKS since the middle of the year.

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2050.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2050)

 Okay, so we kind of walked through the company's journey for ultra-scale, showing you how we kind of reimagined the data store to support this massive scale. These innovations represent a fundamental rethink in the Kubernetes architecture itself. We have learned a lot in working with customers like Anthropic. Now I want to show you how we're bringing those learnings and making them accessible to every other EKS customer.

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2080.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2080)

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2100.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2100)

### Provisioned Control Plane: Bringing Ultra-Scale Learnings to Every EKS Customer with Predictable Performance Tiers

So this is where provisioned control plane comes in.  Not every customer needs 100,000 nodes. We completely recognize that. But customers are looking for high-performance control plane and predictable control plane behavior. And think of it this way: we took all the learnings from ultra-scale, our deep understanding of being able to optimize the control plane performance at every layer of the stack, and now we are making them accessible to you through provisioned control plane. 

Let's start with why we built provisioned control plane. Using provisioned control plane, you can proactively select a tier that matches your business needs instead of having the control plane reactively scale based on demand. Let's say you're doing a major deployment next week or you have a product launch coming up. You can come to EKS and say, I want to run this cluster on a high performance tier, so you're guaranteed the capacity is there when you need it.

Next, we are also introducing three new performance tiers that offer significantly higher performance such as API request concurrency, pod scheduling rate, and database sizes that were not accessible in standard clusters before. That means you can run exceptionally more demand and workloads than you could previously. Finally, even if you're running on standard most of the time, you can temporarily scale up to a higher tier when you have an event coming up, and then you can scale back down. This gives you the ability to optimize for both performance and cost in your EKS environments.

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2170.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2170)

Starting last week, we give you two control plane scaling operations.  I talked about standard, and we have provisioned. Standard control plane continues to be the right choice for most workloads. It's automatic, it's battle tested, and it handles dynamic scaling beautifully. Provisioned control plane is when you need predictable high performance capacity. The best analogy I've been able to think about is standard is driving a car where the engine is automatically tuning itself based on how you're driving. It's great for everyday use. Think of it as a Toyota. Provisioned control plane is being able to step into a sport mode because you know you're going to need that performance tier when you're driving.

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2220.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2220)

I wanted to spend some time talking about the standard control plane because this is what most of you are using today. As I mentioned previously, standard control plane scales up and down automatically  based on workload demand. We constantly monitor several signals: CPU memory utilization of the control plane, IO, object counts, the total database size. When we detect that the cluster needs more capacity, such as you're deploying a large number of pods or you're seeing an increase in your API traffic, we reactively scale up the control plane in approximately ten minutes. Once the demand decreases, we scale the cluster back down. It's worth noting that this dynamic scaling is happening within the standard tier levels.

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2270.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2270)

If you're thinking about when you should stick to standard control plane or the standard mode, if your workloads have a predictable pattern, if you're good with automatic scaling, or you don't have a critical business event where you need guaranteed capacity, I would recommend continuing to use standard. But let's talk about provisioned control plane.  This is where you take control of your own capacity planning. You can proactively provision a specific performance tier to match your business needs.

Here's how it works. We are providing you three additional tiers: XL, 2XL, and 4XL. These tiers provide significantly higher capacity than standard, more API request concurrency, faster pod scheduling, and larger database sizes. When should you use provisioned control plane? Let's say you have an anticipated high demand event next week, or you have performance critical workloads that need the minimum possible latency from the control plane, or you have massively scalable workloads like the ones we described with AI model training, inference, or data processing.

If you're wondering how do I start to use it, we've made it incredibly simple. You can use the cluster create or update APIs that you're familiar with. Add one parameter, the cluster control plane scaling config, choose your tier and you're done. There's no need for migrations. There's no complex configuration. There's no downtime. You can start to use provisioned control plane on any Kubernetes version starting 1.28 or later.

[![Thumbnail 2340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2340.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2340)

I want to dive into what  capacity each tier provides so you can choose the right tier for your workload. Let's start with the API request concurrency. This is the number of concurrent API requests that your control plane can handle at any given time. Think of it as the number of conversations it's having with clients such as kubectl, Kubernetes operators, and controllers. Next is the pod scheduling rate. This is the number of pods per second the scheduler can place onto your nodes. If you are doing a large deployment with thousands of pods frequently, this parameter will significantly determine how quickly they become ready.

Finally, you have the maximum database size. Remember the conversation from ultra scale? As your cluster grows, so does the database. Each provisioned tier supports up to 16 gigabytes in total capacity for etcd, which is double what you get in standard today, giving you significant headroom for growth. You must be wondering, okay, I still need some more guidance on how do I select a tier. If you find yourself running a lot of custom operators and controllers, I would consider looking at how you're doing with respect to API request concurrency and select a higher tier.

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2430.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2430)

If you do frequent deployments with a large number of pods, such as Spark jobs, I would start to think of using tiers with higher pod scheduling rates. And if you have a large cluster with many objects, we see this often with customers using really large custom resources or config maps, you want to make sure you never exceed the total etcd database capacity. So I encourage you to go check out the pricing page.  We try to make the pricing as transparent and predictable as possible for each tier so that you can balance and optimize for both performance and cost.

So here's the next critical question. You've selected a tier. How do you know you're effectively utilizing it? And this is where monitoring comes in. We're making several new metrics available to you to monitor your tier utilization in real time. Starting with the API request concurrency, this will tell you how many API requests your control plane is handling currently. So say you're in Excel and you see yourself being close to 1600 or you're around 1600 out of 1700 API request concurrency, you may want to think about scaling up your control plane. Next is the pod scheduling rate. This will tell you the number of pods per second that the scheduler is attempting to place, how many are succeeding, and how many are failing. And then finally, the database size. Each tier has 16 GB in total capacity. If you're running close to the tier limit, I highly encourage you to start thinking about cleaning up your unused objects or start to look at some architectural changes.

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2500.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2500)

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2530.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2530)

So here's how this would work in practice. Before a major event, say a product launch  or a big deployment, check your baseline utilization. If you're already at 70 to 80% of your current tier's capacity, you should proactively scale up. During the event, monitor these metrics in real time. If you see sustained high utilization or a spike getting close to the tier's capacity, you can just call the cluster update API and upgrade to the next tier on the fly. And then after the event, review your utilization again. Say you upgraded to 4 Excel, but you're only  using 30% of that capacity now, you can scale back down to 2 Excel or even Excel.

[![Thumbnail 2540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2540.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2540)

All right, so we just walked through provisioned control plane as a feature, showing you how we're taking the  learnings from ultra scale, the architecture itself, and then making them easily accessible to you as EKS customers through these three new tiers. I'm just going to quickly summarize a couple of points here. So it's incredibly easy to use, right? You don't need a new cluster. You don't need to migrate your workloads. You can just call one API to get started. Second is the flexibility. You're never locked into a single tier. You can upgrade your control plane to higher tiers and you can go back all the way down to standard. And then finally, we have built this feature with the operational excellence that we acquired from running tens of millions of clusters. You're running effectively the same architecture that you get from ultra scale, just in a much more easy to use way. So I'm really excited to see how all of you can use this feature to optimize for both performance and cost.

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2590.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2590)

### Anthropic's Journey with Ultra-Scale Clusters: Nova DasSarma on Building Claude with EKS

All right, so on that note, I'm excited to welcome Nova.  Nova is the infrastructure lead at Anthropic. Nova and her team have been working with us over the last few years as they've scaled up their compute environment on EKS. They've been running machine learning workloads successfully on the ultra scale configuration for the last six months. Nova will go into the motivations behind their own architecture. Please welcome Nova to the stage.

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2620.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2620)

Hey, thank you so much. It's hard to follow. So let's talk a little bit about  what we do and why we do this. So I work at Anthropic. We do Claude. You can see that on the slide. We recently released Opus 4.5. If you haven't tried it, you should consider it. It's the best in the world at code and also most of the other things. So I'm pretty proud of that. And that was something that we built on EKS. So we do a very large amount of machine learning on AWS, and as part of that, almost all of our compute is done inside of EKS. There are very few cases where we have some raw EC2 instances, but I would say over 99% of our capacity is managed by EKS today.

And I'm going to talk a little bit philosophically about why we do things the way we do, why we push for things like ultra scale clusters, because I think that it's an interesting architectural decision. Because if you look at upstream and you look at how Kubernetes has been used historically, there's been a big push to try and have manageable clusters, manageable failure domains, and there are a lot of reasons to do that, so it's still worth thinking about. On the slide we can talk about Trainium. We use a huge amount of Trainium today. I think the vast majority of our workloads, especially around inference, are served on Trainium, and we use these very large clusters.

[![Thumbnail 2710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2710.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2710)

So I guess one question I ask is what makes a cluster a cluster, so it's not tied for us to a physical  domain. This is the big important thing that makes choosing when to make a new cluster weird. If in some cases you had an application that was only in one physical location, so we didn't have distributed training, we didn't have distributed inference, then it would totally make sense to try and align your cluster control plane with the physical domain because this gives you predictable properties around resiliency. It gives you predictable properties around maximum cluster size, and it doesn't give you this problem of continuing to scale things forever. But I work at Anthropic and we're a scaling lab, and so we want to scale things forever.

So when do you actually need a new cluster? The way that we think about this is it's when there's a single point of failure that you can't engineer out in some other way. We talked previously about what the architecture of the control plane looks like. There are many different places where there is resiliency within a single cluster. You have to do a lot of things to protect the control plane from runaway workloads. Obviously when you are in a multi-tenant environment, it's harder to think about some of those things, but Kubernetes does give you the tools to do this today with things like API Priority and Fairness, with things like quotas, and with namespaces to put things inside of the same cluster.

[![Thumbnail 2800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2800.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2800)

So why ultra-scale clusters for us? A big one is that we get a single  pane of glass for observability. When we think about how we're designing our applications, I really want Kubernetes to work for the application. I don't want to design my application around Kubernetes. There are two different axes, and this isn't on the slide, I'm very sorry. There's scale out and there's scale up, so a single application should fit inside of a single Kubernetes cluster. That's the thing that we believe pretty strongly here. If you're going to take a single application and put it across multiple clusters, then you're just building Kubernetes on top of another Kubernetes, and we already had Kubernetes for that, so I'm not going to do that if I don't have to.

For us that's driven by things like the size of our largest training job. Our largest training job is huge, and it's also inside of one namespace inside of sometimes one stateful set. So this really pushes us to get these really big clusters working, and then everything else is what drives things like the QPS. There's a database scaling piece and then there's a QPS piece. There are reasons to scale out. I think that if you actually do have applications that are localized, then you might want to use multiple clusters to meet that need. The other thing is the scaling around for scaling for our customers, and when I say customers here I mean oftentimes our internal customers for the infrastructure team.

Efficient multi-tenancy for ops is important. Every one of these instances is very expensive, which you all know already. Any time that we are spending where we either idle an instance or we are waiting for an instance to scale up or scale back down again is time that we could be putting more training workloads on it. It's dollars that are out the door. It's slop that's inside of our overall architecture that is a KPI for me. It's this thing that I'm going to try and drive down. Having one logical resource pool, even if it's across multiple domains whether across availability zones and things like that, makes our job easier.

[![Thumbnail 2930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2930.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2930)

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/2960.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=2960)

One of the things we talked about this earlier, we have  extremely large images. Our images are something in the 35 gigabyte range. We've worked with Amazon EKS on the streamable OCI stuff to optimize that in a bunch of different ways. On the slide you can see, well, we can see something. You can see we've got these different layers that are different sizes, and so when you have large layers, those large layers are going to oftentimes be the bottleneck for how fast  your whole thing completes.

You want to think about your application as something that's either IO bound or CPU bound or network bound. In these cases, we were seeing that we were actually CPU bound. I think that being able to use multiple cores for doing the parallel decode or the parallel unpack and things like that has improved our P50 time for parallel pull by almost 5 minutes, which is a huge amount across thousands of servers. For us this means that oftentimes with a large training run or something like that, that is the time,

this sort of MTTR when there is a failure of a node might be determined by the time it takes to pull that image onto a new node. Obviously we do things like prefetch that help us with that, but it's just really not, I don't know what's going on with this slide. I'm very sorry guys. Sorry, it's very distracting. I think that's a thing that we optimize for. It's trying to optimize the amount of downtime that we have, and so the other part of that is optimizing our scheduling.

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3030.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3030)

 We actually use two different schedulers today. One of them is an in-house scheduler that I'm going to talk a little bit about. We actually do use the default scheduler for a lot of things today. The vast majority of our daemon sets are scheduled via the default scheduler. All of our CPU workloads are scheduled via the default scheduler, and EKS has made that work even for extremely large Spark workloads, tens of thousands of nodes. That's been very exciting for us as well, but one thing that we do is we try and scale on something that is a smaller number.

When I say that we've got these very large training workloads that have thousands of pods in them, I actually don't care about how fast any individual pod schedules. I'm not going to be able to do anything until I have all of them. In that case, the thing that I'm looking for is something that scales with the number of workloads, not the number of pods. Today, the way that we do this is we have something called Cartographer that we're considering open sourcing. Please talk to me if that's an interesting thing to you that scales with the number of workloads.

We use an informer and we have a Rust-based thing that looks at the entire cluster at the time and sort of loops through and schedules a whole workload, does bindings to every node at the same time with topology awareness for network optimization. Especially in large clusters, this is super important for us. We want to be able to co-locate even within a single job different parts of the job to different network domains. The Amazon architecture includes things like spines for networking, which you might be familiar with from things like placement policies.

We want to be able to support an ML workload being co-located inside of a Kubernetes cluster that is ultimately multi-tenant while being network optimized, and this can make a difference of somewhere in the 2 to 3x range. It can also be the difference between being able to do it or not because I really don't want to get paged by EC2 next time I do something like this. I'm very excited to say that this is coming upstream soon. We're not clear that we're going to use this, but we are hoping to do that.

[![Thumbnail 3170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3170.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3170)

We're hoping that Kubernetes will be able to do the kinds of things that we do custom today for everyone because I think that this is very exciting. There are many kinds of batch workloads where this is the dimension that you want to scale on. It's not the pods.  One thing that we have had to do is we've had to scale our DNS. DNS is oftentimes the bottleneck in these things.

Core DNS has a couple of different things upstream. I actually didn't mention it on this slide, but we're really excited about being able to do multi-core Core DNS upstream, which means that you can vertically scale a single Core DNS instance at a time. One thing that we do is we've actually been able to scale down the total number of instances that we've had in these large clusters. We've often had hundreds of different replicas of Core DNS, which has a severe downside which is that a cache hit is much less likely, which means that pushing through those requests to the upstream DNS, you're just putting a lot of load on that.

That means that you run into things like network limitations for how many UDP packets you can and DNS entries you can shunt out of each of your Core DNS instances. Being able to pack those onto smaller instances means you have warm caches, which means you have ultimately a whole lot less upstream load. This is pretty positive for me from a vertical scaling perspective. Today, we really want to avoid having a service mesh unless we really need to.

Today we find that basically we can do that service discovery via DNS for almost all of our applications because a lot of our applications just really aren't microservicey. A lot of them are, I'm from an academic background, they're cluster applications. They're parallel applications. For those we really want to know what pods are inside of the same workload. We don't care as much going between those workloads, and so that's something that we've been able to do without a service mesh today, which is pretty exciting at this scale.

[![Thumbnail 3290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3290.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3290)

Endpoint slices has been one of our bugbears though, so that one has not scaled for 100,000 services, so we're hoping to see some upstream improvements on that, especially going into CB 135, along with some other optimizations there. Talk to me after the  talk. I'm going to give a couple of shout outs to some of the other services inside of Amazon that we use, so we actually don't use parallel file systems today for the vast majority, or explicit parallel file systems for the vast majority of our workloads. We use a lot of object stores.

[![Thumbnail 3350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3350.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3350)

I've given this slide roughly before, but the way that we do this is we have an object store, we have a prefetch buffer, and a queue basically so we can absorb not arbitrary latency, but seconds of latency at times to the job leader, which then broadcasts things out to all the workers. It sounds a little obvious when I'm saying it like this, but this has been what's led us to scale without having a Lustre or something else that is like a big parallel file system for some of this stuff. We're looking at the S3 CSI for doing some of that prefetch in an implicit way. Today we use it explicitly, and this is last year's slide.  You know, 800 gigabytes per second with no provisioning required, so it's now 5000. So we're really pushing S3 very hard on this. I didn't ask them before the slide, hopefully they're okay with saying this, but it is pretty exciting to see that we have been able to continue to scale on this object store without necessarily having other layers in there.

[![Thumbnail 3370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3370.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3370)

We do use parallel file systems in one place, which is that  we talked about Jupyter. There's a lot of Jupyter users at Anthropic, and those Jupyter users oftentimes have a multi-pod workflow. Historically, the way that we did this is we had something where we synced the code from one pod to a bunch of other pods, and that works fine. It has bad consistency properties, and EFS has much better consistency properties than that. So that is one place where we've been excited to see multi-attach and the EFS CSI for that.

[![Thumbnail 3400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3400.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3400)

And then one last  slide that I've got for you guys is things that we're looking for in the future that we're excited about. We're excited about namespace controllers. When we're talking about that single point of failure, the KCM, it's great for a small cluster to have all 30 controllers inside of the same process. For a large cluster, it means that when you have one of those controllers get unhappy, then you have 30 controllers to worry about. So I think seeing that get namespaced out, getting charted out is going to give me the failure domains and the properties that I care about for large scale. There's things like multi-VPC architectures for ultra clusters where you start running into the NAT limits and things like that. We're excited to think about Karpenter for capacity reservations. This is something that's fairly new to us, but we're pretty hyped for this. We use a lot of Karpenter internally for our CPU workloads, and it's exciting to be able to do that for GPU and Trainium workloads to move away from autoscaling groups if possible, because it means that we have one thing to manage. We have one set of configuration for the way that we provision nodes.

And then finally, we said IPv6 came last year. IPv6 has been coming for 20 years, but we want to be able to move basically everything that we can to IPv6 for this big scaled flat network to make it so all of our services are able to talk to each other again without a service mesh. And with that, I'm going to welcome folks back to the stage and we can do a little bit of closing thoughts and Q&A. Thank you folks.

[![Thumbnail 3500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3500.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3500)

[![Thumbnail 3510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3510.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3510)

### Closing Thoughts and Resources: Exploring EKS Best Practices and Advanced Sessions

Right. Thanks, Noah.  That was super exciting to see all that work done together. I think just as a roundup, we talked about  how EKS has become the foundation trusted way to run clusters that, you know, battle-tested reliability went to ultra scale, like our sort of motivations behind why we build that for specialized workloads like the ones that Anthropic is running, and then really how we're making all of that available to you through provisioned control plane. So please try it out, give us feedback. We're always looking forward to that.

[![Thumbnail 3530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3530.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3530)

[![Thumbnail 3560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b22c1ffcc3ab11ef/3560.jpg)](https://www.youtube.com/watch?v=eFrSL5efkk0&t=3560)

These are some  of the sessions that are still remaining from EKS, and I want to highlight the one at the top. That's a chalk talk. AWS is doing a 500 level series for the first time this year. If you're interested in this, slots left, I highly encourage going to that chalk talk. It's from two principal engineers, one from EKS, Sham, and one from the transaction journal team, Nami, who are going to go into all the dirty details that I didn't cover. I'm probably not equipped to cover. And then some useful links, our user guide.  We do workshops monthly. You can sign up for them, and then we have an AI on EKS website, which is a comprehensive set of tooling blueprints that helps you sort of try out some of these things on your own. We try to keep it updated with all the latest and greatest that's happening in machine learning right now. So shout out to the best practices guide, and the best practices guide is really, really good. You guys should check it out. Yeah, that's it. We'll take some questions. Thanks for spending time with us.


----

; This article is entirely auto-generated using Amazon Bedrock.
