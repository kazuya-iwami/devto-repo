---
title: 'AWS re:Invent 2025 - Accelerate analytics and AI w/ an open and secure lakehouse architecture-ANT309'
published: true
description: 'In this video, Pavani Baddepudi and Mahesh Mishra from AWS, along with Achal Kumar from Intuit, explore lakehouse architecture for AI and analytics. They discuss how AWS Glue Data Catalog serves as the metadata backbone, handling billions of requests weekly and managing hundreds of millions of tables. Key announcements include the MCP server for Glue Data Catalog enabling natural language data interaction, 23 Zero ETL integration sources, catalog federation with remote systems like Snowflake''s Polaris, and Iceberg V3 support with deletion vectors and row lineage tracking. AWS Glue provides automated compaction, snapshot retention, and materialized views for Iceberg tables. Intuit shares their real-world implementation using dual native catalogs (AWS Glue Data Catalog and Unity Catalog) with Apache Iceberg format, managing 300,000+ tables and 200+ petabytes while enforcing row-level and column-level security across 70,000+ production pipelines without user disruption.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/0.jpg'
series: ''
canonical_url: null
id: 3088514
date: '2025-12-06T09:16:10Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Accelerate analytics and AI w/ an open and secure lakehouse architecture-ANT309**

> In this video, Pavani Baddepudi and Mahesh Mishra from AWS, along with Achal Kumar from Intuit, explore lakehouse architecture for AI and analytics. They discuss how AWS Glue Data Catalog serves as the metadata backbone, handling billions of requests weekly and managing hundreds of millions of tables. Key announcements include the MCP server for Glue Data Catalog enabling natural language data interaction, 23 Zero ETL integration sources, catalog federation with remote systems like Snowflake's Polaris, and Iceberg V3 support with deletion vectors and row lineage tracking. AWS Glue provides automated compaction, snapshot retention, and materialized views for Iceberg tables. Intuit shares their real-world implementation using dual native catalogs (AWS Glue Data Catalog and Unity Catalog) with Apache Iceberg format, managing 300,000+ tables and 200+ petabytes while enforcing row-level and column-level security across 70,000+ production pipelines without user disruption.

{% youtube https://www.youtube.com/watch?v=qIsxBWfVgsM %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/0.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=0)

### Introduction: Building AI-Ready Data Foundations in the Age of Agentic AI

 Hey, welcome everyone and thank you for joining us today. Today we'll explore how to accelerate your analytics and AI using open and secure lakehouse architecture. I'm Pavani Baddepudi, accompanied by Mahesh Mishra, Principal Product Manager from AWS. We also have a special guest, Achal Kumar, Director of Data and Analytics from Intuit, who's going to be discussing his real world implementation of lakehouse architectures. So whether you're just getting started or you're looking to optimize your existing lakehouse architectures, you'll walk away with practical insights to help you succeed in your data journey.

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/50.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=50)

 Agentic AI is the next step beyond generative AI. It doesn't just answer questions, it also takes actions on your behalf. As you embrace AI-driven decision making and automation, the quality and accessibility of your data becomes even more important. And that starts with a strong data foundation. For a strong data foundation, you really need a strong storage management system. What that means is that you're able to bring data from different systems and formats and make sure that it's securely stored, organized, and is accessible.

Then comes data processing and transformation. Here we're talking about raw data being transformed and enriched into these high-value AI insights that the models can take decisions based on. Next we're talking about data catalog and governance. Data catalog gives structure and governance gives trust. With data catalog you're able to discover your data in any part of your organization, and with governance you ensure it's compliant, consistent, and used responsibly. It's more essential than ever to get your data foundation right, and rather than building new systems or new silos for AI, you basically leverage your existing systems while maintaining security and governance. But building a foundation in the real world isn't easy.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/150.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=150)

 In most enterprises, data lives in dozens or even hundreds of different systems: operational databases, SaaS applications, data lakes, and data warehouses. Each system has its own access patterns, its own security models and APIs, so for AI agents that need comprehensive context to make decisions, this fragmentation creates massive integration overhead. When agents need to query multiple systems, each request adds latency, network hops, authentication handshakes, and format conversions. This fragmentation leads to performance bottlenecks and makes it hard to scale AI workloads efficiently.

