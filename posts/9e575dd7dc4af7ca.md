---
title: 'AWS re:Invent 2025 - From Trigger to Execution: The Journey of Events in AWS Lambda (CNS423)'
published: true
description: 'In this video, Julian and Rajesh from AWS Lambda explain the service''s internal architecture, processing over 15 trillion invocations monthly with 99.99% availability. They detail three invoke types (synchronous, asynchronous, and event source mapping), the separation of sync and async data planes for resilience, and how Lambda applies queueing theory principles. Key topics include Firecracker and Lambda Managed Instances, worker host architecture with execution environments, Event Source Mapping features like filtering and batching, poller design for streams versus queues, and operational strategies for handling dependency outages, scale inversion, and availability zone failures using techniques like shuffle sharding, token buckets, and static stability.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/0.jpg'
series: ''
canonical_url: null
id: 3087088
date: '2025-12-05T18:03:19Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - From Trigger to Execution: The Journey of Events in AWS Lambda (CNS423)**

> In this video, Julian and Rajesh from AWS Lambda explain the service's internal architecture, processing over 15 trillion invocations monthly with 99.99% availability. They detail three invoke types (synchronous, asynchronous, and event source mapping), the separation of sync and async data planes for resilience, and how Lambda applies queueing theory principles. Key topics include Firecracker and Lambda Managed Instances, worker host architecture with execution environments, Event Source Mapping features like filtering and batching, poller design for streams versus queues, and operational strategies for handling dependency outages, scale inversion, and availability zone failures using techniques like shuffle sharding, token buckets, and static stability.

{% youtube https://www.youtube.com/watch?v=4zFJ3zDWgeM %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/0.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=0)

### Introduction: AWS Lambda Team and the Magic Behind Serverless

 I'm Julian. I'm a developer advocate at AWS working with the Serverless team, and I love helping developers turbocharge how they build applications using all the cool things Lambda can do. Rajesh, hello everyone. I'm Rajesh Pande, principal engineer with AWS Lambda. I've been with Lambda for six years now, and in that time I have watched Lambda grow from thousands of customers to millions of customers, and it's been an incredible journey for me. I'm looking forward to sharing some of those fun stories with you all today.

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/30.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=30)

 So today we're going to continue a series of different Lambda under the hoods from previous re:Invents to help you understand Lambda a little bit more. I'm going to go over some Lambda fundamentals, how various invokes work, and spend some more time on our polling capabilities. Then Rajesh is going to come back in and head down the rabbit hole and do some interesting lessons on how we built Lambda, the choices we made, and even more information on the pollers. To be honest with you, a part of the magic of Lambda is a lot of what we're going to cover today you don't actually need to know. We just handle it all for you. It's a super amazing service and we love explaining how it works so it doesn't seem so mysterious, just maybe magical in how it works at such great scale.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/70.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=70)

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/80.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=80)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/90.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=90)

 So let's start with the trivia. Who wants to know probably the biggest secret of serverless? Well, actually there are quite a lot of servers underneath, sorry to tell you, hundreds of thousands in fact, and you as a consumer never see them because we at AWS manage all of this underlying infrastructure for you, allowing you to focus on your business scenarios.  With Lambda, we like to think of it as you just bring your egg, your code, the life of your business, and we've built an incredibly resilient nest at scale to look after your egg.  Because it means you don't have to hold, you don't have to build a whole bunch of things yourself. Things like auto scaling logic, availability zone failover, retry logic, connection pooling, backpressure, lease management, all these different kinds of things we take on so you don't have to.

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/110.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=110)

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/120.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=120)

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/130.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=130)

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/140.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=140)

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/150.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=150)

### Lambda Fundamentals: Firecracker, Lambda Managed Instances, and Compute Models

 This obviously runs at massive scale, more than 15 trillion invokes each month with a whopping 1.7 trillion invokes just on Prime Day alone.  We have 99.99% availability and resilience built into the service, so you don't have to pay extra for it.  Let's do a recap of some Lambda fundamentals. One of the major technology foundations of Lambda is the open-source Firecracker, but now we're actually expanding the foundation to include good old EC2.  Lambda Managed Instances announced on Sunday gives you Lambda's operational simplicity with EC2's range of instances. You can choose to use Graviton or CPU or memory or network optimized instances for your Lambda functions, and you don't have to manage any of these instances yourself.  You also get up to 12 times savings for at-scale workloads and multi-concurrency and EC2 pricing incentives all part of the package. There's also a session CNS382 which you can find out more about it.

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/180.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=180)

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/190.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=190)

With normal Lambda, so-called on-demand, everything runs in the Lambda service VPC and invokes come in from clients and ultimately run on EC2 worker hosts owned by Lambda in the Lambda service VPC.  With Lambda Managed Instances, the control plane stays in the Lambda account, but we provision and manage EC2 instances in your account, in your VPC, and then route function invokes to the containers running on them.  So Lambda on-demand with Firecracker and Lambda Managed Instances are two different compute models, but the rest of the architecture basically stays the same.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/210.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=210)

### Three Invoke Types and the Lambda API Architecture

 Lambda has three invoke types. Synchronous is when the caller calls either directly or via API Gateway, and this sends the request to the Lambda service, does its processing, waits for a response before returning it to the client. When you do an async request either via client call or maybe something like an S3 change notification or an EventBridge rule matching, sending an event, you don't wait for a response from the function code. You hand the event off to Lambda and Lambda just handles the rest. It places the event in an internal queue and returns a successful response to the caller saying I got your event, and then a separate process goes and processes it.

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/260.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=260)

An event source mapping is a Lambda resource that reads items from a stream like Kinesis and DynamoDB or from a queue like SQS. A producer application would have put messages onto this, and then Lambda manages the pollers which we're going to cover today, so read the messages and then send them on to your invoke for processing.  The Lambda API is the front end where all requests to the Lambda service land, and it is multi-AZ with a load balancer resolving to lambda.region-name.amazonaws.com. This routes the invoke request to the data plane and then ultimately lands up on a worker host, yours or ours. That's the journey of events in Lambda.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/290.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=290)

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/300.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=300)

 The front-end load balancer distinguishes control plane requests from invoke requests. Control plane requests can be function configuration or resource management from the console, CLI tools, or an SDK. The data plane then routes to the separate data plane, one for sync invokes and one for async invokes. 

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/340.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=340)

