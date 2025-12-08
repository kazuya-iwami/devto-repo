---
title: 'AWS re:Invent 2025 - Networking and observability strategies for Kubernetes (CNS417)'
published: true
description: 'In this video, AWS container specialists demonstrate how to achieve network observability and security in Kubernetes environments using EKS Auto Mode and the newly launched container network observability feature. They explain the challenges of managing microservices architectures with limited visibility into east-west traffic and pod-to-pod communication. The session showcases EKS Auto Mode''s one-click cluster creation with production-ready components like Karpenter, CoreDNS, and native network policy support using eBPF. A key highlight is the new container network observability feature that provides granular metrics, service maps, and flow tables in the EKS console, enabling platform teams to track traffic patterns, detect anomalies, and create accurate network policies. The presenters demonstrate shifting left on security by using observability data to implement default-deny policies without breaking applications, reducing security fix costs by 640x compared to production environments. They also announce EKS managed capabilities for Argo CD, Karpenter, and ACK to further simplify operations.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/0.jpg'
series: ''
canonical_url: null
id: 3087056
date: '2025-12-05T17:40:52Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Networking and observability strategies for Kubernetes (CNS417)**

> In this video, AWS container specialists demonstrate how to achieve network observability and security in Kubernetes environments using EKS Auto Mode and the newly launched container network observability feature. They explain the challenges of managing microservices architectures with limited visibility into east-west traffic and pod-to-pod communication. The session showcases EKS Auto Mode's one-click cluster creation with production-ready components like Karpenter, CoreDNS, and native network policy support using eBPF. A key highlight is the new container network observability feature that provides granular metrics, service maps, and flow tables in the EKS console, enabling platform teams to track traffic patterns, detect anomalies, and create accurate network policies. The presenters demonstrate shifting left on security by using observability data to implement default-deny policies without breaking applications, reducing security fix costs by 640x compared to production environments. They also announce EKS managed capabilities for Argo CD, Karpenter, and ACK to further simplify operations.

{% youtube https://www.youtube.com/watch?v=9HTSa_JsoAQ %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/0.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=0)

### Welcome to CNS 417: Network Observability and Strategies for Kubernetes

 Good morning everyone. Can everybody hear me well? Give me a thumbs up. So remember this is a silent room. Don't forget to put your headphones on. Welcome to Las Vegas. Welcome to Reinvent 2025. I can't believe it's already December, and today this is the session CNS 417 for network observability and strategies. In this session we will be guiding you through a journey to achieve observability and improve your overall security posture on Kubernetes environments by simplifying overall operations and application lifecycle. We want to simplify our daily tasks so we can achieve more velocity and faster time to market.

One thing to keep in mind is that we want to make sure that all the features that are coming out have that goal to simplify your overall operations. Today we are announcing a very nice feature that I wish was there when I was a platform engineer managing a lot of Kubernetes environments. How many of you are using Kubernetes in production today? Give me a show of hands. How many of you are aware of EKS Auto Mode? We'll be covering some features that cover overall EKS and overall Kubernetes, but also how EKS Auto Mode can help you simplify those daily operations.

My name is Joricoberza. I'm a container specialist solutions architect at AWS, and my goal here is to help customers overcome challenges and build solutions to simplify their daily activities. I have the pleasure to have here with me Master Luke. He's not Luke Skywalker, but he's also a master Jedi in Kubernetes. Thanks, Rodrigo. Hey everyone, I'm Laconde Mula. You can also call me Luke, and I'm a product manager on the Amazon EKS team.

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/130.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=130)

### The Challenge of Network Visibility in Distributed EKS Environments

 I want to start by asking a quick question. By show of hands, does this architecture diagram resemble your environment to some degree, at least at the fundamental layer? I see a couple of hands going up. More going up, that's great. So this is something that we put in front of a lot of customers, and many of them were able to resonate with this, emphasizing on the fundamental layer. What we have represented here is an application environment that has Amazon EKS as the foundation for its platform, and this is what a lot of customers are doing now. It's not the entire picture, and that's something that we're going to be taking you on a journey about.

You have a lot of Kubernetes workloads, or to be more specific, microservices that are running on EKS, and a lot of these are communicating with each other in order to drive business functionality. These microservices are speaking to each other within the scope of the cluster, so there's a lot of east to west traffic. In addition to that, a lot of these microservices also rely on AWS infrastructure. For example, you could be running some kind of storage application that relies on S3 to actually store its objects there, and then in order to have some metadata associated with those objects, you could be storing it in DynamoDB.

Or perhaps you're using a traditional three-tier application and you're using ElastiCache as the caching layer, RDS or DocumentDB for persistent storage. We also have a lot of customers that are training their ML models and using S3 for checkpointing. But again, it doesn't stop there. A lot of customers are also integrating these environments with additional services and systems that are running on-premises or on the internet. Does this pattern sound familiar? Any show of hands on that?

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/240.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=240)

 So what we have here in the bigger picture is this widely distributed environment with a lot of threads that essentially tie back to EKS as the hub or one of the primary hubs. The core fabric that ties all of this together is the network. The more that companies or organizations continue to innovate and introduce additional enhancements, that will typically translate to new microservices that land either in your EKS cluster or perhaps on-premises or some other service on the internet, but still ties back to this foundational layer.

In such environments it is becoming increasingly critical to understand the landscape of the network, to understand its security, to understand its performance and its behavior, primarily so you can measure how well that aligns with your internal standards. Because the goal at the end of the day is to optimize it so that it's serving the ultimate purpose of driving the kind of business value that you want. This should ultimately be serving that bigger purpose. But that becomes especially difficult if you have limited visibility.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/320.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=320)