Data quality and governance becomes inconsistent because you're repeating rules across systems, and when AI agents need access to sensitive data, they're basically introducing a new attack vector which requires you to have fine-grained access controls to make sure that you have better control on what it's accessing and when. And you also need auditability, and to top it all, you need to make sure that you have reliability guarantees so that agents don't make decisions on stale or incomplete data. All these challenges make it difficult for teams to really execute efficiently with the speed, agility, adaptability, and flexibility that the AI agent systems need.

[![Thumbnail 250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/250.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=250)

### Lakehouse Architecture: Solving Data Fragmentation with Unified Access and Governance

 The lakehouse architecture directly addresses these challenges through three technical capabilities. First, discoverability. Your AI systems and developers need to quickly find the right data. The lakehouse provides rich metadata management, making it easy to discover the available datasets. Next, consistent data access policies. Instead of managing security rules across multiple systems, the lakehouse implements centralized access. One set of policies govern all your data, ensuring consistent security policy and compliance across all AI operations.

Third, we build this on open, scalable systems. By supporting open formats, you leverage a broad range of tools, right? What this means is you can bring the tools of your choice, whether it's Spark, Presto, Athena, Redshift, all working with the same underlying data without having to worry about conversions or duplications.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/320.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=320)

Let's talk about the lakehouse architecture of Amazon SageMaker. It starts with Amazon S3  providing the foundation layer, that's your storage layer. S3's object storage model is really ideal for data lake architectures because it can scale independently. The storage and compute can scale independently.

We also have S3 Tables which provides a fully managed storage layer for Apache Iceberg tables optimized for analytics workloads. Then we have Redshift, which is your data warehouse. It contains your structured, highly curated analytical data. Redshift really works well when you have complex queries and you have to join across large tables, and you require consistent performance.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/370.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=370)

Next we have Glue Data Catalog that sits in the center of this as a metadata layer.  It maintains schema, partition information, table statistics, and table locations for both S3-based tables as well as Redshift tables. Starting this re:Invent, every new Redshift cluster automatically registers with Glue Data Catalog. So what this means is you define your metadata once and all services can discover and use it.

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/400.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=400)

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/410.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=410)

Then we have a critical layer that's the Iceberg layer.  Iceberg basically gives you the standardization. What that means is it gives you a standard way to read and write the data. And that makes it easy  for Apache compatible engines to start reading the data from storage without really having to worry about conversions.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/420.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=420)

We also have fine-grained access  control. We do that through Lake Formation that's closely tied with Glue Data Catalog. When a query engine makes a request, Lake Formation intercepts that, checks permissions, and returns data after applying column and row level filtering. With this, what you get is every query engine basically reads the same data. It gets the same consistent security policies and can benefit from the same metadata.

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/460.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=460)

### AWS Glue Data Catalog and Model Context Protocol: Making Metadata Accessible Through Natural Language

So Glue Data Catalog  is truly the backbone of our lakehouse architecture. It operates at unprecedented scale. Today it's used by hundreds of thousands of users and basically runs billions of requests weekly. It's also managing hundreds of millions of tables. These numbers don't just demonstrate scale, they also demonstrate the reliability that customers depend on for their mission critical workloads.

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/490.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=490)

The Glue Data Catalog acts as a  central metadata repository. It's highly available, scalable and durable, and it offers high availability in a serverless cost effective fashion. The catalog is Open API compatible, Hive compatible, and supports open table formats, namely Apache Hudi, Iceberg, and Delta. It also integrates deeply with the AWS analytics services and the governance layer providing fine-grained access controls.

It can federate with external catalogs. What that means is you can discover your data across your organization using Glue Data Catalog in one single interface. It also provides strong security and auditability capabilities. In a sense, Glue Data Catalog is basically transforming your lakehouse into a self-describing trusted data platform where every data set is discoverable, governed and optimized for intelligent use.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/560.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=560)

Now we talked about the metadata.  Next is, how do we make this metadata accessible? Typically scientists, analysts, and users have to learn APIs, they have to execute SQL queries or navigate consoles. In today's AI driven world though, we need a more intuitive way to be able to interact with our catalog, one that speaks the user's language and not just the systems.

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/590.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=590)

We now introduce the Open Standards MCP server for  Glue Data Catalog. MCP, which stands for Model Context Protocol, is an open standard that enables AI to directly interact with your data.

