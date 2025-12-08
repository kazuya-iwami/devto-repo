---
title: 'AWS re:Invent 2025 - Architecting the future: Amazon SageMaker as a data and AI platform (ANT351)'
published: true
description: 'In this video, Brian Ross and team introduce the next generation of Amazon SageMaker Unified Studio, addressing enterprise challenges in data analytics and AI. They demonstrate how the platform unifies data producers, consumers, and governors through a single interface supporting SQL editors, notebooks, and visual ETL tools. Key features include IAM-based domains for quick setup, polyglot notebooks with Spark Connect, serverless Apache Airflow workflows, and S3 tables with Apache Iceberg for optimized storage. The demo showcases multiple personas—business analysts, data engineers, ML scientists, and GenAI builders—working seamlessly across data sources including Snowflake integration. Carrier''s Justin McDowell shares their successful lakehouse implementation, achieving 66% lower costs and 38% improved accuracy for natural language to SQL agents using SageMaker Catalog, Lake Formation, and Iceberg-based architecture across federated AWS accounts.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/0.jpg'
series: ''
canonical_url: null
id: 3086676
date: '2025-12-05T14:10:22Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Architecting the future: Amazon SageMaker as a data and AI platform (ANT351)**

> In this video, Brian Ross and team introduce the next generation of Amazon SageMaker Unified Studio, addressing enterprise challenges in data analytics and AI. They demonstrate how the platform unifies data producers, consumers, and governors through a single interface supporting SQL editors, notebooks, and visual ETL tools. Key features include IAM-based domains for quick setup, polyglot notebooks with Spark Connect, serverless Apache Airflow workflows, and S3 tables with Apache Iceberg for optimized storage. The demo showcases multiple personas—business analysts, data engineers, ML scientists, and GenAI builders—working seamlessly across data sources including Snowflake integration. Carrier's Justin McDowell shares their successful lakehouse implementation, achieving 66% lower costs and 38% improved accuracy for natural language to SQL agents using SageMaker Catalog, Lake Formation, and Iceberg-based architecture across federated AWS accounts.

{% youtube https://www.youtube.com/watch?v=j_iCJNwaksA %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/0.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=0)

### Fantasy Football and the Next Generation of Amazon SageMaker

 There is nothing more important to a teenage boy at this time of year than winning his fantasy football league. My son is obsessed with it. There are tons of websites out there that will help prepare you for the draft, and they will go through all the players, look at every quarterback, rushing yards, passing yards, and touchdowns, trying to help you figure out who to pick. But everybody uses those websites. So if I could pull all the available NFL data and perform some predictive analytics to help him pick some people down a few rounds who will catapult him to the top, how great of a father would I be?

Good morning. My name is Brian Ross and I run engineering for SageMaker Unified Studio. We launched the next generation of Amazon SageMaker in Matt's Keynote last year, and my team and I have been working hard to evolve this platform to meet the needs of all of our varied customers who are trying to put their data to work to power analytics and AI. I'm here this morning with Saurabh Bhutyani who works directly with customers like you to make them successful in their AI journeys. Our special guest is our awesome customer Justin McDowell, who runs the data platform at Carrier.

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/90.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=90)

 This morning we're going to talk a little bit about the next generation of Amazon SageMaker and the types of problems that customers have faced, which caused us to build this platform. We're going to dive deeper on how to architect with SageMaker to make you successful. Then Saurabh will come on and show you a demo, including a lot of the new features that we just launched this week. Finally, Justin will come and talk about Carrier's journey with SageMaker.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/120.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=120)

### The AI Investment Paradox: Building Strong Data Foundations for Enterprise Success

 The data landscape is experiencing a fundamental shift. AI is taking center stage in everyone's enterprise strategy. When McKinsey ran a survey earlier this year, they found that 78 percent of companies are investing heavily in AI across all their lines of business. But over three-quarters of those have seen no meaningful impact either on revenue or on cost savings. Everybody wants to make this work, but they can't figure out how to do it at scale. We're hearing the same thing from our customers over and over again.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/150.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=150)

They're all going back to basics in order to build a strong data foundation first, in order to expose good data to power AI. As they do that, they face a bunch of challenges.  First of all, we see the lines blurring between what used to be formerly distinct roles: data engineers, data analysts, data scientists, and AI engineers. It's no longer one group doing something and throwing it over the wall to another group. Secondly, customers want to embrace cutting edge technologies, but they don't want to throw away everything they have. They've invested millions of dollars sometimes into building platforms for data. How do you get the benefits of AI and the benefits of all the new technologies that are coming out without having to rip it out and optimize what you have for higher value?

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/180.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=180)

 Finally, AI hinges on data you trust. How do you have an end-to-end AI governance strategy that can lead to trusted outcomes? From talking to our customers, we see that the journey from data ingestion all the way to analytics and AI has roughly three distinct roles, although they might be performed by the same people. Data producers spend their time ingesting data from operational data stores and from external systems. They're cleaning, transforming, and curating data, providing clean metadata, and then making it available for others in the organization to consume. These are often data engineers using engines like Apache Spark, or they're ETL engineers looking for a drag-and-drop experience.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/240.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=240)

 Data consumers meanwhile are consuming that data and using it to build solutions to real business problems. They might be data scientists training a model, analysts building a reusable dashboard to help drive decision making, or they might be building knowledge bases to power agents. Finally, data governors are responsible for enforcing the company's rules and compliance to make sure that the outputs conform to expectations like data classifications and data quality and guardrails.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/320.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=320)

