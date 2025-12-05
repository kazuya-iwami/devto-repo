---
title: 'AWS re:Invent 2025-From fragmented to unified platform: Rocket Companies data and AI journey-IND3306'
published: true
description: 'In this video, Rocket Companies shares their data transformation journey from fragmented systems to a unified data foundation supporting AI-driven homeownership. Garima Sharma and Ilia Fischer detail how they consolidated 10+ petabytes across 12 OLTP systems into a single S3-based data lake using Apache Iceberg, EMR, and Glue. They created standardized data products like Customer 360, Mortgage 360, and Transaction 360, reducing duplication and establishing single sources of truth. The architecture follows three layers: ingestion, processing (raw/processed/conformed zones), and consumption. Results include 40,000 servicing leads integrated in 9 days, 3-day loan closings, 10% conversion lift, and 3x industry recapture rate. The platform now powers 210 ML models and agentic applications, enabling non-technical users to query data through natural language interfaces. The session emphasizes three patterns: aggregate data into unified formats, curate into reusable data products, and expose via APIs for downstream consumption.'
tags: ''
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025-From fragmented to unified platform: Rocket Companies data and AI journey-IND3306**

> In this video, Rocket Companies shares their data transformation journey from fragmented systems to a unified data foundation supporting AI-driven homeownership. Garima Sharma and Ilia Fischer detail how they consolidated 10+ petabytes across 12 OLTP systems into a single S3-based data lake using Apache Iceberg, EMR, and Glue. They created standardized data products like Customer 360, Mortgage 360, and Transaction 360, reducing duplication and establishing single sources of truth. The architecture follows three layers: ingestion, processing (raw/processed/conformed zones), and consumption. Results include 40,000 servicing leads integrated in 9 days, 3-day loan closings, 10% conversion lift, and 3x industry recapture rate. The platform now powers 210 ML models and agentic applications, enabling non-technical users to query data through natural language interfaces. The session emphasizes three patterns: aggregate data into unified formats, curate into reusable data products, and expose via APIs for downstream consumption.

{% youtube https://www.youtube.com/watch?v=nfVRNsFIfBA %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/0.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=0)

### The Hidden Cost of Innovation: How Data Fragmentation Becomes an Enterprise Tax

 Good morning everyone, and welcome to day one of Reinvent. For those who celebrated Thanksgiving, I hope you had a great time with family and friends. If you traveled here, I hope your journey was smoother than mine, because my travel was a disaster. I flew in right in the middle of a winter storm hitting the Midwest. Due to the bad weather, hundreds and thousands of flights were canceled, and it was a mess. When I reached the airport, I got 13 different gate updates. My phone said B12, whereas the email alert said B17, and the monitor in front of me at the airport confidently said C7.

I was standing at the terminal having a moment of clarity, thinking that if something as simple as a gate number can become your adventure story, that is exactly the challenge we are going to unpack today: from fragmented systems to unified data, to power, accelerate, and transform AI and ML experiences across your organizations. If that was your travel journey, welcome. You are among friends. However, we are finally here together in this room, and we are going to kick off day one of Reinvent.

Along with me today, I am joined by two speakers: Garima Sharma, Vice President of Engineering at Rocket, and Ilia Fischer, Director of Engineering at Rocket. You are going to hear directly from them about the transformation, innovation, architecture, wins, and hard-earned lessons that can help you accelerate your journey. Let me start with a simple question: What happens when your data grows faster than you actually use it? I will give you a second to think about that. Honestly, that is every enterprise right now.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/150.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=150)

 Every enterprise is accumulating and collecting data more than ever from multiple systems and in different formats, faster than their teams can organize it. What is the side effect of this? Your data may become richer, but turning that data into insights, into actions, into AI becomes poorer. Why? Because that is your constraint. That is when fragmentation creeps in.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/190.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=190)

 How does this actually happen in an organization? Organizations do not sit down and say, "Let us create a fragmented organization." Definitely not. They set goals to move faster. The three main things every organization sets as goals for their teams are: one, innovate; two, scale out faster; and three, reduce time to market. These are the three things we always hear. But what happens with every product launch? When you stand up every new use case, you are going to solve a problem for the local team, and you celebrate the wins. But fragmentation creeps in.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/240.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=240)

 Under the hood, when you launch a new product or a new use case, it comes with its own customer data, its own schema, its own pipeline, and its own data assets. Because of that, you end up having different multiple sources of truth. Individually, they are successful, but when you collate all these things together collectively, this is fragmentation. What happens when this continues over time? When this innovation cycle keeps going on in your organization, you end up accumulating multiple sources of truth.

