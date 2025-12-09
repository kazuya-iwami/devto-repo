---
title: 'AWS re:Invent 2025 - Introducing Amazon S3 Access Points for FSx for NetApp ONTAP (STG217)'
published: true
description: 'In this video, Luke Miller and Jacob Strauss introduce S3 Access Points for FSx for NetApp ONTAP, enabling customers to access ONTAP file system data through the S3 API without data movement. They explain how this bridges enterprise file data with AWS AI and analytics services like Amazon Bedrock, QuickSight, Athena, and SageMaker. The session covers authentication layers combining IAM and file system permissions, demonstrates creating access points with file system identities, and shows practical use cases including disaster recovery data utilization, analytics integration, and application modernization. A live demo illustrates querying financial data stored in FSx using Athena and QuickSight Q for AI-assisted business intelligence analysis.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/0.jpg'
series: ''
canonical_url: null
id: 3093033
date: '2025-12-08T19:22:52Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Introducing Amazon S3 Access Points for FSx for NetApp ONTAP (STG217)**

> In this video, Luke Miller and Jacob Strauss introduce S3 Access Points for FSx for NetApp ONTAP, enabling customers to access ONTAP file system data through the S3 API without data movement. They explain how this bridges enterprise file data with AWS AI and analytics services like Amazon Bedrock, QuickSight, Athena, and SageMaker. The session covers authentication layers combining IAM and file system permissions, demonstrates creating access points with file system identities, and shows practical use cases including disaster recovery data utilization, analytics integration, and application modernization. A live demo illustrates querying financial data stored in FSx using Athena and QuickSight Q for AI-assisted business intelligence analysis.

{% youtube https://www.youtube.com/watch?v=NVZV0gfV-jA %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/0.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=0)

### Introduction: Unlocking the Potential of Enterprise File Data

 All right, welcome everyone. We made it to day four of re:Invent. Thank you all for being here. This is STG217, Introducing Amazon S3 Access Points for FSx for NetApp ONTAP. My name is Luke Miller. I'm a product manager on the Amazon FSx service, and I'm joined today by Jacob Strauss, Principal Engineer for File Storage at AWS.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/40.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=40)

We've got a packed agenda today.  We're going to start off with an overview of FSx for NetApp ONTAP and talk about S3 Access Points for FSx. We'll go through what this means for your ONTAP data, specifically how it enables you to do more with your data. We'll go through some use cases, we'll take a look at how this works at a high level, and then we'll walk through a demonstration showing getting started, accessing your data, and a sample integration with an AWS analytics service.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/70.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=70)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/90.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=90)

But before we get into it, I want to take a  big step back. At AWS, we know that innovation, now more than ever, relies on data and the ability to leverage that data effectively. That's why we commonly say that data is the basis for modern innovation. And if we take a look at enterprise today,  we see that hundreds of exabytes of data is stored in file systems across organizations worldwide. To put that into perspective, that's more data than the entire internet contained just a few years ago, and it's sitting in file systems today.

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/110.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=110)

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/130.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=130)

 Eighty percent of that data is unstructured. We're talking about documents, videos, images, and more, the rich contextual information that truly represents your business knowledge and institutional memory. Lastly, storing all that data incurs cost  and overhead to manage, but only a small fraction of that data is utilized today. That means most of your organization's data may be sitting idle, not contributing to innovation or decision making.

Think about what that means for your organization. You have decades of institutional knowledge, customer insights, research findings, and business intelligence locked away in file systems, disconnected from your analytics platforms, your AI models, and business applications. So the question is, how do we unlock it? How do we bridge the gap between where your data lives and where your data needs to be to drive innovation? That's what we're here to solve today, and that solution starts with Amazon FSx for NetApp ONTAP.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/190.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=190)

### Amazon FSx for NetApp ONTAP: Fully Managed File Systems Powering Mission-Critical Applications

Amazon FSx for NetApp ONTAP  is the first and only storage service that offers like-for-like, fully managed NetApp ONTAP file systems in the cloud. You get all the familiar features, capabilities, and APIs of NetApp ONTAP file systems, everything your teams know and trust, combined with the simplicity, agility, and scalability provided by AWS. This means you maintain all your enterprise capabilities your organizations depend on, such as snapshots, clones, ransomware protection, multi-protocol support, and storage efficiency savings, but now combined with the cloud native benefits like elastic scaling and pay-as-you-go pricing and global availability.