### Core Challenges Facing Data Producers, Consumers, and Governors

Today, each of these personas is facing some significant challenges. 

First of all, data ingestion is hard, and it's always been hard. You have to find and create network paths to access all the data wherever it lives. You need to manage all the credentials and take the data that's in various formats and normalize it so that you can compare it or join it. Then when you do this, scaling ETL to large data sets is really hard. There's a lot of infrastructure to deal with, and even if you can figure out how to spin up and manage infrastructure, you want to do that at a reasonable cost and not pay for stuff that you don't need.

Then there's the question of how to generate business metadata out of this—not just the technical data, but how do you have columns and rows and tables that are actually understandable to the people who are going to use them? And once you've figured out all that, how do you productionize it so that you can run it over and over again while you're not there? Then you need to monitor it so you see how things change day over day as the data coming in evolves.

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/390.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=390)

At the same time, your data consumers have their own set of problems.  Everybody knows that the data they need is somewhere in the organization, but where is it? How do you find it? And even once you locate it, how do you get access to it? How do you collaborate together with people—maybe some engineers and scientists are working together on a project? And once you've done some analysis, how do you share that with others who may not have access to the same systems as you have or may not have the right skill set to understand how to use those systems?

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/440.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=440)

If you're trying to do AI, how do you find all of the pre-trained foundational models and customize them to your needs? Or how do you take the data that you've got access to and build knowledge bases to expose them to agents? While they're dealing with those problems, the data governors have their problems.  Everyone is trying to democratize access to data, especially in large organizations. But how do you do that while maintaining the right controls? How do you enforce rules? How do you make sure that data is provisioned only to those who are allowed to see it, and then you can take it away if they no longer need it? And how do you ensure that the data being used in your organization is of a quality that meets your expectations?

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/470.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=470)

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/480.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=480)

### Introducing SageMaker Unified Studio: A Single Platform for Data, Analytics, and AI

To solve problems like these is the reason that we built the next generation of Amazon SageMaker, the Center for Data Analytics and AI.  The next generation of SageMaker includes virtually all the tools that you need for fast SQL analytics, for big data processing, data prep, data integration, model development, training, and generative AI—all in one place with a single view of all of your enterprise data. 

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/520.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=520)

You get a single development environment with SageMaker Unified Studio and a lakehouse architecture that unifies access to all of your data, whether it's S3, Redshift, external systems, SaaS applications, other clouds, or on-premises. With the SageMaker Catalog built right in, you get end-to-end governance for data and AI workflows. 

To support different personas and different skill sets, the Unified Studio is a single place to access all the tools that you need to get your job done. That includes query editors, notebooks, and visual editors all together, and the tools are now consistent across all the services. Think one query editor for all the SQL databases that you access wherever they are. You can build your workloads together, test them together, collaborate together with others on them, and then deploy them together for production usage. I was meeting with a European bank a couple of months ago when they saw a demo of this, and they said, "We've been waiting years for AWS to finally solve this problem for us."

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/570.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=570)

### End-to-End Workflows for Data Producers and Consumers

Let's dive a little bit deeper into the workflow for the producer.  The next generation of Amazon SageMaker empowers data engineers and analysts to complete the end-to-end workflow producing high-quality usable data for their consumers. This includes data ingestion from a wide variety of data sources—Amazon services and also those of other providers. Once you pull that in, you use our ETL capabilities. You can do it through zero ETL or regular ETL, and regardless of what you choose, the compute will scale effortlessly to even your largest and most complex workflows. You can use simple drag and drop, or you can write Python, or you can write SQL.

It's all backed by very flexible engines like EMR and Athena and Glue. You can be fully serverless and ephemeral, or you can manage your own customized persistent clusters, but the tools all work the same way. So you've done all that and now you've produced your silver data.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/630.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=630)

So next  you want automatic generation of human-readable descriptions of your data. You can use AI to generate understanding of your data and generate it automatically, and also of course give you the power of manual curation as needed. Please don't underestimate the importance of this step. When we started on this journey, and you probably all feel the same way, business metadata was documentation for humans to better understand what's going on. But in the age of AI, you need both technical and business metadata in order to form a semantic understanding of your data, because the AI is the consumer of both of those sets. So getting this right is now critical.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/690.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=690)

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/710.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=710)

Now you want to do automatic data quality assessments and anomaly detection, allowing you to customize the rules so that you can track quality over time and even prevent updates to the data if it falls below your thresholds. Now you've got your gold copy.  So now you can publish the data, make metadata available to others in the organization, and have control over the visibility of that metadata within the enterprise. You can also easily grant access to consumers.  Along the way you need repeatable serverless workflows to automate the pipelines all the way to production and DevOps pipelines for deployment across your stages.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/730.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=730)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/740.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=740)

And then no matter which of these stages you're at, full lineage is automatically  captured and made available to you to view so you can have visibility into the entire data lifecycle.  So now you've got clean, high-quality data ready to be used. Your data analysts and scientists are now ready to use it to build real-time dashboards or train models or publish knowledge bases, agents, and all that sort of thing.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/760.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=760)

