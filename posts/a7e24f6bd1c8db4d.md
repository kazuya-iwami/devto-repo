---
title: 'AWS re:Invent 2025 - What''s new with Amazon S3 (STG206)'
published: true
description: 'In this video, Paul Meighan and Anna Bouchet from the Amazon S3 team present 35 launches from the past year, covering security enhancements like tag-based access controls and organization-level controls, durability features including data integrity checks, S3 Express updates with 85% price cuts and 2M TPS limits, conditional writes expansion to copy and delete operations, 50TB maximum object size increase, batch operations improvements with 10x performance boost, S3 Tables advancements including table replication and Intelligent-Tiering support, S3 Metadata with journal and live inventory tables, and the new S3 Vectors service that reduces vector storage costs by up to 90% for AI workloads. The session includes a live demo using Amazon Transcribe, S3 Metadata, SageMaker Unified Studio, and Bedrock knowledge base with S3 Vectors to semantically search through uploaded files.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - What's new with Amazon S3 (STG206)**

> In this video, Paul Meighan and Anna Bouchet from the Amazon S3 team present 35 launches from the past year, covering security enhancements like tag-based access controls and organization-level controls, durability features including data integrity checks, S3 Express updates with 85% price cuts and 2M TPS limits, conditional writes expansion to copy and delete operations, 50TB maximum object size increase, batch operations improvements with 10x performance boost, S3 Tables advancements including table replication and Intelligent-Tiering support, S3 Metadata with journal and live inventory tables, and the new S3 Vectors service that reduces vector storage costs by up to 90% for AI workloads. The session includes a live demo using Amazon Transcribe, S3 Metadata, SageMaker Unified Studio, and Bedrock knowledge base with S3 Vectors to semantically search through uploaded files.

{% youtube https://www.youtube.com/watch?v=Sy2LHRyMXAo %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/0.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=0)

### Introduction: A Year of S3 Innovation with a Live Demo

 Hello friends. My name is Paul Meighan, here with my friend Anna Bouchet, and we work on Amazon S3. Our job today is to bring you up to speed on everything that the S3 team has been working on over the course of the last 12 months since we were all here at re:Invent together last year. Now I've been doing this session for a number of years now, and I think this is my last year doing it. Every year I threaten to do a demo, and every year the marketing team talks me out of it, but not this year. This year we have come up with the most complicated demo we could think of, and we brought in our best engineer to help run it, and it's going to go great.

That's the plan. In fact, it's so complicated I need a few volunteers from the audience to participate, and we're going to take a video as I gather some names of our volunteers. I need two, and all you have to do is yell out a name or yell out a fake name. It works either way. So who is open to volunteering for our demo today? Yes, Matt is our first volunteer. Another one over here. What? Indra Neil. Alright, Matt and Indra Neil are two volunteers in our re:Invent session today. Anna, do we have that? Alright.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/80.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=80)

 So we're going to cover 35 launches over the course of the next 60 minutes together. I have six topics to take you through really fast. We're going to do security, durability, we're going to talk about S3 Express, which is our high-performance storage, and dive deep into S3 Objects and the S3 API, and then round it out with S3 Metadata, S3 Tables, and S3 Vectors. That's the plan. Now every year we key this course in as a 200-level session, and the reason that we do that is because we just cover so much ground here in the what's new. We just don't have time to dive deep into every single feature. But Anna and I will stay after the session in the hall for as long as it takes to answer any questions that you have. We're committed to giving you the detail that you need to take some of this home with you and put it to work next week.

We've also tried to build as many links to source material as we can throughout the deck, and so you can get that once it posts to YouTube after the show. So that's the plan, and the clicker is working now, so let's get started.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/140.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=140)

### Security Enhancements: Tag-Based Access Controls, Organization-Level Features, and Encryption Updates

 So we're going to start with security. Security is the most important thing we do at AWS, and it always will be. So it's fitting that we're going to start with the new security content this year at the top of the session. We're going to talk about access controls, organization-level controls, and encryption here over the course of the next few minutes.

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/160.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=160)

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/170.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=170)

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/180.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=180)

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/190.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=190)

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/210.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=210)

 Now when you're managing access to S3 and you just have a single bucket and a single application or client, it's pretty straightforward once you kind of get the hang of IAM, right?  But as you add more and more clients and more and more buckets to the mix, things get a little bit more complicated.  You want to maintain least privileged permissions to each one of these buckets, and so you have to tailor sections of the policy for all the clients that need to come in and access your data.  And since every bucket likely has different clients coming in to access different pieces of data, you really want to tailor each policy to each bucket. What you end up with is a whole bunch of different policies that you have to juggle and manage across many buckets, which is complex and becomes even harder when you have to manage changes over time.  Because you have to now start swapping in policies, and management becomes more and more difficult.

[![Thumbnail 220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/220.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=220)

 And so to simplify this, a couple of weeks ago we launched tag-based access controls for S3. It allows you to control permissions to your S3 resources using simple policies in conjunction with resource tags, and it simplifies things by quite a lot. Here's how it works.

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/230.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=230)

