---
title: 'AWS re:Invent 2025 - Scaling Amazon Redshift with a multi-warehouse architecture (ANT318)'
published: true
description: 'In this video, Naresh Chainani, Alex Rabinovich from Vanguard, and Anusha Challa discuss scaling with Redshift''s multi-warehouse architectures to solve workload interference problems. They explain two design patterns: hub and spoke for workload isolation, and data mesh for decentralized team ownership. Key features covered include Redshift Managed Storage enabling single-source-of-truth data access, federated permissions for unified governance, and Apache Iceberg integration. Vanguard shares their evolution from a data swamp to a centralized warehouse, then to hub and spoke, and finally toward data mesh architecture, managing over 20TB in Redshift and 150TB in S3. The demo showcases building a multi-warehouse architecture using zero ETL integration from Oracle, SageMaker Catalog for permissions with IAM Identity Center, separate endpoints for ETL, reporting, and generative AI workloads, and Bedrock Knowledge Base for natural language querying. The session emphasizes eliminating data copies, achieving workload isolation, and democratizing data access while maintaining performance SLAs.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/0.jpg'
series: ''
canonical_url: null
id: 3086850
date: '2025-12-05T15:36:23Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Scaling Amazon Redshift with a multi-warehouse architecture (ANT318)**

> In this video, Naresh Chainani, Alex Rabinovich from Vanguard, and Anusha Challa discuss scaling with Redshift's multi-warehouse architectures to solve workload interference problems. They explain two design patterns: hub and spoke for workload isolation, and data mesh for decentralized team ownership. Key features covered include Redshift Managed Storage enabling single-source-of-truth data access, federated permissions for unified governance, and Apache Iceberg integration. Vanguard shares their evolution from a data swamp to a centralized warehouse, then to hub and spoke, and finally toward data mesh architecture, managing over 20TB in Redshift and 150TB in S3. The demo showcases building a multi-warehouse architecture using zero ETL integration from Oracle, SageMaker Catalog for permissions with IAM Identity Center, separate endpoints for ETL, reporting, and generative AI workloads, and Bedrock Knowledge Base for natural language querying. The session emphasizes eliminating data copies, achieving workload isolation, and democratizing data access while maintaining performance SLAs.

{% youtube https://www.youtube.com/watch?v=WbR_La3h578 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/0.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=0)

### The Challenge of Monolithic Data Warehouses and the Hub and Spoke Solution

 Imagine this: it's a fine Monday morning, like today, and you get a call from your CEO. They're not happy that their dashboards are not loading. You start digging into it and notice that the data science team has kicked off a large training job over the weekend on the same compute. Now you are trying to figure things out. Does this problem sound familiar? If so, you're in the right session. We're going to be talking about scaling with Redshift's multi-warehouse architectures. My name is Naresh Chainani. I'm the engineering director at Redshift, and I'm super excited to welcome Alex Rabinovich, who is the director of data engineering at Vanguard, and Anusha Challa, who is the senior specialist architect for AWS. Together we're going to walk you through some of the best practices around multi-warehouse architectures.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/80.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=80)

 Here's an overview of the flow for today's talk. We're going to start with an overview of some of the design patterns of multi-warehouse architectures, starting with the problem statement and what emerging patterns we see on our massive fleet worldwide. Then we will take a sneak peek behind the scenes at what powers multi-warehouse architectures at Redshift. At that time, we'll segue to Alex, who is going to walk you through Vanguard's journey with how they have scaled over the last couple of years with multi-warehouse architectures. We'll end with a demo that Anusha has specially prepared for today to demonstrate how quickly one can start with this architecture and some of the best practices. We hope to leave some time for Q&A, but don't worry if you have questions and don't get to them. We'll also hang out after the session and be happy to chat and learn from your experiences.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/140.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=140)

 A show of hands: how many folks here in the room are already using multi-warehouse architectures in Redshift? We have a few experts in the room. That's great. This should be a great conversation.

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/160.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=160)

 Before we dive deep into design patterns, let's look at the problem statement. What you see in this picture is a large hexagon that resembles a monolith compute, a single compute cluster. Running on this are a bunch of applications: streaming and batch ingestion, a couple of writer applications for real-time ingestion and batch ingestion, and then consuming teams like the science team and dashboards. This cluster and architecture scales, however, beyond a certain point you run into challenges like what I started off with, mainly around workload interference. What happens is if batch ingestion has a ton of data to be ingested, that can start interfering with your ingestion SLAs if science and dashboard teams are also trying to do work there. This resource contention and workload interference can be completely addressed by the first pattern we'll introduce today, which we refer to as hub and spoke.

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/230.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=230)

 See that large compute cluster we had, the big hexagon? Now that's broken up into smaller parts. There are a couple of advantages here. First, everyone gets their own endpoint or compute. You have your streaming ingestion and batch ingestion sized based on their compute needs. The great thing there is you can also introduce chargeability. For example, the science team might have a certain budget, so you scale independently. Most of all, there is full workload isolation. Every workload proceeds independently without having to coordinate or interfere with any other workload. A good test of any design is not that it just works today, but that it's able to extend to the needs of tomorrow. With this pattern, you can spin up additional endpoints. Say, for example, you have quarter-end processing and you need a pretty large compute to make sure you meet the SEC guidelines for reporting. You can just spin up within a matter of minutes and have another compute attached. In the center, we have something called Redshift Managed Storage. That's your Redshift data. With this technology, one question you may have is: are we creating copies of the data?

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/350.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=350)

