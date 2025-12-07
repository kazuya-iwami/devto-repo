---
title: 'AWS re:Invent 2025 - How customers build AI at scale with AWS AI infrastructure (AIM252)'
published: true
description: 'In this video, AWS experts and customers discuss practical strategies for scaling AI infrastructure. The session introduces a framework covering scaling up (increasing compute power), scaling out (adding more GPUs/servers), and optimizing across the inference stack. Andrea Klein from ARM shares lessons on managing 1000+ GPUs, emphasizing fault detection, consolidating capacity in single ultra clusters, using Kubernetes with EKS, and implementing gang scheduling. Henri Dwyer from Genentech describes optimizing molecular dynamics and LLM training for drug discovery, achieving mid-90% GPU utilization through distributed checkpointing and improved data loading. Shaunak Godbole from Fireworks AI presents principles for building agentic AI, highlighting that enterprises should own their models trained on proprietary data. He details inference optimization across hardware selection (P5, P6 instances), inference engines (model sharding, speculative decoding), serving stacks (prompt caching, session affinity), and global secure serving. Fireworks processes 13 trillion tokens daily serving 150,000 requests per second using these techniques.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/0.jpg'
series: ''
canonical_url: null
id: 3087948
date: '2025-12-06T02:07:01Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - How customers build AI at scale with AWS AI infrastructure (AIM252)**

> In this video, AWS experts and customers discuss practical strategies for scaling AI infrastructure. The session introduces a framework covering scaling up (increasing compute power), scaling out (adding more GPUs/servers), and optimizing across the inference stack. Andrea Klein from ARM shares lessons on managing 1000+ GPUs, emphasizing fault detection, consolidating capacity in single ultra clusters, using Kubernetes with EKS, and implementing gang scheduling. Henri Dwyer from Genentech describes optimizing molecular dynamics and LLM training for drug discovery, achieving mid-90% GPU utilization through distributed checkpointing and improved data loading. Shaunak Godbole from Fireworks AI presents principles for building agentic AI, highlighting that enterprises should own their models trained on proprietary data. He details inference optimization across hardware selection (P5, P6 instances), inference engines (model sharding, speculative decoding), serving stacks (prompt caching, session affinity), and global secure serving. Fireworks processes 13 trillion tokens daily serving 150,000 requests per second using these techniques.

{% youtube https://www.youtube.com/watch?v=_RAFKrdTHQQ %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/0.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=0)

### Introduction: A Framework for Scaling AI Infrastructure

 Welcome, everyone. Good afternoon. We are excited to have this customer panel with us. We have an exciting session for you that is very relevant to what you heard in the keynote. How many of you were able to watch Matt's keynote this morning? Great, so you heard about AI infrastructure and how we're optimizing it. You heard about AI Factory, the Trainium 3 launch which is now generally available, and you heard about ultra clusters and ultra servers.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/40.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=40)

 We have a distinguished panel of speakers. Andrea Klein is Director of Engineering at ARM. Henri Dwyer is Senior Director of Engineering at Genentech, a member of the Roche Group. Shaunak Godbole is Field CTO at Fireworks AI. What we want you to take away from this session is practical tips, tricks, and mechanisms for scaling your AI applications using AWS infrastructure and our stack.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/80.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=80)

We have a framework that works well for thinking about scaling AI. The agenda is that I'll set up the framework. We think about scaling AI in terms of scaling up—increasing T-flops, memory, and memory bandwidth—and scaling out, which means increasing the number of GPUs, servers, ultra clusters, and clusters.  We also think about scaling across the inference stack. Each of our speakers will focus on different aspects of this framework.

### Panel Overview: Three Perspectives on AI Scaling Challenges

Andrea will talk about her experiences scaling out. She'll discuss fault detection and observability tools and what you need to pay attention to when you scale out to thousands of GPUs and thousands of clusters. Henri will talk about scaling up. He has a very interesting application around molecular dynamics, involving not just large language models but also spatial coding. He'll conclude with agentic AI, which Genentech is using for what they call lab in the loop.

That's a good segue to Shaunak's session, where he'll talk about the principles of building agentic AI and how they've optimized across each layer of the stack—the hardware layer, model frameworks, and runtime layer—to achieve 10 trillion plus tokens per day. Fireworks also has a booth, so Shaunak will have more details available there.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/170.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=170)