So SageMaker provides you the tools for this too. First,  you're going to search and discover all the data you need. You're going to view the metadata, the business metadata and the technical metadata like I spoke about. You're going to say, "This is the data that I need to solve my problem," and you're going to request access to that. Automatically, the data owner can look at that request and approve access either to the entire dataset or to a subset of it. Once approved, you can use our new notebook to perform data prep or ad hoc queries and generate visualizations.

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/790.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=790)

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/820.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=820)

 But the discovery doesn't stop with just data. You can also discover pre-trained models and customize them to your needs using the data that you already consumed to train them to work for your business. You should pick a bunch of these models, run some experiments in MLflow, and then pick the winner and register the winner and deploy it through SageMaker inference. Now you can use all this data  to build and deploy agents to things like Amazon AgentCom for real-world usage. So now you've got a producer flow and a consumer flow that really works for your business.

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/840.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=840)

### S3 Tables and Apache Iceberg: The Foundation for Modern Data Lakes

So we've been talking all this time about how easy it is to use the unified studio  to ingest and process your data and make it available for analytics and AI. Unified Studio gives you a built-in set of connectors that allow you to bring all that operational data in from no matter where it is. But where does all this data go? Where do you store it? How do you do that in an efficient, performant way that's also cost-effective?

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/870.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/880.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=880)

Now for many years already, the answer to this question has been Amazon S3. Customers love Amazon S3  for the unlimited scalability, durability, and low cost. To date, customers have built over 10 million data lakes on S3.  There are literally exabytes of Parquet files alone stored in S3 today. Parquet has become the de facto standard for storing this data, and it's one of the fastest-growing types of data in S3. With the emergence of open table formats, it's become really easy to create petabyte-scale tables with trillions of rows, taking advantage of the scale and economics of S3.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/910.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=910)

Apache Iceberg is  now the default choice for everybody. Hopefully everyone is building data lakes now on Iceberg. The Iceberg metadata provides a robust framework for many really cool capabilities like transaction support, time travel, and the ability to evolve your schema incrementally. Last year at re:Invent we introduced S3 tables. S3 tables are purpose-built to store tabular data using Iceberg, a fully managed Iceberg offering. S3 tables solve a few problems that you probably run into if you're managing Iceberg on S3 by yourself today.

First of all, S3 tables optimize performance, giving you about 10x the throughput of regular Iceberg tables on S3. You also get simpler security controls at the table level, including using Lake Formation grants. Most importantly, you're getting automatic storage optimization with compaction built in and garbage collection built in, without you having to worry about the process or the infrastructure behind it. All the tools in SageMaker Unified Studio are fully compatible with S3 tables, including the new notebook, our visual ETL editor, and our SQL editor.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1020.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1020)

While we believe that S3 tables is the best place to store your Iceberg data, we have thousands of customers who have already built and continued to evolve their data lakes on S3 using Iceberg on their own. The Unified Studio has full support for any Iceberg-based data that you want. You can use all the tools in Unified Studio with your existing S3 data and the Glue data catalog, with or without Lake Formation security. In fact,  if you have an externally managed Iceberg REST catalog, we can now connect directly to that as well. You can read tables in a remote catalog with all the analytics engines that AWS provides and apply permissions to them using Lake Formation right within the studio.

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1060.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1060)

### Built-in Governance and Data Mesh Architecture Made Simple

So now the choice is really yours. You can either have AWS manage your data lake for you with S3 tables, you can manage it yourself with Iceberg on S3, or it can be externally managed. The Unified Studio is great no matter which of these options you choose. Speaking of catalogs, it's time to talk about data governance.  The next generation of SageMaker has a built-in catalog for data and AI governance. It's fully integrated with the studio, allowing you to securely discover and access all the approved data, compute, and AI assets through an easy-to-use publish and subscribe workflow without any IT intervention.

You don't need an administrator to provide access to every single person who needs access to every single data asset or model. Data consumers can request access and data owners can grant it and then even revoke it later and monitor how it's used. You're getting semantic search powered by generative AI metadata, comprehensive monitoring from data quality and sensitive data detection, and even full lineage tracking. It's not just for data. You can do the same thing with models, and the number of assets that we're managing and governing within the catalog is actually continuing to grow.

A great governance system has to provide this, but it also needs to address the unique needs of AI, like toxicity detection, bias detection, and hallucination reduction. For this, you have guardrails built right into SageMaker Unified Studio and the catalog. You can use natural language to describe the types of things that you want to make sure are catered for in your organization. For example, when people ask questions about customers, you can't get out their personal information. You can talk about how the customer is using your product and the sales and things like that, but don't give their name and address or something like that. And then that will be honored in the resulting AI.

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1180.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1180)

Let's dig a little deeper into how customers are using this to architect their enterprise governance.  All of our customers are looking to democratize access to data. They want to accelerate time to insight, but they need to maintain controls and share in a secure way. Companies more and more are trying to do this using data mesh architectures. But data mesh architectures are really hard. Let's be honest, they're super complicated, and even those who build them don't understand how they work. I've been showing customers for a few years now how to use data mesh architectures, and I don't think I understand it either.

