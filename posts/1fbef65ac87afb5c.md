---
title: 'AWS re:Invent 2025 - Simplify backup for stateful Amazon EKS workloads (CNS209)'
published: true
description: 'In this video, Santosh Vallurupalli, Senior Solutions Architect at AWS, presents how AWS Backup simplifies data protection for stateful workloads in Amazon EKS clusters. He introduces AWS Backup as a fully managed, policy-driven service supporting 20+ AWS services and hybrid workloads. The session highlights the recent launch of native EKS support, enabling backup and restore of Kubernetes objects and persistent volumes on EBS, S3, and EFS without third-party tools. Key use cases include compliance monitoring with AWS Backup Audit Manager, disaster recovery, and ransomware resiliency through logically air-gapped vaults with immutable backups. The presentation demonstrates the two-step setup process and concludes with a reference architecture featuring Backup Delegated Admin Accounts, Key Vault Accounts, data bunker accounts for regional resiliency, and forensics accounts for testing backup workflows.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Simplify backup for stateful Amazon EKS workloads (CNS209)**

> In this video, Santosh Vallurupalli, Senior Solutions Architect at AWS, presents how AWS Backup simplifies data protection for stateful workloads in Amazon EKS clusters. He introduces AWS Backup as a fully managed, policy-driven service supporting 20+ AWS services and hybrid workloads. The session highlights the recent launch of native EKS support, enabling backup and restore of Kubernetes objects and persistent volumes on EBS, S3, and EFS without third-party tools. Key use cases include compliance monitoring with AWS Backup Audit Manager, disaster recovery, and ransomware resiliency through logically air-gapped vaults with immutable backups. The presentation demonstrates the two-step setup process and concludes with a reference architecture featuring Backup Delegated Admin Accounts, Key Vault Accounts, data bunker accounts for regional resiliency, and forensics accounts for testing backup workflows.

{% youtube https://www.youtube.com/watch?v=kgmgtiOYrS0 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/0.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=0)

### Introduction: Simplifying Backups for Stateful Workloads in Amazon EKS

 All right, good afternoon, everybody. Thank you for joining me in today's session. First, I know we're towards the tail end of re:Invent 2025. It's already Thursday today, and we're also close to lunchtime. So I really appreciate all of you taking time to be with me in today's session.

Before we get started, just a quick show of hands. How many of you are already using AWS Backup for your data protection requirements today? Good, good. Not yet? Okay, good. How many of you are using Amazon EKS today? Oh, that's a lot of fans. Okay, good. Sounds good.

[![Thumbnail 60](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/60.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=60)

All right, so welcome everybody to today's session. I am Santosh Vallurupalli, Senior Solutions Architect with AWS, and in today's session we're going to be talking about simplifying the backups of your stateful workloads in Amazon EKS clusters. Here's a quick agenda. We'll start off by talking about what AWS Backup is,  and then I'll give you an introduction of what's changed with that service, what we've done recently with AWS Backup, and then we'll look at how easy and simple it is to get started with AWS Backup. Finally, we'll conclude the session with an example reference architecture that gives you real-world insights into how you deploy backups and how you use all the best practices during deployment times. Let's get rolling.

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/90.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=90)

What is AWS Backup? AWS Backup is a fully managed  service that centralizes and automates your data protection requirements across all of the supported AWS services and also across your hybrid workloads, as you see on the far right side corner of the screen there. So it supports both your AWS services and your hybrid workloads. Really important to keep that in mind.

It is a simple policy-driven service that helps you work backwards from what your data protection requirements are. These requirements could be things such as what resources do you want to back up, where do you want to back up, how many copies of backups would you want to have, what are the data lifecycle policies associated with those backups, and so on and so forth. After you've identified those requirements, you could just define them as policies within AWS Backup, and we do the undifferentiated heavy lifting on your behalf.

AWS Backup also provides you with the ability of simplifying the way you enforce data protection requirements across your organizations. You could be using either a decentralized approach in which you will have your application groups and application developers be responsible for creating their own local backup policies across the AWS accounts they operate within. That's a decentralized way of doing it. Or if you prefer a centralized approach, you could also use AWS Organizations. You can centrally define your backup policies there and then you can apply those policies to all of your member AWS accounts within your AWS Organizations, and those will help you in establishing a baseline path really quickly. So two flexible ways in how you enforce your data protection requirements.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/190.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=190)

### AWS Backup Use Cases: From Cloud-Native Solutions to Ransomware Resiliency

Let's talk about the AWS Backup use cases a little bit before we start jumping  into what's changed and what's launched in this space. First, AWS Backup is a cloud-native backup solution. We've just talked about it. It supports 20 plus different AWS services as of today. It is a simple policy-driven service, really easy and flexible to start off with.

Second, when we talk about compliance and governance, you can use AWS Backup in conjunction with AWS Backup Audit Manager, which is a feature of AWS Backup. When you use both of those together, it gives you insights to monitor, audit, and report all the different data protection policies that you've configured within your AWS accounts. It provides you with real-time analytics and insights into which of your data protection policies are compliant as per your organization's needs and which ones are not, so that you can take corrective actions from there.

Disaster recovery and ransomware resiliency are a couple more core primitives offered by AWS Backup. The goal of AWS Backup is to not only let you take backups of your data and your infrastructure, but also to provide you with the ability to copy your backups, move them into multiple different target AWS accounts and regions as you see fit, so that during the events of disaster recovery, it will be really easy for you to recover and restore from those backups with minimal human intervention.

AWS Backup also integrates with logically air-gapped vaults. Vaults are a storage place where all of your backups go and get stored into, and in logically air-gapped vaults, any backups that you store within them automatically become immutable in nature. What this means is you will not be able to edit, modify, or delete your backups. This applies even to your root users, your system users, your power users. Nobody will be able to edit, modify, or delete your backups that are sitting in logically air-gapped vaults. Especially during ransomware events, this is even more helpful because you have a copy that is immutable, that is sitting in logically air-gapped vaults,

which you can recover back and get your applications up and running in no time.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/320.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=320)