When I think of scaling up and scaling out, two workloads come to mind.  I spend most of my days working with accelerated compute and understanding customers' needs around AI and HPC. AI, as we think of frontier models with 100 billion plus parameters, requires a lot of compute. Even smaller models that are network latency sensitive, like video and image models, require scaling up and scaling out for inference, especially for aggregated batch mode inferencing and real-time scenarios where latency is critical.

These models process huge amounts of petabytes of data and perform massive amounts of parallel computing and scientific calculations. That's why scaling on infrastructure becomes important, and it's what our team at AWS spends every day discussing with customers. I should introduce myself—I'm a Principal Specialist in our Gen AI Accelerated Compute team. Everything related to accelerated compute at the hardware layer comes through our team. We work with Trainium and Inferentia, of course, but also NVIDIA GPUs and other GPUs day in and day out.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/280.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=280)

### Understanding AI Workload Characteristics: Training, Customization, and Inference

HPC also has interesting characteristics. It has both tightly coupled and loosely coupled workloads. I've had the opportunity to work in various domains. Henri will talk about his protein folding use case. These use cases also lead to large-scale requirements.  If I dive deep into one of the workloads, training workloads are typically episodic. They take a few weeks to a few months, but they are tightly coupled. Often when customers ask us for GPUs, they request them in a single availability zone. In fact, they ask for a single spine. One of the reasons we introduced ultra servers that you heard about in the keynote is because even a single spine was not enough. We needed to create a high-performance domain for them.

But customization is a little bit different. These runs are short but very frequent, so the runs could run anywhere from 1 to 4 weeks, and you could have anywhere between 4 and 10 cycles a year. You often could use heterogeneous computes, so you might have P series GPUs and G series with CPUs. You have to think about how to scale up using containers. Andrea will have a word or two about that and how to use tools like AWS ParallelCluster and Amazon EKS to scale up a heterogeneous compute cluster.

Inference is completely different. It's often spiky and sporadic. Even batch mode inference goes on for a few hours, let's say document processing or summarization, and these are often loosely coupled workloads. Depending on whether it's real time or batch mode, you could have different latency requirements. Let me think of a mock example of a chatbot, which is a very simplistic example of chatbot producing one output token at a time. Of course, now chatbots are much more sophisticated, but by and large input token length still happens to be higher.

The initial portion, the encoding portion or the prefill portion is compute bound. The intuition there is you load the hyperparameters from the HBM memory. Depending on the length of the prompt, you could essentially be using the entire prompt computation using those hyperparameters, so you're essentially amortizing the one-time load operation across a large context length. Now if you go to the decoding portion, it's the same intuition. You load the hyperparameters again from HBM memory, but now you're processing output one token at a time. So you're amortizing that load operation across one token at a time. Of course it's simplistic. Now you can have many more tokens at a time and in fact Shaunak will talk about how they optimize that at the runtime layer.

Think of memory capacity as inference. Some of the bigger models like Llama 4 and DeepSeek require at least a P5E, if not P5EN, which is our H200s, so memory capacity becomes important. But even for training, if you think of pipeline parallelism, if you're trying to compress a model into a single GPU and the memory is not enough, you start getting into pipeline parallelism, and the more pipeline parallelism you have, the less efficient memory usage you have, so that starts hurting performance as well.

Some of the models, especially mixture of expert models, MOE models, mixture of agents, some of the multi-modal models, video processing, and some of the large frontier models, they are network bound. We introduced a couple of years ago 10P10U networking, and as the name suggests, this is our goal to deliver tens of petabytes of non-blocking bandwidth across tens of thousands of servers in under 10 microseconds. This is something we use ultra clusters for, and we'll talk about that in a second.

### AWS Infrastructure Solutions: Ultra Clusters, Ultra Servers, and the Hardware Stack

This is the framework I want to present. When we think of scaling up or scaling on infrastructure, we can either scale out so you can add more and more servers and clusters and ultra clusters, which is Andrea's topic, or you can scale up, which is if you have smaller models or less than 10 billion parameter models, but even if you have bigger models and you want to homogenize your procurement and you have a certain cluster that you're purchasing, which is Andre's domain. You can scale up so you can now go from A100s to H100s to H200s and now Blackwell, or you can scale across which is the inference stack and that depends on what kind of models you have. Is it distributed high performance inference on the right or single node GPUs on the left.