You cannot accurately optimize what you cannot see. There are some additional challenges to be aware of, especially in the context of an EKS environment. When you are running a Kubernetes cluster  in AWS, you have to be mindful of the fact that you have a broader network plane, which is the VPC. In addition to that, Kubernetes also has its own network plane. So you essentially have two landscapes or two network planes, which introduces additional operational complexity. If you are trying to achieve the objective of optimizing for security, performance, and ensuring that application traffic behavior aligns with your internal standards or designs, you need to be able to consolidate these two planes and easily understand the data points between them.

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/400.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=400)

Network observability is multidimensional. You are concerned about network observability with respect to security, performance, and cost optimization. Ensuring that behavior aligns with applications that you have designed, verifying that the right applications are talking to each other as expected—these are key things. The main point to draw out in terms of the problem definition is that when you have a lack of visibility or limited visibility, you end up with imprecise data. The challenge with having imprecise data is that you have inaccurate conclusions and slow decision making, which finally leads to suboptimal actions.  You get stuck in a loop of trying to figure out exactly what a root cause is, and fundamentally it is because of a lack of visibility. That also eventually ties to a negative impact on the business.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/420.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=420)

### Business Impact of Insufficient Network Observability

There are many scenarios in which the business is ultimately disrupted because of some kind of network issue. It could be congestion at the pod layer, specifically an ENI issue, but without sufficient visibility and data, it is hard to actually detect that quickly.  What you are interested in is reducing the amount of time it takes to quickly detect an issue so that you can fast track resolving it. Or perhaps it is a bandwidth issue related to a specific worker node, but again, that requires having sufficient network visibility at both a pod level and a worker node level and having enough context. When you detect an issue, you need to quickly tie it to the relevant metadata. What cluster is this coming from? What namespace is this coming from? What is the workload associated with this, and exactly what pod is this tied to? What are the ENIs associated with this pod?

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/480.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=480)

There is also the challenge of increasing costs when you have insufficient visibility.  A lot of customers try to adopt different strategies and techniques to manage the flow of traffic. One of the key things when it comes to cost optimization is trying to ensure that when traffic is flowing from a particular workload to a certain destination, you try to ensure that traffic ends up in the same Availability Zone in which it originated from. There are different ways of doing this. You can use native Kubernetes functionalities like topology-aware routing. We also know that a lot of customers use Istio for this, for locality-based routing. Or you can even use Envoy for this. But how can you quickly verify that those optimizations that you have put in place actually are working as expected? How can you review the topology setup in your Kubernetes environment and verify that the flows are working as expected based on certain techniques that you would have put in place?

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/540.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=540)

Finally, there is the impact of security as well, which is  fundamental. The point is to ultimately deliver business value faster, and it is hard to confidently do that if security is unsure about how well the network has been optimized to meet certain internal standards. Are traffic patterns aligning with the designs that were set up for specific applications? Is workload A speaking to workload B as expected? Based on the designs, ensuring that workload C should not be receiving any traffic from workload D, how quickly can you verify that so that you have sufficient confidence to move from dev to stage to prod? Another reason why this is so important is because this is an increasing trend. We have a lot of customers that are using microservices.

Of course, it introduces its own challenges, but we don't want to take a step away from it because of those challenges. So we have to be mindful of these hurdles, but at the same time, the key thing on our part is how do we simplify operations on your part so that as you adopt a usage pattern that certainly has a number of advantages—in this case, microservice architectures—how do we ensure that the ways in which we are simplifying the operations in your life helps you to continue the trend or lean into specific usage patterns that you're trying to adopt in your EKS environments. That is going to be a key thing that we'll speak about here, and Rodrigo already alluded to earlier.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/670.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=670)

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/680.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=680)

### The Evolution of Multi-Tenancy and the Security-Efficiency Dilemma

So where do we go from here? How do we translate those kinds of business challenges to technical solutions? Because if you think about it, they're not really business challenges. They are technical challenges that are leading to business challenges. So let's bring back that architecture  that Luke just showed us, but let's dive deep on the core center of our Amazon EKS cluster, our Kubernetes cluster.  The communication through storage or to a database—it was always there. It doesn't matter what kind of application you have, whether it's running on EC2 or if it's containerized. But the growing trend of containerized applications and multi-tenant applications is what's causing this complexity to increase over and over.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/720.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=720)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/740.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=740)

How did we get to this very complex microservices scenario? Let's start from the beginning. The multi-tenancy evolution started with a monolith.  You had one team or two teams managing one specific application. There's not much to do. It's just one piece, a black box. They don't have too much interaction or connection like microservices. With the growing trend of Kubernetes, we started containerizing those monoliths, not really in the right way.  We just made those big applications into containers and put them in a cluster, like a single tenant cluster. This is what we call hard multi-tenancy, where you have one application running in a specific cluster. This is not wrong. It may be required if you have a very PCI compliant environment or if you have specific security restrictions.

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/770.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=770)

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/790.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=790)

But then we started learning that Kubernetes is very effective at managing multiple workloads.  So we transitioned basically to two patterns. We have one cluster with multiple applications segregated by these logical pieces called namespaces, as we call it soft multi-tenancy.  The other pattern is when you have several applications running in the same namespace sharing the same namespace. But how can we ensure that we have security among all of them? We usually start seeing that this is an overly permissive environment because we're not controlling this. Are we sure that application A can communicate to application B? We're not.

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/830.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=830)

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/840.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=840)

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/870.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=870)