[![Thumbnail 250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/250.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=250)

Customers are already able to realize more value from their data on FSx.  FSx allows you to eliminate the undifferentiated heavy lifting of self-managed file storage and instead focus on innovation. For example, you no longer need to spend valuable time, expertise, and money on hardware procurement, software updates, patch management, troubleshooting, and the various other operational overhead required to manage storage.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/280.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=280)

 FSx for NetApp ONTAP is powering mission-critical applications across a wide range of industries and use cases, ranging from enterprise IT applications, user shares, line-of-business applications to backup and disaster recovery. Let's take a look at a few.

FSx for NetApp ONTAP serves as the foundation for corporate file services, home directories for thousands of employees, departmental shares for collaboration, and workspaces that need to scale dynamically with business needs. We also see a diverse set of line of business applications. Financial services are running their trading platforms and risk management systems. Semiconductor companies are processing massive workloads for chip design. Healthcare and life science organizations are managing genomics data, medical imaging data, and clinical research datasets, all requiring the advanced data management capabilities that ONTAP provides. And of course, data protection use cases, backup repositories, and secondary data copies for disaster recovery targets that take advantage of ONTAP's built-in replication and snapshot technologies.

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/390.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=390)

### Announcing S3 Access Points for FSx for NetApp ONTAP

Customers are able to power these critical applications with FSx for NetApp ONTAP today using the existing set of native AWS security, compute, monitoring, and data management services such as AWS Directory Services, CloudWatch, and AWS Backup. Beyond these existing file-based  applications and services, customers want to be able to do more with their data. Over the years, AWS has built a wide range of AI, machine learning, and analytics services that integrate with S3, and customers want to be able to use these services with their ONTAP data.

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/450.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=450)

For example, they want to build, train, and deploy machine learning models with Amazon SageMaker. They want to process and catalog their data with AWS Glue. They want to interact with their data, such as querying data in place with Amazon Athena and visualizing their data with QuickSight to drive better business outcomes. Lastly, they want to build generative AI powered applications with Amazon Bedrock and use their data with AWS's generative AI powered business intelligence platform, Amazon QuickSight. 

So today, I'm pleased to announce that we launched S3 Access Points for FSx for NetApp ONTAP, offering the ability to access your FSx for NetApp ONTAP data as if it were in an S3 bucket. Using this new capability, customers can access their ONTAP data using the S3 API and a broad range of S3 integrated AWS services. S3 Access Points are S3 endpoints that customers use to securely manage access to shared datasets in S3 buckets and in FSx for OpenZFS file systems, and we're extending them now to support FSx for NetApp ONTAP file systems.

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/510.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=510)

Now customers with data stored in ONTAP file systems, either on-premises or in AWS, can access their data as if it were in S3. This new capability offers the easiest  way for you to do more with your ONTAP data. With S3 Access Points for FSx for NetApp ONTAP, all of that rich enterprise file data that we just talked about, from departmental shares to clinical research datasets and financial trading data, is instantly S3 compatible, meaning it's accessible via Amazon S3 and the S3 API and can now be seamlessly connected with a wide range of AWS analytics, AI, and compute services.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/560.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=560)

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/570.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=570)

### Transforming Enterprise Data with AI-Powered Business Intelligence

All without having to take your data out of your ONTAP file systems, and at the same time, that data continues to be accessible from file protocols, whether it be SMB or NFS.  Now let's take a deeper look at how you can maximize your file data potential  with S3 Access Points for FSx. Let's start with what I believe has the potential to be one of the most transformative use cases, integrating decades of enterprise file data with AWS's latest AI powered research and business intelligence tools.

Think about your organizations for a moment. How much institutional knowledge is sitting in file systems right now? Research reports, clinical trial data, engineering documentation, and legal contracts.