### Native Amazon EKS Support and Getting Started with AWS Backup

Now let's talk about what's new with AWS Backup. Last month at  re:Invent, we launched native support for Amazon EKS with AWS Backup. Customers can now use AWS Backup as a way of protecting their EKS clusters, as a way of backing up their EKS clusters. So what do I mean by protection? Using AWS Backup, you will now be able to back up and restore all kinds of Kubernetes objects. When I say Kubernetes objects, these are cluster-scoped resources and namespace-scoped resources, both of them. For example, these could be things such as your deployments, your config maps, your secrets, your cluster roles, cluster binding roles, your storage classes, and so on and so forth.

We now give you the ability of backing up all of your Kubernetes objects and then also providing you with the ability of restoring them as you need it. In addition to the cluster objects, we also support backing up of your stateful workloads, so all of your stateful workloads and their corresponding persistent volumes that are running on EBS, S3, and EFS. We provide native ways for you to actually back those up and then also restore them as needed. So this is a really powerful feature. Now you have the ability of backing up your clusters, the objects, and the persistent volumes alongside with that.

Let's actually talk a little bit more about what this feature means for day-to-day operations with EKS. First, it's important to keep in mind that this is totally a cloud-native service, meaning you don't have to install any third-party add-ons or custom scripts or any third-party utilities to make it work. All you have to do is just jump into the AWS Backup console, select EKS from the dropdown, and then select the clusters of your choice and get started. It's as simple as that.

Second, it is really important for platform engineers and cluster admins to be able to back up your EKS clusters before the beginning of any maintenance windows. Things such as when you want to perform EKS version upgrades or when you want to perform API upgrades or your AMI rollouts or security rollouts, you will now have the ability of using AWS Backup to create a consistent global golden backup or a golden snapshot before the beginning of your maintenance window. If you run into any issues or if you run into any unexpected circumstances during the maintenance window, you now have the ability of restoring objects back from that backup and get your cluster into a running state as to how it was before the beginning of the maintenance window.