The vision takes some thought and planning, so we try to make it really easy for you. My advice here is don't overcomplicate this. Your goal is to ensure compliance while giving data workers the freedom to innovate.

The SageMaker model allows your teams the autonomy and the flexibility to work right within their own accounts. You use SageMaker Unified Studio with your team, and you can freely share the data that you're ingesting, preparing, and creating with the other members of your team. When it's ready, you would then publish that data to the enterprise catalog, and now other people can see the metadata and decide whether or not to request a subscription.

This permits a more centralized control for those who want it, while allowing the governors to define business glossaries, enforce metadata assignment, and control metadata visibility. The decision of how to architect this is really up to you. You might choose a more complicated architecture than the one on this slide if you really must, but if you keep it simple, you'll find that both your data engineers and your data scientists will feel like they can get their work done and they won't hate you.

At the same time, you can make sure that any data that hits the enterprise has the right data classification applied to it. Whether it's confidential data or has PII or things like that, you can ensure that while many teams are working independently, it doesn't become the wild west. The goal is to ensure that everyone can get their work done without thinking about infrastructure or governance, but those things are applied to them in an unobtrusive way.

[![Thumbnail 1320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1320.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1320)

[![Thumbnail 1330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1330.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1330)

### Latest Feature Launches: IAM-Based Domains, Native Notebooks, and Serverless Airflow

My team and I have been really hard at work the past year delivering new features on a constant basis into  SageMaker Unified Studio, including a few that just landed, which I'll take you through. One of the things we've heard from our customers  is that when we launched Unified Studio, we took a very nice, beautiful, clean approach and customers really wanted to provision access to their data using human end user identities and groups. They wanted to think through their data architectures and build really nice, clean solutions.

However, those journeys are really long for most people because you've got ten or more years invested in provisioning data to existing IAM roles and managing federation into those IAM roles. Customers, while they wanted to go on this journey with us, wanted to get started and get productive right away. So we launched IAM-based domains, which allow you to take the existing federated IAM roles that you already have and get started in Unified Studio right away. Within two minutes, you're already querying your data using the permissions you already have without having to go through an onboarding process.

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1400.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1400)

One of our core goals has always been with Unified Studio to give builders a really  easy to use, immersive, and rewarding experience working with their data and their models, and we've raised the bar here with a brand new native notebook experience. The notebook starts up immediately and is fully polyglot, so you can write in Python, in SQL, in PySpark, and not only write in all of these, but exchange the data seamlessly between them without you thinking about how to send data around between various engines. It automatically just happens for you.

Spark Connect is built right in. You don't have to wait for a cluster to spin up or anything like that. Just start coding and Spark is sitting there for you and automatically scales to the needs of your data. We've got great visualizations in there and a brand new data agent for AI assistance, which can not just do the code generation that you'd expect from any model or code debugging, but also can debug your infrastructure configurations of your clusters and can even do full advanced planning.

You can describe your entire use case, like I did with my fantasy football draft picker. Of course, it's tightly integrated with the rest of the SageMaker platform. If you want a deeper dive on these new notebooks as well as building CI/CD flows within SageMaker, I'd encourage you to go half an hour after this session ends, up one floor on ANT 21214, and there's a chalk talk where we're going to take a deeper dive on this. Please come to that if you're interested in learning more.

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1500.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1500)

And then the last one I'll go through  is that when we launched the platform, we had great support for repeatable workflows.

We chose Apache Airflow as the basis for that because it's the best, most widely adopted, and most flexible way to build repeatable data workflows. We relied on MWAA, AWS's managed offering for Apache Airflow, and on top of that we built a visual editor and monitoring experience right within SageMaker Unified Studio. But customers asked us to do a little bit better. They didn't want to have to provision an environment, wait a half an hour for it to come up, and then pay for it for the rest of their careers.

So we just launched the world's first and only fully serverless Airflow service, and it's built right into SageMaker Unified Studio. Now you just create a workflow either visually or in code, whichever you prefer, and then schedule it and it just starts up right away, runs right away. You can see all the results and monitor it, and you only pay for the time that the workflow is executing. This is really going to not only simplify but give you much better cost control over workflows, and I'm really excited that customers are going to get this.

### Live Demo: Financial Portfolio Analytics Across Multiple Personas

We have seen the problems that organizations are facing and how the next generation of Amazon SageMaker can help you solve them across all of your key functions and roles. I have just been telling you about it and showing you some architecture slides, but seeing is believing. So I'd like to invite my colleague Sarah Bhutyani, who is a principal solutions architect in the analytics space, to actually show it to you.

[![Thumbnail 1600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1600.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1600)

Thanks, Brian, for sharing all the latest updates on what we have been doing with SageMaker. I'm really excited about all the new features that we have launched recently, and I would like to show a demo to see that in action.  But before we get into the demo, let's understand what is the use case that we are going to build as part of the demo today.

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1650.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1650)

 The use case that I'm going to do today as part of the demo is a fictional customer use case for a financial services firm. It's a global investment portfolio analytics and AI/ML use case. The main use case as part of this overall company is to manage customer portfolios across multiple regions. They also look at risk metrics and regulatory compliance, and they want to build an ML model for detecting risk and also for portfolio optimization.

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1680.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1680)

 Let's look at the various personas that we will see in our demo today. These are primarily the data worker personas who will work with SageMaker and try to accomplish their jobs. First, we have business analysts. Business analysts typically look at data exploration and validation. They would like to go and define business metrics and KPIs. So we will see how a business analyst interacts with SageMaker and does their work.