Engineering documentation and legal contracts represent petabytes of data that can revolutionize how your teams work, but this data is disconnected from modern AI applications. We've heard from our customers that they want to be able to use Amazon Bedrock and QuickSight to build AI-powered research assistants, but their proprietary data is in file systems, not S3. Before today, that meant building complex ETL pipelines, duplicating data, and managing synchronization, all while trying to maintain security and governance. With S3 access points for FSx, this becomes simple and easy.

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/650.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=650)

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/670.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=670)

 You attach an S3 access point to an FSx volume, and suddenly QuickSight can index your file data. Bedrock knowledge bases can use it for RAG workflows, and Q Business can answer questions about decades of institutional knowledge. That data never moves. It stays within your file system,  accessible via NFS and SMB for traditional applications while simultaneously available via the S3 API in AI services.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/710.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=710)

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/720.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=720)

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/730.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=730)

So let's talk through an example. I mentioned earlier that one pattern we see is customers using FSx for NetApp ONTAP as their secondary site for disaster recovery. Today, customers manage primary ONTAP environments on-premises for their primary workloads, and they deploy FSx for NetApp ONTAP on AWS as a secondary copy. Today that sits idle, unused,  replicated from their primary site to FSx using ONTAP's built-in data replication technology, ONTAP SnapMirror.  Now with S3 access points for FSx, they can put that data to work. You can attach an access point to your destination volume,  and then integrate that with QuickSight.

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/740.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=740)

### Enabling Analytics Without Data Duplication

Now let's take a look at analytics. You've got valuable data, as we discussed,  genomic sequencing, seismic surveys, manufacturing telemetry stored in file systems because that's how your existing file-based applications work. But before today, if you wanted to run analytics with Athena, you'd have to copy that data to S3, managing synchronization and paying for duplicate storage. Now with S3 access points for FSx, you can point Athena directly at your FSx data through an access point. Glue can crawl and catalog your file data as if it were in S3, and EMR can run Spark jobs against that data.

The analytics services don't know the difference. They're using the S3 API, but your data stays within FSx, accessible via NFS and SMB for your file-based applications. For example, imagine you're an organization in manufacturing and you want to build an end-to-end solution for quality control analytics on manufacturing data. You've got years of process data being ingested and stored in ONTAP on-premises. Now you can SnapMirror that data to FSx, to an FSx for NetApp ONTAP file system, and run real-time analytics with Athena or Redshift without maintaining a separate S3 data lake.

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/830.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=830)

### Application Modernization: Bridging File-Based and Cloud-Native Architectures

 Now let's shift a little. Let's talk about new opportunities for application modernization. Today, customers' ONTAP data is serving existing applications that have been running for years, maybe decades, that depend on file protocols. Your SAP systems, your electronic health record platforms, your trading applications, they all expect to mount a file system via NFS or SMB. You can't just flip the switch and rewrite those overnight. But at the same time, your teams want to build new cloud-native applications using serverless architectures with Lambda because they scale automatically, you only pay for what you use, and developers love to work with them.

With S3 access points for FSx for NetApp ONTAP, you can serve your data to both existing file-based applications and new object cloud-native applications. Your existing applications don't need to change, and you don't need to take your data out of an ONTAP file system. Here's a real-world example. Imagine you're a media company with video files in FSx.

Your editors access these files via SMB on Windows workstations for editing, but you want to automatically generate thumbnails, extract metadata, and run compliance checks whenever new content arrives. With S3 Access Points, you simply attach an access point to your FSx volume and configure Lambda to trigger on a schedule or a poll-based mechanism. When a new video is uploaded via SMB, Lambda automatically processes it using the S3 API. You can have one function for extracting metadata, another for generating thumbnails, and another for running AI-powered content analysis, all serverless, all accessing the same file data that your editors are working with.

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/970.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=970)

This is how you can augment and modernize without disruption. You're not forcing a big bang migration or rearchitecture. You're gradually adding cloud native capabilities alongside your existing workloads, all with the same data.  Next, I'm going to hand it off to Jacob who's going to walk us through how this works.

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/980.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=980)

### How S3 Access Points Work: Architecture, Authentication, and Performance