These are concerns about the development or application teams, and security is not really their main focus or keeping this access control.  There's one team missing in this picture: the platform team or the security team. The security team and the platform team are responsible for all of those environments to help control access, to help make sure it's secure, to avoid outages.  Whereas our application teams want to develop fast, they want to experiment, they want faster time to market, they want to speed up the application lifecycle. If the security team doesn't have clear visibility, it's really hard for the platform team to have a secure environment.  And then it creates an overly permissive environment because if the security teams put too much restriction, they block the application teams, and it's not good for anyone.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/890.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=890)

So how do we strike this balance to have the control and security that's required for highly regulated environments and have team autonomy so the development teams can be efficient?  Usually, we fall back into an overly permissive environment because there's no visibility across networking communications and we don't want to block the development. Does this resonate with anyone's environment here? Does this match anyone's application lifecycle?

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/910.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=910)

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/920.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=920)

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/930.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=930)

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/940.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=940)

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/950.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=950)

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/960.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=960)

### Simplifying Infrastructure with EKS Auto Mode and Network Policies

This is where we really want to go . We want to have assertive controls so we are secure and efficient at the same time.  How can AWS help? Our goal is to simplify overall operations and the overall application lifecycle. We started tackling  a layered approach to cover all of that. Starting from the bottom, we want to simplify overall infrastructure management so platform engineers can really focus on  what's important. We want to give you full clarity and full visibility of all the network paths and everything that's happening in your environment  so you can achieve an improved overall security posture, which is one of the most critical pieces of everyone's environment. 

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/970.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=970)

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/980.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=980)

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/990.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=990)

Starting from the bottom again, last year at re:Invent we introduced Amazon EKS Auto Mode. Remember I talked about Auto Mode?  I know some of you are already using it. What Auto Mode gives you is basically one-click cluster creation with all of the production-ready best  practices capabilities out of the box, so you don't need to worry about deploying any of those. It provisions all of the cluster infrastructure and automatically does  compute scalability through a key component called Karpenter. Everybody here knows what Karpenter is. So just a quick recap for those who don't know, Karpenter is a node autoscaler that scales out compute resources based on resource requests. So if you have pending pods, Karpenter will read those and in a few seconds spin up any compute that's required for those workloads to run based on cost efficiency and performance efficiency.

[![Thumbnail 1040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1040.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1040)

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1050.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1050)

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1060.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1060)

It uses basically two components that are completely customizable and it's managed entirely inside Kubernetes. You don't need to worry about managed node groups or autoscaling groups. Together with that, Karpenter also continuously optimizes  for cost, so it doesn't just scale resources, it optimizes your overall environment. And then the part that's most important for this session,  Auto Mode comes with core components for networking, and we'll be talking about those three components in a bit. But also it keeps all of those components  updated with patches and security fixes on the AWS side.

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1080.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1080)

This is where we started creating a simplified infrastructure baseline for you. Let's talk about those three components. Starting from the edges, CoreDNS and kube-proxy are very well-known  components from the Kubernetes world. CoreDNS basically handles name resolution inside your cluster, so you don't need to memorize IP addresses. Kube-proxy is the component that understands the egress and ingress communication and knows how to direct the right communication to the right pods because you have a couple of ENIs probably in your nodes but you have a hundred pods, so kube-proxy will do this transition and direct the traffic to the right pod.

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1170.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1170)

In the middle we have the CNI. CNI is one of the most important parts for our session today. There are a lot of CNIs out there in the open source world, but last year, in fact in 2023, we released native support for network policies on CNI. In the past you needed to use a third-party CNI to have access to all the network capabilities in your Kubernetes cluster. How many of you are actually using network policies? Just a few. That's really common because doing network policies is not easy. Why? Because you don't have that kind of visibility into east-to-west communication, pod-to-pod, namespace, who communicates to who. All of these components run inside the node, and you don't need to manage all of them. 

[![Thumbnail 1190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1190.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1190)

Let's talk a little bit more about network policies which will improve the security posture of the cluster. In the past before Auto Mode, there was custom setup required for you to enable those network policies. You needed to worry about roles and how to enable that, and this could cause a cluster-level blast radius. In EKS Auto Mode  it's much more streamlined. Network policies are enabled by default. You just need to tell them to use it. Basically you need to create a ConfigMap and say, "I want to use network policies," and boom, it will be there.

One of the components for Karpenter that helps you manage the nodes is the NodeClass. The NodeClass is pretty simple baseline infrastructure, and you can define the default behavior of your network policies. By default it comes with allow policies, but you can change that to default deny if you want to. If you're running in highly regulated environments, you can also separate specific node classes. If I want to have one application that's fully isolated from the rest of the applications running in my cluster, I don't need to create another cluster. I can do that at the compute level in the same cluster using EKS Auto Mode and network policies.

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1260.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1260)