The answer is no. If in your businesses you see reasons that you're creating copies, I would highly encourage you to take a look at why, because creating copies becomes a problem, especially as data volumes grow. Governing copies becomes a nightmare. So you want to have a solution where there is a single source of truth for data. In this architecture, both streaming injection and batch injection are writing to that one copy of data, and all the consumers are consuming it using snapshot isolation. So you get all the benefits of transactions that you're used to for your data warehouse. 

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/420.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=420)

### Multi-Region Architecture and Data Mesh: Empowering Enterprise Teams

This is a flavor of this architecture, and at AWS we love hearing and seeing how creative our customers are. One of the things we saw with several customers, especially multi-tenant ISP providers, is that they took advantage of this multi-warehouse architecture and the capability where compute clusters can be in different regions. The idea there is that you can have tenants in different parts of the world, so you can have compute that is actually closer to where your tenant applications are. This is where the physics of network latency helps in terms of the overall experience. 

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/420.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=420)

Even in the multi-region use case, there is again one copy of the data that is stored in the primary region. The great thing is now you can also provide different qualities of service and different tiers to your different providers. There might be some very large customers that can benefit from larger compute, or you can group a bunch of smaller customers into a single endpoint. There is a lot of flexibility there. 

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/480.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=480)

Let's look at another problem and then a design pattern. This is very similar, where now again you have a monolith cluster, but this time instead of different applications you have different teams. This is what you typically see at large enterprises. You have finance, marketing, HR, all different teams, sometimes silos within the organization that are sharing resources. This leads to complex coordination once again. For example, if finance is trying to get their month-end closing and marketing is trying to figure out an upcoming campaign, they have to coordinate if their needs are not predictable. One thing we have seen increasingly with data is a lot of new use cases because you don't want to handicap or handcuff yourself with predetermined use cases. You want to explore what values you can derive, especially with data and AI coming together. 

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/550.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=550)

So once again, using the same pattern, you are able to break up that monolith into multiple parts. In this case, every team now has their endpoint, and this endpoint for HR and finance basically has a very clean chargeback model where you charge it back to the business unit within the enterprise. This architecture, remember the previous one was Hub and Spoke where there was a centralized team that curated rights and opened it up to a bunch of consumers. In this case, every unit, every enterprise unit owns their data and their portion of the data. Finance data is different from, for example, R&D data. So this architecture is referred to as data mesh. 

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/560.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=560)

In this architecture, the data owners decide who they want to share their data with and what data they want to share. This sharing can be at either a database level, schema level, table level, or you can go even more fine-grained at a row or column level. Let's now take a look at what powers multi-warehouse architecture behind the scenes. 

### Behind the Scenes: Redshift Managed Storage, Serverless Computing, and Federated Permissions

At the storage layer, what we have is Redshift Managed Storage. This is high-performance columnar storage that uses some of the most advanced and proprietary encodings with the goal of cache-efficient processing and CPU-friendly pipelines. We actually take advantage of hardware instructions to optimize the line rate at which we can process the data. This is where you see some of the performance benefits from Redshift when it is powering your two-second SLA dashboard, for example. On the compute side, you have a mix of Redshift provisioned and serverless, and there is one emerging pattern that we are seeing with our customers in a hybrid architecture where you use provisioned capacity for your steady-state, predictable, always-on workloads, while using serverless for your line of business units to handle use cases with sporadic or spiky access.