OK, thanks, Luke.  OK, so let's start by setting the stage for how we go from what we have today to what it looks like in the new environment when we add S3 Access Points to your FSx file system. So today, just to get started, this is a very simplified view in terms of how I'm sure your real systems look, but you've got an FSx file system. It sits in a particular AWS region. You've got clients that are running on EC2 instances or other compute platforms, and then you've got users that are interacting with those applications, and these are running over traditional file system protocols, NFS for Linux applications, SMB for Windows applications. So this is just what we have today.

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1030.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1030)

So then the new pieces that we're going to add here are creating an S3 Access Point for this file system, and this starts letting you  do a couple new things that weren't possible before today. So as part of this setup process, which we'll go through in some more depth in a minute and go through a demo and such, but I want to sort of walk through what all these steps are. When you create an access point, one of the things that happens as a result of that setup process is you get an alias for the name of that access point, and what this allows you to do is you can use this name anywhere that you would have used an S3 bucket name today.

So this means that you can make direct calls via the S3 SDK, whether that be the CLI or the SDK that's built into applications, and anywhere that it asks for a bucket name you can use the access point alias instead of that bucket name. So this means that you can now make put and get calls, list calls, head calls directly, and those get forwarded through the access point to your FSx file system. These can be in the same AWS region itself. You can run these commands also from EC2 or from other compute platforms, but they don't have to be. They can also be out on the internet. They can be on machines or other applications that you control.

This is a capability that wasn't possible before today, because before today you would have been limited to accessing your FSx data from within your VPC. So now there's a way around that for applications if you want to ingest or export data from somewhere else. And in addition to being able to run applications where you are directly creating, where you're directly controlling the application or the client, you can also pass the access point alias to other AWS services that are integrated with S3. So those can be things like Athena, those could be things like EMR, Glue, and so on, and we'll go through a few examples of these in a minute.

Now I want to talk about two other topics just very briefly and then we'll get into more detail there. One of them is how authentication and authorization works in this environment and then a little bit about some of the performance characteristics. So the way to think about how authentication works here is that there's two layers. There's one which is the S3 authentication model, and then there's one which is the traditional file system model, and they both happen. They both behave the way that you would expect them to.

So let's say that an application is sending a get request or a put request to my S3 Access Point. The first thing that will happen is the access point itself is going to look at the policies that are defined for that particular access point. These particular users are allowed to perform gets on these prefixes or these particular users are allowed to list objects or delete objects and so on, and it'll decide whether that particular request is allowed or not. If it's allowed, then the access point is going to forward that operation onto the FSx file system, and that will also make a decision based on whether the Unix or Windows user and group configuration is allowed for that particular file that the operation is trying to access.

Both of those happen, and the way you can think about them is that they both happen in a layered form.

Switching over to performance, we're not creating multiple copies of your data. There really is one copy of your data, and it's in the FSx file system. What this means is that when you're asking questions such as what's the overall level of throughput that your file system has when accessed through an access point, or the overall number of IOPS, it's the same as your underlying file system. If you want to make provisioning decisions in terms of how much throughput is permitted or how many IOPS that file system supports, those will be available to you either through the file system interfaces or via the S3 access point.

Another implication of having one copy of the data that's stored in FSx itself is that you don't have to reason about what happens when I perform an update via one side and when is it visible to the other side. There's really only one copy of your data. As soon as you create a file or put an object via the S3 API, it's immediately visible to applications that are using the other mode, the other interface.

[![Thumbnail 1290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1290.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1290)

 I also want to talk about how this setup is commonly used in some of the most common scenarios. One of them is that if you have a large existing data set, let's say in an on-premise data center where you've been building up libraries of data over years or decades, often what you'll do is you'll set up an FSx file system running in AWS and configure replication between those, usually for disaster recovery purposes. If you now add an S3 access point on top of the file system that's running in FSx, you've now gained a bunch of new capabilities that didn't exist before. You've taken that data that prior to today was really only there just in case of disaster, if you had to stand up your applications in a new environment, and you can now get value out of that extra copy of the data because it's available as a read-only data set or a read-only library to all of these services that are integrated via the S3 API. For example, you can do analytics on the disaster recovery copy of your data and get a whole bunch of new value out of this data that didn't exist before.

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1370.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1370)

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1380.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1380)

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1400.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1400)

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1410.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1410)