So this is a really powerful feature. We're excited with a lot of feedback that we're getting from customers in this space. We're also providing customers with a lot of options at the time of restoration to make it a really smooth and comfortable experience for you. As an example, at the time of restoring your EKS backups, you could either choose to restore onto an existing EKS cluster that's already up and running in your AWS account. That's one way to do it. Or you could also choose to have AWS Backup create a brand new skeleton cluster first as a prerequisite step and then run the restore workflow on top of that.

You can also have AWS Backup create brand new EKS clusters and then restore on top of that as a way of restoring back your namespaces and objects. Also, during the time of restoration, we are providing customers with the ability to choose how granular they want the resources to be. You can either select to run a full cluster restore, meaning all of your cluster objects and everything from the source cluster will be restored as it is on the destination target cluster. That's one way to do it. Or you can also get granular and only select certain namespaces that you would want to restore as part of your restoration mechanism. So you have two different choices, two different models to experiment and play with.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/550.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=550)

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/560.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=560)

 It is really easy, quick, and simple to get started with AWS Backup. It's just a two-step process.  The first step of the process is you would want to create a backup plan. As we discussed earlier, a backup plan is where you would define what to back up, how many copies of backups you would want, and the lifecycle policies associated with backups. So the first step is to actually create a backup plan, and there are three ways in which you can create a backup plan.

You could either start off by using a pre-existing AWS-provided template, which covers a lot of common customer scenarios to begin with. That's one way to do it. Or if you prefer to have more customizations or if you want the flexibility of customizations and add your own constructs, you would use the second option, which is basically to build a new backup plan of your own. Clicking on the second option will yield you all the options that you're seeing on the far right side.

It gives you a lot more control and a lot more customization capabilities. Alternatively, there is also another way in which you can define all of your requirements ahead of time within a JSON file, and then you can import that into AWS Backup. Regardless of which option you choose, we evaluate and run through all of your backup plans created using these three approaches. That's step number one.

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/640.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=640)

Step number two is after you've created a backup plan, which defines when, how, what, and who.  This is the section where you would assign resources to your backup plans. For example, all the resources that you want to protect using AWS Backup would be assigned in this section. There are two ways to do this again. You could either instruct, as you see in option number one, AWS Backup to pick up on all the supported services and all the supported resources as a way of doing it. Or you can also be granular and select only the resources that you want to back up as part of that. It's a little bit manual intensive because you have to browse through it, figure out, click on drop-downs, and select the resources that you want to back up, and so on and so forth.

That is where the second option comes into the picture to make it a much more seamless experience. As part of the second approach, you can just inject your existing AWS Backup plans with a certain set of tag keys and tag values, and AWS Backup will essentially look up all the supported resources that have those specific tag keys and values and add them as the scope of the backup plan. This will help you in seamlessly importing all the resources with supported tags, and these resources will now automatically start to be within the scope of your backup plans. So there are two different ways of doing it.

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/740.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=740)

### Reference Architecture: Building a Comprehensive Backup Strategy with Data Bunker Accounts

So far we've talked about what AWS Backup is. We've talked about the recent feature launch as to how EKS now natively integrates with AWS Backup. We've also talked about disaster recovery, resilience, and ransomware resiliency early on as core principles of AWS Backup. Let's actually visualize how  all of these different pieces come together with the help of a reference architecture. This is a reference architecture, so feel free to use the whole building components as is, or feel free to use bits and pieces of the architecture that match your business case requirements.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/760.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=760)

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/780.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=780)

Firstly, you'll start off with this architecture,  and as you see on the very top side, you have AWS Identity Center and you have AWS MFA. These are both the mechanisms through which your backup admins will gain access to your AWS accounts and your AWS Organizations. After your backup admins gain access to your organizations, the pink boundary that you see around is the AWS Organization.  It is recommended to have two designated accounts. The first one is called the Backup Delegated Admin Account. This is where your backup administrators will log in. This is where we want backup administrators to centrally create your backup policies and then share those policies out onto all of your workload accounts that you see on the left side here.

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/800.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=800)

 The reason why we recommend going with this approach is that in the absence of a Backup Delegated Admin Account, you would have to otherwise have your admins gain access to all of your workload accounts that are sitting within your AWS Organizations. These could be dozens, hundreds, or sometimes even thousands. Provisioning access thousands of times for your backup admins and then having backup admins go ahead and create backup plans locally within those accounts will quickly start to become a very tedious process. To streamline this is why we recommend starting off with having a Delegated Backup Admin Account where you would centrally create your backup policies and share them out onto all of your workload accounts.