I mentioned ultra clusters. In fact, today when Trainium 3 was announced, we also announced UltraCluster 3.0. Even UltraCluster 2.0 delivers 60 the bandwidth. This is our way of managing tens of thousands of GPUs or Trainium accelerators in one high-speed domain. So this becomes really important when you're scaling out to hundreds or even thousands of GPUs. We have another paradigm called UltraServer. Last year you heard how we were able to use UltraServer in a single high-speed domain of either 72 GPUs or 64 Cranium accelerators, and this morning Matt announced Cranium 3, which is our way of packing 144 Cranium chips in one single high-speed domain that gives you 5 times the performance per megawatt.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/630.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=630)

In terms of scaling up,  we have had a 15 year plus partnership with Nvidia. We have been launching instances almost every year, and over the last 6 months, the pace has increased, as you can see. A couple of weeks ago we announced a B300 base P6, which delivers a whopping 6.4 terabits per second networking speed and 270 gigabytes of memory per GPU. This is the biggest, baddest GPU that we have, but you can see and read the specs on others as well. We have GP 200 base P6E, and a number of you should be using Hopper-based instances as well.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/670.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=670)

The stack  consists of the hardware layer, which is what our team primarily focuses on at AWS, but on top of the hardware layer we have many other abstraction layers right from security hypervisor using Nitro to storage to EFA networking and then managed services like SageMaker. This is the framework I wanted to set, and now each speaker will speak about their experiences optimizing this. In fact, this will be the best part of the session where you can hear from them. We will have Q&A, and we would love to devote as much time as possible for the Q&A if you can hold your questions until then. It will help us go through the narrative first, and then we are open for questions after that and will pass the mics around for your questions.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/730.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=730)

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/750.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=750)

### Andrea Klein on Scaling Out: Managing 1,000+ GPUs with Kubernetes and Observability

Next up is Andrea. She is a director at ARM, and she will speak about her experiences.  Hi, my name is Andrea. I am here from ARM where I deal with GPUs, and I have prior experience dealing with a lot more GPUs at once. The following advice is really targeted at someone who is dealing with a large number of GPUs for the first time.  Maybe you have had a few servers and run a few experiments, but 1000 GPUs sounds like a lot, and you do not really know what to do. If you have not really budgeted for more than a few machines, most of this you do not need to worry about. If on the other hand you have raised something like 1 to 10 to 100 billion in a seed round and you are making a pre-training cluster, this advice is not for you either.

I am going to have a lot of different suggestions for you. The basic idea is that there are certain problems that emerge predictably at scale for GPUs, and 1000 is about the line where you cannot ignore them anymore. Somewhere between 1000 and 10,000 you will have all of these problems, and then exciting new problems emerge beyond that which we are going to ignore. Do not worry, we are going to solve all of these problems in about 10 minutes, so just pay attention if this is you. For my sake, let us assume you are my target audience and you have money. The money is not the problem. You need to buy some GPUs and you need to manage them.

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/810.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=810)

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/830.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=830)

 This is great. Congratulations, you now have a GPU farm, but you do need to do some planning in advance and think about how you are going to handle these things. It is a little bit harder than it looks, especially if you have dealt with a few machines before and think you know how to run GPUs and everything is fine.  I think there are a few things that are really important to pay attention to at this scale. The first thing is you are adding more GPUs, so problems that happen with GPUs happen more often. You are probably also adding more jobs and larger jobs. Larger jobs are big jobs that use more GPUs at once.

When you have bigger jobs and longer running jobs, any problem is going to occur more often. It is more likely that your workload gets disrupted and fails, and you need to be very good at spotting this and recovering from it. At this scale, you probably have a few different use cases going on. You may have multiple tenants or customers who are sharing the capacity. You may have different types of jobs, maybe some are training and some are inference and some are doing something else. You think you have bought a lot of GPUs, but if you ask your users, you have not really bought that many. Very quickly, all of the workloads pile up and are demanding GPUs all at once and you get yourself into some trouble.

The second thing is you are probably with this type of machine and this type of scale doing some kind of distributed workloads, probably distributed training, maybe even distributed or multi-node inference. Since you have to pay attention to networking and you have to pay attention to distributed job optimization and failures. Finally, as I mentioned, everything fails more often at scale. Rare failures become constant failures.

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/920.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=920)