Second, we have a data engineer. She wants to build ETL pipelines to do transformation. She has multiple tables which she wants to join and then create a flattened table for another application. Next, we will see an ML scientist in play, where the ML scientist will go and explore the different models available as part of SageMaker AutoML and then use them to build and deploy a machine learning model and an inference endpoint.

Finally, we will see a GenAI builder who is going to look at what different foundation models are available as part of SageMaker and whether they can deploy a model as part of their organization. But before we go into all of these data personas, as Brian has been talking about, we have simplified the experience with the IAM-based domains. I wanted to give you a glimpse of how easy it is for a platform admin to go and set up this entire process.

[![Thumbnail 1760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1760.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1760)

 Let's look at the architecture. If you look at the left-hand side, we have all four different personas out there who interact with SageMaker Unified Studio as part of this process. And as part of SageMaker Unified Studio, we have all of our known services, including Amazon EMR, Glue, SageMaker AI, and MWAA, which we will use as part of the demo today. From there, the personas interact with S3 buckets and tables stored on S3 in the Iceberg format as part of the SageMaker Lakehouse, going through the Glue Catalog, and then the permissions are controlled via Lake Formation to enable that.

Also, we will see that we have data stored in Snowflake as part of this use case and how easy it is for a data worker to go and connect to Snowflake and consume that data.

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1810.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1810)

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1830.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1830)

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1840.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1840)

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1850.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1850)

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1860.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1860)

Now let's go into the platform admin part  where the platform admin wants to set up a SageMaker IAM domain and create the projects. Let's see that in action. I'm on the AWS console, and from here, I'll click on Amazon SageMaker. As I come here, I see there is a get started option. I click on that. From here, I see there are options to set up  SageMaker Unified Studio, and I see I can either have SageMaker create a role for me or I can specify an existing IAM role which has access to the S3 tables,  Glue catalog, and other resources that I need for this particular use case. So I'll pick one of the roles that already has access to all of these resources.  Apart from that, you will see that I have S3 table integration enabled by default, so I'll keep it as is, and I'll click set up. I see that the setup has been completed. 

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1880.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1880)

[![Thumbnail 1890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1890.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1890)

Now that the setup is completed, I see that I'm greeted with an admin project. On the left-hand side, I see all the various tooling that is available to me. I can click on the user profile and see the IAM role and the federation role that I came in with. I can go to the domain management as an admin and create different projects for different use cases.  I see there is a portfolio optimization project already created for today's demo. All right. So we go into the portfolio optimization and the first persona that we will look at  is the business analyst.

[![Thumbnail 1910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1910.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1910)

[![Thumbnail 1930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1930.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1930)

[![Thumbnail 1940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1940.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1940)

As a business analyst, I'm responsible for analyzing the top performing securities by unrealized profit and loss as part of this use case. All right. So we go in as a business analyst into SageMaker Unified Studio. Now, the first thing as a business analyst I want to do is find out what are the different tables  available to me as part of the use case that I'm trying. So I go and hit search and I enter financial as the term. I see there are various Glue tables available to me. There are some models available as well. I can go and click on one of the tables. When I click on the tables, it brings me into the data catalog view where I can see the schema of the table. I can also go and click on preview  data to look at the data in action. And then I can also click on details to look at various details about the tables, including advanced properties, like  what are the formats and other things.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1950.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1950)

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1960.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1960)

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1970.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1970)

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/1980.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=1980)

Perfect. Now, next thing I want to see is that I have an S3 table with Iceberg, which is an audit table as part of the business analysis.  And this table stores all the metrics related to what actions users are performing as part of this use case. So I go and open that and I click on query.  As I do query, I see that I'm opened in a query editor, and the query opens there, and I can see the results here. Now, as a business analyst, if I wanted to download  these results, I can do that and send it via email to my management if I need to. I can also do a quick visualization by clicking on the chart view, and I can go and change  the different parameters like X-axis, Y-axis, and do a couple of other things.

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2000.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2000)

Plus, this is a SQL notebook which offers you the option to add markdown and SQL. I can select the different catalogs which I want to work with, and I can also do a one-click scheduling for the SQL notebook. Finally, I can save this and run the final query that I want to do as part of  my business analysis, which is the top performing portfolio questions. So here I see the results, and that's pretty much as a business analyst, my job is done. If I wanted to go and schedule this notebook further, I could do that.

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2020.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2020)

[![Thumbnail 2030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2030.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2030)

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2050.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2050)

Next, let's look at the data engineer persona. Now, as a data engineer, my task is that I am building a web  application called Portfolio Advisor for this use case, and I want to build an internal data product or a flattened table that will be used as part of this web application.  All right. So let's see the data engineer in action. Now, as a data engineer, on the left-hand side, I see that there are various tools available, but I want to see if there is a low code, no code option for me where I can go and visually build my ETL pipeline. So let's see that in action. So I'll go and click on Visual ETL which is our ETL tooling. I can click on create job here. 

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2060.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2060)

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2070.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2070)

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2080.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2080)