[![Thumbnail 1270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1270.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1270)

[![Thumbnail 1280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1280.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1280)

Let's create a quick EKS Auto Mode cluster here for the sake of the presentation. We're going on a very quick journey.  Let me create one cluster. I'll just put in the name. I see we don't have the role specific for that cluster. You can just click a button and it will come  with all the best practices, granular least privilege for the cluster. With just a few clicks you have the recommended role to create a cluster in your environment. 

[![Thumbnail 1290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1290.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1290)

[![Thumbnail 1310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1310.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1310)

[![Thumbnail 1320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1320.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1320)

Secondly, we have the node role. The node role and cluster role have different personas, so they need to have different levels of access.  We can create the node role the same way we did for the cluster. With a few clicks, we create the role and have the node role running. Now let's go to the network part. Because of the way that Auto Mode runs, you can create a very specific network for your cluster.  All the best practices will be there. I'll just click create VPC. The only things that I'll be changing are that I want to run in three availability zones, not just two,  and I want to use this new regional NAT gateway feature. With that, in just a few minutes I can create a cluster, roles, and VPC all together for my environment.

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1340.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1340)

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1360.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1360)

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1370.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1370)

As you can see, all of that will be created in a few minutes. The only thing that takes a little bit is the NAT gateway to be there and propagate it,  but then we have IAM roles, networks with subnets, everything ready. Auto Mode also knows that we want to run the pods just on the private networks. We don't want to have pods on public-facing networks with public IP addresses, so we are protecting our pods by design.  If you want to dive deep, there are some specific configurations that you can access.  All the pods and services networks are already there and there's some stuff that you can opt out of after you create your cluster. It's pretty easy and straightforward.

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1380.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1380)

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1400.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1400)

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1420.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1420)

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1430.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1430)

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1440.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1440)

Now we have a cluster running.  In a few minutes the cluster will be activating. One thing that I want to mention is that the only add-on that will be created is the metrics server. All the other add-ons like core network capabilities, Karpenter, and Pod Identity are embedded in the cluster to simplify your daily operations. The only thing that you see there is the metrics server  that's not embedded, but the metrics server is required to have the scalability and cost optimization that we talked about. We see the cluster is already active. Now let's enable the network policy on this cluster. The cluster comes with the network policy already there as a capability,  but we need to tell it on that specific ConfigMap that we want to use network policies.  So we just have those two pods. Everything else is away because the goal of EKS Auto Mode is for you to just deploy your applications.  As you saw, there's nothing there other than the metrics server and the nodes where the metrics server is running.

[![Thumbnail 1460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1460.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1460)

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1470.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1470)

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1480.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1480)

[![Thumbnail 1490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1490.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1490)

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1500.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1500)

Let's see the NodeClass where I show that configuration for the default behavior of the network policy. Let me take a look at that configuration file.  I'll just filter the output so it's easier to see. We don't have a big one. So we have network policy as default allow and event logs are disabled by default. Event logs will log everything that happens, everything that matches a network policy will be logged on the Kubernetes events log.  I have this manifest already done and I'll just create it, just double checking it's not there yet. I'll just be enabling that specific ConfigMap  so we can have network policy support fully enabled in the cluster.  We cover this bottom part for simplifying overall infrastructure with those core components and being able to enable network policies. 

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1510.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1510)

[![Thumbnail 1520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1520.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1520)

### Network Policy Implementation Using eBPF: From Overly Permissive to Controlled

Let's talk a little bit more about network policies. As I mentioned, this was introduced in 2023 as  a native feature for EKS CNI, and it works differently from the standard network policy that you can get from the community.  It's a single plugin that is fully integrated. You don't need to deploy anything else, and it uses eBPF to perform as it does today.

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1560.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1560)

Let me provide a quick recap on what eBPF is. eBPF stands for extended Berkeley Packet Filter, but it's essentially a kernel engine or kernel module that is enabled to filter all the system calls happening from the user space. So when an application or anything performs a system call, eBPF will inspect that packet and then pass it back to the Linux kernel after inspection. This is a common use case for load balancing, steering,  security, and tracing. However, our goal here for network policies is security, though we'll also be using it for tracing as well, but I don't want to spoil the surprise that will be presented later.

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1580.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1580)

How does it work under the hood? A user can create a network policy. The network policy controller will read it  and trigger an endpoint CRD that is exclusive for EKS. Then the EKS, using eBPF, will inspect that packet and decide whether to apply a network policy restriction or isolation. When we make these kinds of changes and apply network policies, changing the default behavior for a node class, a specific rollout happens. We covered the EC2 node class in the previous slides, and the other component from Karpenter is the node pool. The node pool defines the behavior, whereas the node class defines the baseline infrastructure for the nodes.

[![Thumbnail 1630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1630.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1630)

What I'm highlighting on the slide  is just the way that the rollout will happen. For example, if I want to disrupt just 10 percent of my nodes at a time, and let's say I have a huge environment, it will not disrupt and roll out and apply network policies or any other behavior for more than 10 percent at a time. This is just an example; you can do 1 percent, 50 percent, or any other percentage. The nodes expire from time to time to renew the nodes and apply new patches and new configurations.

[![Thumbnail 1670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1670.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1670)

Let's see this in action. This is the cluster that we just created.  Now what I'll do is deploy an application here. This application consists of a few namespaces and a few deployments. I'll use customization for the sake of time. Of course, we should be using a GitOps approach for this, but that's a topic for another time. I've created all of the necessary components, and we have our application running. Because there are no policies in place, everything communicates with everything. Remember, this is an overly permissive environment. So we can see all the pods.

[![Thumbnail 1720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1720.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1720)

Now let's get the URL for us to access this application and see if it's really running and behaving as expected.  Let me paste this in our browser. We have a specific secret shop where we can buy anything. Let's explore. So in this application, we have a UI with a catalog for all the products, carts, and orders. All of those pieces need to communicate together in order for this shop to really work.

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1770.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1770)

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1780.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1780)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1790.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1790)

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1800.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1800)