And you have to worry about this. Don't lose all hope—it's not that bad. There are established ways of dealing with these machines at this scale, but before I talk  about what to do with your machines, I'm going to consider the possibility that you have not yet made any purchasing decisions. Maybe you just have money, but you don't have GPUs yet. If you're in that scenario and you're seeing many nice slides with many different types of GPUs and accelerated instances, it can get overwhelming trying to decide what to pick.

There are a couple of rules of thumb that are helpful. One thing, if it's not obvious, is to try and get all of your machines in one place. This is a little bit hard to do, especially with constrained availability. Maybe someone can offer you a few on one coast right away and a few in another place, but then you're going to do a lot more work managing them and your utilization is probably going to be a lot lower if you have a fragmented system. Given the option, especially for newer GPUs, I recommend either at the beginning or pre-negotiating over time to consolidate your capacity into a single location.

Increasingly for modern GPUs, this does not just mean one region or one zone with one control plane. It means one physical ultra cluster build or equivalently one networking spine. Try and get your machines of one type in the same physical cluster with very fast interconnects all in one place. You have a broad selection of instance types, and it can be tempting to hyper-optimize your machines to your workloads because maybe you have many different workloads and you benchmark them all and one is faster on one machine and one is faster on another machine.

However, you run a bit the same fragmentation risk trying to get greedy and get capacity wherever you can get it right away. If you try and get a super-optimized machine for each workload, you may not have consistent traffic to keep all your machines busy all the time. I tend toward what I call workhorse machines—machines that are fast enough and you have a lot of the same thing so they can handle most of your traffic and remain consistently busy. If you manage this and make a choice of maybe one or two accelerated instances all in one place, there are a lot of ways to manage capacity.

I think it's a fairly safe recommendation that at this scale, you should think about running your own clusters and that you should do so with Kubernetes. I think it's also fairly safe to say that for the majority of use cases, there's no major downside to using a managed Kubernetes service like Amazon EKS. I think there are exceptions to that, but you kind of have to have a reason not to use it as opposed to a reason to justify it. This will simplify your cluster management a lot.

There are some good practices about how to set up your cluster that we'll look at in a moment with an example diagram. You should standardize your cluster. All your machines should be on the same versions of things so all your jobs can run on all your machines and tend to have the same dependencies and same failures. You should do something to deal with GPU failures, which we'll talk about more as well.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/1120.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=1120)

Here's the diagram I promised, and it's just an example. You may have a slightly different setup, but there are a couple of things that I want to draw your attention to. The first thing is that you have somehow got your machines all in one place and so there is only one account in this diagram. There's only one  cluster, and all the machines are in one cluster, which is very nice.

The second thing to notice is that there's a mysterious scheduling or orchestration component implied in the cluster other than the built-in Kubernetes scheduler. There's a lot of selection and we'll look at some options for this in a moment, but especially for batch computing, I think it's broadly accepted that Kubernetes is not really going to have all the scheduling properties that you want for AI workloads. You're probably going to end up layering something on top to manage the way your workloads are scheduled to machines.

The third thing to notice is the node pools. This is deliberately vague. It could be auto-scaling groups or managed node groups of some kind, but they're divided up. The machines are not all in one giant pool somehow. Probably in Kubernetes, they've got different labels or taints and workloads are kind of pointed at one node pool or another. There's some CPU-only capacity in there. Don't forget to budget for that. You will probably need it.

There's a couple of different types of GPU. They're in different node pools with maybe different driver versions. In this scenario, there's been more than one GPU procured for the batch use case. There's some P5s and P6Es, and those are in different node pools and kept logically separate. Workloads are either going to one or they're going to the other. If you do end up combining some kind of real-time inference use case with offline batch processing, which there can also be reasons not to do, you'll need to think about how to manage that.

To manage this, I think you should at least have some kind of logical barrier between the real-time capacity and the offline processing capacity. Finally, I'm not going to make a sales pitch for any particular storage technology, but planning out your storage hierarchy is going to be a really important part of planning your cluster. It does help that the newer GPU instances increasingly come with a lot of built-in SSD in the form of NVMe, especially P5 and definitely P6E. That is your first and best bet for storage during things like training. It's very fast, it is very free, and it is built into the machines.

However, that is not a durable place to store anything that you care about long term. So probably there's some other storage in the picture that's keeping longer term records of training data and checkpoints or data that your workloads are processing while they run in the cluster. Finally, it is just a bit implied by this that you need to budget for things other than GPUs. I pointed out there are some CPU-only machines in there, so do not forget these. There's some storage, do not forget to budget for storage. And then finally, these things talk to each other, so there is definitely some networking traffic likely to be happening here unless all of your data is inside the cluster and never leaving. Probably there's some data flow in and out, so don't forget to plan for and budget for networking.