Consider the same flight from the same airline. I am getting different gate updates. Why? Because you have multiple sources of truth that can derive the gate number. The same thing is going to happen because of the multiple sources of truth that coexist in your organization. You do not know who owns the data, what part of the data is useful, how the data is going to be cleaned, or whether it is the golden source of truth or not.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/350.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=350)

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/360.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=360)

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/370.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=370)

Let me take a quick poll to test the room. I want to see how many of you resonate with what I've discussed so far regarding fragmented systems and inconsistency in the matrix. Is your data spread across dozens of systems in your organization? I see a few hands. Next question: does your organization have different teams defining the same metrics differently? Awesome. And the third one: how difficult is it for you to find the data, or in other words, how much time are you spending finding the data rather than using it?   I see hands raised. And of course, all of the above—when we don't know the answer, all of the above is always the perfect answer. 

Everything we've discussed about fragmentation is a problem and a side effect of innovation. You can think of it as a tax that we're paying for the innovation we've accomplished. So what exactly does this matter, and why does it even matter? When fragmentation happens, it's not just one team that gets bottlenecked. It impacts how you turn accumulated data into insights and how much value you're deriving from that data. That's where your downstream systems get impacted. You realize you have fragmented systems when your teams are spending more time finding the right data to use, when your AML teams are waiting for you to get your data cleanliness and have the data ready for them to try their models, and as AI becomes the biggest thing in the market, when you want to adopt AI and everyone complains about accuracy. Accuracy matters when you talk about RAG patterns and retrieval augmented generation. Every time you want to implement this, if the data isn't clean enough, you're going to impact your accuracy. That's why it matters, and different organizations feel this pressure at different magnitudes.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/520.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=520)

Let's imagine you are the number one mortgage provider in the United States, serving one in six mortgages in the country. If all these things are happening—your data flowing from hundreds of systems and thousands of processes built on top of each other—and now imagine you are the leader tasked with unifying all these systems together. How would you feel? What other things come to your mind? Rocket is going through this exact transformation right now at Rocket Companies. To share firsthand information and insights, let me welcome Garima Sharma, Vice President of Data at Rocket Companies, to talk about how the transformation is progressing at Rocket. 

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/550.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=550)

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/560.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=560)

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/570.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=570)

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/600.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=600)

### Rocket Companies' Journey: From Innovation at Scale to Unified Data Foundation

Good morning, everyone. I'm Garima Sharma from Rocket Companies. Rocket's mission is simple: to help everyone experience home, which comes with stability, hope, and a sense of belonging. Every architecture decision we make, every automation we deploy, and every capability we build with AWS ultimately serves that mission. Because for us, in the end, it's not about building technology—it's about creating pathways to home.  Rocket has always been an innovative company, leading the transformation in the housing industry. We were the first to launch online mortgages in 1998.  We were the first to launch fully digital mortgages in 2015.  We were also the first to take them mobile and the very first to deliver full e-closings for all 50 states in 2019. Just about two years ago, we were the first in our space to declare an AI-fueled homeownership strategy. 

When you innovate at that pace, naturally new systems, new data, and new tools come with each milestone. And here's the important part: in that growth and progress, we realized that we needed a data foundation, because for a mission this big, you need a foundation that can match.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/620.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=620)

All that innovation was incredible for business.  We were moving fast and opening doors to new capabilities, but as teams were innovating and building, we began noticing three distinct patterns in our data landscape—not problems, but signals that we were outgrowing the way things were done before.

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/650.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=650)

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/660.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=660)

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/680.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=680)

The first was siloed and duplicated data. As teams were building great solutions, you can imagine they were doing it often  independently, which meant similar datasets existed in multiple places. Second,  as our data grew in volume and variety, finding the right data for the right use case became harder and time consuming. Third,  as our data grew in volume, we had very rich datasets, however utilizing them at scale for analytics, machine learning, and AI became harder than it needed to be.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/720.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=720)