[![Thumbnail 250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/250.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=250)

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/260.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=260)

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/270.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=270)

 So here's an example of the sort of policy fragment that you can now use on the client side, right? And you can see that this policy is very simple. It just says give access to this principal to any resources that is tagged with a blue tag, very simple fragment, much simpler than what we saw on the previous slides.  And then to grant access, all I need to do is tag my buckets with the appropriate tag values, and access is granted.  If I want to make changes over time, I can just add a tag to grant access and remove a tag to remove access, right?  So it's much simpler, much easier to reason about than juggling policies across my resources.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/280.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=280)

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/290.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=290)

 We've also given you some new APIs to help manage this that are also better than what came before. We used to have a bucket tagging API in the current namespace, but we added some new APIs that look much more similar to what you're used to across AWS.  So we have an untag resource API now that can remove a single tag off a bucket. We have a tag resource API which can add a single tag onto a bucket. And what's different from these new tagging APIs compared to what came before is that single tag operations. Previously, you had to replace the entire tag set even if you only wanted to change one tag on the resource.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/320.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=320)

So this is much safer and easier to use. Additionally, we have a list on my tags or list tags for resource API as well that spits out this handy dandy  JSON list showing all the tags that you have on a given bucket. So that's tag-based access controls. We think it simplifies things quite a lot.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/350.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=350)

We went through general purpose buckets here in this example, but this works for all S3 bucket types, directory buckets, access points, table buckets, tables, vector buckets, vector indexes, which we'll talk about here in a few minutes. So that's what we have on access controls. Now, every year we come in and talk about these useful  features on the security side that we'd like you all to turn on and use. We always recommend that you turn features like Block Public Access on at the account level so that any new bucket you create in that account automatically has public access blocked by default.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/370.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=370)

But more and more customers are running AWS Organizations, and so  what we've done is we've leveled these two features up to the org level now. So now you can apply Block Public Access out at the org or OU level. And this 403 context feature provides additional context and information about your access denied errors. It used to only work when a request started and ended within an AWS account. That's also stretched out to an entire org now, so you get those improved error messages for any request within an org. So you're going to see more of this from us over time, adding more and more org level content. We're happy to get these two org level controls out to you here just over the last few weeks. So that's our org level controls content.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/420.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=420)

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/440.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=440)

And just want to say a few things about encryption. Now one thing that is true about encryption is that it's constantly evolving.  Here you see all the encryption types that we've launched over the years satisfying different use cases, and there are different reasons why you would want to encrypt stuff in different ways. And we added another encryption type just about a week ago, and that is PQTLS, which provides  post quantum encryption for TLS, for data in flight. It works across all S3 endpoints. If your client supports a PQTLS method, it'll automatically take that path, automatically encrypt in that way. And while quantum computing isn't here yet, this protects against that capture now and decrypt later threat vector and just gets the quantum computing threat sort of out of the way moving forward. So PQTLS is the latest addition to the S3 encryption portfolio.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/480.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=480)

Now in the security space over the last few years, one thing that we've done  is we've updated the defaults. We did it with Block Public Access, which is now default. We did it with access control lists, which are now off by default. We did it with encryption at rest. And our thinking on this is that we wanted to update the defaults with best practice recommendations so that you start from a place of best practices. It's really important for security in particular, right? And the way that we think about it is for those features that have smaller, more niche use cases, we'd like to have those off by default. They're opt-in so that you don't have to think about them if you don't need them, which most of you don't.

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/530.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=530)

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/560.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=560)

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/570.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=570)

Now SSE-C, which is an encryption type that allows you to pass in an encryption key generated by you outside of KMS, is one of those features. And so a change that we made here in November is to give you the ability  to just turn this option off, so you can update your bucket configuration to not accept SSE-C objects regardless of what the client says. This is an optional tag that you can flag now, an optional property that you can turn on now. But in 2026, we'll update the default so any new bucket created will have SSE-C off by default. So  some updates to encryption, a big update to access controls with tag-based access controls, and then some new org level controls. That's the update that I have for you on security this year from 2025,  so that out of the way.

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/590.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=590)

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/600.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=600)

### Demo Part 1: Demonstrating Enhanced Error Messaging and Tag-Based Access Controls

Let's check in on our demo, Anna. Alright, hello everyone. As Paul mentioned, we have a very complicated demo ahead of us. Many things can go wrong, hopefully none of them will. And to kick it off, we took a video. So as Paul was talking about the three security  improvements, I used Amazon Transcribe to create speech to text representation of this video. We captured the names of our volunteers  here as well, Matt and Indrani. Here's what I'm going to do next.

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/610.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=610)

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/620.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=620)

I also downloaded about 700  customer stories from the AWS public website. I will rename all these files to have unique names so that we can no longer determine which  one has the transcript of the video. I will upload these files to an S3 bucket which I created ahead of time, and then I will tag with color tags some of these files at random. I will be using shades of purple for my color tags. Alright, I have a script. I'll start with the script.

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/640.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=640)

 Oh no, no. Is it a familiar situation? Access denied, so frustrating, but no worries, no worries. This is 100% a staged failure to show how easy it is to debug access denied issues with enhanced error messaging. So what my script is doing is trying to list my bucket to confirm that it is empty. But it got access denied, and from this message I can see the actor, I can see the permission, and I can see the resource that is being accessed. I can also see that access was denied explicitly via bucket policy.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/690.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=690)

Let's quickly hop to the S3 console. So in my bucket policy,  I have denied for any principal for this bucket action if my principal environment tag does not match my bucket environment tag. Policies like this can be very suitable if you want to achieve highly dynamic access controls, because in policies like this you can just manipulate the bucket tag to make this deny go away. One thing I forgot to mention, if my principal doesn't have an environment tag like all the principals in my presentation, the default value of reinvent-2025 will be used in its place.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/730.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=730)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/740.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=740)