We technically call this the front-end invoke service and async the event invoke front-end service, but naming things gets a little confusing, so we're just going to stick with sync and async to keep things simpler. This is actually a deliberate choice to separate the data planes, which was built into the system a few years ago when we had a huge async spike that overwhelmed the sync service. Now we can protect the sync service from async floods. The data planes are also a fleet of status hosts. Lambda is a multi-AZ service, so you don't have to worry about load balancing your functions across multiple availability zones. Lambda handles it for you. 

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/360.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=360)

For performance and stability, the data plane needs to get information about functions from the function versions DynamoDB table, which we cache both on the hosts and also in an L2 AZ cache. We want to make invokes as fast as possible, so we want to avoid having to look up things on every single invoke. This approach also helps with availability. 

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/390.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=390)

### Deep Dive into the Synchronous Invoke Path

Let's now look at the synchronous path in more detail. The ALB distributes the invoke request to a fleet of hosts as part of the sync invoke service. I'm simplifying the diagram here as it's going to get a little complex, but Lambda is built so everything runs across multiple availability zones. The sync invoke service first performs authentication and authorization of the request to ensure that Lambda is secure and only authorized callers can get through Lambda's front door. 

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/410.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=410)

The service then loads the metadata with the request. The front end also calls what's called the counting service. This is going to check whether any quota limits may need to be enforced based on the account or reserved concurrency. The counting service is optimized for high throughput and needs to be under 1.5 millisecond latency, as it's called on each invoke. As it's critical to the invoke path, it's also highly available across multiple availability zones and extremely fast. 

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/430.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=430)

The front end then talks to what is called the assignment service and also a control plane service, another component which manages coordination and background tasks. It handles control plane things like creating functions and manages the lifecycle of assignment service nodes. The assignment service coordinates the placement service. 

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/450.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=450)

For the first invoker of a function, a new execution environment needs to be started on a worker host. The placement service creates an execution environment on a worker with a time-based lease. It also monitors worker health and makes the call when to mark a worker as unhealthy. Once the execution environment is up and running, you can see the init path. The assignment service gives you the supplied IAM role and environment variables with the privileges you define, and then gives them to the worker. 

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/470.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=470)

The execution environment then starts the language runtime, whether it's Java, Python, or another language, and downloads the function code or runs the container image. Then the function init process runs. The sync invoke service runs invoke and your function runs and sends the results back to the caller. To minimize latency, it calls directly to an invoke proxy on the worker hosts. 

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/480.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=480)

When subsequent requests come in, the front end talks to the assignment service, which says yes, we already have an execution environment up and running, and it routes the invoke payload directly to that execution environment and runs the handler. When the lease of the execution environment is up or there's some kind of error, the assignment service is able to gradually drain the connections, spin down the execution environment, and stop future invokes. 

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/510.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=510)

### Inside Lambda Workers: Execution Environments and Host Architecture

Let's take a deeper look at some of the Lambda workers. A worker host is just a standard bare metal EC2 instance in which we place execution environments. To be efficient and get invokes happening as fast as possible, we maintain empty microVM pools on the hosts in various different memory sizes. When an invoke comes in, the host management can allocate the smallest memory size to satisfy the request and then download the code or image. This speeds things up a lot since it can take time to get an execution environment up and running, so we prepare them in advance. 

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/540.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=540)

For Firecracker workers, Firecracker is the process that sits above the OS and manages the single secure execution environment for invokes. Within the execution environment, we maintain what we call sandboxes, which are basically privileged container namespaces, one for your function code, the runtime and extensions, or your container image. We also maintain a separate managed sandbox which your code can't access, but allows us to manage things inside that execution environment. 

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/550.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=550)

An interesting thing is we've actually completely rearchitected how this works this year to improve Lambda for how it works today and also to build future functionality. It's been rolling out across our fleet and you wouldn't know it unless you knew where to look. Trillions of invocations are happening and we're basically swapping the tires of Lambda while the car is moving at crazy speeds. 

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/590.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=590)

Outside of the sandboxes, we also run a management agent for control plane functionality and a telemetry agent which allows us to get metrics and logs out of the execution environment transparently. The worker host then has a bunch of other components. Host management coordinates with other services and builds and destroys those execution environments. 

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/600.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=600)

The invoke proxy is the direct link from front end to your code. 

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/630.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=630)

We also need to store microVM files like runtime and binaries on the host for faster loading, and we maintain a cache for container images and as many things as we can for performance. We also have agents that need to talk externally to the container image control plane for downloading container images and to S3 for Zip archives to get your function code into the execution environment. Of course, this needs to connect elsewhere, so there's networking to connect to the outside world. 

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/640.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=640)

For Lambda managed instances, it's actually pretty much the same, although we're not using Firecracker but containerd for logical separation and then containers instead of microVM execution environments. 

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/650.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=650)

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/660.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=660)

### Asynchronous Invokes: Internal Queues and Poller Management

Let's look at async invokes. It works in a similar way to the sync service but only handles async event requests. We've also recently increased the payload to 1 megabyte, so you can now use it for even more use cases.  The frontend then sends the invoke request to an internal SQS and responds with an acknowledgement to the caller to say that Lambda is going to invoke your function asynchronously. We got your request, and Lambda is going to manage the SQS queues, and you don't actually even have visibility of them. 

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/690.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=690)

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/700.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=700)

We run a number of queues and then dynamically scale them up and down depending on the load and the function concurrency. Some of the queues are shared, but we also send some events to dedicated queues to ensure that Lambda can handle a huge amount of async invokes with as little latency as possible. We then have a fleet of poller instances which we manage, and we'll cover that in more detail.  These pollers read the messages from the internal SQS and then determine the function, accounts, and the payload, and then ultimately send the invoke request synchronously again to the sync invoke service. 

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/730.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=730)

As you can see, all Lambda invokes ultimately land up as a sync invoke, and the function uses the same sync pathway I talked about before. The function returns the response to the poller, which then deletes the message from the queue. If the request is not successful, the poller doesn't actually delete the message from the queue and uses the same visibility timeout as you would with your own SQS queues, and then the same or another poller can pick up the message and try again. 

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/740.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=740)