The properties of serverless, like scaling up and auto-scaling, are beneficial in these scenarios. You pay only for the compute you're using, and the nice thing about this architecture is that you can mix and match with either hub and spoke or data mesh patterns. You can have any combination—you can go all provisioned, all serverless, or a mix. We highly recommend you look at serverless. Customers love the ease of use it brings, and it's able to query the same single source of truth when it comes to the data layer.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/690.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=690)

These compute clusters don't have to be in the same AWS account. You can spread them across multiple accounts, or they can be across regions as well. This last point can be important, especially for colleagues who have to deal with data sovereignty. The EU tends to be far ahead in terms of what data can leave the boundaries of the EU. There are some amazing capabilities around permissioning that come into the picture, which I'll discuss in a few minutes. 

This is a new capability that we launched last week. This unifies governance for your multi-warehouse architecture. Throughout my talk so far, I've mentioned how there is a single copy of the data. One of the pain points our customers told us about is that they still have to replicate permissions. As a standard practice, these are additional compute clusters where they have to put their fine-grained access control and permissions again, and that was becoming harder for them to scale beyond a certain point. Just the act of managing those permissions consistently was challenging.

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/740.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=740)

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/790.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=790)

With federated permissions, here's what happens.  Let's look at an example. The finance team wants to have fine-grained access control, so they create a low-level security policy. In this case, the policy ties back to data sovereignty where the user logging in must be from the region for the customer data they're trying to access. This policy is then attached to the customer table. The policy is then assigned to roles in the Identity Center, such as a sales role. When a sales analyst logs in using their own compute cluster,  they query this customer table as they normally would, and at that point, they only see the data that the policy allows them. They will only see the customer data for the region from which they belong to.

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/830.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=830)

### Extending the Analytics Ecosystem: Iceberg Integration and SageMaker Unified Studio

This feature is federated permissions. I highly encourage folks to take a look at this—it launched last week and is still rolling out worldwide. The whole idea is just like you have one copy of the data, you also have a single copy of your permissions.  We've talked a bunch about Redshift's managed storage, which is your highly optimized data for analytics stored in Redshift. The other trend we see is that more than half of our largest customers are also using, in addition to Redshift, some lake data. Iceberg is an emerging trend we see across AWS. Open table formats like Iceberg are extremely important because they provide transactionality benefits, which are important to many applications. You also have the flexibility that any consumer, any engine that can read Iceberg data because it's an open standard, gives you interoperability.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/900.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=900)

With Redshift, you can combine data that is in Redshift with your Iceberg data or Parquet data. In fact, the same query can even join data across these two tables. If you have, say, landing zone data that's in your lake and then highly curated data, as you are building applications, you are joining them within the same unit of work.  Since the beginning of the year till today, we have launched more than 2X performance improvements to Iceberg query processing when you're querying using Amazon Redshift Serverless.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/950.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=950)

This is just keeping up with the custom. There are a bunch of things around advanced statistics that we collect on the data, including the high performance query processing techniques that we have for Redshift, which we are actually extending to Iceberg. We are not done yet. This journey is going to continue. We also introduced writes to Iceberg. So far we have been talking about reads, writes for append-only workloads that started, which was launched a few weeks ago. 

We talked a bunch about Redshift. However, the analytics world has more than just Redshift. There is a data warehouse, and then there is an ecosystem around it. So how does Redshift fit in that, and how do you extend and leverage that entire ecosystem? Last year, we launched SageMaker Unified Studio. This was a landing place for all things data and AI. So what is SageMaker Unified Studio? It is basically supposed to be a one-stop shop. It has your SQL notebooks, your Jupyter notebooks, and you have natural language to query abilities. The whole idea is to be able to build your data and AI applications in one place.

At the storage layer in the SageMaker platform, you have all your data. This includes Redshift data, this includes Iceberg, and this Iceberg can be either self-managed in your regular S3 bucket or it can be managed by AWS in S3 tables. One of the advantages why you may want to consider S3 tables is you actually get better performance because S3 takes care of heavy lifting like compaction. Small data files tend to be a problem when accessing lake objects, so S3 by compacting takes that heavy lifting away, and this is going to continue with more and more stuff coming in collaboration between engines and S3 tables. I highly encourage folks to take a look at this.

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1070.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1070)

All of this data is then visible into the SageMaker Catalog, so you can discover and govern that data. So far we have been talking about how you can use a mix of Redshift Serverless and Provisioned. All of your data warehousing use cases are satisfied.  Now, taking a step ahead, you may have other engines like Amazon Athena, or you may have a Spark job that is doing data processing transformations. All of that is part of the same experience where you are able to consume any of this data, say, for example, from Spark.

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1150.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1150)