So let's give it a try. I navigate to the Properties console. I'm adding a tag.  It is an environment tag with the value reinvent-2025. I'm saving changes and I'm also making sure that my  ABAC configuration is in the enabled state, as this is what enacts my tag. Now I can go back to my script and restart it.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/750.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=750)

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/760.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=760)

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/770.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=770)

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/780.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=780)

 Oh no, it is not empty. Let's go back to my bucket and let's empty it first to make sure it is indeed empty.  Alright, going back. Third time is a charm, yes.  Okay, it will take us a moment to upload the 700 objects, and we will check on this upload in a second.  Alright, thank you. That was the pick a card, any card part of the demo.

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/800.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=800)

### Durability: Achieving 11 Nines and New Data Integrity Checks

Now we're going to talk about durability. So S3 is famously 11 nines durable, and my plan was to get up here and sort of dive deep into how we achieve that, but I just don't have time to do that today. So if you click on  one link in the deck, I would click on this one. It links to a YouTube video with one of our senior engineers really diving into how we achieve 11 nines durability. It's a really interesting topic, and I encourage customers to really understand it because it just helps to build against S3.

Just in short, there are really three things that we do to get you 11 nines of durability. The first thing is that we do end-to-end integrity checking to make sure that bits stay good as they flow through the system from our front door down to storage. The second thing is we always store data redundantly on multiple storage devices so that we can tolerate the failure of multiple devices with no impact to the integrity of your data. And the third thing that we do is that we sweep back through and audit the durability of your data over time to make sure that your data stays good and is not corrupted by a bit flip or something like that.

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/870.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/880.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=880)

So at a high level, those are the three big things that are going on within S3, and we're executing billions and billions of checksums every single second in order to achieve these three things. This third one, the durability audit, is something that we've heard about from customers quite a lot, especially customers that run  video archives or other sorts of high-value digital archives. These customers often have  very important digital assets. They need to go back and validate that they're good over time.

And so for that use case, as well as just a general capability to give you a general capability to validate that we have what you think we have, we launched some new data integrity checks. What this feature is, is it allows you to efficiently run checksums on data at rest in S3 to validate that in fact your data is as expected.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/910.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=910)

You run this out of our Batch Operations system. You just submit a job,  submit a list of objects to go check, and we'll quickly process that and give you back a report with all the checksums of the objects in that manifest. Now this is far more efficient than you can achieve on your own because you don't have to spin up any compute to process checksums. You just give us a job. It doesn't affect the tiers if you're running Intelligent Tiering, so cold data will stay cold to save you on storage costs, and it works with Glacier objects as well without you having to go orchestrate and manage Glacier restores back to S3. So it's a great feature for validating the durability of your data within S3.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/950.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=950)

### S3 Express Updates: Performance Improvements and Cost Reductions

 The next thing I want to talk about is S3 Express. So the way to think about S3 Express is it's sort of a stripped down, lean, mean version of S3 that is just built for low latency. It's able to achieve single digit millisecond requests very consistently. It's typically used for high throughput use cases. There are some very big training workloads on S3 Express right now, but customers are coming up with all kinds of interesting ways to leverage low latency S3. I've heard a bunch of interesting examples of that here at the show this year.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/990.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=990)

We updated S3 Express in a number of ways over the course of the year. We did a price cut, reducing  the cost of S3 Express by up to 85%. We raised the TPS limit, or the transactions per second limit, up to 2 million reads per second and 200,000 writes per second. We introduced an object rename API in S3 Express buckets so that you can atomically rename individual objects. And we added access point support to give you more granular control over permissions to S3 Express. So there are a bunch of interesting, important updates to Express here over the course of the last year, and we're very excited about the trajectory of this service heading into next year.

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1030.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1030)

[![Thumbnail 1040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1040.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1040)

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1050.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1050)

### S3 Objects and API: Conditional Operations, Larger Object Limits, and Batch Operations

 Now, the next thing that I want to cover for you is S3 objects. So we're going to talk a little bit just about the S3 API and some things that we're doing in order to help you manage  lots and lots of objects across your buckets. Now last year we launched conditional writes. This is sort of a long time coming,  but what conditional writes are is it helps you in multi-writer scenarios to make sure that one writer doesn't accidentally overwrite the work of another, and it comes in two flavors.

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1070.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1070)

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1080.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1080)

You can do a put if absent using the if absent condition.  This is where you want to write an object and you want to make sure you don't overwrite an object if it turns out there's an object there to overwrite. And we also have a put if match condition where you pass in a checksum with your  put request and you will only overwrite an object if the object you're about to overwrite matches that checksum in the put request. This is also referred to as a compare and swap operation as well.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1120.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1120)

So we put this out last year and the response from developers was just amazing. So many of you took these new APIs and built on them. It's one of our fastest adopted features in recent memory for sure. We're actually doing millions and millions of conditional puts today in just a short amount of time. And so we decided to kind of double down on conditional writes and added two more. First, we added conditional copy.  This works the same exact way with the same conditions as conditional put. It allows you to lock down a bucket to make sure that there's no way to write any data into your bucket, either using put or copy, unless it has the appropriate conditions on it.

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1140.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1140)

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1170.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1170)

We also added conditional delete. Conditional delete works like  put if match in that you pass a checksum in with a delete request and the delete will only go through if in fact you're deleting what you think you're deleting, right, and the checksum has to match. This is for sure a durability best practice, something that we do internally when we're deleting data, sort of part of how we achieve that 11 nines story, and it's an example of us sort of externalizing the stuff that we are doing internally. So two new conditions that you can use with both copy and  delete.

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1180.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1180)