None of this was unique to Rocket. Every high-growth company and innovative company finds itself in such a situation. But the key point for us was that we recognized we needed a strong foundation if we were to be successful in our mission. Once we recognized what rapid innovation brings, we were able to make a core decision: move everything to one unified data lake, build on open table formats, standardized ingestion, and shared governance.  This was more than an infrastructure choice for us; it was an operating model choice and a speed choice.

At our scale of about 10 plus petabytes of data back then and 30 plus petabytes of data now, we knew that innovation cannot happen rapidly if our data was living on islands. So we made a very simple decision to adopt a universal pattern: bring everything into one place—clean, consistent, governed, and make it usable for everyone. Let me show you what that looked like.

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/790.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=790)

### Building the Unified Data Lake: Architecture, Curation, and Business Impact

When we talk about a unified data foundation, this is what we mean. Rocket's data comes in every shape:  structured, semi-structured, and unstructured. Traditionally, each data type lived in its own system. Once we got the lake, that gave us a place where everything flew in in a standardized way, regardless of the format. Everything followed a standard pattern and architecture. The data lake gave us a place where we finally had a unified foundation, and with this foundation, you can imagine the value explodes upward.

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/850.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=850)

BI now has standard definitions. Machine learning and AI teams now have clean features at scale. Analytics and streaming become faster. When we began modernizing our data landscape, our vision was simple:  build a platform that powers every workload—real-time, batch, machine learning, and AI. We knew that we needed to have one foundation, one lake.

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/770.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=770)

So here's how we went about it. Step one was hydration. We brought data from about 12 different OLTP systems and landed it into one lake on S3 based Amazon technology. Step two was automation. We built standardized ingestion pipelines using EMR and Glue and adopted Parquet as our default storage format. Step three was security from day one. 

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/920.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=920)

We encrypted everything—disk, file, field, address, in transit, and in use. 

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/950.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=950)

Aggregating 10+ petabytes of data into a single lake gives access. But as you all know, access doesn't mean understanding. The next step for us was curation to give the entire company a shared language around data. Instead of 30 teams building their own versions of customer data, we created Customer 360,  a single governed and trusted view of the client. The same happened for Mortgage 360 and Transaction 360, which are single governed trusted views of mortgage loans and a client's transactions. Each one has standardized definitions, consistent metrics, and clear ownership.

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/980.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=980)

This wasn't just a data project for us; it was actually a cultural shift.  We moved from duplication to standardization, from tribal knowledge to data products, and from searching for data to discovering it. Once we curated our data layer, we finally had the foundation to operationalize machine learning, AI, analytics, and real-time decisioning at scale. We not only improved how we used data, but we also unlocked what we could build on top of it.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/1020.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=1020)

With this strong data foundation, we created the runway for something bigger: to reimagine home ownership end to end.  Traditionally, the housing industry works in silos. One company helps you find a home, another helps you finance it, and another helps you own and manage it. But homeownership isn't three distinct disconnected stages; it's one journey. By bringing Redfin, Rocket Mortgage, and Mr. Cooper together, we are now able to support a client across the entire journey—helping them find a home, finance it, and own it.

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/1080.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=1080)

This foundation was important for us because it helped us create solutions, impact clients, and drive business performance in ways we hadn't imagined. Results showed up immediately.  During the early phase of integration, 40,000 servicing leads seamlessly flowed into our unified platform within just 9 days, clean, governed, and ready for activation. On day 12, a client from the integration went from application to closing in just 3 days, something that previously would have taken the industry weeks.

Our engagement engine began analyzing conversations and client behavior, surfacing the right outreach at the right moment. That drove up banker follow-ups by up to 9 points. We saw a 10% lift in conversion across daily credit pulls and applications. When the refi wave came in September, our systems were able to instantly pull qualified clients and activate personalized outreach for them, which led to a 20% increase in refinance pipeline overnight.

We can now unify servicing data, client intent, and behavioral signals, and that has led us to achieve a 3x recapture rate compared to the industry. When you have such a strong foundation, nothing is impossible for you. But it took hard work, effort across the team, and alignment across teams to show the value of a unified data foundation. Everything I've shared so far—the platform, the AI impact, and the data foundation—was not accidental at all.

It was the result of three things: intentional architecture, repeatable patterns, and governance. So let me now hand it off to my colleague Ilya Fischer who will walk you through how we made all that possible under the hood. Please welcome Ilya Fischer.

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/1260.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=1260)