The key thing that enables this is that all of the interfaces are using Iceberg's IRC, which is the Iceberg REST Catalog API. It is an open API. Then you have your BI and orchestration tooling, Amazon QuickSight and Airflow to orchestrate your jobs. Last but not least, then you have your Gen AI applications, and we are increasingly seeing a lot of conversations this year with our customers have been, hey, how do I make it easy for my agentic applications to consume my Redshift data, and I will talk a bit about that in a second. We see data and AI are really two sides of the same coin. The right context can make really a huge difference to the efficacy of your agentic AI applications. Remember I mentioned that the key thing here is all the access is through the IRC, the Iceberg REST Catalog API. 

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1170.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1170)

### Generative AI Use Cases: From Natural Language Queries to Bedrock Integration

What that means is that also gives you the power that this access to the whole ecosystem is not limited to AWS tools and engines. You can actually bring any third-party engine that speaks that API.  So as promised, I will close with a couple of Gen AI use cases of what customers are doing today with Redshift. First and foremost, through either the SageMaker Unified Studio or query editor, your analysts can chat with their data. This is an extremely easy solution. It helps with productivity. No longer does your analyst have to go first talk to your development team to build a SQL dashboard before they can start exploring.

That's the first thing, and I highly encourage folks to give it a try. On the query editor, it's Amazon Q Generative SQL. You'll see this branding around Q for all the Gen AI applications. Next up, remember how we talked about how data is important for the reliability and accuracy of inference? You can actually use your Amazon Redshift data as a knowledge base for your Bedrock applications. There used to be a time where you had to unload the data and make it available. All of that has been greatly simplified. We took it a step further. From your Redshift SQL command line, you can invoke Bedrock directly. In this example, we create an external model pointing to Anthropic Claude, and here you take the reviews table and perform sentiment analysis using Claude from your Redshift SQL editor.

Lastly, for a lot of AWS services, we have MCP servers, which stands for Model Context Protocol. The whole idea is that as orchestrator agents are coordinating to build your extensive agentic AI applications, we're making it easy to be part of the MCP standard. This was something that we launched about three months ago. At this point, I would like to welcome Alex to talk through Vanguard's journey with multi-warehouse architectures and various design patterns on how to build multi-warehouse architectures. Raise your hand if you are a customer of Vanguard. All right, I see a few hands. Thank you so much for being a customer of Vanguard and trusting your money with us.

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1340.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1340)