[![Thumbnail 1190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1190.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1190)

Now I also want to talk about big objects. So there are use cases out there, especially with  video and seismic and genomics that are generating really big objects, and so it was time for us to go ahead and lift the maximum object size, which we did yesterday.  So now you can store really big objects. The previous limit was 5 terabytes, now it's 50. There's not really anything else to tell you about this feature other than you can store really big objects right now. But this is a tough one for us to implement. Changing invariants in S3 is a lot of work, and so we're happy to get this out there, especially for those customers that need those huge objects.

[![Thumbnail 1220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1220.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1220)

Another thing that's been true in recent years is that you all are managing more and more objects in your buckets. It's not uncommon for customers  to have millions or even billions of objects in an S3 bucket.

[![Thumbnail 1240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1240.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1240)

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1260.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1260)

We really give you two methods for managing large groups of objects, and those are object tags and batch operations, which we talked a little bit about already. On object tags, what we did this year is we did a price cut back in the spring, reduced the cost of object tags by 35%.  So if economics were holding you back from using tags, you should take another look at the math on that. We also did a bunch of updates to batch operations. I'll talk through those in just a second, but just real quick for those of you who aren't familiar with batch operations or BOPs as we affectionately  call it internally.

Batch operations allows you to process many, many objects at scale, and it does this very reliably because we handle all of the retries and job management and all the undifferentiated work that you have to do in order to do anything a billion times over. It's also very flexible. It comes with 11 sort of prepackaged operations that you can execute against your data like adding and removing tags, copying data, adjusting object lock settings, and a bunch more. One of those operations is actually firing a Lambda function, which gives you a whole bunch of flexibility on top of those pre-canned things that you can do.

[![Thumbnail 1310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1310.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1310)

So we made a bunch of updates to BOPs in 2025. We added a no manifest option for batch operations, so  instead of passing in a list of objects to go process, you can just point a batch operations job at a bucket or a prefix, and we'll go process everything in that container. We also added IAM role creation automation as well, so you can more easily get a job going. We raised the scale of each job so you can now process jobs up to 20 billion objects, and we increased the performance of batch operations by up to 10x here just this week. So batch operations in 2025 got some nice updates. It's easier to use, more scalable, and faster than the last time we were all here together.

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1370.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1370)

So that is batch operations, and these are the updates that we had to objects. Just to take a very unnecessary side quest in our complicated demo, we're going to hand it to Anna to actually show what BOPs looks like. All right, so we ended up with uploading 744  objects, all successfully uploaded to my bucket, all of them renamed to some unique names so that we can no longer know which one is the transcript, and some of these objects are also tagged with color tag, which is just some random shade of purple.

[![Thumbnail 1390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1390.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1390)

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1410.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1410)

So now what if you want to assign a different color tag to this  object, and most importantly, what if you want to assign it in bulk? This is where our batch operations really shine. In a couple of clicks, no manifest, we can create a job just by picking our bucket, picking some match criteria for the object. For today's demo, we will be using 03.  That's because it's December 3rd today.

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1420.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1420)

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1430.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1430)

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1440.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1440)

[![Thumbnail 1450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1450.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1450)

[![Thumbnail 1460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1460.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1460)

And in a couple more clicks we can ask Paul also what color is his favorite. Black, black,  clearly. All right, black football. Now we picked to start the job  as soon as it is ready. We opt out of completion report as we don't need it for this demo, and as Paul mentioned, now we can also create a new role  that has all the necessary permissions to do tag reassignment for all the objects, and that's it. We submit this job. We will check  on its progress in a second. It will take probably a couple of seconds to complete, and we're jumping back to Paul. 

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1470.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1470)

[![Thumbnail 1490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1490.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1490)

### S3 Tables: Maturation of Fully Managed Apache Iceberg with Replication and Intelligent-Tiering

Thank you. So next I'm going to talk a little bit about S3 Tables. S3 Tables gives you fully managed  Apache Iceberg tables in S3. We launched at re:Invent last year, and the response from customers was just amazing. We got so much great feedback, so much interest, and it sounds cheesy, but the team really went home from re:Invent just inspired by all the interest that came from customers. And since then,  you all have created more than 400,000 tables in the system. We're very happy about that. It's proven to be useful for you, and we've done a lot of work to mature the service, covering a lot of ground in the first year.

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1510.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1510)

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1540.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1540)

There's actually more than I can cover on S3 Tables in this session that we've done this year.  It would just be the S3 Tables what's new, and we've got a lot of other stuff to cover, but I wanted to show this slide just in case there are folks here in the audience or watching that saw the launch last year and decided that they'd wait for the service to mature a bit before taking a second look. I just want to flag this for you because we've put a lot of development in over the course of the last 12 months. It's matured a lot, but I am going to go through some of my favorites here in the next few slides  just to give you a feel for the progress that we've made on S3 Tables.

We introduced an Iceberg REST catalog endpoint for S3 Tables buckets. This allows you to communicate directly with your tables and manage them using open source Iceberg APIs. If you have a very simple use case like with PyIceberg where you just want to write directly to S3 Tables and skip having to go through a third catalog system, you can do that by reading and writing directly from that REST endpoint. We raised the table limit up to 10,000 tables per table bucket. We added sort compaction, which rearranges the rows in a table on compaction to make your queries faster and more efficient. We did a compaction price cut, reducing the price by 90%, and we added some new options to the create table API so that you can set schema on create. So that's just a few of the smaller features that we delivered over the course of the last year.