With this, you can now explore your data. You can actually even create or update table schemas using plain English requests. The MCP server extends to Lake Formation policies, so the policies that used to protect your analytics will also protect any access by AI tools. And because we built this on the open MCP standard, it works with a wide range of MCP-compatible agents, not just AWS services. You can use Claude, you can use custom agents built on LangChain and any other MCP-compatible tool, and they all respect your security policies.

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/650.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=650)

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/660.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=660)

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/670.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=670)

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/680.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=680)

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/690.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=690)

So let's walk through this demo.  Here, what we are looking at is Oktank, a multi-regional company.  Particularly, we're examining the customer feedback table. What we've done is for the MCP, we've restricted access, and it's not able to access the PII data. So the first thing we're doing here is listing all the databases.  Next, what we'll do is query the feedback table, the customer feedback table. Notice how your English request is converted into a query.  And you can see that you get all the results except the customer's name and  email address.

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/700.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=700)

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/710.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=710)

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/720.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=720)

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/730.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=730)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/740.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=740)

Next, what we'll do is try to drop the table to see if that works.  And you can notice that it wouldn't work because the MCP server doesn't have the permissions to be able to do that.  Lastly, now what you can do is explore more of your customer data itself.  So you can watch how your natural language request is converted to analyze the customer sentiment  across regions. And you can further explore furthermore. 

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/770.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=770)

### Three Pathways to Enterprise Data Integration: Zero ETL, Query Federation, and Catalog Federation

So now that the MCP server makes it easy for you to discover your data and interact with your data through AI, we need to ensure that all your enterprise data is available for this lakehouse architecture regardless of where it originates. Our goal here is to bring the data from wherever it originates in any parts of your organization and make it accessible without you having to create complex pipelines or having to create new data silos.  And we have three complementary methods to be able to do this.

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/780.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=780)

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/810.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=810)

First, Zero ETL integrations.  Zero ETL is a pattern where data is automatically and continuously replicated from the source systems into your lakehouse with minimal manual intervention. Within seconds of transactions being written into your operational databases, it's available in Redshift. Zero ETL handles both initial full load as well as ongoing change data capture, giving you near real-time access to your operational data. 

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/850.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=850)

We've significantly expanded our Zero ETL offering and now support 23 unique sources across three categories: AWS databases. This year we added support for RDS for Oracle, RDS for Postgres, and Oracle database at AWS. We also added support for self-managed databases. This re:Invent, you can run Zero ETL integrations between MySQL, Postgres, SQL Server, and Oracle that's deployed on-premises or on EC2, and you can replicate the data to Redshift. Zero ETL also supports S3 Tables as  a target. What that means is you can ingest data from DynamoDB and you can ingest data from third-party applications into S3 Tables in Apache Iceberg format.

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/870.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=870)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/900.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=900)

Second, we're talking about query federation. This is ideal for on-demand access with no data movement.  Query Federation lets you run SQL queries across multiple source systems that span across different clouds as well without having to move data. The query engine federates these sources in real time, queries are pushed to the source systems, and the results are returned. So this is perfect for on-demand analytics. We have a wide range of support for these sources.  Third is something that we released just this re:Invent: catalog federation.

It provides centralized discovery with optimized performance. This is our newest capability. Catalog Federation in AWS Glue Data Catalog provides direct and secure access to Iceberg tables that are stored in S3 but are cataloged in remote catalogs. You can use AWS engines now to directly access this data.

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/930.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=930)

 So how this works is, first you establish the connectivity with the remote catalog. Once Glue Data Catalog communicates with the remote data catalog, it discovers where your Iceberg tables are. It also discovers the metadata information. And next what it does is it registers this catalog as a federated catalog in Glue Data Catalog. What that means is now your query engines, when they're going and reading the data, they have the location of that particular data. They already have the location of the data, so they can directly read the data from S3 rather than having to push it to the source systems. You can also apply security policies with Lake Formation. So Lake Formation becomes the centralized place where you can build governance for your data.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/990.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=990)

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1000.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1000)

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1010.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1010)

So let's take a look at the demo. Here we're seeing a demo with Snowflake integration.  In this scenario, what we're doing is we see that Snowflake handles data ingestion and processing with metadata cataloged in Polaris.  The customer wants to use AWS services to be able to query this data. So what we are doing is we are first  establishing connection using OAuth, and you can see that this IAM role that you see is what is being used by Glue Data Catalog to connect to the remote catalog, that is the Polaris over here.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1020.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1020)

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1030.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1030)