Now what we're going to do is apply a default deny policy for this environment and see what happens. Remember, today there are no policies in place. Let's change this to a default deny policy and wait for Karpenter to do the rollout of all the nodes. For the sake of time, I've cut the part of the rollout because it takes a few minutes, but you can see it won't take more than 3 to 5 minutes. Let's change the network policy  to default deny and enable the event logs so we can see what's actually happening.  Done. Let's watch for the node behavior. We didn't trigger the nodes yet; they have around 20 minutes.  Now let's watch. In a few seconds, everything will be triggered. You can see new nodes running with the new configuration, so this is what the rollout is about.  See, we have a new node. This is the time that Karpenter takes to spin up a new node. It's really, really fast.

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1810.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1810)

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1830.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1830)

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1840.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1840)

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1850.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1850)

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1880.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1880)

Now let's go to our application and check.  It doesn't seem to be running. So when you receive a call from your CEO or from a financial person saying our application is offline, what you would do is just roll it back. We have the new nodes there, but the  applications are not really communicating, so what we are going to do is investigate. You can see the applications weren't even able to spin up because there's no communication.  The liveness probe, readiness probe, everything just failed. And how do we investigate this if we don't have proper visibility on the network? Even if we have access to the logs,  it's not traceable. We don't have clear visibility on who's communicating to whom. To approve this configuration, we do what other teams asked and go back to an overly permissive environment, and this will sit there forever. The application will go back to running, but then how can we fix those things to get back to a controlled environment with our application running efficiently? 

[![Thumbnail 1890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1890.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1890)

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1920.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1920)

[![Thumbnail 1940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1940.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1940)

So how do we control access? Let's say we go back to the default deny and then we need to apply a network policy  to open the communication to the orders namespace from the catalog and UI namespace. You can see this is a quick example of a network policy. In the spec, we have the pod selector, so all the orders pods in the orders namespace are allowed to receive ingress communication from the UI, as you see at the bottom. But then you need to open communication for the checkout as well.  And then you need to go to the UI and say the UI can go to these two namespaces, and then you need another policy and another policy. What about DNS resolution? Do I need to open communication to CoreDNS so I can have name resolution?  Do the pods need to access the internet? Who talks to whom? All of these questions fall to the platform team because they don't have visibility. Even if the application teams know where their application is going, they usually don't know who's accessing them.

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/1970.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=1970)

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2000.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2000)

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2010.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2010)

### Introducing Container Network Observability in EKS

So how do you overcome that if we don't have proper visibility? We cover the top layer to improve overall security,  but if we go to an overly restrictive environment, things break. Something's still missing. Can you help me out with this one? Absolutely. So as Rodrigo pointed out, it's very hard to accurately optimize without sufficient visibility. You can have the capabilities, but normally what ends up happening is a lot of customers end up bouncing between two extremes: overly permissive and overly restrictive, and that's what impacts their environments. So there's a critical layer that is needed, and that's  the missing piece from the diagram that Rodrigo just showed, and that is essentially observability. We're really excited for this new feature that we recently launched: container network observability  in EKS. This is a key and fundamental aspect to achieving the right kind of optimization you want for security, as Rodrigo's been emphasizing, but also for performance and also for cost, and ensuring that traffic behavior in the cluster aligns with design.

This allows you to have your EKS network environment much more observable, which is very important when it comes to successfully scaling in widely distributed environments. You need to be able to track east-to-west traffic within the scope of the cluster, but also you want to be able to track traffic between pods and certain AWS services. In addition to that, as I saw some hands raised, a lot of you are also running applications in the cluster that are speaking to services outside the scope of AWS as a whole. There are some key ways that this new feature can essentially enable you or empower you in this particular regard. I want to focus on the use cases before we get into the specific feature sets with container network observability. The first one that I want to touch on is measuring network performance. This is especially important because this is something that a lot of customers run into, to be more specific, platform teams being able to quickly pinpoint where an issue originated from. A lot of teams have already standardized on their observability stack, and a lot of customers will essentially be sending data from their Kubernetes cluster or their EKS cluster to some kind of observability stack that may be managed solutions within AWS.

You could be using CloudWatch, Amazon managed Prometheus and Amazon managed Grafana, or you could be using open source solutions and managing them yourself with Prometheus and Grafana. It was especially important for us to ensure that if we're going to enable our customers in the right way, then we need to align with your usage patterns.

We're providing more key metrics now that are much more granular at the pod level and the worker node level that are related to networking, allowing you to quickly pinpoint issues like network congestion at a pod level, seeing the ENIs that are associated with that, and being able to quickly determine if there was a bandwidth exhaustion issue in a specific worker node, whether it was related to ingress or egress. Being able to track the flow count for pods and see in a very quick way what the particular issue is reduces the mean time to detection as well as mean time to resolution, which is extremely important when incidents arise.

Measuring your network performance to detect anomalies is normally the starting point. You'll have some kind of observability stack where an alert or alarm will notify you. But after that, there's a tail end of the investigation where you want a way to scope down the particular area where the issue originated from. You want to double click on it essentially and then carry out your investigations. Sometimes this can be especially painful if you're solely relying on TCP dump in the cluster. That can be a lengthy, painful, and costly process.

We thought it was especially important to provide additional capabilities for the tail end of the investigation as well, something that complements proactive anomaly detection with whatever observability stack you're already using. There are now native visualizations in the cluster that you can make use of, being able to track east to west traffic within the scope of the cluster, traffic between pods and AWS services. At launch we support traffic between pods and S3 as well as DynamoDB, but also being able to see pods that are speaking to cluster external destinations and more specific destinations outside the scope of AWS as a whole.