[![Thumbnail 1350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1350.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1350)

### Vanguard's Journey: From Data Swamp to Data Mesh Architecture

I'm Alex Rabinovich, Director of Data Engineering.  Vanguard is one of the world's leading investment companies,  offering low-cost products such as mutual funds, ETFs, advice, and other services. I support the Financial Advisory Services division. Think of us as a B2B business. We provide various products and services to intermediaries such as banks, insurance companies, and registered investment firms. I'm here to tell a story of how we evolved our architecture to meet our business needs.

[![Thumbnail 1390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1390.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1390)

We started our journey with building a Client 360 system.  Client 360 is an aggregate of our internal and external data around our intermediary firms. We are bringing data from our internal operational systems as well as data we can purchase from outside data aggregators. We use data for various use cases. Analytics and business intelligence use cases are a major one. We use data to set our sales goals, sales compensation, and yearly goals, and we track those goals via dashboards that sit on top of the data. In addition, we have predictive intelligence use cases such as customer segmentation, call transcription, and sentiment analysis.

[![Thumbnail 1490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1490.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1490)

We also use data for real-time customer experiences. For example, hyper-personalization. When our external advisors at intermediary firms log in to advisor.vanguard.com, we customize their experience based on what products they're looking at. That intelligence goes into our sales team, and now our sales team is equipped with information so that we could connect with the right advisors and give them the right information at the right time. Several years ago, we looked at our architecture and realized that all of our data sources are  stored in a data lake in various folders on S3. We stored data in Parquet on S3.

It was very difficult for our data analysts to build insights based on the data. They had to manually pre-aggregate data from various data formats using different tools. This resulted in multiple versions of truth because one analyst couldn't easily share their insights with another analyst. There was duplication of efforts and poor performance. It was actually very hard to join CSV files with Parquet files and derive insights from them.

[![Thumbnail 1570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1570.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1570)

We also faced inconsistent reporting. Sometimes data analysts would replicate the business rules and hand them over to another data analyst. Scaling was a challenge because there was no unified golden record on which we could build our business intelligence. We had a data swamp. We decided to go into our first wave of modernization.  We decided to build a centralized enterprise data warehouse called Client 360 on a Redshift provisioned platform. We took all the data from our data lake, transformed it through Amazon AWS Glue services, and loaded the data into Amazon Redshift provisioned server.

[![Thumbnail 1610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1610.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1610)

We unlocked several use cases including business intelligence, analytics, and data science. We saw a lot of benefits after the first wave of modernization. We now had a golden source with the  data in one place. It was very easy for us to put business intelligence on top of the centralized data warehouse. We saw significantly better performance because the data had been pre-aggregated, and whenever we queried, we could utilize the high performance compute and storage of Redshift.

We also saw better trust from the business because the data showed up consistently across different business intelligence use cases. We were able to unlock new use cases. My favorite one is comparative product analysis, where our data analysts were able to look at Client 360 data and compare the products Vanguard offered to the products that our competitors offered. As a result of that deep insight and research, our analysts recommended new products to be offered by Vanguard, which Vanguard later offered.

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1680.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1680)

Here's how much data we store.  We store over 20 terabytes of data in Redshift managed storage, over 150 terabytes of data in an S3 data lake, over 600 tables and 400 views. We have around 100 active users composed of analysts, data engineers, and data scientists who place over half a million queries a month. All of that is powered by thousands of batch jobs.

[![Thumbnail 1720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1720.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1720)

[![Thumbnail 1740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1740.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1740)

After the first centralization wave, we  saw exponential growth in the numbers of database objects, storage, and use cases. The number of database objects doubled year over year. We became victims of our own success.  We started to see resource contention issues as well as challenges with scale. For example, when we ran our ETL jobs, competing analytical workloads such as business intelligence or ad hoc queries would place stable locks, and our ETL jobs would be delayed and our SLAs would not be met.

We also saw issues with peak time performance. If our ETL jobs ran during business hours, our data analysts' user experience would be impacted because they were all sharing the same compute. Workload management complexity was another challenge. We prioritized jobs to run at a higher priority compared to analytical workloads run by analysts, which resulted in subpar user experience for our analysts.

In addition, we've seen certain database objects take a long time to be recreated. For example, materialized views would take us a long time to rebuild. In fact, for certain types of database operations, we had to rebuild materialized views on the weekends because it took so much time to rebuild them. This put a lot of strain on my data engineering department, and we decided to take our weekends back.

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1850.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1850)

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1860.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1860)

We moved from a centralized monolithic architecture  into a decentralized multi-warehouse architecture, or hub and spoke model. You can see we have provisioned multiple  Redshift instances on the right side. There are several instances provisioned by workload type: analytics, business intelligence, and data science workloads. On the left side, the architecture is the same. We still bring data from S3 into Redshift provisioned servers via AWS Glue. We store data in Redshift managed service and then we use Datashares as a mechanism to expose the data to our various workloads.

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1900.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1900)

We have seen a lot of benefits after the second wave of modernization.  We improved SLAs because we have seen reduced contention. Now our ETL jobs didn't have to compete with analytical workloads. We have also seen significantly better analyst experience because we could now put analysts on a dedicated Redshift compute instance. Analysts didn't have to compete for the same compute as our batch jobs. Previously, analyst workloads would run at a lower priority. In the new architecture, analytics workloads run at the highest priority.

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/1960.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=1960)

We've also enabled the concept of sandbox for our data analysts in the new compute where analysts could build permanent tables so that they could do more complex analysis and insights. We call it self-service analytics. We saw additional challenges emerge.  The biggest one is centralized data ownership. Managing 1000 database objects in a centralized data warehouse brings complexities such as data governance. We've also seen challenges such as a single endpoint for writes. Our ETL jobs still compete among each other, and one job may place a lock that impedes another job from completing. We saw resource contention issues on the right. We've also seen cross-domain interdependency challenges such as materialized views, and that reduced our agility.

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2020.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2020)

You're looking at our future state architecture, which is a work in progress data mesh.  On the left side, we are loading data into S3 buckets. We transform data using AWS Glue into an open file format, Iceberg. The key difference here is that we separate domains of data into separate Iceberg tables so that we could run our ETL jobs in parallel. We use incremental materialized views for high performance and we load data from Iceberg into Redshift instances. All of that data in Redshift managed storage that you see in the middle is then shared across our consumers, which are partitioned by workloads. We have business intelligence, analytics, and data science workloads.

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2100.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2100)

To recap, we can now fan out workloads on the right. All of the data producers have their own compute and all of the data consumers have their own compute. Lessons learned: start simple.  Start with either provisioned or serverless architecture with a single server and grow into your architecture.