[![Thumbnail 1600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1600.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1600)

 I'm going to take you through a few more, but I just want to take one minute to talk about this concept of tables as AWS resources. Now this is a characteristic that is unique to S3 Tables compared to the rest of the ways that you can run Iceberg. Every table in S3 Tables is a first class AWS resource, which means it has an ARN, and it paves the way. It opens up a lot of options for us in terms of the sorts of features that we can deliver to you because so many services and capabilities across AWS expect to operate on AWS resources.

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1640.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1640)

[![Thumbnail 1660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1660.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1660)

[![Thumbnail 1670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1670.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1670)

 Just a few examples this year are table level CloudWatch and CloudTrail. So you can get observability down to the table level. We throw events at the table level that tell you, CloudWatch events that tell you when we compact your data. Cost management tooling, so you now have tables in the cost and usage report and in cost explorer so that you  can understand which tables are driving your bill. KMS, we implemented table level encryption keys with KMS  so that you can assign a different KMS key to every single table in a table bucket. And these are examples of features that would be difficult if not impossible to implement when tables are logical entities just defined in metadata in a file in an S3 bucket.

[![Thumbnail 1700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1700.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1700)

[![Thumbnail 1730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1730.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1730)

A big feature that we had in mind when we launched S3 Tables and that relies on this concept of tables as AWS resources, and a feature that customers started asking us for immediately,  is table replication. We launched this yesterday and we're super excited to get this out there to you. This feature provides read-only replicas of Iceberg tables that are in S3 Tables. Now we've done replication for a long time. We actually replicate more than 150 petabytes a week, and so to be honest, I didn't think this was going to be a big deal for us to add table replication into the mix, but it is actually a different animal  replicating tables compared to just replicating objects, and I'm going to quickly take you through why here in a minute so you understand what this feature does.

[![Thumbnail 1760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1760.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1760)

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1780.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1780)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1790.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1790)

Very high level, when you run an Iceberg query, there's really two parts to it. The query goes down and gets the metadata, and the metadata tells the engine what data files it needs to go read in order to satisfy the query, right? And so naively if we were just to shift all of this over to the replica side,  the results would look something like this, right? Because that metadata is bit for bit the same as the source side and it still references those original data files, and this is obviously not going to work, right? And so it's not really metadata replication that we're doing, it's a rewrite. We're rewriting the metadata for you onto the replica side so that it's referencing local  data files so that your query can actually do what it's supposed to do, but we have to do more in order to make this a reliable service  because our replication is asynchronous. Not all objects are going to arrive at the same time, and when a query comes in, you don't want it referencing, you don't want the metadata referencing an object that's not there yet, right? That'll just crash and burn and give your end users a bad experience, right?

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1810.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1810)

The appropriate behavior is for that query to hit the most recent complete snapshot  so that query results can be successfully returned back to your app and then for the metadata to be appropriately updated once all of the data is over so that we're always giving you data that represents the most recent complete dataset available. So that's what this feature does. There's a catalog consistency element to it, and we're happy to get it out there. So that's one of our big launches here at re:Invent this year.

[![Thumbnail 1870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1870.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1870)

The other big launch that we have that we launched yesterday is a feature that personally I didn't really consider S3 Tables to be complete until we had it. It's a feature that's widely adopted on the general purpose side, a feature that customers have saved more than $6 billion on storage costs with over the course of the last several years since it was launched, and that feature is Intelligent-Tiering, which is now supported with S3 Tables. This gives you automatic cost optimization  for your data lake, reduces storage costs by up to 80%, and here's how that's achieved.

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1880.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1880)

So for those of you who are familiar with Intelligent-Tiering from general purpose buckets, this will look very  familiar to you. It's the same exact tiers, same exact cost reductions, same exact tier names as you have with general purpose. The way that it works is that we tier data down as it's not accessed over time. So if you don't access data for 30 days, we go down to the Infrequent Access tier, which reduces your storage cost by 40%. After 90 days, we reduce your storage costs by an additional 68% in the Archive Instant Access tier. This all happens automatically without you having to do anything at all. It's exactly the same as with general purpose buckets, the Intelligent-Tiering that you know.

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1920.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1920)

[![Thumbnail 1930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/1930.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=1930)

What's different  is that in tables, our compaction and table maintenance systems are aware of the underlying tiers, and this is a characteristic that's unique  to S3 Tables compared to the rest of the ways that you can run Apache Iceberg. We only do compaction in the Frequent Access tier. Our compaction systems are aware of the tiers, and this prevents us from accidentally tiering data up out of these cold tiers, and that reduces your storage costs and protects the cost savings that Intelligent-Tiering is delivering to you.

Additionally, as we're compacting in the Frequent Access tier, the reads that we need to do against your data in order to do that table maintenance that we're doing on your behalf don't count against that timer that I talked about on the previous slide. So if we have to compact a few times over the course of two weeks in order to get you to the right, properly maintained table, you don't have to wait another two weeks or another 30 days after the last compaction to tier down. You only have to wait another initial 15 days or however much you have since the last time you read that object. So very happy to get Intelligent-Tiering for S3 Tables out there, and again it works specifically for Iceberg, which is nice.

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2010.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2010)

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2020.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2020)

Now I've run through a long list of feature content here, and oftentimes when I do that, especially in the analytics space, customers ask me about how we're also making things simple  for them. The big simplifier that we've done this year for S3 Tables is to add support  into SageMaker Unified Studio, and we updated Unified Studio just a couple of weeks ago to enable IAM-based one-click onboarding. I pulled this into the S3 deck this year because it really is the easiest way to run and interact with data in S3 Tables.