You can also leverage these native console capabilities to track the top talkers. That's something that a lot of customers are particularly interested in. What are the workloads responsible for the most traffic that is being sent within the scope of the cluster or outside the cluster? Being able to see traffic that is traversing different availability zones helps you verify accurately that the cost optimization techniques that you're adopting are actually working as expected.

[![Thumbnail 2290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2290.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2290)

A lot of what I've been speaking about is essentially captured in this diagram over here.  We took a closer look at the usage pattern among our customers, and we didn't want platform teams to have to deviate to fit into a new norm, but rather how can we make the EKS network environment more observable, aligning with what customers are already practicing. Teams will essentially be relying on data that they can leverage to measure performance, which is a key one for a lot of customers. How do we accurately measure the network performance in our environment?

You've already got some observability stack, and the new metrics that we're providing are available in open metrics format. EKS is using Amazon CloudWatch Network Monitoring, a feature that was launched last year. We partnered together to further align with EKS use cases, and CloudWatch Network Monitoring has an eBPF agent that runs on the worker nodes and captures the network flows. More specifically, it captures the top 500 network flows on every worker node as well as the flow metrics associated with each of those flows.

We have two sets of metrics: system metrics as well as flow metrics. The system metrics can be scraped directly from the worker node and sent to your preferred monitoring solution. If you are using Prometheus, for example, then you can directly scrape these from the network flow monitoring agent. Alternatively, you may be using Amazon managed Prometheus and you could use a scrapeless mechanism as well to ingest those.

On the tail end of it, as I pointed out, you'll see that there's an arrow from the platform team that's also going to the EKS console, and that is where you'll essentially find the additional capabilities that we are now providing to help you in accelerating and having more precise troubleshooting.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2410.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2410)

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2420.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2420)

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2430.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2430)

### Container Network Observability in Action: From Detection to Resolution

Now I think what you're looking forward to seeing is what this actually looks like  in action. If you're using the console to enable this feature, you would essentially just click this checkbox to enable  network monitoring, and it will automatically create the underlying dependencies that are in CloudWatch network flow monitoring. We've documented these clearly for you to see exactly what will be provided , and we'll also guide you through the process of installing the relevant NFM add-on. In this case, it's already enabled, so you can also see the capabilities. There's a service map and a flow table, and those are provided in the console.

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2460.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2460)

The performance metrics endpoint is something that you can leverage and directly scrape from. We can see the status of the monitor, and from what we gather, everything looks like it's already set up and enabled. We're going to follow the normal workflow that a  lot of customers already use. The starting point is typically that you've got your own observability stack. This is a Grafana dashboard, which is something that a lot of customers rely on already. They're already standardized on using Grafana, and we fit right into that.

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2480.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2480)

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2490.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2490)

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2500.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2500)

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2510.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2510)

This dashboard visualizes all the key metrics that we're providing now. You can see the top 10 pods  by ingress bandwidth, and this allows you to actually track these things. We've had a lot of customers speak to us to say, how can I easily understand  the amount of bandwidth being leveraged by CoreDNS, for example, because that's a critical component that lots of pods are essentially communicating with in order to resolve their communication with other pods.  We can do the same for egress bandwidth as well. We can look at ingress bandwidth allowance exceeded and detect any anomalies associated with the worker  nodes, and we've got the metadata that you can capture here as well. You can see that in your Grafana dashboard now, everything consolidated in one place, fitting right into your existing usage pattern.

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2530.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2530)

[![Thumbnail 2540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2540.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2540)

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2550.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2550)

We can also see packets per second allowance succeeded, connection tracking allowance succeeded, and you can see all the worker nodes that this is  associated with. The key thing here is to further enable you to easily detect an issue and trace it back to the specific point of origin  without having a long stretch in getting to what the issue actually is, detecting it, and resolving it.  You also have the ability now to track the egress bandwidth for your pods as well as the ingress bandwidth, ensuring that this aligns with what you expect, or if you need to be modifying your environment to align with usage by your customers. We can see as well the TCP flow count for the different pods.

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2570.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2570)

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2580.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2580)

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2590.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2590)

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2600.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2600)

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2610.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2610)

We can see the packet rate from ingress rate as well as egress for the pods.  Drilling in more and more and giving you more granularity  so that you have sufficient information. As Rodrigo pointed out before, we get to the point of optimizing things. We need to have this detailed  layer of visibility that can give us informed decision making and that empowers our customers or enables them to respond quickly to issues when they arise.  Now the next thing that follows after this is you may get some kind of page or alert that would happen, and you would have sufficient information  to then zone in on a specific cluster.

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2620.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2620)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2640.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2640)

In this case, we have a very basic e-commerce application that uses the GraphQL API,  and this is just showing again that this is already enabled. I'm going to switch back, and we're looking at the service map now. This is specific to the e-commerce namespace. We've got three microservices here: a GraphQL API that speaks to an orders application and a products application. You can change the time if you need to scope it  to a one-hour session or if you want to reduce that to five minutes to see what's happening.

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2650.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2650)

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2670.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2670)