### Live Demonstration: Creating Access Points and Integrating with Amazon Athena

Now what we'd like to do is to start going through a demo of some of these sequences.  In order to make this a little bit more real, I just want to walk through what those steps are going to be, and then Luke will take over and walk us through how those work.  We're going to go through three different parts of the demo.  The first part is going to be how we get started, how do we create an access point, how do we configure it, and what are some of the decisions we have to make in order to get things working.  After that, we're going to go through examples of accessing data using both file interfaces and S3 interfaces, mixing and switching between file reads and writes, S3 puts and gets on the same underlying data set, and show how that works in practice. Finally, we're going to go through examples of some of these service integrations, how you can get value out of the data by making use of integrations with AWS analytics services such as Athena, and how you can query your data and get use out of it that wasn't possible before today.

[![Thumbnail 1450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1450.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1450)

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1500.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1500)

I'll switch over to Luke to start going through this. Thank you, Jacob. Let's switch over to the demo.  Let me just set the stage here a little bit for this demonstration. We're going to walk through a hypothetical scenario. We are a financial institution. We have some credit card data, some transaction data, and we're playing the role of two roles, a storage administrator who will go and set up an access point, and then also as a data scientist and user who's going to use some of the analytic services to run some analysis against the sample data set. We're going to start somewhere familiar. This is the AWS Management Console, specifically the FSx console.  We have an existing FSx for ONTAP file system for this demo.

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1510.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1510)

You can see this is a 10 terabyte file system with 384  megabytes per second. Again, this is provisioned storage, provisioned throughput, as Jacob had mentioned. Within an ONTAP file system, you can have one or more volumes. The way to think about an ONTAP volume is that it's a logical container for data. When you create an access point, you attach it to a volume. In fact, you can create up to 10,000 access points on a single volume. 10,000 is the max quota for S3 access points in an account and region, and you can put all of those on a single volume. You could put them across multiple volumes within a file system or even multiple file systems.

[![Thumbnail 1570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1570.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1570)

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1580.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1580)

So we're going to navigate to a volume here called Volume One and go to its details page. We can see this is a 100 gigabyte volume.  It's Unix security style, so Unix permissions. The first thing to see here is we have a new tab called S3. From this new tab, we can see all  of the S3 access points that are attached to this particular volume or this container of data, and you can see here we already have two, and we'll come back to those in a moment.

[![Thumbnail 1600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1600.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1600)

Step one is creating an access point. So first, we'll give it a name. I'm going to  call this my EC2 user S3 or V1 S3 AP. Now is a good opportunity to double click on the authorization story that Jacob had mentioned earlier. Again, you can think about this as being two levels of authorization. You're going to get the native AWS IAM authorization the same way access to an S3 bucket is authorized. You have an IAM principal caller. It's got a policy and permissions associated with it. You have a resource, in this case an access point. It has a policy, and access is either authorized or denied. Those permissions are evaluated by S3.

But beyond that, we want to ensure that the caller actually has permissions to the underlying file data. Again, this is secure access. The way that works is when you create an access point, you associate a file system identity. It could be a Unix user if you're in a Unix environment. It could be a Windows user if you're in a Windows environment. The way that identity, that user, is used is all access through that access point is going to get authorized at the file system as if it were that user.

I'll give you an example. Suppose I have an FSx for ONTAP file system that's integrated with Active Directory. Within that Active Directory, I have a user, Alice. Alice has a home directory in this volume where Alice has full read-write permissions on her home directory, but not Bob's. I can create an access point and associate Alice's identity to that access point, which means access via the access point is going to get authorized as if Alice is reading or writing data. So for example, if I do, using that access point, if I do a get object against an object that would be within Bob's home directory, I get access denied because Alice does not have file level permissions to that.

[![Thumbnail 1760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1760.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1760)

So again, two levels of authorization when accessing data via S3: IAM authorization and file system level permissions. In this demonstration, I had a Unix security style volume. I'm going to create a Unix user. I'm going to use EC2 user, which is the default local Unix user for Amazon Linux instances.  Beyond the file system identity, there are a few other mechanisms or properties of an access point that allow customers to secure access. The next is network configuration, and Jacob spoke about this a bit earlier. You have the option to make your access point accessible to the Internet, or you can restrict access to within a VPC.

In this case, because I'm going to demonstrate uploading and downloading data from my local laptop over the Internet, I'm going to choose Internet.

Just like with an S3 bucket policy, you can put an access point policy, and it behaves the same way. A bucket without a bucket policy will authorize access the same way as an access point without an access point policy.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1820.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1820)

Lastly,  some folks when they hear that their data is now going to be accessible over the internet get a little nervous. I want to draw a distinction between access over the internet versus public anonymous access. S3 access points for FSx have S3 Block Public Access enabled by default, and you cannot change it, meaning that there's no anonymous access via the access point.

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1860.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1860)