You can also configure event destinations for async invokes to provide callbacks after the processing, whether the invokes are successful or they failed or after all the retries are exhausted. Some other control plane services are involved. There's a queue manager which needs to look after the queues, monitor them if they need backups, and handle the creation and deletion of new queues.  It works nicely with the leasing service which then manages which pollers are processing which queues and can also detect when a poller fails so that their work can be passed to another poller.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/760.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=760)

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/770.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=770)

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/790.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=790)

### Event Source Mapping: Polling from Streams and Queues

Let's look at now the event source mapping or the poller invokes. A producer application is going to put messages onto the stream or queue asynchronously.  We then run a number of slightly different pollers as we have different clients depending on what the event source is. This is a very similar architecture to async invokes from then on.  The pollers read the message from the stream or queue, can also filter them, batch them up, and then send them up to a single payload and send to your function synchronously with the same sync frontend service. Remember, all Lambda invokes eventually land up being synchronous. 

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/800.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=800)

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/810.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=810)

Then for queues, the poller can delete the message from the queue when your function successfully processes them. Depending on the event source, you can then send information again by the invoke to SQS, SNS, EventBridge, S3, or even Kafka for Kafka sources, which we announced recently.  There are also a number of control plane services, again, state manager, stream tracker, depending on the event source, manage the pollers, manage the event sources, discover work, and then handle scaling the poller fleets. 

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/830.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=830)

The leasing service is going to assign pollers to work on a specific event or streaming source. If there's a problem again, it's going to then move its work to another poller. There are also architectural differences between two types of event sources: queues and streams.  Queues are for individual task processing when each message is independent and messages are deleted after the processing. Streams are for when you're doing multiple consumers, maybe needing the same kind of data and order particularly matters, and then messages are going to be retried for replay. These are two different architectural styles.

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/860.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=860)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/880.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=880)

The Lambda Event Source Mapping provides a bunch of functionality across all of these event sources. We've got filtering, batch controls, including for streams, being able to split a batch to find a faulty record. We got choosing where to start in a stream, retry and failure handling, analytics for Kinesis, and some platform performance configuration options.  The cool thing is the Lambda Event Source Mapping handles this all for you. You don't need to think about the different characteristics beyond just configuring your Event Source Mapping. We take on the task of making everything actually happen. 

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/890.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=890)

Let's look a little deeper at the Event Source Mapping and see how it works with both streaming and queuing. The Event Source Mapping pulls the event source, the queue or stream for available records.  Now, we need to think about even getting actually to the event source. SQS and Kinesis are on public endpoints, but we also have event sources that are connected to private subnets or even outside AWS, often with Kafka. So we need to handle that as well as the actual functions which could be connected to a VPC.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/910.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=910)

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/920.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=920)

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/930.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=930)

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/940.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=940)

Authentication depends on the event source. For AWS  services, we can use IAM, but for Kafka we want to support all possible authentication methods so the ESM can authenticate.  If there's ordering, such as with SQS FIFO or streaming sources, we always need to ensure that the ordering is maintained with message processing. For streams, you can configure the starting position  as part of the ESM config from the beginning, from a particular timestamp, or just getting the latest records. The ESM also needs to remember where it was in the stream using the offset or sequence  number.

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/960.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=960)

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/970.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=970)

We need to handle retries and resharding. Streams can grow or shrink, so we need to continually monitor the source to see if there's a new Kafka broker online or new shards or partitions to process from one of the event sources. When the parent shard is split, creating a new child shard, we need to maintain ordering during this process  so we can send the right records to new Lambda functions to scale up the processing. For Kinesis, there's a helpful additional setting called tumbling windows which you can use for data aggregation.  You can pass the state across multiple separate Lambda invokes to aggregate data in real-time, providing a quick and easy alternative for some analytic solutions.

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/980.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=980)

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/990.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=990)

There are two ways to consume from Kinesis.  You have shared fan-out where up to five consumers can share the read throughput of a stream, and each shard supports a read rate and data rate with a latency of about  200 milliseconds. But for faster responses to stream data, you can set up Kinesis Enhanced Fan-Out or EFO. This changes the flow from the ESM pulling from Kinesis to Kinesis sending to the ESM using an HTTP push mechanism to get data to the ESM faster, as low as 70 milliseconds. The ESM figures this out and manages the change in logic flow transparently, something you don't have to manage yourself.

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1030.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1030)

There's also a dedicated poller offering for SQS and Kafka where we can provision pollers in advance to handle spikes.  You can configure various settings, mainly the minimum and maximum pollers, to control the throughput, and this can dramatically increase your polling scaling and throughput. You can achieve 16 times more throughput for SQS, up to 20,000 concurrency compared to the standard polling of 1,250 for SQS. Kafka also simplifies the networking and cost models when you enable this, though there is an additional charge based on an event processing unit which you can manage. Provisioned mode is great for high-traffic sources where you want low latency and can save you money at scale.

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1070.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1070)

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1080.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1080)

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1090.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1090)

Provisioned mode allows us to build and bring new functionality to the pollers like supporting schema registries and allowing efficient  binary formats like Avro and Protocol Buffers. Filtering allows you to drop records you don't want to process. Why? Well, filtering  is a CPU-intensive process, and you don't want to waste Lambda invokes in your code doing that. You can have positive filtering to include messages that you do want to process or negative filtering to exclude  all messages that don't match particular specific criteria. This is very flexible using the same syntax as EventBridge filtering. In this example, we are processing messages for tire pressure less than 32 to send only those to your function.

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1110.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1110)

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1120.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1120)

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1140.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1140)

### Batching, Scaling, and Throughput Control for Different Architectures

Next is batching, which groups  the records for efficient processing, allowing you to control your Lambda invokes for efficiency. How are batches defined? You get to configure and control this in a number of ways.  You can configure the size, which ranges from 1 up to a default of 10,000. We normally start at 10, the number of items in the batch, but you can go up to 10,000, which is quite large. The batch window helps improve efficiency when you don't have much traffic, so you can process messages before a whole batch is formed. You also need to stay within the Lambda 6 megabyte payload limit. 

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1150.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1150)

Once there is a batch, we invoke the function via the sync control plane, so we need to consider different scaling for different architectures.  Streaming is actually an upper bound problem. You want to process data from a stream generally as fast as possible, so there's a maximum throughput you want to factor in. With Kinesis and Kafka streams, you're trying to consume and process messages at the highest rate possible to keep up with the incoming data and minimize the lag. The challenge is in scaling up to meet or possibly exceed the ingestion rate.