### From Weeks to Minutes: The Three-Layer Modern Data Platform Architecture

Thank you, Garima. The microphone is working. Good morning, everyone. There is so much energy in this room. Do you feel excited for this whole week of events? I am excited. I am excited to see so many folks here willing to hear about the modern data architecture because it means to me that  whatever we share with you today, you will take back home to your companies, to your businesses, and you will evolve. In the years to come, we will be coming to reinvent and we will be seeing new patterns, new technologies that maybe just one of you will keep driving.

Let me start the presentation with a personal story. About ten years ago, I was buying my own home and I had to secure a mortgage. I did not go to Rocket Mortgage. I went to a different broker, so I had a very interesting experience. Maybe many of you went through it when buying a home: collecting pay stubs for six months backwards, bank statements, investment statements, tax forms, scanning every document as a PDF. I had an HP scanner so I could not put the bulk of them in at once. I just had to scan page by page and then email them to my mortgage broker. I was very much worried about two things. Firstly, I hoped that I would get the mortgage to buy the home. I did, and about two weeks later I received the answer that I got the mortgage. Secondly, I was very much worried that there was not anyone else buying their own home with my identity since I was sharing all these confidential documents through email.

Ten years later, it seems like that has not happened yet, but who knows. I am sharing this story because maybe not as insecure, but in terms of latency and manual labor, this is what Rocket Mortgage was doing decades ago. It is an entirely different story today. Decisions like this, actually millions of them, happen in under eight minutes. Everything goes through a secure portal and secure connections, and people get an idea if they can buy a home literally in minutes or in rare situations in days. How did we go from waiting for weeks to know if you can get that loan to buy a home to a maximum of a few days?

It happened because we threw away the old playbook of treating data as a call center, and we turned data into the engine that runs the company. Two hundred ten machine learning models, thousands of data pipelines, and all that runs in full automation with zero human intervention. This is what Rocket Companies does today. This is what we achieved by building a modernized data platform without hiring hundreds of engineers, without purchasing any silver bullet software. All of that by building a modernized data platform which I am going to share with you. So buckle up. In the next twenty minutes or so, we will go through the steps of building that platform through the architecture patterns, through the design patterns, and I hope you will find it interesting and insightful.

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/1470.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=1470)

Before I dive deep into the actual services that run the platform, let me step back and share with you how  a typical modern data platform looks like, which most companies converge on. It boils down to three clean decoupled layers. The first layer you can see on your screens is called ingestion. Its goal is to get everything in fast as it arrives: real-time streams, file drops, SaaS connectors. The Swiss knife of tooling, if you wish, is supported to get the data quickly into your platform in a raw immutable format.

Ideally, as Garima was mentioning, using open formats like Parquet, so they're easy to integrate the data with a plethora of tools. This is the first layer, which is called ingestion. Just land your data as fast as it arrives, reliably and securely in the raw format.

[![Thumbnail 1530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/1530.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=1530)

The next comes the processing layer.  Transfer once but serve many. Our data lake intends to support so many different use cases, and we don't want to ad hoc process data for each of them. So we want to transfer once. This is the key statement: once. But then again, the data will serve many.

The overall data lake is partitioned into three zones. The first zone is raw. This is where the ingested data lands. It's exactly what's landed, forever unchanged. The next level is processed data. The processed data is enriched, cleansed, and managed according to these standards, standardized to the standards you define in your platform. This is the process layer, still not ready for your business consumption.

So the conformed layer is the third one, and the data gets the shape of business-level, domain-aligned form so that many consuming applications from the platform and many consumer use cases will find it useful for their specific business case. Now again, remember: transfer once, serve many. There are many use cases and many business use cases. However, we want to ensure that our transformation goes only once and produces the data that we need. All those transformations are event-driven when possible, scheduled when necessary, declarative, and version-controlled, so you always know what's going on. You have a solid audit trail of how these transformations go, and this is what the platform's goal is to provide.

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/1640.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=1640)

And last but not least is the consumption layer.  I call it the gateway to your platform. One version of truth for every possible consumer. So again, one version of truth is the key. We don't want to have siloed data. We don't want the users of our platform to think about which version is the right one. So our platform provides a unified service, a unified data set for a variety of business use cases, including analysts doing business intelligence, generic applications, data scientists training their models. All of them point to the very same well-governed data set that our platform provides.