[![Thumbnail 2040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2040.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2040)

You get a link now to  jump off of the S3 Management Console from the detail page of a table that you have. You can jump straight into a SQL editor right from there with just a single quick click. Once you're in Unified Studio, you can also navigate off and get access to our new notebook that you can use to process data in your tables with SQL or with Python or even with natural language. So just a very easy way to go browse and process and manipulate data in tables.

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2080.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2080)

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2090.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2090)

### S3 Metadata: Automatic Metadata Generation with Journal and Live Inventory Tables

Those are our updates on S3 Tables.  Again, very excited about the trajectory of the service and very happy with the uptake. And with that, we'll talk about S3 Metadata. S3 Metadata is another launch that we announced last  year in preview at re:Invent, and what it does is it automatically generates metadata about the objects in a bucket. We announced preview last year at re:Invent, and in January we launched what we call a journal table. What a journal table does is it records

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2110.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2110)

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2120.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2120)

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2140.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2140)

 all of the changes  in S3 that you make to a given bucket automatically down to an Iceberg table that you can query with standard SQL. So as mutations are flowing in, puts, deletes, changes to metadata, we're recording those line by line  in the table that you can go query.

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2150.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2150)

That was in January and then we came back in July with two updates. The first update to S3 Metadata was a price cut where we reduced the price of that journal that we just talked about by 33%. We also added this concept called a live inventory table.  This gives you a point in time view of all of the contents of your bucket at any given point in time, sort of a snapshot of the metadata for all of your objects.

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2170.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2170)

How it works is that the journal rolls and it  records all of the changes that you're making to your data. And every few hours we come in and do what we call a coalesce where we generate this inventory view of everything that exists in your bucket. And this is useful because you can just jump onto a SQL prompt, write simple SQL statements to understand the contents of your bucket. For example, this is just a simple query that I wrote in Unified Studio that allows me to quickly get after all of the objects in my bucket that are greater than about 4 megabytes in size. So it's easy to see how it's useful to go troll through and find stuff out about your data sets just from a SQL prompt based on tables that are generated for you automatically.

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2220.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2220)

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2230.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2230)

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2250.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2250)

Okay, so that is S3 Metadata, a price cut, the journal and the inventory, and now we're gonna swing back to the demo,  and check in on our uploads and show you the stuff in the flesh. All right. Let's get back to our job. It completed successfully in 5 seconds,  which is fairly fast considering 700 objects that it needs to go through. It found 75 objects matching our criteria and it indeed put a color black tag on that. And as we can also see, the IAM role was automatically created for us. 

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2260.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2260)

[![Thumbnail 2280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2280.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2280)

Now, what I didn't mention when I created my bucket, I also enabled metadata configuration. So now I have two tables, both journal table and live inventory table, and I can navigate to them  through S3 console as well. These tables store data in Iceberg format and we can use any Iceberg compatible engine to query them. As Paul mentioned, there is a single click experience to go straight to the Amazon SageMaker Unified Studio. 

[![Thumbnail 2290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2290.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2290)

[![Thumbnail 2300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2300.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2300)

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2310.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2310)

In the query editor that opens, we can just start exploring our data right away. For example, we can count how many objects we created  during this presentation. We do it by querying  journal table. We know the answer, it should be 744. Yep, that's right. From this view we can also navigate to the data view where we can  examine the schema of our data and in one click, we can also navigate to the notebook experience.

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2320.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2320)

[![Thumbnail 2360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2360.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2360)

In the notebook experience, we can use our natural language to start writing queries right  away. For example, let's create a distribution of updated keys per color tag value. It might take a while to generate the proper query. It's a complex prompt that we are given that. And with that, Paul, do you still remember your SQL? Absolutely not. What will we do if Gen AI doesn't generate it right? Gen AI is going to work great. If you got our query,  let's look at it. It indeed seems complex to me. Let's give it a try.

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2380.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2380)

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2390.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2390)

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2400.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2400)

Okay, all right. As I mentioned, we have black and we have 75 objects tagged with black. That's the color Paul picked.  The rest of the objects are tagged with purple. This is the color I picked ahead of this presentation. From this UI we can also navigate to different views,  for example, bar charts and pie charts. So as you explore your data, give SageMaker a try. It is by far the easiest way to start  getting insights from your data.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2410.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2410)

### Understanding Vectors: From Concept to Storage Requirements for AI Applications

All right. So now I'm going to talk a little bit about S3 Vectors.  I've been working on storage and storage accessories for almost 20 years now. I'm really comfortable in the engine room. I've got to say though that working on AI over the course of the last 12 to 18 months has been super fun, just because we're all kind of learning about it together. The technology is moving so fast, and it's been fun and rewarding to learn about it.

I'll admit that 12 months ago, 18 months ago, it would have been hard for me to explain what a vector is, let alone how to use it. I bet there are people in this room who are there right now and don't quite understand what a vector is. So I'm going to very quickly explain what a vector is, what customers told us, sort of why, what we were responding to when we launched this product, and then how this product looks and how to use it. That's my plan for the next 10 minutes or so.

[![Thumbnail 2470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2470.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2470)

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2480.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2480)

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2490.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2490)

So I have a list of data points in my pocket right now on my phone, which I'm sure all of you do too,  and each one of these data points has a machine-readable name and resolution or frame rate and number of kilobytes in storage and a bunch  of other attributes like that, right? But if I were to describe one of these images to you, I would not use those terms. For example, if I were to describe this image,  I would describe this as a funny-looking dog sitting in front of two chairs in an orange room, right? And what I'm communicating to you is the meaning of the image and not its physical characteristics.

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2530.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2530)