[![Thumbnail 1300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/1300.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=1300)

I mentioned scheduling and orchestration. You probably need to make some choices beyond just turning up Kubernetes and deciding that you're done.  For batch jobs, there's a longer list of quality tools than I can list, and so I don't want to sell you on any particular one, but I think anyone who tells you that there is a perfect scheduler for all workloads is a little bit suspicious. You should really evaluate a range for what you're doing. If you're looking at hosting serving workloads, you're probably looking at a different set of tools than someone who's doing mostly batch processing.

When you're thinking through the options, especially at this scale, 1000 GPUs is not as much as it sounds. If the size of your large jobs is anywhere within an order of magnitude of the size of your cluster, and if you do not have something that does gang scheduling on top of Kubernetes, your jobs are going to deadlock very fast. You will have two or three that are half started and nothing goes, and the whole system locks up. So you definitely cannot ignore this at the scale for this type of workload.

In addition to scheduling, you need to worry a little bit about resource management. You are definitely going to have a range of workloads and probably a range of tenants fighting over this capacity, and all you can really do is have some kind of policy that everyone has agreed to that the system enforces on their behalf. Whatever tool that ends up making your scheduling decisions probably also has something to say about your resource management decisions. So you need to look at your interest in both of those properties when you choose that tool.

Most of these things are fairly solid, but you should look not just in principle how the scheduling works and what promises it makes in the docs, but also make sure that it can handle the load and the traffic that you expect in the system when it's live, because the scheduling will be a single point of failure for your system. I will give a special shout out to GB 200. If you're dealing with any rack scale capacity like GB 200 or upcoming GB 300, at this point, you really cannot avoid dealing with topology in your choice of scheduler. Most likely some workloads have some constraints. Maybe a multi-node workload needs to go to the same rack or a big training workload needs to use several whole racks in training, and so you also need to choose a scheduling component that can express that the way you want.

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/1440.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=1440)

Finally, observability. If you have spent all this money and bought all these GPUs, you better know what is going on with them. This is partially for your own sanity, so you can tell if the system is working. It's partially for your users' sanity, so they can tell if it's working and they can also tell how busy it is.  And finally, it is also for what I will refer to as the powers that be, people who maybe have a financial interest in what you are doing with all the money and all of the GPUs. By the time they come asking what your utilization looks like, ideally, you should already know.

There are certain things I think you should really monitor in real time and for long periods of time so that you always have this information. You should know how much of your capacity appears to be working and you should know how much of it is used by end user workloads, what kind of scheduling utilization you're getting. And then depending on your use case, you should have some notion of live traffic to the system. The graph shown is for something like a batch system that has queued jobs and running jobs, and this also is very entertaining to people who want to know why their job is queued and whether the queue is very long.

Over longer periods of time, not necessarily in real time, but maybe on a daily or weekly basis, you should be tracking additional metrics to understand your system's performance and resource utilization patterns.

I really recommend at this scale having some notion of how responsible your end users are being. Some notion of workload efficiency that catches if workloads are requesting GPUs that they never use or running with really low average utilization or have really wonky resource configurations is a great idea. It will let you catch these things quicker before they tank your overall metrics. Finally, you should know how the computing time or the cost in the system is being spent between your tenants or your use cases and have some kind of monitoring or showbacks.

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/1580.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=1580)

[![Thumbnail 1590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/1590.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=1590)

### Henri Dwyer on Scaling Up: Genentech's Journey from Molecular Dynamics to Lab-in-the-Loop Agents

With this, I'm going to pass it off to Henri, who's going to talk about his lived experience in this domain. Hi, my name's Henri and I wish I had heard Andrea's talk a few years ago because I was the target audience. What I'm going to do is walk you through some of our learned experience scaling AI compute for lab in the loop.  I'll start by giving you a brief overview of what I'm talking about. I work in AI for drug discovery, and what we do is  make machine learning models to design molecules. When we talk about lab in the loop, what we mean is essentially you start with some experimental data and train machine learning models on it, use those models to come up with new potential molecules in our case, and then make those run experiments, get new data that hopefully feeds back into the models, and eventually by iterating, you reach the level of quality of your molecules that you're hoping for, or at least that's the dream.

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/1650.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=1650)