[![Thumbnail 1040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1040.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1040)

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1050.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1050)

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1060.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1060)

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1070.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1070)

 Once you have this established, it gets mounted as a federated catalog, and now you can see  the tables over here. You can also now set up tags on this data.  Here what we are doing is we are restricting the query engines to only access any columns with public tag.   So once we've established this connection, typically this is administered  by an admin. Then we go to SageMaker, SageMaker Studio, and now you can directly query the data.

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1080.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1080)

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1090.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1090)

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1100.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1100)

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1110.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1110)

 And you'll notice that it's only returning the data, the columns with public tags, showing Lake Formation column-level security in action.   So, which one to use?  Each approach offers distinct advantages. Many organizations use a combination of these approaches as part of their data strategy. You may use Zero ETL for your core operational data when you need the latest or the fastest query response times. Query Federation really works well for on-demand cross-system analytics. And Catalog Federation really works well when you want to integrate with remote catalogs while you want to leverage the price performance benefits of AWS analytics engines.

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1150.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1150)

[![Thumbnail 1160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1160.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1160)

### Apache Iceberg: Transforming Data Lakes with ACID Compliance and Advanced Features

Now we brought the data in, and the next question is, how do you  ensure that it's stored and managed in a way that supports reliability, performance, and flexibility that modern AI and analytics workloads demand?  Open table formats bring you these capabilities. Apache Iceberg has emerged as the leading open table format that transforms the way you're managing your data lakes and lakehouses. Let's see why it's so powerful. First, it provides ACID compliance: atomicity, consistency, isolation, and durability, ensuring your data lakes and lakehouses can handle concurrent transactions with reliability, which was previously only available in databases. Multiple users can now read and write your data simultaneously without any corruption, without having to worry about any corruption. Second, it provides scalable metadata handling.

Iceberg maintains consistent performance even with billions of files and petabytes of data. Third is schema evolution and enforcement. Iceberg allows you to add, drop, rename, reorder columns without downtime or compromising data integrity. Next, last but not least, is time travel. It lets you maintain a history of table changes, allowing users to query previous versions for data auditing, debugging, and recovering from errors.

[![Thumbnail 1240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1240.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1240)

[![Thumbnail 1250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1250.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1250)

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1260.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1260)

When integrated with AWS Glue Data Catalog,  Iceberg provides high consistent performance data access control across all your engines from Athena to Redshift to EMR and SageMaker.  At re:Invent, we also announced support for Iceberg V3. AWS services now support deletion vectors  instead of rewriting the entire files when the rows are deleted, which can be extremely costly and time consuming. Deletion vectors maintain a lightweight bitmap that marks the rows that have been removed. Where this helps is it improves your query performance because the engines can now skip deleted rows efficiently during scan.

Next is our support for row lineage tracking. This capability gives you complete visibility into where the row originated and how it was transformed. It's extremely useful for audit trail compliance and impact analysis, that is to understand how the downstream services get affected before you make any changes to your data pipelines.

### Operating Iceberg at Scale: Automated Maintenance, Materialized Views, and Performance Optimization

Now that we understand the powerful capabilities of Apache Iceberg, I call upon my colleague Mahesh to discuss critical aspects of managing Iceberg tables and security controls at scale. My name is Mahesh Mishra. I'm a Principal Product Manager with AWS Analytics. Before we start talking about how to operate an Iceberg compatible lakehouse with Iceberg as the open table format as well as Iceberg REST catalog as the interface, let's do a poll. How many of you really use Iceberg in production? About 50%, this is good.

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1370.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1370)

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1380.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1380)

So basically what we do with AWS  Glue Data Catalog and Lake Formation is we make it incredibly easy for you to operate an Iceberg compatible lakehouse. So the key problems that customers  experience when they have Iceberg as a table format is when you write data into your tables, you create new versions of the data and you create a lot of stale data sets in your lakehouse that increases your storage cost. As your scale increases, you see performance issues, and then transformation of data becomes harder. Finding the right data set, finding the data of what changed, and being able to transform it in the right way becomes harder.

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1440.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1440)