And as I do that, I can see that there's an option where I can even describe my entire ETL pipeline as a prompt. But for today's demo, we will just do a drag and drop and build that pipeline.  So first, I take the Glue catalog and I pick up the Financial Services DB and the table customers. And what I want to do is make sure that this table does not have any null records dropped out from this table, so it's a clean table when I'm using it. So this is my clean data customers ETL pipeline. And as part of drop nulls, I say that I don't want the customer ID to be anytime null.  Right? If it is null, then drop those records. I save that and I do the other thing for the transactions table as well. And then I click on run. 

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2090.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2090)

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2100.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2100)

And apart from that, I have the option to go and create schedule as well, right? So I can go and run this ETL pipeline on a schedule. I can also click on view script, which shows me the code that is generated. This is an  open source Spark code that is generated behind the scenes as part of the Visual ETL. I can also clone it as a notebook and download it. 

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2110.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2110)

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2120.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2120)

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2130.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2130)

Let's click on view runs. I see the run has succeeded.  Now let's see what we do next. We'll go back to the visual ETL and I see that I have both the ETL jobs.  I built the final ETL job, which is a flattened join of the transactions, customers, and holdings tables, and I run it. Now as it is running, I can see output logs generating here like a tail, so I can also see what is happening with the job. 

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2140.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2140)

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2150.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2150)

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2160.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2160)

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2170.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2170)

But hold on, what happened here? I ran into an error. What do I do? I have an option to troubleshoot with AI.  What troubleshoot with AI does is it not only tells me where the problem is, but it also tells me the code where the problem is. I see the problem is that I'm joining two tables which have the same column names, so I need to go and rename one of the columns.  I rename the holdings table's quantity column to holdings_quantity. That's what I do, and I save the job, and then I'll go and run it again.  So the job run started. I go back and click on view runs, and I see the job has succeeded.  So AI came to my rescue for helping me solve that problem.

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2180.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2180)

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2190.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2190)

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2220.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2220)

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2230.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2230)

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2240.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2240)

Now, since I have built all these three jobs, I want to create an orchestration so that I can schedule this. So as Brian was talking about, we have the Apache Airflow service integrated into SageMaker Unified Studio.  So I take advantage of that. I go to workflows, and I say create new workflow. I'll click on create new workflows.  So create new workflow. And when I do that, I see I have options for various operators in Airflow and I have even the SageMaker Unified Studio operators. I take the data processing operator, which gives me the Glue task as an option. And here, I can go and give a name to the task, like I have the clean data for customers and other things that we did as part of the Glue ETL job.  So I do that and I finally build my workflow here. And under actions, I can even go and look at the code and see that it has created the entire Airflow DAG for me without even writing a single line of code.  So very beautiful. I can also download this if I need to and run it in another place if I want to. 

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2250.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2250)

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2260.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2260)

[![Thumbnail 2290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2290.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2290)

And then here I can go and schedule this workflow, right? So if I wanted to get this new flattened table and the data product created on a nightly basis and refreshed it, I can go and set up the settings here.  So awesome, right? We could see how the data engineer can do their work. Now let's move on to the next persona, which is our ML scientist.  Now as an ML scientist, my job is first to understand what are the risks across the customer portfolios that's happening as part of this use case. And once I understand the risk, I want to build an ML model which can do portfolio optimization for these customer portfolios, right? And obviously, I want to at the end deploy this model for doing real-time predictions as changes are happening on customer portfolios. 

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2310.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2310)

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2320.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2320)

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2330.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2330)

[![Thumbnail 2340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2340.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2340)

So we again go back here. Now as an ML scientist, what I also see is that I have some of my data sitting in Snowflake. It's not in Glue catalog or S3 tables, right? It is sitting in Snowflake Horizon catalog. So let's see that in action, how ML scientists can go and connect to Snowflake and consume that data. So I'm in Snowflake right now here, right? And I go and search for the database, financial portfolio analysis in the Horizon catalog, and I see the database.  I click on that and I can see various tables under the public domain, including analyst ratings, currency exchange rates, and economic indicators.  So I want to connect to that. So what I do is in SageMaker Unified Studio, I go to connections and click on create connection. I see there are various connection options available here, including Snowflake.  I can click on that and I can fill in the parameters, which I skipped here. And then I see that the connection is created. 

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2350.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2350)

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2370.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2370)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2380.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2380)

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2390.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2390)

Now I go back to my data catalog and under connections, I can see all the three tables showing up from Snowflake for me within Unified Studio. I can click on sample data and see the data from Snowflake coming here.  Also, as a data scientist, typically I would like to work with S3 bucket and upload files or download files. So there is also a good option in the data catalog where I can look at the buckets and do that. All right. So now the data scientists typically work with notebook experience, right? They love that. So we have this new notebook experience that Brian was talking about, and we have the polyglot notebook experience that you just saw with the various options where I could select one of them.  I do a query, I can filter the results here. I can also sort the results quickly. And then I can even do a visualization on top of this data using Python itself.  I can change the chart type from here easily to a pie chart, and it makes it so easy to do that. 

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2400.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2400)