Queuing is different. This is actually a lower bound problem. You do want to process data efficiently, but you also want to ensure you're not overwhelming downstream services like APIs or databases. With SQS, the queue acts as a buffer or shock absorber, and you control the rate at which you drain the queue to protect those downstream resources. The challenge is in maintaining a minimum safe processing rate that respects those downstream resources. These are two different architectural styles, and for SQS to do this, we want to manage the flow control, the rate at which Lambda consumes the messages.

[![Thumbnail 1210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1210.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1210)

 You can configure the maximum concurrency on the Event Source Mapping to control how many concurrent executions Lambda will attempt to send to Lambda to prevent overwhelming downstream services. This is the all-important buffering control. Reserved concurrency is a separate setting on the Lambda function which you set to reserve capacity from the function, and this ensures it can scale up if needed within the available account concurrency. You actually want to use the maximum concurrency to maintain and manage the buffer and the flow, but you can also use both together. If you do, you want to make sure that the reserved concurrency is higher than the maximum concurrency to prevent any throttling.

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1260.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1260)

[![Thumbnail 1280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1280.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1280)

[![Thumbnail 1290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1290.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1290)

For streams, we need to scale up how the stream scales. This happens automatically; the Event Source Mapping just needs to figure this out. Kinesis scales by adding shards and Kafka by adding partitions.  Lambda is automatically going to figure this all out and scale up the consumer pollers to match the incoming throughput, still maintaining message ordering in each partition or shard. More shards or partitions means more processing. Customers wanted even more processing for Kinesis, so we came up with what's called a parallelization factor.  By default, it's one function per individual shard, but you can scale up to ten for massive Lambda throughput, still maintaining the order.  This is something built specifically for Lambda for high throughput stream processing.

### Error Handling Strategies: Partial Batch Response and Bisecting Batches

For error handling, streams and queues have different semantics. Streams need to preserve ordering, but you need to decide what to do with the poison pill message. Should that stop processing? If it's something like log data or bank transactions, absolutely, you want to stop the stream processing and fix the problem before continuing. But maybe for frequent sensor data or GPS data from a hire car, you can handle some data loss because in a few seconds the next result is going to come in and you can still continue shortly. In this scenario, you don't want to stop the stream because you want to keep the message processing going. For queues, generally you don't want to stop for a failed message and you want to continue processing the rest.

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1340.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1340)

We have two batch error handling options to give you more control.  Partial batch response is when you know the failed record and you can turn a successful response and then tell the Event Source Mapping which message has failed. That's going to be retried and processing continues. AWS Power Tools for AWS Lambda is a great utility for lots of languages, and it has a batch processing utility to help with this. For streams, bisecting batches can be super useful when you don't handle the error, and it's not something you normally need to consider for queues.

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1380.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1380)

[![Thumbnail 1390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1390.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1390)

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1400.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1400)

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1410.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1410)

In this example, message three is a bad one, although we wouldn't actually know it yet. We process the batch and get a failed response. Instead of endlessly retrying the same batch, the Event Source Mapping can then split the batch and try the first half to keep  ordering and try to get more messages processed. The split batch fails again as it has the faulty message, so the Event Source Mapping splits again and we try to process the new batches.  This time, the first batch processes successfully and the single batch with the poison message fails again. We can then continue with the original second half of the first batch to keep that ordering of the successful messages  and then fully process as many messages as possible without having to reject a whole batch and deal with a faulty message.  This is a really efficient way that you can control error handling.

We need to handle function errors, so we need to configure Lambda on-failure destinations for function invoke issues. You might wonder why there are actually two different error handling paths for SQS. The SQS Dead Letter Queue is going to capture messages that fail repeatedly during the polling or processing. While on-failure destinations is going to capture invoke errors with your Lambda function, and this could be things like network issues or throttling or maybe you've deleted the function or there's a function permission issue. Both serve different purposes and can be used together for comprehensive error handling.

[![Thumbnail 1450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1450.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1450)

That takes us through the Lambda fundamentals.  We've covered sync, async, workers, pollers, and all the features of the Event Source Mapping. It just looks like a configuration option, but Lambda underneath the hood is doing a huge amount of work, doing polling, doing pushing, doing pulling, being as efficient as possible to get the messages from the different architectures of your streams and your queues. Let me now hand back over to Rajesh, who's going to dive deeper into the pollers and talk about some of the lessons we learned about when we were building Lambda.

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1500.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1500)

### Lambda as a Queuing Service: Mental Models and Buffer Design

Hello, thanks Julian. 

Let me give you an overview of this segment. I have divided this part of the talk into two sections. First, I will walk you through the mental models that we used while designing Lambda and the operational complexity that comes with it. This complexity actually fueled a lot of innovation that we have baked into Lambda's design. Let's start with the mental model first.

[![Thumbnail 1530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1530.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1530)

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1540.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1540)

Last time we stood here, I convinced you that Lambda is, in some ways, a storage service  and applies a lot of learnings from building services like Elastic Block Store. This year, I want to convince you that Lambda is also  a queuing service and how we have applied a lot of learnings from building queuing services and queueing theory concepts. I know compute as a queuing service seems a bit counterintuitive, but let me explain what I actually mean by that, and then I will walk you through the thought process of designing the Lambda service.

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1560.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1560)

What is a queue service? Let me start with that.  I started with Amazon back in 2013, so it has been more than a decade. The first thing that I did was build a queue processor. Basically, a queue processor has a message buffer where a lot of events and messages are stored. There is a worker which pulls from the queue, and you write a way to deal with the queue. Then you have business logic that processes your messages and does something with it.

For that, I had to build a lot of simulations to model my upstreams and downstreams, identify what the failure points were, and determine what it would take to build a resilient service. Back then, Lambda did not exist, so I had to do a lot of heavy lifting and build the queue processor from scratch on my own. When I joined Lambda around 2018 to 2019, I realized that the same pattern shows up everywhere. Many of the features that we built and many of the things Julian just walked you through are inspired by that same analysis I have been doing since 2013, over a decade.

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1640.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1640)