With AWS Glue Data Catalog, we have built primitives that make it easy for you to operate this lakehouse. So before we go and understand what those primitives are, let's try to understand what happens really underneath the hood, why over a period of time you see performance regression on Iceberg tables, and why your cost profile goes up.  Think about the time you started the Iceberg lakehouse. You created a table, you started writing data to it. As the time progresses, what happens is you accumulate a lot of data files in S3. And if you are actually writing small data in chunks, you are accumulating a lot of small files in S3.

Then what happens? Iceberg is basically a metadata layer on top of your S3 files. Your metadata becomes big, and every time you fire a query, you have to process large amounts of metadata, a lot bigger metadata files. Similarly, when you have a large number of files in the storage, you make round trips to the storage, which is S3, to get those data files. Eventually what happens when you have a large number of small files in your Iceberg tables, you see the performance going down.

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1500.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1500)

Similarly, Iceberg is a model where every time you write data,  the table format creates new versions of the data, and it marks certain versions of the data as stale, as older snapshots. So the data that is sitting in your storage, if it is an older version you are no longer using, you're just paying for the storage.

[![Thumbnail 1530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1530.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1530)

Iceberg has control plane operations where you do compaction and snapshot retention, but you still have to manage it all by yourself. So with AWS Glue Data Catalog, we think about these as primitives that just work for you.  You just focus on reading and writing data into a lakehouse, and all the operations, the maintenance work that you are doing today, goes away with a single click of a button.

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1560.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1560)

Two core capabilities here are that compaction becomes a background job, and snapshot retention, or removing the older files or versions of data, is also a background operation. Let's understand how it actually works in practice. First of all, it is a background job. You just set  it once and forget about it. Once you have set it up, it intelligently decides when to run a compaction job and what is the right size of file that it needs to create. Once the compaction is done, you will see sudden improvement in performance.

[![Thumbnail 1590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1590.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1590)

This year we added more advanced capability to compaction. We went beyond just small file compaction into something like sorting and reordering.  These are mechanisms where you can sort your data on the disk, and we do that while compacting your tables in AWS Glue Data Catalog. You should be able to define what is your compaction strategy. Default is bin pack. If you have sort columns defined on your tables, you can actually enable sort compaction, so during the time of compaction we'll sort the data. If you have queries that are running across multiple dimensions and you need multi-dimensional analysis, Z-order helps you to sort data. It'll cluster the data in a way in the storage that you should be getting better performance.

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1640.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1640)

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1680.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1680)

Let's look at snapshot retention.  Just as we talked earlier, every time you write data, some files become obsolete and some new files get created. Customers don't want to store all those versions of data. They only care about some versions of the data so that they can go back in the history and do time travel on it. In AWS Glue Data Catalog, we allow customers to define those policies. Those policies can be time-based, number of snapshots to retain based, or it's a more flexible time-based policy that you can define where you can essentially say, hey, I want last five days of snapshots to be retained,  or I want last five snapshots to be retained. So it's a flexible policy model. You define that policy and set it once.

[![Thumbnail 1720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1720.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1720)

Once you do that, AWS Glue Data Catalog will have a paired compute which will go look at the policy and apply that policy on a dataset. Now think about a case where you are just reading and writing data into your Iceberg tables, and your lakehouse is fully managed by AWS. How much more value can you generate from your data? There is a third type of maintenance procedure that Iceberg has to go through, which is orphan files.  Someone who has operated a large-scale lakehouse would have seen that ETL jobs fail, and when ETL jobs fail, what happens is you have created a bunch of files in your S3 storage and those files stay there. In AWS Glue Data Catalog, we allow customers to define those policies so that these orphan files, which are not referenced by any table, those stale files, we delete them so that your cost remains in check all the time.

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1780.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1780)

So if you are building a lakehouse and Iceberg is your format, it's fully managed by AWS, but customers told us that that's not enough. They said, okay, data lands in my lakehouse now, but how do I transform it? It's incredibly hard. Can we come up with a primitive where transformation becomes a background job? That's interesting. Before we started thinking about this, we looked at the problem.  We saw that every customer who creates data transformation starts with this vicious cycle. It starts with understanding what changed in my source tables, then write a business logic, apply it on the change data, then that change data gets persisted in a separate table. The pipeline has to be orchestrated. Infrastructure needs to be scaled. Jobs fail. You monitor and manage everything. This is taking you away from your real value from data.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1820.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1820)