On the right, this is just an example of one class of molecules that we worked on called antibodies. We were optimizing across expression, how easy they are to make, and affinity. You can see over subsequent rounds we're kind of pushing the Pareto frontier forward, which is what you're hoping to do in this kind of process. In order for this to work,  one of the things that you need is good machine learning models. Nowadays, in order to have state of the art models, what that means is training large models on multiple nodes. In particular, I'll focus on one of the kinds of models that we've been working on, which are these LLMs in the 7 billion to 70 billion parameter range. I guess they still count as large. Very briefly on our infrastructure, we have our GPUs on AWS ParallelCluster, which is one of the schedulers that Andrea was talking about, and most of our long-term data storage is in Amazon S3.

By scaling up our model size and compute, we ran into all of the following challenges. The first one is hardware faults. When you're working on one machine, one node, a few GPUs, typically you don't run into this, but at scale they're inevitable. I think we ran into these as soon as we were scaling across tens of nodes. These are typically seen as the training job crashing with unclear reasons. In order to solve this, what we did was test the nodes. There are a variety of different ways you can do this. In the end, we sort of run a very simple PyTorch script on a new node, make sure that it's able to use the GPUs, load data on the GPU, run simple calculations, and then remove bad ones.

A trick here that took us a little while to figure out is just because you evict a node you might actually just get it back, especially when you start worrying about challenge number two, which is networking. With networking, these are typically really hard to debug errors. You don't often have clear reasons why some networking error happened or can see it in the logs. We usually saw these as nickel timeouts. What we found was really helpful is to really try to avoid hops. If you can avoid cross zone traffic or even cross spine networking, you can get much better stability and also better performance. But in one spine there's only so much capacity. That's why this issue of getting back the same node can happen. The final challenge is around the model implementation, and this is something that's really fully under your control.

One strategy that really helped us was carefully testing the various libraries used, such as CUDA, your EFA kernel, and if you're using P instances, as well as nickel OFI plug-ins and things like that. All of these are really brittle, and making sure that you test all of the versions that you're going to run together and upgrade them all together will lead to tremendous improvements in your stability.

On the data loading side, the common challenge is really feeding data fast enough to the GPUs so that you can get good utilization. This is the most common source of bugs that I see when machine learning scientists come to me with bad GPU efficiency. This is also a Python problem because in order to load data quickly you need to have multiple threads and multiple processes. This is notoriously finicky in Python, and spending a lot of time stress testing your data loaders is really useful.

On the checkpointing side, no matter what you do, there will be failures for your models. Being able to checkpoint and resume is critical, and that will really help you save wasted compute. Lastly, we found that as you start training larger models, moving away from abstractions and libraries that wrap PyTorch to native PyTorch helped us improve the efficiency and the stability.

[![Thumbnail 1910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/1910.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=1910)

Putting these into practice, here are some charts of our GPU utilization.  If you don't have the pleasure of staring at GPU utilization charts in your day-to-day job, I feel sorry for you. They're pretty fun, but what you're looking at here essentially on the Y axis is the percent average GPU utilization for an LLM training job. You can see a couple of issues with the one on the left.

The first one is low GPU utilization. We're around mid-60 percent, which means if you're running on 100 GPUs, about 40 or 35 of them are not being utilized on average. The other issue we see here are these pauses every couple of tens of minutes where the GPU utilization drops to zero. This is often a symptom of checkpointing where everything stops, you're writing to disk all of the weights, and then resuming. We can also see relatively short training runs on the order of several hours, which is because we had some stability issues.

By implementing some of the improvements and addressing some of the challenges I mentioned in the previous slide—distributed checkpointing, really investing in our data loading, and dropping down to fully sharded data parallel—we're able to really bring up our GPU utilization to where you want to be, around mid-90 percent. This is the chart I want to show to the powers that be to demonstrate how good we are at using our GPUs, not the previous one.

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2020.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2020)

Of course, using your GPUs is not enough. It's really important to  use them in ways that are effective. For us, we run varying types of compute, but I'll show here two major ones. On the left is molecular dynamic simulation, showing how long it takes to generate a one nanosecond trajectory. Here, less is better, so you want to take less time to generate that one nanosecond.

What you can see is kind of interesting: an L40S takes about as long as a B200 to run this compute. This is just one GPU. The L40S is relatively old in GPU generation time—it's three years old—versus the B200, which is pretty recent. This means if you have a lot of specific kinds of workloads, you should really benchmark them to see if it's worth considering having dedicated compute and tuning the architecture for that.

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2100.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2100)