As you hit the limits of your architecture, be ready to rearchitect. We collaborate with our AWS solution partners every time we hit the limits of our architecture, and we work with them hand in hand. Be flexible and open to adopting new features. Over several years, the Redshift team has released major features such as data share and Redshift Serverless. Every time we saw a new feature, we had to step back and understand how we could take advantage of the new features in our architecture. Track metrics to measure success. We track the number of active users, number of tables, storage costs, queries, and query timeouts.

### Demo Part 1: Building Any Company's Multi-Warehouse Architecture with Zero ETL Integration

To recap, we started with a data swamp. We moved into a centralized warehouse architecture, then we moved into a hub and spoke model, and now we're in a data mesh world. That is the journey of our evolution. With that, I wanted to give the stage to Anusha, who is going to take us through a demo. Thank you so much, Alex, for sharing your journey. It's been a pleasure for the AWS team to work with Vanguard. In this next part, I'm going to show you a demo on how to build multi-warehouse architectures.

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2210.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2210)



Let's start by looking at a graph. The graph that you're seeing on this slide is an example of a monolith. Both Alex and Naresh spoke about the challenges that you would face with monolith architectures, and this graph represents those challenges in numbers. What you're seeing in red are the number of queued queries, which means that you have submitted a query, but the query did not run and is waiting for its turn to get executed. Queuing is one of the signs of contention, and as you can see, there is a lot of red on this graph.

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2250.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2250)

If you rearchitect this as a multi-warehouse architecture with each department having their own endpoint,  in the updated graphs, you hardly see any red. This means much lesser contention in a rearchitected world. This is the positive difference that multi-warehouse architectures can bring to your workloads.

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2270.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2270)

Now let's move to the demo.  In the demo, I'm going to show you how to build an end-to-end multi-warehouse architecture. The use case that we are going to see belongs to a fictitious company called Any Company. Any Company has two main sources. One is an Oracle database that contains Any Company's sales data. The second is a clickstream source coming from their websites.

Any Company has three main use cases. First, they want to ingest all this data and extract insights from it by transforming the data and applying certain business logic to it. Second, they want to run reporting on top of the extracted metrics so that their business decision makers can use them to make business decisions. Third, they want to do data democratization. They want this data to be available for anybody who wants insights from it, regardless of whether they know how to write SQL or not. So they want to provide a natural language interface using which their users can start querying this data through the power of generative AI.

Those are the three main use cases. One thing that they want to make sure is that these use cases should not interfere with each other, especially the ad hoc queries that are coming through the generative AI applications. They should not be impacting the business critical ETL or the business critical reporting. The reporting should not interfere with ETL, and the ETL should not interfere with reporting. They should be completely isolated. The best way to achieve this is through a multi-warehouse architecture.

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2390.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2390)

So let's see step by step how Any Company achieved this through a multi-warehouse architecture. First, they have created three separate endpoints, one for each of the workloads they want  to execute. On the left, you see a data load and transformation endpoint. On the right, there is the reporting warehouse on which reporting will be done completely isolated. The third is the generative AI warehouse.

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2420.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2420)

On which they will use natural language querying to access this data. Using the first endpoint, which is the data load and transform, any company will ingest data from the sales  Oracle database using a zero ETL integration. Zero ETL integration, as some of you may be aware, allows you to bring data into Redshift in the easiest way possible if your source is supported. Does anybody in the room use zero ETL integrations today? I see some raised hands. If your source is supported by zero ETL integration, it is the easiest way to bring your data into Redshift. Your data will be continuously replicated into Redshift. All you need to do is set up once, and in real time you will have the data replicated.

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2490.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2490)

We released a zero ETL integration for Oracle this year, a couple of months back. So if you have Oracle sources, you can benefit from this. Using the data load and transform endpoint, any company is replicating the data from the Oracle RDS database into Redshift in columnar format, so their transactional data is made available for analytics within just a few seconds. The second source, Clickstream, is coming from Firehose. Firehose data can be written in Apache Iceberg format  into the fully managed Iceberg tables in S3. There is a separate pipeline that is loading clickstream data into S3 tables in Apache Iceberg format. This forms the lakehouse, both the data warehouse and the data lake.

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2510.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2510)

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2520.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2520)