We'll create our access point now. When we come back to the list view, you'll see that there's now a third access point for this volume  and it is in a creating state. It takes about 10 to 20 seconds or so for it to create.

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1880.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1880)

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1900.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1900)

[![Thumbnail 1910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1910.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1910)

While this is creating, let's take a look at the details for an access point. Again, these are Amazon S3 access  points, just like an access point that you attach to a bucket. You can see that it has an Amazon Resource Name, an ARN of an S3 access point. You can even link to the S3 console where you can view the details of this access point from the S3 console,  and you can see that here you can tag them and you can specify an access point policy here. You can also link back to the FSx console.  You will also see the file system identity that we configured as well as the permissions.

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1920.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1920)

Let's go back to our volume.  You see, the access point that we just created is available for use. Another thing that Jacob spoke about earlier that I want to highlight again is the alias. An access point has an alias that's automatically generated. Anywhere you can use a bucket name for data access, you can use an alias. So if you're doing a put object, you would use an alias in place of the bucket name. If you're integrating or connecting with an S3 integrated service, you would specify the alias as part of the S3 URI, and we'll demonstrate both of those.

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/1980.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=1980)

We now have our access point. Let's put it to work. The next thing that we're going to show, as Jacob mentioned, is basic data access. I'm copying my access point that I created. Quick orientation.  We have two terminals. The left hand terminal is my local laptop. The right terminal is going to be SSH into an EC2 instance in my client VPC where I have my volume mounted.

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2010.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2010)

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2020.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2020)

First, I'm just going to make note of my access point alias, which we'll use. And then over here, we're going to SSH to my  EC2 instance and we'll show  that I do have this volume mounted here. And we can look at the contents of the volume. Again, we are a financial firm. We have a credit card dataset and we're going to do some analysis.

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2050.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2050)

Here is a look at that sample dataset. We have some data for cards, we have some data for customers, some data for fraud alerts, merchants, and transactions. Transactions is actually partitioned by year, month, and day, and we can take a look at that now. So on the right, all we've done  here is what you can do today, just access data via NFS.

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2060.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2060)

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2070.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2070)

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2080.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2080)

Now, from my laptop, we'll do  an ls against that prefix credit card directory. And we see that it returned all the directories within that volume. Again, cards,  customers, fraud alerts, and so on. We'll do a list of objects V2. Let's take a look at the transactions prefix.  And you see a bunch of objects returned.

The first thing that I want to draw your attention to here is storage class. When you access your data via the S3 access point for FSx, your ONTAP data appears as having an FSx ONTAP storage class.

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2110.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2110)

We can perform some additional read-only operations. Let's execute a head object command against the customers CSV file.  You'll see that just as you can put metadata on an object, you can also put metadata on an object in your FSx for NetApp ONTAP volume. Again, notice the storage class. Another important thing to call out here is the server-side encryption. You'll see a new mode that is AWS FSX. This represents the standard data encryption at rest that you get with FSx for NetApp ONTAP. All FSx for NetApp ONTAP file systems data is encrypted at rest using an AWS KMS key that is specified when you create your file system, and that's the server-side encryption that's happening here.

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2160.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2160)

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2170.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2170)

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2180.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2180)

[![Thumbnail 2200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2200.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2200)

Let's download this object.  We've just fetched that object and we can look at the contents of it.  I apologize, that's hard to read.  What I've done here is on the left pane we're looking at the contents of the downloaded file, that customers CSV, and on the right this is our NFS mount showing the same contents of the data. To this point we've just shown read-only  operations, but it's not just read-only. You can also write data. Let's put some data as well.

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2220.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2220)

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2230.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2230)

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2270.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2270)