So this year, we announced Materialized Views that are Iceberg-compatible on Glue Data Catalog. What is a materialized view?  Think about it as an Iceberg table that is fully managed by Glue Data Catalog. You set it once and forget about it. Glue Data Catalog manages compute, does the change detection, brings the data over, and writes it in the materialized view. You don't have to manage pipelines. You just set it once and forget it.

[![Thumbnail 1870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1870.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1870)

So think about the primitives that we have built. You are bringing data into your lakehouse. Don't think about storage management anymore. It's just taken care of by the Glue Data Catalog. You want to transform your data? Set it once and forget it. Your data transformation happens automatically underneath the hood. It's compatible with Spark SQL, so you have a declarative SQL language now to define your transformation and deploy it as a materialized view.  Your first-party Spark engines, which include EMR, Glue, and Athena Spark engines, should be able to detect that there is a materialized view. If they see a query that can be run using a materialized view, they'll rewrite the query.

### Security and Compliance: Fine-Grained Access Control with Lake Formation

We changed the optimizer of Spark to automatically identify materialized views, and our benchmarks showed we got up to 8X performance improvement on Spark because of materialized views. In summary, materialized views make it incredibly simple for you to transform your data. We talked about bringing data over. We talked about managing the data in the lakehouse. We talked about generating insights by transforming the data. Now, how do we secure it?

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1920.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1920)

[![Thumbnail 1930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1930.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1930)

 The security controls have two key pillars. The first one is fine-grained access control.  You want to be able to control who has access to what at a table level, at a database level, at a column level, and at a row level. You should be able to scale your permissions through tag-based or attribute-based access control. And you should be able to do zero-copy data sharing, which means you can share data with your partners, with your customers, with your teams without making a physical copy of the data. All these three capabilities are powered by Lake Formation.

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/1960.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=1960)

So let's look at how fine-grained access control  works. Think of a table like this where you can define policies on the tables, providing a list saying here are the inclusion columns and here are the exclusion columns. As a customer, you can say Mahesh can have access to everything in the customer table except for the customer ID or customer email address. Or you can say Mahesh has access to only the customer ID column and nothing else. Both inclusion lists and exclusion lists are supported. You have filters for row-level access control, and when you combine them together, you can restrict your data access at the cell level.

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2020.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2020)

Customers asked us about these permission models. They are very useful, but they want to scale. They have thousands of users of their lakehouse now, and with agentic workloads, they want every agent to have their own right access controls as well. So to scale your permissions,  we had tag-based access control. Think about tags as a collection. You create a collection of datasets and say a user has access to the collection. This is called tag ontology. You define your ontology based on your business requirements and give access to your users.

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2050.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2050)

Customers said that's not enough. People move around the company. People change departments. How do you solve that? This year,  we announced attribute-based access control. What that does is policy management on dynamic user attributes. The user attributes will be evaluated at runtime, and the policy will be evaluated at runtime to decide who has access to what. What that means is if Mahesh was part of, say, the sales department and moves to something like marketing, his policy automatically changes because the attribute changed. So your security controls are there.

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2100.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2100)

But we also have customers who have regulatory requirements. We provide best-in-class authentication, access control mechanisms, auditability, and compliance. We are certified with SOC, PCI, FedRAMP, and HIPAA. 

If you are in a regulated industry, you should be able to use it out of the box, and we have encryption for data as well as metadata. So if you're an enterprise struggling to build a lakehouse, AWS is the right place to start. With this, I'm going to invite over Achal Kumar from Intuit, who is going to talk about how they're using these capabilities for their future lakehouse. Thank you.

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2150.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2150)

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2160.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2160)

### Intuit's Real-World Implementation: Dual Native Catalog Architecture for Unified Governance

Hi, everyone. I'm Achal, and I'm part of Intuit Data Analytics Organization. And with me, I have my team here. So if you have tough questions, go out to them,  not me. So what's Intuit's mission? Our mission is to power prosperity around the world. And how do we achieve it?  We want to provide more money to our customers' pockets with no work and complete confidence, and our customers are consumers, small businesses, and mid-market companies. And we want to build an AI-driven expert platform which sells for them.

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2180.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2180)

So if you look at Intuit's platform advantage,  luckily for us, we have been focusing on data for the last two and a half years. We have embraced data mesh concepts, we have embraced data as a product concepts. So all consumable data is easily available and discoverable for our data workers. And if you look at the scale, we are talking about 86 million consumers, 10 million small and mid-market businesses. And look at the data, 70,000 tax and financial attributes per consumer. We are talking about 625,000 customer and financial attributes per small business. This is huge, and on this data, we can run our machine learning models, our agentic AI experiences on all that.

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2230.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2230)