This kind of overarching schema architecture gives you a highly scalable infrastructure. It gives you pay-only-for-what-you-use economics, and we'll talk more about that. It drastically lowers operational overhead because you have a well-formatted three-phase process that your data goes through. Your processing takes care of so many use cases. Your ingestion and consumption are like Swiss knives, easily extendable to support new ingestion patterns and new ingestion systems if you wish, and on the consumption side, all the new scenarios that your platform must support in your business.

[![Thumbnail 1730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/1730.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=1730)

### Deep Dive into Platform Layers: Ingestion, Processing, and Consumption at Scale

But now let's go into more detail about each of those  layers and how it works. Today it's not about picking a tool to make data work. It's about building a clean, modular pipeline to ingest, process, and consume that supports so many of those use cases. And again, going back to ingestion, get any data in fast and reliably. We would support the gateway running on Elastic Kubernetes Service. We would support replication of data between services into S3 buckets. We will support streaming data with Amazon Kinesis. All that is included in our ingestion layer supporting all the business use cases.

And as soon as a new business case arrives to source data from a new platform or a new source, we can easily extend the ingestion and accommodate it. And a bonus is that the ingested data is landing in an S3 bucket, which is a raw data lake, the raw data zone in the lake. As a result, we get out of the box versioning and audit trailing of the data.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/1820.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=1820)

We built a resilient data link with geographical replication and availability when needed, and that's like an extra bonus that we get when relying on AWS. Next  is the processing layer. Transform ones used everywhere. So again, you see those three standardized buckets at the bottom: the raw, the processed, and the conformed data. Some people might have heard call them bronze, silver, and gold. On top of them, the data processing orchestration is event driven when possible and time-based for heavy workflows.

We rely on Glue as the most general use case for highly scalable compute situations with provisioning. Amazon managed streaming service helps us to deal with streaming data, and for very lightweight workflows, we find Lambda and Step Functions giving very good economics to make lightweight data transformations. It's a personal hint: if you rely on Step Functions, it's an amazing tool, very addictive with its friendly UI. However, if your data ends up being virtually streaming, I noticed in my experience a situation where my Step Functions became to run 1000 times in a minute, and the bill spikes very quickly. So it's always important to monitor very well your expenses and costs, and using elastic services in your compute, but also being elastic yourself to see how you restructure your architecture to send data via the right channels.

Again, this is the processing layer using all these tools. There is no religion, as I mentioned. You should be smart to see which tool works for each use case, what gives you the right economics, and they all are elastically scalable services. In the data world, you have certain periods—the year, month, week, day—when you have high scalability needs, and other times when you need low scalability needs. So using these services, we are getting that elasticity of computing of the processing, and that gives us near perfect economics so that we pay exactly for what we need and when we need it, and we get that compute when we need it with not much overhead.

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/1960.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=1960)

Lastly, the consumption layer. One platform, many consumers, same data—that's the importance here. The same data served for every possible consumer without duplication. This is what I saw so many times in businesses: our data scientists read from their data set and our application reads from their database, and they're not the same. So my data scientist trained model, and something fun, and eventually their machine learning based application doesn't work as expected with real data used in a different application.  This is what platforms know to solve well by again having that one platform, one data set for so many use cases: analytics, operational use cases, machine learning, SageMaker features store pulling straight from the lake. Sometimes external sharing for integration with other platforms like Redshift data sharing or just S3. As a result, all those teams work on the very same governed data set without worrying that they use something different or something wrong.

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2080.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2080)

### Automation and DevOps: Operating at Scale with Self-Healing Pipelines and Agentic Applications

Building this platform with a plethora of those AWS services is call it table stakes now. But how do you make sure you can actually operate at scale? Because you could see there are so many services, and I was saying, well, yes, we need to take care of the governance, make sure that all our data, all our resources are tagged properly for right economics, for right ownership identification. We need to be flexible to add new ingestion patterns, new consumption patterns. Do we allocate like 100 people to maintain the tool? No, we do not. All that drills down to very well-defined envelopes and DevOps systems behind the scenes that  automate that all, running it on autopilot, repeatable, governed, and self-healing. There are no tickets, there is no manual clicking, no exceptions, no heroics. Otherwise, we would just not be able to scale it for so many consumers, for so many resources and use cases.

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2130.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2130)