Here we're going to upload some more data to go along with this dataset. This is customer rewards, and you can see that just succeeded. If  we go back to the NFS mount, we can see the contents where we have our new  files that we just uploaded. Now remember when you create an access point, we assigned it a file system identity, right? That is what is used to authorize the file system operations and it also represents ownership. If we stat the files that we just uploaded, you'll see  the ownership. The owner of this file is the BISIS account. This is the Unix user that I had associated with the access point, one of the ones that I created, again demonstrating that this was the file that I had uploaded via the access point and it has that ownership.

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2310.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2310)

[![Thumbnail 2340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2340.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2340)

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2350.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2350)

[![Thumbnail 2360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2360.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2360)

Okay, so let's do something a bit more exciting. This has just been getting started with basic data access. Let's actually connect to this data with an AWS analytics service. As a data scientist  for this financial firm, we want to do some analysis. I want to be able to query this data in place using a SQL-based query editor, and to do so I'm going to use Amazon Athena. In Athena, we're going to create tables that represent the underlying data, so we'll have a table for customers data, for cards, and so on. When you create a table in Athena,  Athena is querying against the S3 endpoint. We'll create one for customers.  You can see there's the new customers table, its properties, and the location is  the S3 URI that includes the alias and the specific prefix for our customer data.

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2370.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2370)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2380.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2380)

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2390.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2390)

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2400.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2400)

I'll quickly create the rest of the tables   here. Remember, the transactions data is partitioned. We partitioned it by year, month, and day.  We'll see what that looks like.  That just discovered all the partitions.

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2420.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2420)

[![Thumbnail 2440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2440.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2440)

Create the fraud alerts table. Lastly, the credit cards table, rewards table. Now, we're doing this mainly manually using Athena. We don't have to do that.  We can use AWS Glue to automatically crawl and discover this data and schema. So if we go back again, transactions is partitioned, we can view this table in Glue. And you see here again this is the schema representation for the transactions table  pointing at an S3 access point alias that's at a specific prefix. Now remember this data lives in an ONTAP file system. It could even be the primary copy of it could even be on-premises, and this could just be a read-only secondary copy for disaster recovery.

[![Thumbnail 2470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2470.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2470)

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2480.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2480)

Schema and partitions and again, beyond the scope of this demonstration, we could have created a Glue crawler to do this automatically.  Let's jump back to our Athena query editor. So we now have our tables. Let's look at some data.  I'm just going to do a basic select of the first ten customers. Again, this should look familiar. We just looked at this data after we downloaded it via NFS.

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2490.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2490)

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2510.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2510)

 As a data scientist, I want to do a customer segmentation analysis now. Here we're doing a little bit more of an advanced select that is joining two tables, and you can see it is producing results.  Now, we're still doing read-only, but you can also create tables from selected results. This is called CTAS. You would potentially want to do this if you were doing some end of day, end of week, end of quarter, end of year reporting. You wanted to populate dashboards and you wanted those query results to be quick.

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2550.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2550)

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2570.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2570)

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2590.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2590)

So one thing that you could do is do a select and actually create a table from those results and we'll show that right now. Here  we're going to create a summaries table, customer summary table. And there are a few things different about this one. Let me just fire this off. So this is going to do a select against the existing tables, and it's going to create a new table  from the results and we're writing to a different alias at a different prefix within the same volume. Not only is it a different location, but we're also going to do it in a different format here. We're writing Parquet files. Previously we'd been reading CSV. Remember,  data stored in ONTAP.

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2600.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2600)

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2620.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2620)

Let's check it out. You can see here now on the left, we have the credit card  data analytics customer summary, and you have the Parquet files returned from that table that was just created. Now I can query this table and see some results. And there is the customer summary table.  This has been the traditional way that a data scientist would do some basic analytics.

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2650.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2650)

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2660.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2660)

### AI-Assisted Analysis with Amazon QuickSight Q and Key Takeaways