Let me quickly walk you through queueing theory so that it will set the foundation for the rest of this session. Queueing theory is nothing fancy, but it is basically the study of waiting lines: how things arrive, wait, get processed, and leave.  This diagram shows the basic shape of every queuing system you have probably seen some version of it. Events arrive at some rate, and that rate is never constant. Some customers go crazy and spike, while others have a slow and bounded arrival pattern.

On the right side, you see a worker. Their speed is the service rate, and just like arrivals, this is not constant. Sometimes your data or your dependency can take more than ten milliseconds, sometimes it can take one hundred milliseconds. So this is also not fixed. You have to model different things around it. Next, what you see here is buffer, burst, and variability. Variance is when you are building a queuing service. There comes a lot of variance in terms of if there is a spike, if there is a thread issue, or if your services are not able to handle the workload, then it may run into some issues. Variance in the system can lead to buildup of the backlog.

I will talk more about it when we go into specific sections. Let's look at the lessons that Lambda has learned from queueing theory and how it applied them. The first thing I want to talk about is the lesson that we learned from queueing theory: buffers smooth variance. Arrivals are never uniform. They are bursty, they spike, they are unpredictable. So we do not try to handle them right away. That is impossible and expensive. You can always provision for the peak, but then you would end up paying a lot more for it.

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1770.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1770)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1790.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1790)

The second lesson is that we specialize workers for workload types. I will talk about how this led to the design of Lambda in terms of how we separated pollers from the Lambda execution environment and why the nature of different pollers is different than the execution environment.  The third lesson is to control variance to prevent instability. Even if an average arrival rate is less than average service rate, variance can cause temporary instability, and that variance kills the system. The fourth lesson is to coordinate shared resources through centralized control. 

In queueing theory models, work conservation means if there is work and if there is a server, the work and server should never sit idle. But theory assumes there always exists a global state. In a distributed system, you have to build it.

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1810.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1810)

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1820.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1820)

I'll talk about how Lambda has to build it. Let's start with  buffers smoothing variance. As I was talking about, the traffic pattern is always variable. There are customers with steady state traffic and customers with random traffic.  Some customers spike extremely—they can spike from 10x to 50x—and you have to have a system that takes care of this. There are some customers who are slowly ramping up and suddenly become real customers, and some customers can have micro-bursty patterns. You have to have a system that can actually handle this.

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1850.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1850)

What you're basically building is an injection tier  where there are data sources, an injection tier, and storage. The traits of that system are that it can accept highly variable multi-tenant traffic. After you've taken the traffic, you need to normalize and validate the request, and you have to write the message or events to the right queue. You have to persist it and acknowledge it. Before acknowledging, you need to make sure that it's persisted durably. You also have to apply admission control and backpressure to protect yourself.

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1900.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1900)

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1920.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1920)

[![Thumbnail 1930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1930.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1930)

[![Thumbnail 1940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1940.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1940)

If you have to build something of this sort, you would need a durable architecture that helps you build this injection tier. What Lambda did is this: we have a layer of customers with variable traffic patterns.  We have a load balancer where, whenever the first time that you invoke—as Julian was talking about front end and invoke and event invoke front end—this is the front end that is the event invoke front end. It has a bunch of things like distributed load, health checks, and all. Once you have that, it hits the upper front end, which is called a reverse proxy, which is a multi-AG service with a bunch of hosts in it.  At the same time, it also has auto-scaling rules so that if there is a spike in CPU, memory, or something, a bunch of these things can auto-scale on their own.  

Apart from that, once you have gotten the injection tier done, you also have to process the message. You have to have a buffer, and this buffer needs to be durable in the sense that you can't lose a message once you have told your customers that we have accepted the message. But at the end of the day, what you want to do is have it go into the queue where you can process it. We don't want to just index the message, but we also want to process it. So how do we build that routing layer in between? Let me walk you through that.

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/1980.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=1980)

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2010.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2010)

First, there's an invoke request that comes in with the event message.  Then it goes through a chain of validators. You do authentication, then you validate the payload, then you look at the function to see whether there's an active function or not. Once you pass this validation layer, you need to find the queue where that message can be ingested. Again, as I was talking about, we have millions of customers. We can't have millions of queues, so there has to be some sort of multi-tenancy within the queues. For that, the most popular pattern is building a set of queues and then hashing them onto a hash ring. 

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2020.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2020)

[![Thumbnail 2030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2030.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2030)

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2050.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2050)

So what we do is when a message passes through all the validation stages, we find the queue finder. We hash it onto the partition of the hash ring.  Once we have found the partition, we take that message, we have gotten the queue, and then we write it to that queue, whatever queue we found on the hash ring.  But as you know, even a single partition can get really hot because millions of customers can overlap onto the same partition. So what we do is we use a technique called shuffle sharding. 

Shuffle sharding is basically instead of hashing a customer onto a single queue, now you can hash that customer onto multiple queues. What we do is we have a single customer based on the account ID and there are some heuristics that we have. We hash that customer into two hash ranges. We pick the queues based on those hash ranges and then find the queue which has the lowest queue depth at that time. Then we insert a message in there. This is what we call shuffle sharding, and it does wonders for us. It helps us remove this hard partitioning, and it goes away.

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2090.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2090)

In addition to this, we also have something called express lane.  Once we know that a customer is going to be noisy and will have a lot more traffic, we already create a sideline queue for them. We let them absorb and we absorb the traffic, and then once they're done, we can reap that queue and back them into those dynamic shared queues.

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2120.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2120)

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2150.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2150)

What I have talked about so far are the queues that are managed by Lambda. Those are event invoke queues.  Whenever you ingest Lambda or invoke Lambda asynchronously, that is where it goes. There is another part of the buffer which helps us smooth the variance, and that is the customer-managed side. This is the event source mapping side of things where the queues and streams stay on the customer boundaries. All we do is provide the processing, and from this point onwards the polling becomes the same for both customer-managed and service-managed queues. 

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2190.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2190)

### Specialized Workers: The Anatomy and Architecture of Lambda Pollers

The second lesson that we applied is specializing workers for workload types. Let's look at how Lambda applies this lesson. To understand it, let's first understand what a worker is. A worker is a processor that looks at the queue, and whenever it sees a message or an event, it processes it. Queuing theory showed us that heterogeneous servers with different service rates need to be handled differently for best performance. We applied this principle and separated the polling workloads from execution workloads. 