Now computers know all about the physical characteristics of our data, but they can't directly calculate meaning. Only we can do that. And so what a vector tries to do is it tries to bridge that gap because computers are so good at processing and calculating numbers. What a vector is, it's simply a long list of numerical values  generated by a specific type of large language model called an embedding model. And these values represent all of the characteristics of that given piece of data. So is there a dog in the picture? Is there a chair in the picture? Is it indoors or outdoors? What color is the room, and thousands of other dimensions.

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2580.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2580)

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2590.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2590)

And so we get this mathematical approximation of meaning that becomes more and more closely resembling reality as the science progresses and the models become better and better. And this gives us some interesting capabilities. This ability to mathematically quantify similarity and dissimilarity. Specifically what it allows us to do is it allows us to go through large data sets  that are otherwise unsorted and arrange them into categories that are meaningful for humans. And we can do this by just comparing  the mathematical values that we derive from vectors between one piece of data and another. We can calculate the distance, which represents the similarity or dissimilarity between two data points in a data set.

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2640.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2640)

We can also cluster data points together, and these clusters represent data points that have a similar meaning. And now when we load all of this up into a vector store, it becomes very interesting because now we can go build applications that very efficiently calculate similarities and differences and do groupings across thousands and thousands of different dimensions. And this is a very powerful concept and it's sort of the underlying kind of driving force into,  when you see agents sort of behaving very humanlike.

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2680.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2680)

Now this sort of multi-dimensional expression of relative meaning is often expressed on slides in three dimensions, but that's only because it's really hard to get 2000 dimensions onto a PowerPoint slide. But this multi-dimensional space that is kind of created by the math of many data points that are vectorized, we refer to as a vector space. And we can do interesting things with vector space that's loaded up with a bunch of vectors. Specifically, if we bring a new data point in and we don't know anything about it, we can learn a  lot about that data by determining which cluster it belongs into, like finding out what are its cohorts across various dimensions in vector space.

To do that, all we need to do is calculate a vector for this new piece of data using the same embedding model that we use to create the vectors for all of the other data points in our vector store, and we can then compare across all of those existing data points in order to find what would be this new piece of data's nearest neighbors in vector space.

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2720.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2720)

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2730.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2730)

This ability to  find and identify similar bits of data in terms of meaning is called semantic search, and this is the foundation of  how all modern AI systems retrieve and interact with information.

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2760.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2760)

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2770.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2770)

You can imagine over the course of the last 12 to 18 months, as many of you have been exploring AI, running prototypes, and taking AI-based systems into production, the uptake in semantic search deployment over that time. When stuff like this happens, customers come to us with questions about storage.  There are really two big storage questions that we've been fielding from customers on this  new vector data set.

The first is just what is the durable, reliable storage that I should use for this new data set. What you're effectively doing here is you're building a new metadata layer on top of your data sets, and it's got to go somewhere. Your AI-driven applications depend on it, and customers need that reliable storage to host it. Now this is a storage overhead as all metadata is, and it's a small amount of overhead for image and video data, but for text it can actually be a lot of overhead. The vector data for text data sets can actually be more storage than the underlying text data, and this is very interesting because it just goes to show just how much meaning is packed into human language. So that's the first thing: customers were asking us what is the right durable, reliable storage for this new data set that they need to power their applications.

[![Thumbnail 2830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2830.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2830)

The second set of questions is really around the price-performance curve. Now  this is a question that we work with all the time for all storage products. This trade-off between fast storage at a high price per gigabyte versus slower storage at a lower price per gigabyte is why we have a range of storage classes in S3, from S3 Express on the fast side to Glacier Deep Archive on the slower side. This range of storage along the price-performance curve is very helpful to match storage to the appropriate application requirements.

[![Thumbnail 2870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2870.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2870)

Most of the semantic search that's been deployed over the last several years  has really been for hybrid search, so taking semantic search and complementing the keyword search that we've been doing for a long time now. For this use case, there's really a low latency requirement for the underlying storage. This is because a lot of times in this use case, it's a human sitting in front of a search box, and if your application is sitting between a human and the buy button, you need to wring every millisecond of latency out. But as AI has proliferated to more and more use cases, there's this universe of use cases that are happy to trade off latency for a lower storage cost.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2910.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2910)

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2920.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2920)

 Just a few examples of that: retrieval augmented generation, or RAG, is  a technique that you can use to add knowledge that's specific to your business to the knowledge that's trained into the big large language models. It's what allows us to drive agents that know about our own specific data and workflows. A lot of times these workflows are asynchronous. You can send an agent off to do a multi-step process, and oftentimes across a bunch of text as well, which, as we saw in the previous slide, can be more expensive. So in this case, it's easy to envision cases where trading off 100 milliseconds or so for lower storage costs makes sense.

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2960.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2960)

We also have a bunch of customers in the video space that want to run semantic search against video.  The next step in the workflow is to go recall video data from Glacier, which is a multi-hour process. So for these use cases in particular, they're very willing to trade off latency for a lower storage cost. In fact, that's the whole point of Glacier in the first place.