Now, recently, AWS announced Amazon QuickSight Q. QuickSight Q is the AI-assisted business intelligence platform, and using QuickSight Q you can integrate your enterprise data as knowledge bases and do similar analysis but AI-assisted. Let's take a look at that.  The next part of this demonstration will be through QuickSight Q. So step one of QuickSight Q, we have  all of our enterprise data. We want to be able to bring it, make it accessible so we can automate workflows, so we can do deep research and so we can do some analysis.

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2680.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2680)

Step one in QuickSight Q is creating an integration, creating a knowledge base, and I have one already. This is called the credit card  dataset BI knowledge base. Configured the same way you would configure a knowledge base in QuickSight or QuickSight Q against an S3 bucket. The way that this works is QuickSight Q, once you've created your knowledge base, will periodically sync and index your data and then make that available to chat assistants, so I have this knowledge base that is available, it's been synced.

[![Thumbnail 2710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2710.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2710)

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2720.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2720)

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2730.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2730)

[![Thumbnail 2740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2740.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2740)

 And let's go to chat agents. So here we have a Data Science Analyst.  What we're going to do and using this we can do similar  analysis to gain insights from the same set of data. I'll show you a few examples. So here is  a previous report or analysis where I said analyze customer spending patterns in the credit card data set and returned some results can create tables, bar charts.

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2760.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2760)

Let's start a new conversation. So we're going to fire off our prompt.  What the chat agent's going to start to do is it's going to do some thinking, some reasoning. It's searching through the documents. You can see here it's reading the cards CSV again, that data stored in your ONTAP file system. It's going to do some more analysis, some more number crunching, some more searching. This goes on for about a minute or so while this is running.

[![Thumbnail 2790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2790.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2790)

I want to take a step back and emphasize here what this enables.  Customers have massive amounts of data stored in ONTAP, that enterprise data that really represents their institutional memory and their business context. Now they can easily bring that data to FSx and put that data to work with these tools in a way that they had not been able to do before. If you wanted to do this before, what you'd have to do is copy that data into S3, pay for two copies, different formats, rearchitect, manage it all so that it's synchronized. All this can be done seamlessly through ONTAP's data replication technologies like SnapMirror.

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2850.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2850)

[![Thumbnail 2880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2880.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2880)

Here it's reading some more data again in the knowledge base. What QuickSight is doing here, it has its knowledge base that's pointing to the S3 location  that is the alias or the access point. That access point is attached to my volume and QuickSight is performing the S3 API operations to read the data. So it's read the merchants data, the customer CSV, customer rewards, now it's analyzing our query. It's running a numerical analysis.  Putting the pieces together. Again, this takes about one or two minutes.

[![Thumbnail 2900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2900.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2900)

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2910.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2910)

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2920.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2920)

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2940.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2940)

And here we go. We have some results. Based on my comprehensive analysis of the  credit card data set, I've identified several key customer spending patterns and behavioral insights. It's done a customer segmentation analysis.  It's done a merchant category spend analysis. It's done some transaction characteristics all against that data stored in my ONTAP file system.  Created some tables, some bar charts, all of this can be downloaded, exported, used in however I'm going to present this data to my team to drive better business outcomes. It's even recommending some actions. 

[![Thumbnail 2990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/2990.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=2990)

So in summary, we started off creating an access point. We used that access point and its alias to do some basic data access S3 API operations to read and write and demonstrated how that plays with your NFS, your existing file-based access. Next we showed integrating with Amazon Athena to query data in place, which is more the traditional way, and we ended with QuickSight demonstrating an AI assisted analysis for a similar exercise. Let me switch back over here. 

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/01fd8a9bbe84ca8c/3030.jpg)](https://www.youtube.com/watch?v=NVZV0gfV-jA&t=3030)

So to wrap up, I want to cover a few big takeaways. First, S3 access points for FSx ONTAP is the easiest way to do more with your ONTAP data. Your data continues to reside in your ONTAP file systems and accessible via file protocols. No rearchitecting, no data movement, no taking that data out of the file system. And now for the first time that data is accessible via Amazon S3 using the S3 API and a wide range of AI, ML, and analytic services that work with S3.  Lastly, thank you all for attending this session and making time to.


----

; This article is entirely auto-generated using Amazon Bedrock.