Now next, I'm doing a query on the Snowflake data that we connected to. So you can see here on the right-hand side that we have selected next to SQL, the Snowflake connection type. 

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2410.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2410)

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2420.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2420)

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2430.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2430)

[![Thumbnail 2440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2440.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2440)

I can see the data now. What I will do is take the output from both of these cells. I queried the Glue table  and I queried Snowflake, and I'm joining these two using Python. I can see the data in the notebook coming from both datasets. Now if I wanted to use the output of this data  and create a new cell which is of type chart, I could do that by selecting the dataframe, which is our Pandas dataframe from Python that came out of the join, and I can do a quick visualization.  This is amazing. I can do all the visualizations and all the queries across multiple datasets, no matter where the data is sitting. Then I continue my risk analysis  by querying some of the other tables like customer portfolio summary by region. I ran into an error again and I need some help. So I click on fix with AI and I see the data agent comes into action and starts printing out.

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2460.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2460)

[![Thumbnail 2470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2470.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2470)

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2480.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2480)

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2490.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2490)

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2500.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2500)

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2510.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2510)

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2520.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2520)

[![Thumbnail 2540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2540.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2540)

The data agent comes into play  and it analyzes what's happening across this particular cell and why the error came. It actually understands that this error is coming because I'm trying to run a query with Python, but the table is actually in Glue, so it needs to go against Athena SQL for running this query.  It understands that and it goes and rectifies the error in the cell. It tells me whether I can accept or accept and run this particular cell or reject it.  I can see all the changes with the version differences, and I go and accept those options.  I see that it has updated my type to Athena SQL, and I can go and run this particular query. Let's run this query, and hopefully it should succeed if the AI is good.  I see the results. That worked for me. Now I continue doing the rest of the analysis, which I will do in fast forward. We're doing some Plotly graphs and other things as an ML scientist in the notebook.  All of those features and libraries are already available out of the box. Finally, I calculate the risk ratings for these customer portfolios and look at the top 10 highest risk customers by value.  I can see all of those results. Since I was running this as Spark code, I would like to look at the Spark UI and analyze what's going on. We have an option where you can do a one-click to the Spark UI and you come into the Spark UI.  As a typical Spark developer, I would also like to look at the driver logs in case something goes wrong, so I can do that as well. It's pretty easy to do all of that.

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2550.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2550)

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2560.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2560)

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2570.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2570)

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2580.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2580)

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2590.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2590)

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2600.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2600)

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2610.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2610)

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2620.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2620)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2630.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2630)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2640.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2640)

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2650.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2650)

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2660.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2660)

Now, next thing I'm doing as an ML scientist is I want to start building my model. For that, I want to make sure that the experiments are tracked with MLflow. That's where I'm in MLflow and adding all the details for the MLflow server.  All good. Now we'll go back to the notebooks and start building the model using SageMaker to do the portfolio optimization and recommendation.  First thing I do is I have the notebook and I run the boilerplate code to get the project details and the S3 bucket and all those things. Next, I go in and I start selecting some of the data from another table, portfolio optimization.  And again, I run into an error where I'm connecting to MLflow. I see the AI detected that I have an MLflow server in my project, but the name is different and it corrected it.  Very good. Next, I continue with my SageMaker AutoML job.  I use the AutoML features to build the job. I can see the updates in MLflow.  So I go to MLflow, I see there is an AutoML portfolio job created here. I can look at that. Plus, I can see that on the left-hand side, I have other jobs as well, like fraud detection and other things that I've been doing.  I can click on any of these options and look at the model metrics as well. It makes it so simple that I can jump to MLflow and look at all the metrics.  Since we used AutoML with SageMaker, there should be training jobs that were launched, so I can go to training jobs UI and look at all of the jobs.  Obviously, I want to deploy an inference endpoint after doing the training. So I go to the inference endpoint and I see the endpoint is deployed.  Now, finally, since the endpoint is deployed, I want to run a real-time inference to test it. So I will go into the notebook again, and I have some sample data here to go and test the inference endpoint and the model  before I put it into production mode. So I go and run that, and I can see the scores coming out with the portfolio recommendations as part of that.  This is beautiful. As an ML scientist, I was able to go in and build my model, do the risk analysis, do visualizations, use Spark, use Python, connect to Snowflake, all of that as part of SageMaker Unified Studio in a seamless fashion.

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2680.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2680)

Now, next, we will go into the GenAI builder persona.  As a GenAI builder, since I'm working with the financial services domain and the project, I have a restriction that I cannot use the publicly available GenAI models out there. My organization has tasked me to deploy a model internally that will be used for powering the chatbot agent as part of the web application that the data engineer is building.

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2720.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2720)

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2730.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2730)