[![Thumbnail 2980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/2980.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=2980)

[![Thumbnail 3000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3000.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3000)

So basically what we found talking to customers is that  on one hand, we have this universe of applications with growing data sets that are looking for reliable storage to host them, and on the other hand, workloads willing to trade off latency for a lower storage cost. That's why we launched S3 Vectors, which is a  native vector store in S3 that reduces the cost of vector storage by up to 90%. It gives you latency in the 100 millisecond range.

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3030.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3030)

### S3 Vectors: Native Vector Store with 90% Cost Reduction and Simple Integration

You can store up to 2 billion vectors per index, up to 10,000 indexes per vector bucket. Now, normally when I'm out here talking about a big new feature like this, I have a whole bunch of knobs and dials to take you through, but I just don't with this product. It's very simple. It has some APIs that you can  use to update and manage vector space. You could almost predict what those APIs are, not even knowing what the product is. Put, get, list, delete, and then importantly, a query API to go query vector space.

[![Thumbnail 3050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3050.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3050)

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3080.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3080)

What's a very important characteristic of this service is that it scales from absolute zero,  and you only pay for what you use. There's no pre-provisioning. It works just like S3 in that way. What's really powerful about this for this use case in particular, and what's been really awesome for my team, is this means you can do tiny little AI prototypes using this vector store, and it costs you very little, right, because there's nothing to provision, there's no purchasing decision to make. But if one of those prototypes or experiments takes off, you're kind of okay because the vector store scales with the characteristics  of S3, scales elastically, has the durability, reliability, all of that stuff that you expect from us when you place data on our storage. So that's sort of generally what it is. It's the vector bucket with some simple APIs.

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3100.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3100)

[![Thumbnail 3110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3110.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3110)

[![Thumbnail 3120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3120.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3120)

[![Thumbnail 3130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3130.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3130)

[![Thumbnail 3140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3140.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3140)

Really quick before I hand it back to the demo, here's just a quick overview on how to interact with it.  It's just based on a simple 70 line Python example. So you take an image to insert into the vector space,  invoke an embedding model via Bedrock to generate the vector, and then insert that into the vector store. It's really three simple steps and as you can see it's  quite simple to plan out in Python. To actually run a query, it's actually very similar. Same thing, start with an image,  take, invoke the embedding model again to calculate that vector, but instead of inserting it, I'm using it as an input to my query, passing that  vector in as an input, and then that'll bring back to me the most similar results in vector space to that image that I used to query. So that's how you build against this thing, just a quick example of how to actually use the APIs.

[![Thumbnail 3160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3160.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3160)

But we actually expect most customers to take advantage of it via  other AWS services. It's already integrated with OpenSearch where you can basically use it as a cold tier behind your hybrid search implementation, so helping to reduce the cost of those hybrid search use cases that we showed on the left hand side of the price performance curve. It also, you can click through and make it an option, use it as an option in Bedrock to create a knowledge base which allows you to do RAG and stand up agents and all of that good stuff with your own data. This is what's been really impactful for my team. I could talk about it all day, but we have only a few minutes left, so we'll pass that back to Anna to actually show what this looks like in the demo.

[![Thumbnail 3200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3200.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3200)

[![Thumbnail 3220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3220.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3220)

[![Thumbnail 3230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3230.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3230)

### Demo Finale: Semantic Search Using Amazon Bedrock Knowledge Base and S3 Vectors

 All right, so here's the challenge. We have 744 objects, but only one of them has a transcript of today's session. So how do we search this corpus of data semantically? To assist with that, we will be using Amazon Bedrock knowledge base plus S3 Vectors and QACLI agent.  For the knowledge base, I created it earlier during the presentation as it takes quite some time to sync it with the storage  with my original data, and I used Amazon Nova multi-modal embeddings model to create vector embeddings out of my data, and I'm using Amazon S3 Vectors to store those vectors.

[![Thumbnail 3260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3260.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3260)

[![Thumbnail 3270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3270.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3270)

[![Thumbnail 3280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3280.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3280)

[![Thumbnail 3300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3300.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3300)

Now, let's quickly hop into QACLI set up. On QACLI, when I start it up, it also comes with  Bedrock knowledge-based MCP server. I installed this server from AWS Labs GitHub, and it's very easy to configure. You basically just need to provide  your AWS credentials and region. This MCP server simplifies interactions between agent and the knowledge base in Bedrock. Now I also created  a context file for my agent. In this context file, I give some directives to my agent. For example, for every question, my agent should first consult Amazon Bedrock knowledge base. It should also ask for the knowledge base ID and I also give some hints to my agent how to query this knowledge base effectively. 

I need to add this context to my agent first, and after I'm done with that, I have a question of the day. What are the names of two volunteers at a re:Invent session today? My agent is asking about my knowledge base ID as I instructed it previously. Let's copy it. Oh yeah, that was the wrong one. Okay, here's the knowledge base ID.

Now the agent tells me that's the query. It's going to run this query. This query looks good to me, so I approve. Paul, what do you think? It has to work at this point. Yeah, congratulations Matt and Ireel. Thanks for volunteering.

[![Thumbnail 3390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3390.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3390)

[![Thumbnail 3420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a7e24f6bd1c8db4d/3420.jpg)](https://www.youtube.com/watch?v=Sy2LHRyMXAo&t=3420)

All right, that's what we have for you. Thirty-five launches in fifty-seven minutes.  We really appreciate you taking an hour out of your re:Invent to spend with us. Thank you for entrusting us with your data. It's a responsibility that we take very seriously on the S3 team. And please rate the session and provide feedback. We read every single comment. It really helps us to determine what to bring to you every single year, and so I encourage you all to hit the app and give us a rating. Thanks a lot. 


----

; This article is entirely auto-generated using Amazon Bedrock.