On the LLM training side, this is what you see in all of the releases of new chips. We saw very similar results where you get more tokens per second, which is good on newer GPUs.  Tying it all together, thanks to some of our efforts in optimizing our LLM training,

we're now working on having intention-aware agents that are trained on all of our internal data so they understand drug discovery and our models. What we're working towards is having these agents be able to translate some requirements. In this case, the example I'm giving you is this phase in drug discovery called lead optimization, where you have a molecule and you're trying to improve certain properties. The idea is you ask your agents if they can improve these properties, and the agent can translate that into a workflow.

[![Thumbnail 2200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2200.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2200)

We run these generative models and property prediction models that we have, which are able to identify specific aspects of the molecule and then rank and filter those. We may run through a few loops of that, and ultimately we can send that for synthesis to the wet lab and look at the predicted and real output and compare them. In order for the agentic loop to work, what you need is to have all of your models deployed. We use Kubernetes to run those and some agentic framework to tie it together. That's how we've been working on speeding up the drug development process and trying to improve the success rate of our R&D. 

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2230.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2230)

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2260.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2260)

### Shaunak Godbole on Agentic AI: Principles of Model Ownership and Data-Driven Development

Thank you, André. As Henri was describing, the most successful agents that are built are built using models that are trained on the data that your enterprises have, and that's what I'm going to talk about. We call it artificial autonomous intelligence, which essentially means  you own your data, so you should essentially own your models and own your deployments as well. I'm Shaunak Godbole. I'm the Field CTO at Fireworks AI. We specialize in inference as well as training for a lot of open models. I think 2025 is the year of agents. 

Almost all of you must have used one of these agents, specifically Cursor or Sourcegraph. They're all in the coding space. You have a bunch of them in the document processing space. You're seeing a lot of enterprises show up that are doing legal processing, healthcare processing, FSNI, and those types of things. Everybody's building one of these types of agents. The agents on the top are some of the ones that have really picked up this year, but we feel and have observed that there are many more that are actually being built out, and we feel that these are the agents that will be more pervasive this year as well.

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2310.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2310)

When you think about agents, there are two main principles that we think about, and both of these principles are based on the fact that your data is the most precious commodity that you have.  The models that are generally available do not align with the data that exists in your enterprises, especially because they have been trained on public data or they have been trained on data that is synthetically generated by a few companies. What we have realized is the first principle: don't treat your model as a commodity. Your model is your product. It needs to be built using a data flywheel. André gave a great example of how they built out their models by iteratively training and constantly improving the model using the data flywheel that they have.

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2390.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2390)

The second principle is that your model is your IP. Your model and your product needs to work together to provide your users with the best user experience that they can have. The overall principles are that your model and your product should coexist and your model is your IP. Do not treat it as a commodity.  When you think about models, when you think about this infrastructure, when you think about where LLMs are, there are so many choices.

There are different types of choices you can make, whether they relate to models or infrastructure. Models now come in different types of modalities. You have text, image, image generation, video generation, embeddings, and more. Different model providers have different expertise. You have Amazon, Meta, Deepseek, and Qwen, which is from Alibaba. They have all been showing signs of catching up with the proprietary or frontier models that you see today. There are also lots of model sizes. I'm specifically talking here about LLMs or large language models. They range anywhere from 1B, even smaller for some of the embedding models, all the way to 1 trillion, which is the latest model from a company called Kimi, which is roughly 1 trillion parameters long.

After you choose the model, the next consideration is model alignment based on the data that you have. There are many choices here. The first one is supervised fine-tuning, where you give your models a few examples to align the model. Then you have DPO, where you give it positive and negative samples. Then you have reinforcement fine-tuning, where you have a reward function that tries to align your model based on the data that you have. The last one is the one that almost all the frontier labs and open model companies actually use to improve the quality of the model because it's cheaper and gives really good results.

Then there's the matter of serving those models and the inference infrastructure. There are lots of trade-offs here. Typically you have three things: your inference can be better, it can be faster, it can be cheaper. Most companies have one or two choices, and we can also talk about whether you can actually make trade-offs on all three choices. Then you have security constraints, whether this model and deployments should be SaaS-based, should they run in your own private VPC, should they run on-premises, do you have legal approvals to use models, and are these models properly penetration tested.

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2590.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2590)