So  let's look at Intuit's data landscape, and I've divided it into three categories. One is scale. We have 300,000 plus tables serving diverse use cases, 70,000 plus data pipelines ingesting and transforming data as we speak. And this says 2,500 plus users, but in this age, everyone uses our data: software developers, data workers, data analysts, machine learning engineers, everyone. And we have 200 petabytes of data or more. I think every day data keeps coming in, fueling ML models and AI agents. And the biggest challenge is the technical heterogeneity we have. I'm sure most of you guys face this. We have two warehouse platforms, AWS and Databricks, with distributed engines requiring unified governance and a seamless user experience for our data workers.

And because Intuit is a financial company, we have a lot of compliance complexity. Like for example, third-party contracts, you can't use the credit report data for a certain purpose. Regulatory requirements, the biggest example is tax data cannot be accessed outside the United States, or tax data cannot be used for anything else except improving your tax experience unless the user has given consent. So a lot of ifs and buts are there. And of course we talk about GDPR, CCPA. And last but not least are the consumer rights. Our consumers opt out. They say, don't use my data for analytics, don't use my data for machine learning. So we have to comply with all that, and that's where things get interesting.

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2350.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2350)

So let's look at the evolution.  I know for most of you, we were and we are still using Hive Metastore for some purposes, and that has hit the scale of cloud limits. So data was stored in S3 buckets. We wanted to apply fine-grained access. The only way to do it was to use AWS policies, and we hit AWS's 20 kilobyte policy limit, so that was scale. The second reason why we had to move beyond Hive Metastore was we wanted finer-grained access control, not just at the table level. The same table can hold data for two users, one has given consent, one has not given consent. So now we have to apply row-level and column-level security, row-level security. Now the same table can have PII columns and non-PII data, so now we have to apply column-level security. So that's where row-level security and column-level security capability is very required.

The last one is the policy-based Attribute-Based Access Control, or ABAC. Until now, data stewards were approving access requests for consumers, but we want to get away from that. We want to make it automated so that based on the policies, data tags, and things like that, we should be automatically approving or denying access.

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2430.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2430)

Now look at the constraints.  Of course, the business has to run, so no user disruption could happen. Teams who are using AWS should continue using AWS technologies, and the same for Databricks. There can be no business impact. We have 70,000 production pipelines running, and you cannot stop them. We have to preserve a unified experience and enable row-level security, column-level security, and dynamic policies while maintaining all of this. The most important thing is there shouldn't be any user disruption, and we can't ask people to migrate from one technology to another.

So what we chose was a dual native catalog. We have to maintain AWS Glue Data Catalog as well as Unity Catalog, which is the Databricks catalog, with a federation layer between those two, which Pavani has talked about, so that there's no data duplication. What we had to build is a unified control plane above both the catalogs so our data workers don't worry about these two catalogs. Why this works is that we are using native technologies, AWS Glue Data Catalog and Unity Catalog, and both of them provide native row-level security and column-level security for us. The federation helps us because the data is not being copied or replicated across these two technologies.

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2520.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2520)

If you look at the high-level picture of the foundation,  we have adopted Apache Iceberg as the format. Until now we were on Parquet, and you can laugh at it, but that's where we were. If you see here, Databricks workloads write data in Delta Lake format. Uniform comes as a savior and generates Iceberg metadata from Delta tables automatically, so AWS workloads interact with that same data in the Iceberg format. In the bottom, the Unity Catalog and AWS Glue Data Catalog are being federated, so each table lives in one native catalog federated to the other.

On top of that is our unified control plane, which we have to build, which provides a single source of truth for metadata across both. We manage compliance tags, lifecycle, access management, and all that is done through our control plane. Users see one unified experience despite dual catalogs, and that was very important for us.

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2580.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2580)

If you look  based on that architecture, all these use cases work the same way as they used to work before. Business intelligence analysts query any data via Athena, Amazon Redshift, and build their dashboards. Data engineers have 70,000 plus pipelines with no change and no migration. Data scientists experiment with federated access to all datasets, and they can switch technologies and get the same experience. So there are no data silos, no forced migration. Teams choose the best tools, whether it's AWS or Databricks, and we're not controlling that. Governance is consistent.

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2630.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2630)