The moment new data lands in the estuary bucket during ingestion on your left, all the downstream components are already pre-provisioned: the events, the ETL pipelines, and the machine learning models that you're expecting to train today run seamlessly. What we did, and this is a good pattern that I recommend to you,  I want to note it specifically. Initially, we started with standard DevOps source code first, of course, infrastructure as code, and you provision your Glue, your Lambda, your Step Function. All that, but then we realized that the challenge still persists. If I provision my Glue job, what about that Lambda that needs to trigger it? I need to provision it as well. And then I need permissions from Lambda to Glue jobs to execute it. And then my Lambda needs to listen to an event coming from an S3 bucket, so I need those permissions as well, and so on.

Even with partial automation, it takes days, sometimes weeks for a team member to build all this architecture pattern. So with the well-defined architecture that you saw, we realized that we can identify those patterns and give engineers literally the blocks of deployment patterns which they can reuse. Those blocks would give them right away the event listener, the Lambda, the permissions, the Glue job, all the tags, all the governance in place for personal information processing and general information processing. All that is a single package so that they can take it and deploy it literally in under ten minutes instead of spending weeks or sprints building all that infrastructure.

Data scientists treat the model exactly the same as code. When they merge changes to their model, it automatically gets deployed in a blue-green deployment style with the right metrics and monitoring. If there is model drift, in other words, if the model starts to produce results which weren't initially expected or there are any other software issues with that model, the rollback happens automatically. They just get an event, an email that says the change you tried to make didn't go as expected. We'll roll back for you. Go take a look tomorrow. You don't need to go to PagerDuty alerts in the middle of the night.

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2320.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2320)

The same applies to application developers building APIs using Canary deployment. They can safely check how things work in a lower volume environment, and later on, all the automation and metrics embedded into the pipeline deployments help them identify any issues early before they impact customers and automatically heal the pipeline and the changes that caused the issues. This is about the platform, and we covered quite many areas: the overall structure, the specific implementation details with those great AWS services that you all know, the open data format, the envelopes and details behind the scenes.  This foundation isn't just sitting there. It's supporting so many enterprise-grade solutions in our company.

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2400.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2400)

Those include Rocket 360 APIs, not 360 of those, but a few APIs which give you an around-about view of your data models. They support data scientists and business analysts with static dashboards. But one of the most exciting features to me, and it's almost 2026, is the agentic applications, which in my humble opinion is the future of business intelligence and will replace static typical BI dashboards. What I'm going to do now is make a quick demo to show how that agentic application works at Rocket. The exact same one that I'm demoing to you is being used by many of our executive stakeholders. The data you'll see is synthetic data, and that's a legal disclaimer, but everything else is true and valid exactly as it works.  Let me see how we make it work here. Just do one click.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2410.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2410)

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2420.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2420)

The system is now working.  What you are seeing in this browser window on your screen is the system we developed, enabling non-technical users of our platform to quickly stand up new agents for their business use cases. I'll be playing a video and pausing to catch up and explain what I'm showing.  The interface is very intuitive. You don't need to be a highly technical person. You explain the data sources that you want to use, you explain the context and the information you want to retrieve, and then the system allows you to create a new bot, a new application which will talk with the same data set behind the scenes and produce results for you.

One important note: probably all of you are familiar with GPT, Grok, and many similar systems. This is not the same as a chatbot that helps polish your text or provides general information from the internet and tells you to verify the data because it's their best effort. In this case, all the data is sourced from the very same data platform we just showed. The BI dashboards and these agents gather from the very same data source, so you can rely on the data as the true source of truth for your analytics and for making executive decisions.

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2520.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2520)

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2550.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2550)

Let's continue with the playback. In this case, we'll ask a very simple question. We'll ask what is the average mortgage amount right now? The agent will go behind the scenes, fetch this information, and retrieve the data in a matter of seconds.  You don't need to think about how to run SQL queries. It's all very interactive.  The average loan amount in September was $288,000. If you're curious about how the agent received the data or want some technical insights, you can go to the SQL tab and it shows you exactly what queries were made to our analytical data stores to fetch the data.

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2590.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2590)