They have cataloged that using SageMaker Catalog where they do permissions management. They control who  can see what using the catalog. Then reporting is done using the reporting warehouse.  And the generative AI apps use the generative AI warehouse. This completes the architecture. There is ingestion happening, there is cataloging happening, permissions management, and on the right, consumption of the data for reporting and for generative AI and natural language querying use cases. In the next couple of minutes, we are going to break this down and see how each of these works.

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2550.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2550)

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2570.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2570)

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2580.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2580)

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2590.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2590)

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2600.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2600)

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2610.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2610)

The first step is to create separate  warehouses, one for each workload: one for generative AI, one for ETL, and one for reporting. Three separate Redshift Serverless warehouses have been created. In this first part, we will use the very first warehouse, which is the data load and  transform warehouse, using which we will see how data from the Oracle sales database is continuously replicated into the data warehouse.  Let's see a video demo of this. The source is the Oracle 19C sales Oracle database. Let's quickly inspect it. It has a schema called sales schema,  which has 24 tables inside that schema. Now let's see how this can be replicated into Redshift in near real-time. For that, we will go  to zero ETL integrations and we will go down and say create a zero ETL integration. Give this integration a name. Then you can choose your source. 

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2620.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2620)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2630.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2630)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2640.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2640)

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2650.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2650)

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2660.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2660)

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2670.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2670)

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2680.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2680)

[![Thumbnail 2690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2690.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2690)

For source, we are going to choose the sales Oracle database. Once chosen, I am going to say replicate only sales by providing an expression,  saying replicate only ORCL.SALES.*. Then I will choose a target here. The target that I am going to choose is the ETL  data warehouse that I created earlier, and that endpoint will be used to do the ingestion. Once you verify everything, you can go ahead and create the zero ETL integration.  The integration is active. You can go into the integration and see a variety of metrics that are related to this integration.  You can see table level statistics, how many tables have been replicated, number of rows, what changes, what size, and so on. Now we are ready to start querying this  data. I am connecting to the ETL workgroup, which is used as a target for the zero ETL integration. There are the 24 tables from Oracle available for querying,  updated in near real time as soon as the data gets updated. At this time I can write queries. This is a query that is generating item level metrics by joining  various tables in the Oracle database and transformed to generate item level metrics, and a table called item_sales_metrics has been created. Like that,  within a few simple steps, we have been able to load the data from the Oracle sales database into Redshift, create a continuous pipeline that keeps the data updated, and we have transformed the data to create the item sales metrics.

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2730.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2730)

### Demo Part 2: Permissions Management, Reporting, and Natural Language Querying with Bedrock

Similarly, there is another pipeline that is pumping data from the firehose into the Apache Iceberg S3 table. At this point, we move to the second step. As you ingest this data, it is automatically cataloged in the SageMaker Catalog. You don't need to take any additional steps. You have the data cataloged  and available for you. Now in the catalog, you can do permissions management. There are two ways you can do permissions management. You can give permissions to IAM roles, or there is a better way. You can give permissions to your IAM Identity Center groups or users. The catalog has native integration with IAM Identity Center. IAM Identity Center, in turn, can be synced with most standard industry identity providers like Azure AD, Okta, and Ping. You can sync them with IAM Identity Center, and at this point you can start giving permissions to your IAM Identity Center users and groups.