Both polling and execution workloads are different in nature. Polling is continuous, stateful, and connection-heavy. Execution, on the other hand, is bursty, stateless, and short-lived. There are actually two types of workers. One is a polling worker that is managed by Lambda, and there is a Lambda function worker that customers write. Julian talked about the execution worker side, and I want to dive deep today into the polling worker side. Let's jump right in and explore the key components that make up a poller.

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2270.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2270)

I divided and looked into what we do today. This changes all the time as we learn new things and evolve, so this is what we have today. We have a work acquirer component that looks at whether there is active work. It looks into an assignment manager store and checks if there is an SQS queue or a stream with work available. It picks up the work and starts the event source configuration. For a stream, we use KCL for Kinesis, Kafka Consumer for Kafka, and the SDK SQS client for SQS. 

[![Thumbnail 2300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2300.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2300)

Once configured, it kickstarts the work by configuring the connector, pulling the records, and writing them into an in-memory queue buffer. Many features that Julian talked about are being done in this in-memory queue where we create a view so we do not pressure the event source. We build this view and this is where we do batching, filtering, and schema validation if it supports that. Then we have an invoke orchestrator layer where once you have your batches ready, the batching can be done in different ways. 

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2330.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2330)

Julian talked about three ways we do the batching. Once you have the batch ready with your conditions defined, you do the invoke orchestrator. It invokes your Lambda in a synchronous manner with the batch and deals with the records. If a record is failing, it hands it off to the error handling component which writes to a Dead Letter Queue or retries based on your configuration. 

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2350.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2350)

In addition to this, if your message is successful, we have a checkpoint manager which deals with advancing the checkpoint. This ensures that before deleting a message, your system has processed it and we are not losing any messages. In addition to this, we have an auto scaler. It looks at the current state of the system and determines whether we need to auto scale.  If we are running hot, we look at the signals of our worker right now and determine if we need to scale further. The scaling can be in two parts. One is vertical scaling. If we have capacity in our host, we look at the footprint of our memory and CPU and determine if we can vertically scale to absorb more traffic. If we cannot, we signal our global coordinator to spin up new workers for us and take on that work.

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2400.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2400)

Apart from that, if we are running hot, we also want to load shed some of the workers. We have a load shedding component which removes the work if we are running hot. We do not want to impact our customers. In addition to that, there is an overall health checker which keeps looking into different parts of the system and making that data available to different components. The auto scaler looks into that data and determines the appropriate scaling actions. 

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2420.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2420)

The auto scaler says, "I'm running hot. I need to auto scale," and the load shedder says, "I'm running hot. I need to load shed something in memory." Key buffers similarly build that view and make it available to different components. 

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2460.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2460)

So far, we've discussed workers and the view of that worker, and what it takes to build a worker. However, not every worker is the same. Some workers are more secure, and some need to go through VPC boundaries. We went back to the drawing board and looked hard at the anatomy of the poller and what can be done there. If you remember, this is what we just saw, and we squinted—literally squinted—and saw that if we slice the worker horizontally,  we basically get the bottom part where all the system touchpoints are. We squinted even harder, and our bottom half can be further divided into three parts with three different touchpoints.

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2480.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2480)

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2510.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2510)

The first part of that worker is dealing with event sources, another part is dealing with internal service communication, and the third part is dealing with Lambda private invocations. All three have different security boundaries that you are operating with. So we said, let's pick this part of  the worker and wrap it into one specific set of security boundaries. Then pick this side of the worker and put it into one specific security boundary. The last part was the Lambda execution environment, which can have a very private VPC function that we can invoke. 

We applied the same principle that we applied to Lambda workers and Lambda pollers within the Lambda worker itself. We said that even there, there are three parts. Now we can automatically scale a part of that worker which is independent and has nothing to do with the rest of the worker. If you have worked with Kafka, you know the problem around consumer rebalancing, with death loops and things like static membership and cooperative rebalance. What if we could separate these two things in such a way that you can deal with the consumers separately while actually increasing throughput by invoking Lambda functions?

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2610.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2610)

### Controlling Variance and Building Global State for Distributed Systems

We wrapped them into security boundaries where one is dealing with the event source, one is dealing with internal services, and one is dealing with function execution. We can have VPC peering between the execution environment and the poller workers. We can have cross-account connectivity with the customer event source, and then internal service where this is within the same boundaries where it is operating. Now that we understand poller workers, let's look at the next lesson from queuing theory, which tells us about the production behavior of these workers. Lesson number three is to control variance to prevent instability. 

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2620.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2620)

Variance scales systems. Let's take a deeper look at it. What you see here is a workload which has  high latency. Once it has gone through and you started to absorb a lot more resources of the system, that leaves workload B and workload C with no resources to work with. Little's Law dictates that if processing latency increases, concurrency will spike even with a constant arrival rate. At this point, we want to build some sort of fairness in our system.

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2660.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2660)

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2670.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2670)

[![Thumbnail 2690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2690.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2690)

To deal with this, we built multiple layers of defense control with different mechanisms to maintain stability in the system. The first layer  is variance reduction. If you're getting messages into the queue, try to batch them, for example. The second one is capacity controls:  set hard limits like capping the concurrency so a single function can't overrun the fleet. Third is admission control: throttle requests when you're full, just like a store locking the door at capacity. And fourth is backpressure:  react when overloaded. You don't have to suffer. Slow down polling or shed low priority. These are four layers that we have built. Some of these layers can be configured by customers, and some are owned and managed by the service team.

[![Thumbnail 2710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2710.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2710)

Now that we have arrived and looked at the arrival pattern of workers and what keeps the service running, let me talk about one more thing.  We need to coordinate shared resources through a centralized component. Queuing theory tells us that there is always a deterministic or stable pattern. Whenever one worker fails, the other worker picks up the work. We don't have that luxury in a distributed system, so we have to build this global state on our own.

[![Thumbnail 2750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2750.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2750)

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2760.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2760)

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2770.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2770)

Building global state requires four traits. The first is work conserving scheduling. If there is work and there's an idle worker, we should try to pair them up. The second is conflict-free assignment.  If one worker is working on something, don't give it to someone else so they're not thrashing each other. The third is capability-aware dispatch.  You are giving work to someone who can handle that specific work. I don't want to give queuing work or SQS work to a stream poller, for example. The fourth is failure detection and recovery.  If someone has claimed the work and is actually not working on it, I should have a way to know that someone is not actively progressing on that task.

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2820.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2820)