But what if I want to split this data and reorganize it by states? In the old world, I would need to think about how to rebuild my queries, test multiple times, and then figure out how to visualize that.  Here, using natural language, you quickly get a table of all the average loan amounts per state and the number of those loans. If you wish, you can visualize this data in a variety of graphs—pie charts, line charts, depending on your preference. You can embed this data in any of your reports and obviously also have that SQL tab to see how it works.

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2620.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2620)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2630.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2630)

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2650.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2650)

 But what if I want a bit more analysis? I can ask the agents to analyze this data for me.  The agent will right away give you key insights. They'll give you the premium markets where people take most mortgages, regional trends, and all kinds of information that can be valuable if you are a business analyst or just a team member generating a report about the last week or last month.  By the way, this can also be automated using this platform.

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2660.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2660)

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2680.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2680)

[![Thumbnail 2700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2700.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2700)

What if you want to cross-reference those loans with the interest rates?  You don't need to think about how to write a query or how to test it. You get right away all the information and the agent spits it out in seconds. You can verify the query it provides, you can filter, and you can visualize in different graphs how those loan amounts, the numbers, and the interest rates all correlate to each other.  I can go ahead and ask to analyze deeper all this information, and at the end of this analysis, I'll get a full-fledged report for my mortgages and for the interest rates in September. 

Learn from that report about the states with high mortgages where people take high mortgages and how it affects the interest rates. The agent will tell you the interest rates are low in those states, and the opposite happens in those states where the mortgages are a bit smaller. All that gives you that kind of interaction which you can grab this report, share in your newsletter, share in your presentation, share it in your narrative, updating your business stakeholders. Also, the insights based on the very long experience of our company show how these trends affect the company. All that is not some kind of a story or fairy tale I'm sharing. It's not a POC running on this laptop. It's actually running in production today with tens of agents like this running in production based on the very same data platform that I presented to you and helping us drive the business efficiently.

With that said, I'll pass the microphone back to Sujan, our senior architect, to wrap up and tell more about some of those services behind the scenes that drive the tool. Thank you.

[![Thumbnail 2800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2800.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2800)

### The Three Pillars of AI Readiness: Aggregate, Curate, and Expose



[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2820.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2820)

Awesome. Thank you, Leo. So far what you heard about from Rocket is how they have transformed their organization to be ready for AI readiness. As the executives at Rocket said, AI-fueled homeownership and how the foundation has been built to power the vision that the executives have set for the organization to take. Let's see how they got there. We always see three patterns at AWS that most of the successful enterprises have followed.  The first one is aggregate. The principle here is you have to unify your data from the different data sources, whether it is structured, semi-structured, or unstructured data. Bring all the data together into a single place and choose one standardized format as an open table format. In this case, Rocket was using Apache Iceberg on Amazon S3 tables. When you do an aggregation or unify your data, it removes the fragmentation and helps accelerate your downstream systems and downstream teams.

[![Thumbnail 2860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2860.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2860)

 What is the second one? Curate. Once you unify the data, as Garima rightly said, instead of multiple teams building the same data again and again, you become the data products. Curate the data into the dimensions or dissect the unified data into the right dimension that can be helpful for your teams to take advantage of rather than rebuilding it. The second one is curate. Which services can be helpful in having a curation stage? Amazon EMR, Glue Catalog, and Data Jones. You can get all these different services. I'm just naming a few, but you can build your systems into more curation systems.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2910.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2910)

 What happens when you would love to go one more step ahead? I don't want just curate and have that data like one more other table within our data lab or the data warehouse or the data lake. Why should you do that? Let's have the data sit into curation into one of the data formats. As Garima said about the data products Mortgage 360, Cusma 360, and the Transaction 360, once you have these data products curated, make them accessible via APIs so that it's readily, easily consumable for all your downstream systems, even the agent demo that Ilia showcased. Because those APIs are readily available, you can accelerate your agentic systems in your organization. How many of you here are building AI agents right now? Good. So if you want to accelerate, you need to have this data foundation ready and have your data products exposed via APIs.

[![Thumbnail 2970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2970.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2970)

[![Thumbnail 2980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b39900af75bfdcce/2980.jpg)](https://www.youtube.com/watch?v=nfVRNsFIfBA&t=2980)

 These are our LinkedIn profile QR codes. If you would like to scan, please feel free to scan if there is any follow-up question that you would like to have. After this, we are happy to help.  Once again, thank you for choosing to spend the first hour with us and happy reinvent.


----

; This article is entirely auto-generated using Amazon Bedrock.