We talked about what AWS provides and Databricks provides, but if you look at the practical example of what  Intuit had to do to bring all this together, we had to do three important things. Every data asset has to be tagged, whether it's a table, attribute, or schema, before data is produced, so that we know, for example, this table has tax data or this column is personally identifiable information. Before a producer produces data, you have to tag it with standard tags governed by us.

The second is that every principal, whether it's human or machine, has to be labeled. For instance, this human is outside the United States or works in a project outside the United States, or this human belongs to a project focused on marketing purposes. We have project-level labeling, and that label applies to all members in that project. Then comes our legal and privacy officers who use that information to define policies in English in our policy management system. They don't know technical aspects, so they define policies in English, and that policy is translated into machine-readable format and stored in our authorization component on the left side.

Now, let me take an example. A data consumer comes to our data discovery portal. As I mentioned, Intuit has invested two years in this, and all consumable data is discoverable in our data discovery portal. The consumer explores the data and finds a tax table that interests them. They request access to it. Our data access control management will now look at the data tags for the table and examine the principal attributes of this user, determining what they are trying to do and from where. It sends the request to our authorization component, and that component will either approve, deny, or semi-approve, saying give access but only to consented data. Then our data lake control plane syndicates that policy across AWS and Databricks at the same time.

[![Thumbnail 2790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2790.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2790)

So that happens during the access request flow. On the right side, you can see there's a query time where the same consumer user comes and queries data. They can go to either AWS or Databricks and get the same results back. In this partnership, if you look at what technologies  we use from AWS and what we had to build, it's pretty straightforward. We use Lake Formation, which gives us production-ready row-level security and column-level security. Query time enforcement is there. Of course, the Iceberg REST catalog really helps. All of our data lake is in Amazon S3, which helps us with cost, intelligent tiering, scale, and reliability. What we had to build was a policy management system, the data tagging system, which is very important to tag your data and label your principals, and our data lake control plane.

[![Thumbnail 2830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2830.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2830)

 Here's a sample use case, and this is all real, by the way. A data engineer creates a table in Databricks and stores data. If you look at it, the user has tagged the table saying this table has machine learning opt-out, so please check before giving access. There's a column called Email which is marked as personally identifiable information. An ML engineer is using Amazon SageMaker and tries to query that table. If the table has 100,000 records, imagine if 20,000 people have opted out of machine learning. The ML engineer will only get 80,000 records, and the ML engineer has no idea what's happening. Everything is automatically happening here. If another user opts out in the middle before the next query, the number will go down further.

[![Thumbnail 2890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2890.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2890)

So this is our current state, and of course with every development  we have some feature requests for AWS. One big ask we have put in is that right now the row-level security and column-level security are based on a single table, but most of our opt-outs and consent tables are stored in a separate table, not in the main data table. So you need to join those two tables before returning the result back. That's a feature request. The other one is column masking. We don't just want the catalog to deny access to a column. We want to mask the column as well. The last one is hybrid attribute-based access control plus tag-based access control policies. That's what we have implemented today, but we want 100% attribute-based access control policies so that Intuit is not in the business of pushing policies for access requests. We can just dump all the policies, all the labels, all the tags, and everything should happen at the catalog level.

[![Thumbnail 2970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d292f1bb43abee4e/2970.jpg)](https://www.youtube.com/watch?v=qIsxBWfVgsM&t=2970)

### Conclusion: The Unified Benefits of Lakehouse Architecture in Amazon SageMaker

So with that, I just want to add it was not easy to get here. It sounds very easy on the slides and the presentation, but it's not easy. So thank you, and I'll hand that back to Pavani. Thanks, Achal. As we close today's presentation, I want to reinforce the benefits of lakehouse architecture  in Amazon SageMaker. First, we bring both the benefits of data warehouses and data lakes together. You no longer have to choose between performance and flexibility. You get both. The unified approach means that your teams can work with one consistent interface, one security model, and one governance framework for all your data.

Next, the openness of this architecture. We use Apache Iceberg as the foundation. You can now connect to external catalogs and discover your data across organizations. Last but not least, security is built into every layer, from Lake Formation integration to fine-grained access controls. Your data is protected while remaining accessible to authorized users, engines, and agents. With that, we are ready to take some questions. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