[![Thumbnail 2800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2800.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2800)

[![Thumbnail 2810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2810.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2810)

Let's take an example. Let's assume that a company has three different groups: data scientists, analysts, and data engineers, all managed using their identity provider Okta. Now, in the next part, let's see how this company can give permissions on this new table that we have created to their analysts users so that they can do reports on top of it. For that, let's go to AWS Lake Formation. In Lake Formation, if you go down, there is  IAM Identity Center integration. You can see that Lake Formation is integrated with IAM Identity Center. This, in turn, is integrated with Okta, which has those three groups that I was talking about: analysts, data  engineers, and data scientists, which are synced with IAM Identity Center. At this point, I can do permission management on them.

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2820.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2820)

[![Thumbnail 2830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2830.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2830)

[![Thumbnail 2840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2840.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2840)

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2850.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2850)

[![Thumbnail 2860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2860.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2860)

[![Thumbnail 2870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2870.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2870)

 Let's go to catalogs. In the catalog, you see the data from Redshift, the Redshift warehouse. You also see the data from the data lake, the S3 catalog. Now let's see the tables.  In the Redshift catalog, you are seeing the item sales metrics table that we have created by transforming the data from Oracle. That's the schema for that table. I can now grant  permissions on this table to the IAM Identity Center principals. When I get started, I would be able to see the analysts  group from the identity provider reflected here, and I can start giving permissions to that. I can verify that yes, this is the right catalog, the right database, and the right table.  I'm granting select and describe privileges. Just like that, I'm able to grant privileges centrally  on both data warehouse and data lake objects to the identity provider entities using the catalog.

[![Thumbnail 2900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2900.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2900)

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2920.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2920)

[![Thumbnail 2930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2930.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2930)

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2940.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2940)

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2960.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2960)

[![Thumbnail 2970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2970.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2970)

We have done permission management. We have the objects, and we have done permission management. We said analysts can now access the item sales metrics table. They can log into any endpoint and start accessing. In this next part, we will get to reporting. The analysts will log in to the reporting endpoint  and start accessing the data. The analysts will use the same dataset. There are no copies of data made. It is the same copy of data that will be accessed using a separate endpoint, completely isolated from the previous endpoint that we are using. Let's go and see how that reporting is done. We go to Query Editor v2. Now  see that we are using the second endpoint and we are logging in using the identity provider privilege. I'm logging in as an analyst person who is part of the analyst group.  Logged in. At this point, the analyst person can see all the objects that they have access to: the item sales metrics table and the S3 tables. Let's quickly see who the current  user is. The current user is an analyst person who is from the identity provider. Now this analyst user can run reporting queries by combining the data from the data warehouse and from the data lake in Apache Iceberg format. Getting  events data and the customer data combined together, a simple query to get what is the total number of customers, how many clicks they have made, and those are the metrics.  Similarly, hundreds of reporting queries can run on this reporting endpoint, and they will not impact the ETLs that are happening on the other endpoint. That way, both these critical workloads' SLAs are met.

[![Thumbnail 2990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/2990.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=2990)

[![Thumbnail 3000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3000.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3000)

Now, the last step is natural language querying using generative AI. For that, let's go to Bedrock.  Does anyone here use Bedrock?  A few hands. Are you aware of Bedrock Knowledge Base? Awesome. So many of you must be aware.

[![Thumbnail 3020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3020.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3020)

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3030.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3030)

[![Thumbnail 3040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3040.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3040)

[![Thumbnail 3050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3050.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3050)

For Bedrock knowledge bases, you can use objects in S3. You can also use Redshift data as a knowledge base in Bedrock. Let's see how that is done. You go to knowledge bases  and when you go down, you can select create and create a knowledge base based on structured data. There you can see Redshift as the querying engine.  You can choose which specific Redshift endpoint this should be using. I'll be using the generative AI workgroup, which  is different from the two other workgroups that we are using. I can pick and choose which dataset this knowledge base should have access to. I can do additional table descriptions  and column descriptions to improve the performance of the agents. That's it—we can create the knowledge base.

[![Thumbnail 3070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3070.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3070)

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3080.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3080)

[![Thumbnail 3090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3090.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3090)

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3100.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3100)

Now you go into the knowledge base and let's test it out. You have three options, and we'll go into each of them. For the knowledge base, you can choose a model, any model that Bedrock supports.  I'm choosing the Amazon Nova model, Nova Pro. Now let's ask a question: What is the total sales amount? The agent  creates a query, runs it on Redshift, and gives the result back to you in simple, plain English. The total sales amount is so and so. Now I can  choose to just generate a SQL query. I ask the same question: What is the total sales amount? I get a SQL query as a response back. I can also just retrieve the  data, not in English, but just as a tabular format. I can say retrieve only and get the data back in tabular format.

[![Thumbnail 3130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3130.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3130)

Using the generative AI workgroup, any company can expose that workgroup to any number of users without impacting the reporting, without impacting the ETL, yet democratize the data using natural language querying. Bringing it all together, this is the architecture we just saw.  We ingested the data from various sources into a central lake house, did permissions management through identity groups and users, and did consumption for reporting and natural language querying. Alex was telling me offstage that last year their team attended this session and took these learnings back to build their multi-warehouse architecture. We highly encourage you to do the same.

[![Thumbnail 3180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ba9dbf88aa66b13a/3180.jpg)](https://www.youtube.com/watch?v=WbR_La3h578&t=3180)

For your use cases, go try out multi-warehouse architectures. Whether you're starting fresh, start with multi-warehouse, or if you're already a Redshift user, try converting that into multi-warehouse architectures. You can refer to these resources to learn more about multi-warehouse architectures.  I'll pause for two seconds so that you can take pictures and refer to them later. That's the end of our session. Thank you so much for attending. Please do take some time to fill in the survey and feedback. It'll help us a lot to improve ourselves. A special thanks to Alex for coming out here and sharing his story. We are now happy to take any questions you may have.


----

; This article is entirely auto-generated using Amazon Bedrock.