[![Thumbnail 2840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2840.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2840)

Here's what we do. We have a control plane. Whenever you create an Event Source Mapping or event invoke config, it goes to a config store. Then we have an ESM Lifecycle Manager, whose job is to bring workers and make them available to the system so they can work on it. We create an assignment, and there's a separate service called Assignment Manager where it stores the assignment details into the config. This is where we build that global state.  The poller worker gets the work, then it sends heartbeats and metadata back to the assignment config store, which is the source of truth for our global state. At the same time, there's an anti-entropy job making sure that whoever is claiming the work actually has it and is acting on it. If they're not acting on it, then we involve human operators so they can take action.  This leads to the Lambda poller worker architecture that you've seen multiple times, and this was the under-the-hood part of building this kind of architecture.

[![Thumbnail 2870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2870.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2870)

[![Thumbnail 2880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2880.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2880)

Let's quickly recap. First, we need an ingestion tier and buffer with a smoothing mechanism.  Second, there are workers and different types. Some have different traits, so you should try to separate them out. Third, stability controls are critical because variance kills your system, so you need controls to maintain and build stability to your system. Fourth,  we need the global state that is needed for you to make continuous progress. This is what we need to keep the fleet healthy. This basically tells you almost everything you need to know about how Lambda applies lessons from queuing theory.

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2920.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2920)

### Operational Resilience: Handling Dependency Outages, Scale Inversion, and AZ Failures

By now, I hope I've given you some idea of why Lambda is also a key service in many ways. Lambda has millions of customers and trillions of events flowing through these systems. So how do we keep everything running? Seeing everything up front is really hard.  As a wise person said, everything fails all the time. So once you have the queue and you have the worker, what do you do if it fails? This second part of the talk is about how we deal with operational complexity when operating a queue service like this. Obviously, we had to innovate on different dimensions to run it reliably.

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2940.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2940)

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2960.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2960)

[![Thumbnail 2980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/2980.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=2980)

In queuing theory, it tells us that failures are independent, systems scale linearly, and saturation happens gradually.  This is the kind of world you would like to see as an engineer. But the moment you operate a real multi-tenant distributed system, you realize that everything gets messier. The reality is there are correlated failures. If one service fails, other services start to fail.  Systems don't scale linearly. If there's a retry loop happening somewhere, they accelerate faster and then there's a cliff drop. It's like you model something in your simulation, but you always miss that one condition which leads to a cliff drop. When I say cliff, I mean this cliff.  Back in 2013, Lambda had an outage where a latency bug in our frontend resurfaced, and that led to an increase in error rate. This is the case I'm talking about. It's really hard to model all the cases while you're testing. Same with the queuing service, you can't model everything, but what you can do is build a system which can recover from this.

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3030.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3030)