[![Thumbnail 2740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2740.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2740)

I want to see what options are available to me in SageMaker Unified Studio. I go in here and as a GenAI builder, I see under AI/ML there is an option for models. I click on that.  I see there are various models listed from various providers like Meta, Hugging Face, OpenAI, and so on. I'm interested in a Mistral Lite model for my application.  So I go and search for it, and I click on it. I can see the overview with the licensing details and the way to use the payload, and I can open this in a notebook as well. 

[![Thumbnail 2750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2750.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2750)

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2760.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2760)

So that's what I do. I open the code in the notebook and I go and run that. I see that the model is getting created and deployed as an inference endpoint. Within a few seconds, we should see  that. While it is getting deployed, I can also go and look at the details, like what instance type and other things the model is getting deployed with.  Let me close that and come back here and refresh, and it should be in service. So it looks like the Mistral Lite model is available for me, and I can now do inference against it.

[![Thumbnail 2780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2780.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2780)

[![Thumbnail 2790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2790.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2790)

[![Thumbnail 2800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2800.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2800)

Let me go back to our notebook and I can see that I have some predictor and I have the option to invoke the endpoint. There is an example payload as part of this  notebook that SageMaker Unified Studio provided me, and I can see that it works fine. It gave me an input and the output that I was expecting as part of this  model. Finally, if I wanted to go and delete this inference endpoint, if I did not like the output from this model and I wanted to use something else, I could go and do it. 

[![Thumbnail 2840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2840.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2840)

As you saw in this demo, multiple personas can use the new SageMaker IEM-based domains and the amazing capabilities we provide as part of the new notebook experience with the built-in data agent. This can help you build the future state architecture and data strategy for your organization. For today's demo purpose, I did not write a single line of code. Everything was generated using the data agent as part of the notebook to create the demo. With that, I would like to invite Justin from Carrier to talk about their success story. 

[![Thumbnail 2880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2880.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2880)

### Carrier's Success Story: Building Nexus Data Lakehouse with 66% Cost Reduction

My name is Justin McDowell, and I lead data platform and data engineering for Carrier. Carrier is a global leader in intelligent climate and energy solutions. Our heritage goes back to 2002 when Willis Carrier invented the modern air conditioning. Today we operate in nearly 160 countries with 48,000 employees and a portfolio of 35 brands.  That scale gives us a massive and complex data landscape, and it sets the stage for our modernization journey.

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2920.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2920)

Our data strategy had to support four business priorities. Customer centricity, which means deeper insights and more tailored service. Energy ecosystems, where our products and services are increasingly integrated. Digital solutions with connected devices driving real-time data needs. And margin expansion, requiring efficiency and avoiding lock-in.  These priorities shaped every architectural decision. That context exposed three core challenges. The first was data sprawl with fragmented systems and rising legacy warehouse costs. Governance with inconsistent lineage, access, and data quality. And lastly, scaling operations, where every project was rebuilding pipelines from scratch.

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/2960.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=2960)

These pain points led us to rethink our approach to a data platform which internally we call Nexus. We address these challenges with an Iceberg-based data lakehouse on AWS.  Data flows from SAP, JDE, and Salesforce and other systems through Qlik Replicate, Amazon Kinesis Data Streams, and AWS Glue zero ETL. The ingestion account separates our producers from the governed lake. In the data lake account, Glue, the SageMaker Catalog, and Iceberg managed schema, governance, and ACID table creation across raw, bronze, silver, and gold layers.

SageMaker Unified Studio provides us with lineage and access control all in one place for these layers. For workloads that still require a warehouse, we integrate Snowflake as a reduced capacity consumer.

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/3030.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=3030)

So how do we scale this? The first way is to ensure that each line of business gets its own AWS account. Those producers all follow the Nexus Lake House standards, with raw data, published data with Glue, data quality, and Lake Formation all built in. A centralized governance account becomes the single portal for access, lineage, and metadata. Teams publish and consume data products consistently without custom engineering, which gives us federation with control. 

[![Thumbnail 3070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/3070.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=3070)

So how long does something like this take? We started in December, shortly after re:Invent, with an Amazon-style PRFAQ and architecture workshops. By March, non-prod was live with our first model data products. In May we onboarded our first production workload. We have spent the rest of this year focusing on onboarding products, projects, and the developer experience, making it even easier for teams to build and publish data products. 

[![Thumbnail 3120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/3120.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=3120)

Nexus is already delivering measurable impact. We have achieved 66% lower implementation costs versus our legacy pattern. We have standardized pipelines that accelerate delivery and reuse. And we have seen a 38% improvement in accuracy for our natural language to SQL agents. Clean governed data pays off immediately. 

[![Thumbnail 3150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/3150.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=3150)

What's next for us is doubling down on our intelligence layer on this platform. We are focusing on integrating more MCP servers to extend coverage across more domains and have tighter integration with AWS services. We are also deploying agentic workflows that automate lineage, quality checks, and publishing. This accelerates delivery while strengthening governance. 

[![Thumbnail 3180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/3180.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=3180)

I would like to share with you three key lessons that emerged from this process. The first lesson is to choose open formats, as they give you flexibility as the technology evolves. The second is that governance is not a retrofit; build it in from day one. And lastly, use AI agents responsibly. Automation is the only way to scale governance and quality without adding headcount. These principles have guided every decision we have made. 

[![Thumbnail 3250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e10e72fc74aeedbe/3250.jpg)](https://www.youtube.com/watch?v=j_iCJNwaksA&t=3250)

I would like to give a special shout out to my AWS account team—Bobby, Keith, Yanni, and Sachin—for helping us on this journey and helping us achieve these results. Thank you all for coming and joining us this morning. We hope that you learned something that you can take home about Amazon SageMaker. 


----

; This article is entirely auto-generated using Amazon Bedrock.