### Building Production Inference Infrastructure: From Hardware Selection to Global Secure Serving

Let me move to the next topic, which is the inference infrastructure. Andrea covered that for inference infrastructure, you have different types of serving stacks for batch jobs and different types of serving stacks for real-time jobs. In the next few slides, we'll talk about when you have your own deployments, how you think about which things to use. The first one is picking the hardware.  There are so many options. You have accelerated compute from AWS and Nvidia providing GPUs. You have what we call the Ampere series, Hopper series, Blackwell series, and as of today, Trainium has Trainium 3 as well. All of these hardware options have different trade-offs. Some have huge amounts of FLOPs, some have lots of memory per GPU, some have really good inter-GPU connects, and so on.

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2630.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2630)

Once you pick the hardware and the hardware provider, you have to start thinking about optimizing your inference engine.  You typically have a few choices that I've listed out. These are the most common ones, but there are many more. You typically think about model sharding. If your model is very large, it might not fit on a single GPU, so you have to shard it across different GPUs. You have disaggregated serving, where you separate out the two phases of generation: the prefill and the generation step. These techniques are very common, especially in generative AI systems where your inputs are extremely long but your outputs are structured and very short JSON outputs.

Then you think about which inference kernel to use. There are many of them. TensorRT-LLM has done a great job, or Nvidia has done a great job of providing a bunch of inference kernels. vLLM and SGLang have all created flash infer and flash attention, which are doing really well. Then you think about cross-shard serving. What if your model is so large that it doesn't fit on a single shard?

When your model is so large that it doesn't fit on a single GPU machine, you need to shard the model across different types of hosts. When you do that, you need really good communication to happen. Amazon provides EFA, where the speed of transfer of KV cache from one machine to another is really quick, and that essentially helps with faster generation and faster throughput. Speculative decoding is one of the most commonly used techniques that allows you to train a very small model that speculates, and the larger model instead of generating just realizes or checks whether the token that's generated is actually correct or not.

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2760.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2760)

Once you've finalized the inferencing engine options, you then have to move to the serving stack.  Andrea already covered a lot of this in her Kubernetes talk around horizontal scaling, but I'll talk a little bit more deeply about specifics to inference. You have prompt caching, which is one of the most common techniques now that's used if you have the same system prompts that get used, what kind of system that is provided by the inference engine, and so you can use prompt caching.

Second is session affinity, which is very useful if you have coding types of agents that you're building or that you're using so that the same request goes to the same machine. In these types of cases, the reliability of machines is extremely important, and so using something like Amazon EKS that's actually managed by Amazon, which is extremely reliable and extremely available, is super important because then the failures around session affinity or your prompt cache dropping reduces significantly. Then you have request failover and load shedding. These are typically the cases when you get spikes of traffic, or you want to fail over to another region.

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2850.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2850)

Once you've picked out your serving stack, you then have to move to global secure serving.  Andrea talked a little bit about trying to get your GPUs in one region as much as possible, but sometimes it is not possible or sometimes you might need to have cross-region traffic or you might need to have regionality. You use global secure serving in that case. You need to have some sort of global scheduling. You need to have private VPCs for secure serving. You might even want to do things like PrivateLink so that your data never leaves your VPC boundary, and all of these are provided by Amazon's ELB systems or through Amazon's PrivateLink, and that's where we are seeing a lot of prevalence of secure and compliant infrastructure.

[![Thumbnail 2900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2900.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2900)

That was a very high level overview of what the inference stacks are.  This is how we actually built our inference stack, taking all of these into account. Today, Fireworks serves about 150,000 requests per second. We process more than 13 trillion tokens a day. We have a bunch of models across different modalities and we host on private secure cloud. We have a booth, our booth number is 1588, so please come and talk to us if you're interested in learning about any of this.

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a5051ebf0a0a4feb/2940.jpg)](https://www.youtube.com/watch?v=_RAFKrdTHQQ&t=2940)

### Closing Remarks and Q&A Session

Thank you. Please prepare a lot of questions. We really hope you guys ask the best questions of all. Please fill out a survey from your app. This really helps us raise the bar at future events, and the more number of people who sign up for the survey, the better data we get in terms of figuring out what worked, what didn't work, and what needs to improve.  So any questions, any burning questions on how to scale AI?


----

; This article is entirely auto-generated using Amazon Bedrock.