[![Thumbnail 2690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2690.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2690)

We can click on any one of these flows, and then what we see here is the forward direction as well as the reverse direction in the case that there is bidirectional traffic.  We can zone in even further because this is the kind of granularity that becomes extremely useful, being able to see the flow between pods that are actually talking to each other at a specific replica level. Because this can be a bit noisy, for example, for the last hour or the last thirty minutes, you can also change this if you want it to be for the last five minutes.  Again, you can zone in further and see the amount of data that was actually transferred on these specific flows, and this is where the flow metrics come in. You can also see retransmissions and retransmission timeouts if you're trying to detect latency. The higher your retransmission timeouts, the more you see a significant impact of latency between specific pods, and you'll be able to use these kinds of capabilities  for that.

Let's drill in even further. We can go to view details, and what we have here is actually a flow table that allows us to see at a more granular level the flows for the different pods that are talking to each other in this specific namespace. You can filter for more granularity and precision, say we want to look at flows where the local pod is a GraphQL pod.

[![Thumbnail 2710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2710.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2710)

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2720.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2720)

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2730.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2730)

[![Thumbnail 2740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2740.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2740)

We can scroll to the side  and we can still see each of those flow-level metrics: retransmissions and retransmission timeouts for the remote pod. In this case, I'm going to choose orders.  There we see the amount of data transferred. You can see right at the top there we've also got events, so this links to the resources  for you to be able to see any pod events in the event that there were particular issues that you saw as well. This consolidates things even further for you to have this  additional information when you're carrying out your troubleshooting or debugging of issues.

[![Thumbnail 2750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2750.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2750)

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2770.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2770)

Next up, we're going to look at a photo gallery application.  Over here we currently see that there is nothing. It might take a little bit of time. We switch it to 15 minutes and there's nothing going on here. So we're going to switch to the flow table view. What this photo gallery application is actually doing is essentially just uploading images to S3 and then storing metadata in DynamoDB.  If we go to the flow table, you'll see that there are three perspectives over here. There's the AWS service view, which is pod to an AWS service. There's the cluster view, which is east to west traffic within the cluster, and then there's the external view, which is pod to a destination that is outside of AWS.

[![Thumbnail 2800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2800.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2800)

[![Thumbnail 2810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2810.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2810)

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2820.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2820)

[![Thumbnail 2830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2830.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2830)

In this case, we can see here for our photo gallery application that the pods are speaking to S3 as well as DynamoDB. We can also see the volume of data transferred. We can see the local pod IP and the remote pod IP. If you want to modify the columns  that you're seeing, you can also do that and customize it for what's specific and more purposeful for the way you carry out your investigations within your team.  Data transfer is one that is typically really important for customers to be able to see. I'm going to switch to the cluster view so you can see this.  For the cluster view, I'll switch back to the e-commerce namespace. At the moment here we'll be seeing things for the photo gallery application, but we switch back to the e-commerce and you'll  see that now we're seeing about 300 flows that are represented over here. You can change the time range, and of course the greater the time range, the more flows that you're going to have for this particular demo.

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2850.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2850)

[![Thumbnail 2860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2860.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2860)

You'll see it'll probably only go up by two additional flows, so we'll see about 302 flows represented. There you go. If we switch back to 15 minutes, it's about 300.  Similar to what we saw when we were going through the service map, even here in this flow table you can filter further if you're looking for something specific. This fits within that workflow that  I pointed out earlier where you would have started from your observability stack. You would already have some kind of alert that's giving you sufficient information to know what you should be looking for. So when you come to the service map or the flow table, you have enough context to just accelerate the troubleshooting process.

[![Thumbnail 2880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2880.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2880)

[![Thumbnail 2890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2890.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2890)

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2910.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2910)

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2920.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2920)

As I mentioned earlier, this is something that can also  inform cost optimization techniques. Customers are typically interested in seeing what traffic is going from one availability zone to another availability zone.  So that's something you can also leverage here. Something that's also especially important in this case and very relevant for this particular session is the fact that you're able to actually track the traffic behavior within your cluster. You can now see if this is aligning with the network security  policies that I've put in place. This can now be the key thing that informs how you define your network policies and is especially important as you're shifting left and you're doing this essentially in the dev environment  instead of having to find out in a production environment when things are going wrong.

[![Thumbnail 2930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2930.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2930)

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2940.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2940)

[![Thumbnail 2950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2950.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2950)

What we have over here is the historical explorer, which essentially launches you to the CloudWatch network monitoring dashboard where you can see this in the broader spectrum of things. It is especially important to see that you now have that consolidated transparency  between the Kubernetes network plane as well as the VPC network plane, as I pointed out earlier. So you can see over there in the corresponding network path you can be able to correlate a specific flow that is taking place within  your EKS environment or your Kubernetes network context and see how that relates to the underlying VPC constructs as well, being able to see the specific EC2 instance and the ENIs associated with that flow's particular path. Zooming in particularly on the Istio Ingress Gateway, we can see the corresponding network path and each  one of those flow-level metrics are also visible here. You'll be able to see the retransmissions, the retransmission timeouts, as well as the volume of data that was sent.

[![Thumbnail 2970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2970.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2970)

[![Thumbnail 2980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/2980.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=2980)

[![Thumbnail 3000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3000.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3000)

If I switch back, now we can see the photo gallery is now generated in the service map. Similar to before, as I showed with the e-commerce application, we can see the API gateway is talking to both the upload service as well as the gallery service. Similarly, if we want the more detailed view where we're seeing flows between the specific pod replicas, we can have that represented as well  in the service map.  

[![Thumbnail 3020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3020.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3020)

All of this is at the tail end of your investigation but meant to help in accelerating things. A key thing that we also see many of our customers doing is making use of a service mesh implementation.  We wanted to highlight this in this session and ensure our customers know that what we're introducing with container network observability is complementary. In many ways, it allows you to be less reliant on a service mesh implementation for observability. We've heard from many of our customers that they would adopt the service mesh primarily because of observability. Now you no longer need to use it primarily for that. Rather, you can leverage it for other features as well.

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3080.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3080)

[![Thumbnail 3090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3090.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3090)

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3100.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3100)

In addition to that, we found that there would sometimes be an inconsistency because not every workload may be running in the mesh. You may have a certain level of observability at the network layer for workloads that are in the mesh, but any workload outside of the mesh doesn't have the same level of observability. Now that can change because you're using container network observability that will surface sufficient data whether an application is in the mesh or not. You can still use your service mesh for advanced networking, for fine-grained traffic control, and advanced load balancing mechanisms.  You can still use it for end-to-end encryption like MTLS, which is a common use case.  For detailed observability, there are key metrics that you can get from service mesh implementations like Istio.  Envoy emits a lot of great statistics. However, it is worth noting that adopting a service mesh implementation does typically introduce some operational overhead as well.

[![Thumbnail 3130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3130.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3130)

Managing a service mesh at scale can be particularly challenging. By introducing container network observability, we help our customers in that they no longer need to adopt an entire service mesh just for a single piece of it. Rather, they can make better informed decisions and see what capabilities they can actually get out of the mesh, knowing that they also have additional native capabilities provided by EKS that can help them in their journey.  We've seen the different layers that we've been covering here. We started by highlighting how we were making it easier for our customers at an operational layer with EKS Auto Mode and taking on that responsibility of managing those core networking components. Rodrigo also highlighted the network security capabilities that can enhance the security posture of your environment at the network layer.

[![Thumbnail 3180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3180.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3180)

[![Thumbnail 3200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3200.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3200)

[![Thumbnail 3210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3210.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3210)

[![Thumbnail 3220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3220.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3220)

[![Thumbnail 3230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3230.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3230)

[![Thumbnail 3240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3240.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3240)

We've also been trying to drive home the point of how this is especially difficult without sufficient visibility. Now with container network observability, you can make more informed decisions and more accurate optimizations. Using that service map,  we can understand what the pod-to-pod communication is, what the east-west or even north-south communication is, and we can start narrowing down the access control. Let's go back to that cluster that was broken with the applications not running and enable that default deny again.  But this time we will create the network policies based on the rules that we observed using the network observability capabilities.  You might tell me, "Hey, I can use AI to create the network policies for me, right?" I tried that and I had to use the network observability feature to really understand what was going on because the network policies that the model generated didn't work.   The application was still broken. So I was able to create the network policies now. Everything is there. You have the cluster running.  The application is still running, but we still have a default allow policy. Now we're going to change that.

[![Thumbnail 3250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3250.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3250)

[![Thumbnail 3270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3270.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3270)

[![Thumbnail 3280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3280.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3280)

[![Thumbnail 3290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3290.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3290)

[![Thumbnail 3300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3300.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3300)

This is what we have done. We observed the networking map in a development environment and understood your networking map. We created your network policies and then changed to a default deny.  These applications will be ready to go to production already with network policies already embedded. That's shift-left security and it's a shared responsibility between both platform security and application teams.  Now we have the default deny policy set. We can see that we have several network policies running on the cluster, each one with specific things,  and we have the application still running.  I can explore the catalog, put things in my cart, and finish buying. 

### Shifting Left on Security and Key Takeaways

This study from CNCF demonstrates exactly that. If we shift left on security, the cost to fix a security issue in a development environment versus a production environment is 640 times more. That's why we should shift left on security in this case. We covered all of those layers, but then I was talking about application development and shifting left on security and putting everything together with the application, so there's more.

[![Thumbnail 3340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3340.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3340)

[![Thumbnail 3350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3350.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3350)

[![Thumbnail 3360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3360.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3360)

Yesterday we announced the managed capabilities for EKS.  This is a brand new feature that was just announced really yesterday at 4 p.m. These new capabilities are managed capabilities leveraging open source tooling like Argo CD,  Karpenter, and ACK where you can use EKS to manage your entire platform and application lifecycle stack.  This will overall increase your velocity and time to market using all of these managed features from the baseline with EKS Auto to network observability, network policies, and now managed capabilities.

[![Thumbnail 3390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ed5f19c99b7f5ffd/3390.jpg)](https://www.youtube.com/watch?v=9HTSa_JsoAQ&t=3390)

So what are the key takeaways for us today, Luke? Thanks Rodrigo. What we're seeing is our customers  are creating complex environments that are increasing in their distribution, growing in their overall scope and their heterogeneity, and the network is a core fabric or a critical layer. In order to sufficiently optimize according to whatever your standards are, you need the right kind of visibility.

In addition to that, we've pointed out how having the right level of security in your network environment is actually going to enable you to move even faster. We hope you see this connection between them. Informed decisions can help empower you in making the right kind of tactics for the security policies that you're going to be rolling out with a lot more confidence knowing what the expected behavior will actually be.

As we've highlighted in the previous slide that Rodrigo just pointed out with EKS capabilities, our commitment is to increasingly simplify operations for our customers. Taking that layered approach, we can see how EKS capabilities fits into that as well. We're very excited for you to try out these new features: container network observability and EKS, and of course what was just launched yesterday, EKS managed capabilities.

Rodrigo and I will hang out a little bit after this. We'd love to chat with any of you that have particular questions and look forward to hearing about how you use these features in your environments. Thanks a lot for coming to the session.


----

; This article is entirely auto-generated using Amazon Bedrock.