[![Thumbnail 3040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3040.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3040)

From this, I captured three main resiliency patterns that we apply to operate Lambda. The first one is dependency outages. What do we do when the core dependencies that we rely on to build the service goes away? The second is scale inversion. What if a bigger fleet starts to hammer the smaller fleet? What do we do?  And the third is availability zone outages. It's very special and unique to the problem, how we solve it, and how it is different for operating a Lambda service. Let's start with dependency outages. 

Let's look at a recent outage in October 2025. I think it's pretty fresh in everyone's memory. This was a recent DynamoDB outage with a bunch of services getting impacted, and Lambda was one of them. We use DynamoDB for various things to operate our data plane. The root cause of this issue was a latent race condition in DynamoDB DNS management. I think there is a talk you should attend if you're interested in what happened here.

[![Thumbnail 3070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3070.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3070)

Now, if you think from the point of view of a queue service operator, this becomes even more interesting. When an outage happens, we start to accumulate messages.  Now when the dependency is recovered, we have to deal with the backlog that we have accumulated along with new traffic that is coming in. To catch up, we have to work twice as hard, right? We have accumulated some messages and the new set of messages are still coming in. So what do we do?

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3100.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3100)

The second kind of thing that we see is a fairness conundrum. A noisy customer is a noisy customer, but a good customer becomes a noisy customer because now there's no way for us to differentiate. That noisy customer has hundreds of thousands or millions of messages. A good customer, a good citizen, also has millions of messages now because we were out and started to accumulate.  How do we differentiate between that?

[![Thumbnail 3130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3130.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3130)

Whenever there's an outage, we want to clear the backlog as soon as possible, but we also don't want to hammer our service down so that we delay the recovery. It's a balancing act. It always turns into a balancing act for us whenever there's an outage.  If we are underflowing, then we are delaying your recovery. Your message stays in the queue longer. But if we are overflowing, then we may tip over the service because there is a finite amount of capacity that we provision and a finite amount of work that you could do. You can have buffer and unlock a lot of capacity, but there's still a limit to the amount of work backlog that you have accumulated and how long it can take.

[![Thumbnail 3170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3170.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3170)

There are a few things that we do, and I'll talk about some of those strategies. This is not exhaustive by any means, but there are a few things which are interesting that I want to share. One thing that we build is a gradual ramp using a token bucket. That token bucket rate is controlled through the view of the system.  We look at how our CPU and memory pressure is looking, how our sync traffic is looking, and how our different modes of invocations are looking. Then we build and tune the rate of our TPS that we are generating through the asynchronous fleet to gradually recover so that we are not tipping over.

[![Thumbnail 3200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3200.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3200)

We also have a handshake mechanism. Every time there's an outage, we build an automated handshake mechanism where we keep looking into dialing up the knob and then looking at the rate at which our service is recovering.  We also try to convert a FIFO queue into a LIFO queue wherever possible. If you have a backlog and you are okay to go ahead and start looking into the first messages, we start to try to create and share the customers. The old message backlog can drain slowly, but your new messages start to see recovery very fast.

[![Thumbnail 3240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3240.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3240)

We also sometimes shard your queues if you have a function, a version, and an alias. We try to shard your queues into a function, a function version, and then an alias so that you start to see recovery much faster. And as I was saying, a good customer can become noisy and a noisy customer is a noisy customer.  So how do we isolate these things? Think about a customer who has millions of messages and sets their function concurrency to one. How do we deal with that? Now that customer is in the queue, we try to isolate that customer from the main queue so it can have its own processing because it's not a noisy customer. It's the outage that made it noisy.

So we have another kind of token bucket mechanism where we assign a set of buffers or give tokens based on whatever the function concurrency or downstream configurations are. We assign that one single specific token bucket to that customer and then try to isolate them so that even if they are in the shared queue, they are now using that token bucket to define their processing capacity.

[![Thumbnail 3300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3300.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3300)

This was about what happens when a dependency is out. There's another kind of outage that we have to deal with, which is scale inversion. This is where a big fleet is depending on a small fleet and a small fleet goes out, or a big fleet behaves weirdly, which leads to a small fleet going out.  So this is another outage that we have to handle.

[![Thumbnail 3310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3310.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3310)

[![Thumbnail 3330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3330.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3330)

[![Thumbnail 3340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3340.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3340)

This is another outage where a large surge of connection activity led  to a retry storm, which caused a small fleet to go out. At the same time, the control plane was recovering, which delayed the recovery. For us, if you think about the global state view with the leasing services out , if this is out for even one minute, we have accumulated messages and we continue getting new messages. So how do we recover from that? It becomes a problem for us. 

[![Thumbnail 3360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3360.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3360)

[![Thumbnail 3370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3370.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3370)

To address this, we built static stability. What I mean by static stability is the ability of a workload to continue correct operation despite dependency impairment without requiring reactive changes. The most critical step to achieving this static stability is separating the hot path from the cold path.  The data plane doesn't need to call the control plane during processing. The control plane only needs to do lifecycle management, and the data plane is sufficient to operate in the steady state. 

[![Thumbnail 3380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3380.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3380)

[![Thumbnail 3400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3400.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3400)

One of the strategies is to push down the configuration. For Lambda's case, we push the Lambda function configuration down to the pollers instead of having the pollers ask for that work.  The second strategy is replicating the state with L1 and L2 caches. We also use disk as a backup so that we can consume the work and restart or resume the work if there's an outage.  Then we build stickiness with flexible timeouts so we try to reduce false positives and false negatives. If there's an outage and we see it's a temporary outage, we don't leave your work and don't release your work. Lease-driven independence means partitions have their own dedicated workers or charters of dedicated workers. The circuit breaker is that if you see errors spread across the whole fleet, that means maybe there's nothing in common with that one specific coast, but it is spread across the whole system.

[![Thumbnail 3430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3430.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3430)

[![Thumbnail 3440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3440.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3440)

[![Thumbnail 3450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3450.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3450)

[![Thumbnail 3460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3460.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3460)

[![Thumbnail 3470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3470.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3470)

[![Thumbnail 3480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3480.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3480)

[![Thumbnail 3500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3500.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3500)

The third kind of outage is availability zone outages.  What I mean with that is that houses are very weird. There can be a thunderstorm somewhere that leads to an outage of an availability zone.  So what happens for us if there is an AZ outage? If there are three AZs in a sample workload and one AZ is out, we have to migrate  because we have millions of queues that we are processing and millions of workers and the relationship between them. So we have to migrate that active workload from that AZ to another AZ.  That migration requires some effort on our side. So what do we do? We have AZ buffer.  This is very standard. If you have capital processing capacity and you need to move the work, you need to have the buffer so that you can take on the work. The second is another balancing act for us.  If one AZ is impacted, you need to move the work to another AZ, but you don't want to impact the other AZ also with that outage. So we have a hardware-based mechanism where we reactively let the leases expire, and then that becomes available and gets shared, but it takes longer time. 

[![Thumbnail 3520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3520.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3520)

What if Lambda has recovered and other workers have moved from the unhealthy AZ to the healthy AZ? Now we need time to migrate, and if you're taking time to migrate, then there's no point in having that recovery. So we need to get more proactive in that. We have divided that into three parts: detection,  evacuation, and observation. We need to first make sure that there's an AZ outage and we are very confident about it, then we evacuate. Then we continuously observe that the work has migrated to the new AZ. When the recovery happens, we need to move it back.

[![Thumbnail 3550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3550.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3550)

So the key takeaways are architectural takeaways: design for robustness in unpredictable environments and reduce permeability. Isolate. And operational key takeaways around multi-tenancy being the default.  Failures are constant, and severity comes before scale. With that, I'll hand it over to Julian.

[![Thumbnail 3570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3570.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3570)

[![Thumbnail 3580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3580.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3580)

[![Thumbnail 3590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3590.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3590)

Thank you very much. We've only got a little bit of time left, but I just want to quickly go through some of the new things with Lambda. We talked about Lambda managed instances, all the flexibility of Lambda with EC2 control plane as well. Lambda durable functions allow you to build multi-step workflows in your favorite programming languages.  Another session is happening on that this afternoon.  Tenant isolation provides execution isolation between tenants. You just give a tenant ID. This is great for SaaS customers who have maybe hundreds of thousands of functions, which makes it much better for them. 

[![Thumbnail 3600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3600.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3600)

[![Thumbnail 3630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3630.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3630)

[![Thumbnail 3640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e575dd7dc4af7ca/3640.jpg)](https://www.youtube.com/watch?v=4zFJ3zDWgeM&t=3640)

Event Source Mapping enhancements include schema registry, which has been released, the provisioned mode for SQS that I mentioned, and also a whole bunch of ESM metrics that are available to help you build your applications.  In the future, of course, there's plenty more to do. We'll always be helping with observability, bringing new updates and new runtimes as quickly as we can. We're working hard on price to make Lambda applicable for all workloads, more deeper integrations with AWS and third-party services, and we always continue to listen to your feedback. Let us know what you would like us to build. Our Lambda roadmap is now public and available on GitHub, so you can help us define the future of Lambda.  There are lots of other kinds of sessions on managed instances, durable functions, and the future of serverless tomorrow. I did a talk on best practices yesterday that hopefully will be on YouTube and useful for you as well. 

There's another slide here with a bunch of service kind of things to continue your serverless learning. But most importantly, thank you so much for coming early in the morning. I appreciate you taking the time to learn a little bit more about Lambda. Hopefully, you can understand there's a huge amount that happens under the hood so you can just consume Lambda as a great service without having to worry about all the flow control and all the other kinds of things that Rajesh and his team are building. So thank you very much for joining us today and enjoy the rest of your conference.


----

; This article is entirely auto-generated using Amazon Bedrock.