Similarly, we also have Key Vault Accounts. The reason for having this account is we all would love for our backups to be encrypted, correct? The way we would encrypt that is by using CMKs, Customer Managed Keys, and we all understand by now that CMKs have a lot of lifecycle events associated with them, such as rotating them, renewing them, and when you want to delete keys, you have to wait for seven days before you delete them. There are a lot of maintenance events and lifecycle events that go through with CMKs in general. Creating all of the blast radius within one account will help you in streamlining all the processes that are involved with creating, managing, and updating your keys. It helps you keep ahead of all of your maintenance activities and also helps you keep a centralized visualizing pane.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/890.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=890)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/900.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=900)

 Within the workload accounts that you see here, I have an EKS cluster and I have three different persistent volumes that are running on  EBS, S3, and EFS, all of which are now going to be natively supported by AWS Backup.

Since today we're only talking about the EKS feature launch, that is the reason why I have EKS up there, but you can swap that with any of the 20+ AWS services, and the reference architecture will still remain the same.

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/920.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=920)

 The backup policies that were created in the delegated admin account will be responsible for triggering the first backup copy of your EKS workloads that are sitting in your workload account. That's the first copy that is local, which you see out there. AWS Backup also has native integrations with Amazon GuardDuty, and essentially GuardDuty has the capability of running real-time scanning and analysis of all of your backups to detect if there was any malware or if there are any things that require further attention from your side. So that is what you see as part of the forensic and recovery validation of this whole backup architecture.

We also have data bunker accounts. The construct of having data bunker accounts is to have logically isolated AWS accounts that are not talking with each other. You would not host any kind of workloads on those data bunker accounts, and they're strictly used for storing your second and third data backup copies. That is what the bunker accounts are for. You also wouldn't have regular access onto those accounts because you want your backups to be as strictly regulated and as closely guarded as possible, and the way you would get into those data bunker accounts is through break glass mechanisms.

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/1000.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=1000)

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/1010.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=1010)

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/1020.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=1020)

So essentially, the intention of having that account in this architecture is whenever your primary account is compromised or if it is taken over, you  would want to have another set of backups that's running in a different account, and that different account is going to be a data bunker account in this scenario, which is going to host your second data backup copy.  Similarly, it's also recommended for you to have another data bunker account with backups running in another region, which is different than where your source workloads  and the prior data bunker account is backing your data up at. The intention for having this second account is to provide you with regional resiliency.

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/1050.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=1050)

So in case the primary region where your workloads and your data bunker one account are down, if that region is inaccessible, you would still want to recover back from those scenarios, and that's when you'll use the third copy of data that's sitting in another region in a data bunker account as a way of recovering back from it. Finally, we also have a forensics account. This is the account where we recommend you  to test all of your data backup workflows. We also recommend for you to periodically restore back your workloads and run all of your forensic evaluations and analysis against them.

You'd also be able to use this account during the times of any doubts or any cyber activities to just ensure that all of your backups are properly running, and if there is any malicious content, you would be able to scan those and identify those and replay events appropriately in that forensics account. And again, this is a reference architecture. Feel free to use what makes sense out of this or the whole entirety of this reference architecture.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/1100.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=1100)

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1fbef65ac87afb5c/1110.jpg)](https://www.youtube.com/watch?v=kgmgtiOYrS0&t=1110)

Finally, if you want to get started with AWS Backup, if you want to get started with Amazon EKS support for AWS Backup, here  are some resources. There's also a QR code you can scan, which will land on some of those training materials as well. That is all for today's session. Thank you very much for your time.  Let me know if you have any questions. I'll be here, and please take a moment to rate the session as well. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
