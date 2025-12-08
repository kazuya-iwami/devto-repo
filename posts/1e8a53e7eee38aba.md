---
title: 'AWS re:Invent 2025 - Innovations in AWS analytics: Data processing (ANT305)'
published: true
description: 'In this video, Kinshuk Pahare and Neil Mukerje from AWS discuss innovations in data analytics, covering four key themes: AI agents, Apache Iceberg, ease of use, and governance. They introduce an AI agent for Spark version upgrades that automates code migration and data quality checks, reducing months of work to minutes. EMR Spark 7.12 now supports Iceberg V3 with deletion vectors and is 5.4x faster than open source. New features include SageMaker Notebooks with embedded AI agents, serverless Airflow, and serverless storage that reduces costs by 20%. Enhanced security includes fine-grained access control with Lake Formation and trusted identity propagation via IAM Identity Center. Anjali Norwood from Netflix shares their successful POC results, showing significant performance gains especially in PySpark workloads, and announces Netflix''s decision to migrate Spark workflows to EMR starting in 2026.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/40.jpg'
series: ''
canonical_url: null
id: 3086727
date: '2025-12-05T14:34:49Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Innovations in AWS analytics: Data processing (ANT305)**

> In this video, Kinshuk Pahare and Neil Mukerje from AWS discuss innovations in data analytics, covering four key themes: AI agents, Apache Iceberg, ease of use, and governance. They introduce an AI agent for Spark version upgrades that automates code migration and data quality checks, reducing months of work to minutes. EMR Spark 7.12 now supports Iceberg V3 with deletion vectors and is 5.4x faster than open source. New features include SageMaker Notebooks with embedded AI agents, serverless Airflow, and serverless storage that reduces costs by 20%. Enhanced security includes fine-grained access control with Lake Formation and trusted identity propagation via IAM Identity Center. Anjali Norwood from Netflix shares their successful POC results, showing significant performance gains especially in PySpark workloads, and announces Netflix's decision to migrate Spark workflows to EMR starting in 2026.

{% youtube https://www.youtube.com/watch?v=KckSvlJdV_o %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
### Introduction to AWS Data Analytics Innovation: Price Performance and Four Key Themes

Good afternoon everyone. Today we are going to talk about innovations in data analytics. My name is Kinshuk Pahare, and I'm head of product for our analytics portfolio, which includes Glue, EMR, Athena, and Redshift. With me is Neil Mukerje and Anjali Norwood, who is a senior engineering manager at Netflix. Today, our topic is going to be around innovation in data processing, so let's dive right in.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/40.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=40)

 This is going to be our agenda. We are going to talk about the latest innovations, specifically touching on some of the agentic innovations we have done. We're going to talk about Iceberg and ease of use innovations that we have delivered. Finally, Anjali is going to share how Netflix is leveraging EMR for their data processing workloads.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/70.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=70)

 Within data processing services, you are familiar with Glue ETL, which is used for data integration. You're obviously familiar with EMR, which is one of those services that has been around since 2009. It is used primarily for open source data processing. Customers use EMR and the multiple engines that EMR supports for their data workloads. And then there's Athena, which is our interactive query service that is primarily used for querying your data lake in S3. Together, millions of customers are using these services to author their data pipelines and query their data for interactive workloads.

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/110.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=110)

 On a weekly basis, 300 million plus Redshift Serverless queries are executed. There's 19 years of innovation that goes behind it. There are a billion plus Athena queries that are executed against the data lake every week. EMR Serverless, one of the fastest growing services, executes about 300 million Spark jobs. Together, these services query about 100 petabytes plus of data from S3 during the course of the week, and these data workloads are primarily built on S3 as the data lake.

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/160.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=160)

 Over the years, S3 has emerged as the best place to build your data lake. There are exabytes of Parquet data that is stored in S3 for the purpose of data lakes and querying. Parquet, as you are familiar, is a columnar format that is typically used for building data lakes because it is much more efficient for reads and writes. The engines that are writing and reading from S3 are behind Glue, EMR, and Athena. There are 25 million plus requests per second against those data lakes. The reason I'm sharing these numbers is that they are exciting and represent a testament to how customers are trusting these data processing services and our S3 for building their mission-critical data applications.

[![Thumbnail 220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/220.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=220)

 Every year we gather here to talk about how we are going to make it even better. This is my fourth re:Invent, but the Athena service was launched 9 years ago, and Glue was launched in August 2017. These are all services where years of innovation have gone into them, specifically for the purpose of data processing and querying this data. The first and foremost priority for us is price performance, and this is a recurring theme year after year.

This year also, our engineering has delivered AWS Spark performance, which exceeds open source Spark performance by 4.5x. It's 2x better for writes, and about 5.4x better if you use Spark 4.0, which is the new innovation that we have done. The reason why price performance becomes important is that the faster the Spark engine, the better you are able to meet your SLA targets. Together, you're also able to realize pricing and cost gains because if a compute or data processing job finishes faster, you're actually paying less.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/320.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=320)

So with price performance out of the way, there are four innovation themes this year that the team is driving. Iceberg  has emerged as the standard for building data lakes. The key advantage is that you can build a data lake in Iceberg and use any engine to query that data lake. AI agents in the context of data processing have emerged and they are solving some of the most challenging problems of data engineering, and I'm going to talk about some of these in my session, and then Neil is going to talk about how we are using data agents for some of the ease of use cases. And finally, governance. Governance is always a topic of interest for a lot of our customers. How do you govern a data lake? How do you actually ensure fine-grained access control? How do you ensure that the identity of an end user is carried forward for all your enforcement decisions when it comes to querying and writing data lakes?

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/380.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=380)

### AI Agents for Spark Runtime Upgrades: Automating Code Migration and Data Quality Validation

So with that out of the way, let's start with AI agents and data.  Now, AI agents have many applications. This has been a year of AI agents in various contexts. For this talk, we are going to focus on one of the hardest problems that data engineers face, which is upgrading a Spark runtime from version X.Y to version P.Q. Spark version upgrades have always been a challenge. The reason it is getting exacerbated is because all the data lake innovations depend on the latest and greatest Spark runtime. Obviously, as a practitioner, everyone should run the latest Spark version to get the best performance out of it, to get the best data lake performance out of it, as well as the features out of it. But it's not usually an easy upgrade to make.

There are two problems that compound the Spark upgrade. Number one, there is a code upgrade part. But then there is also a data consistency part. It's not just about upgrading the Spark code and having it compile well. You also need to verify that it actually does the same thing with the data that your previous version was doing. So there are three elements to consider. In practice, what ends up happening is that data engineers have to iterate while they are upgrading the code. You upgrade the code, see some new error messages, edit the code, rinse and repeat. That process ends up becoming not only time consuming, it is also not a foolproof process because you end up missing a transform which is defined differently in the latest Spark version.

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/500.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=500)

To solve this, we have unleashed the AI agent for Spark upgrade.  The way this agent works is similar to how a data engineer works. It actually builds a plan for the upgrade. To build the plan, it reads through your project structure and generates an upgrade plan. It tells you that this is what I'm planning to achieve. You can look at the plan, edit the plan, and provide your own upgrade plan depending on how your pipeline is structured. The next stage is to build and execute, and this is the stage where it will execute this plan, see the error messages, and edit the code based on the error messages. It will do the same thing which a data engineer is expected to do, but it will do it in an automatic fashion.

Finally, when the code is upgraded and it's compiling well, and all the test plans are passing, it's also going to do a data quality check. What I mean by data quality check is that the AI agent will actually look at your data structure before and after the upgrade. It will check whether the schema matches, whether the column type matches, whether the size of the data matches. It will do all those checks, and then it will say that this code is finally upgraded. Throughout the process, you are in full control. There is a guardrail which can be configured. You can actually say that I want to manually execute every single step in the plan, or I want the agent to proceed.

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/640.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=640)

I trust the agent and I trust it to upgrade the code properly. During all the runs and all the builds throughout the entire process, you have access to observability logs so you can actually see what changes it has done. I'm going to show you a demo which talks about how this works in practice. You can actually upgrade your entire pipeline or multiple pipelines.  The task that used to take six months, sometimes six to twelve months, can now be completed in minutes because this agent is automatically able to achieve some of these very hard to find problems, the Spark problems, and act upon them.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/670.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=670)

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/680.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=680)

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/690.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=690)

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/700.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=700)

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/710.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=710)

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/720.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=720)

Here's a little demo. Help me upgrade from version 2.4 to 3.5.  It will say, okay, you want me to upgrade this. I read through the file and I read through your project structure. It will actually build a plan and say, hey, do you like this plan? When you say I like this plan,  you can proceed. It is going to say, here are the steps which I'm about to execute.  As part of the execution, you can save the plan and proceed. Finally, it will say, okay, I found this diff between the previous version and new version.  Here are all the changes which I'm about to execute. Do you approve? You hit yes and then it's going to keep doing this for every single process.  Finally, it will tell you exactly what changes it made, and once you are satisfied with the upgrade,  you can actually execute this into production.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/730.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=730)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/740.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=740)

### Iceberg V3 Support and Materialized Views: Enhanced Data Lake Management Capabilities

FINRA is using this upgrade agent for the purpose of upgrading multiple data pipelines.  The second innovation which I talked about is support for Iceberg V3. Iceberg V3 is the latest table format. It is based on Iceberg version 1.10 and supports some of the new capabilities around deletion vectors and role lineage.  All of these are available to you as part of EMR Spark runtime 7.12. In case you are not aware, EMR Spark runtime is also shared with Glue ETL as well as Athena. These are all the Spark runtimes which benefit from the same innovations that are delivered with EMR 7.12, so you get the same performance characteristics and the same feature set as part of Iceberg V3.

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/800.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=800)

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/830.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=830)

Deletion vectors and role lineage allow you to manage your data lake more effectively.  It reduces your write amplification problems, eliminates smart delete problems, and makes it very efficient for you to actually build and maintain your data lake. We are also announcing EMR Spark version 4.0. It supports the latest Iceberg version as I talked about. It is also 5.4 times faster than the open source Spark, which means you are able to use a Spark job and achieve your SLA or reduce your cost of operating a pipeline by an equivalent amount.  Finally, there is another launch that we did this year, which is Iceberg Materialized View.

Materialized view, as you're familiar with, allows you to define a view in SQL, and Iceberg materialized views are Iceberg tables that are a result of that SQL code execution. So it automatically speeds up your queries from your Spark engine. It is compatible with the Iceberg APIs, which means when they are executed as part of SQL queries, they manifest as a catalog object. Against that catalog object you can run your Athena Spark or EMR Spark or Glue ETL queries. More importantly, you can actually do incremental refresh to the Iceberg views. So instead of having to configure an ETL pipeline and configure all the triggers and all the scheduling, you can just define a materialized view which is refreshed automatically based on the frequency that you choose or based on when the data is available, it automatically gets refreshed. This is available as part of the Glue data catalog.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/910.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=910)

 The view definitions are stored in the catalog and can be accessed from EMR, Spark, Glue ETL Spark, or Athena Spark. This is an announcement we made yesterday. With that, I'm going to hand off to Neil, who will talk about ease of use.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/950.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=950)

### SageMaker Notebooks with Embedded AI Agent: Unified Python and Spark Development Experience

Let's focus on features that emphasize ease of use. The most prominent feature we have is Amazon SageMaker notebooks. It's important to note that SageMaker Studio  is the new UI and front end for all data processing as well as AI/ML. Whether you're using EMR, Glue, or Athena, your main UI front end for users is SageMaker Studio, and SageMaker Notebooks is the newest addition to SageMaker Studio. It's a new modern notebook experience that comes with a purpose-built AI agent. You can get started in seconds and you can choose the language best suited for your task, which can be SQL, Python, or Spark.

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1000.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1000)

What is great about it is that you can start with Python and seamlessly scale to Spark workloads to handle large data sets. Behind the scenes, SageMaker Notebook steps into Athena for Apache Spark to deliver the Spark  capabilities. Athena Apache Spark uses the same performance runtime engine as EMR. It actually uses EMR 5.12, which is 4.7x faster than open source Spark. Being serverless, it eliminates all management overhead and starts up and scales in a low order of seconds. This means if you go into a notebook and type Spark, within seconds you have a massive Spark cluster ready for you for data processing.

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1050.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1050)

What is unique about it is that the architecture allows a unified authoring, execution, and debugging experience across Python and Spark workloads. SageMaker Notebook connects to Athena Spark using Spark Connect.  Spark Connect is a new client-server architecture in Spark that separates the client application from the remote Spark driver while establishing a protocol to seamlessly exchange data between the two, facilitating remote execution. This means that when you type Spark on a notebook cell, it appears that Spark is running locally in the notebook while it's actually running on a massive cluster behind the scenes. You can debug Spark variables just as you can debug Python variables, which means you can tap into Python and SQL or Python packages like Pandas and DuckDB for small to medium data sets, and you can tap into Spark for large-scale data processing at the same time.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1100.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1100)

Now let's have a deeper look at the AI agent  that comes with the notebook. The AI agent experience is embedded in the notebook interface on the right-hand side of your notebook. It understands your data catalog and your business metadata. It can generate SQL of the right dialect, which is Spark SQL or Athena SQL if you're using Athena. It can generate Python code and Spark code, and it can do more than generate data. It can actually generate an entire plan, a full notebook filled with cells to achieve a specific outcome, so you can really talk to it.

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1140.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1140)

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1150.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1150)

[![Thumbnail 1160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1160.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1160)

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1170.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1170)

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1180.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1180)

Let's have a quick demo of how the data agent works. In this example, in my  data agent panel, I asked a question: show me tables in my sample database. It goes and discovers the tables I have. I have three tables here: the churn table, the LTV table, and the New York taxi trips  table. I can type SQL to query it myself, or I can go to the table cell and visualize the data I have.  But what's even better, I can just talk to it and say, can you please help me visualize data in my LTV table?  It then generates the corresponding Python code to build visualizations and gives me comprehensive visualizations of the data in my LTV table. 

[![Thumbnail 1190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1190.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1190)

### Serverless Airflow and Storage: Eliminating Infrastructure Management and Disk Provisioning

I hope you like the convenience and the power of it and will use it. Moving on to a highly requested feature, we have managed Airflow now in an entirely serverless  deployment. Airflow is extremely popular in the data orchestration space, and now we have it in a serverless deployment model. The UI for this, again, is SageMaker Unified Studio, so you can also create and manage workflows in a single UI. It comes with an enhanced security model where per workflow you can specify an IAM role, which means each workflow can now have a unique permission profile.

[![Thumbnail 1220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1220.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1220)

And of course being serverless, you only pay for what you use.  Let's do a comparison of what we already have within a provisioned Airflow and compare it with serverless. With provisioned Airflow, you have fixed infrastructure that you size first, and you can have a micro size and so on. It's always on regardless of whether the workflows are running or not. There are certain capacity limits based on the size you chose, and yes, you have to manage the environment. But on the serverless side, there's no infrastructure anymore. It's pay per use, it's unlimited capacity, and it's entirely service managed.

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1260.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1260)

From the security perspective, there are some key changes in the provisioned mode.  Your entire Airflow environment has a single security model, so all the workflows have the same permission profiles. If you want to separate these workflows, you have to launch separate environments, which means the authorization is shared and isolation is complex. On the serverless side, you have workflow level security, so per workflow you can define granular access control. This means you can now have the least privileged IAM roles for each workflow, which really simplifies compliance.

[![Thumbnail 1300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1300.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1300)

[![Thumbnail 1320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1320.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1320)

Now all of this is embedded in the Workflow UI in SageMaker  Unified Studio. You can easily drag and drop these activities to build a DAG yourself without any prior expertise in Python or Airflow. Once you have built them, you can submit these workflows, monitor these jobs, and so on, all from a single UI. Now what's also available is  a one-click scheduler for visual ETL. This allows you to easily streamline your ETL flows and queries directly from the same UI in SageMaker Unified Studio. You can create, modify, and monitor these schedules. Behind the scenes, it's integrated with Amazon EventBridge Scheduler to make these schedules happen.

[![Thumbnail 1350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1350.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1350)

Now customers have told us that it's not easy to onboard to SageMaker  Unified Studio because of certain dependencies. You have to configure IAM Identity Center and so on. We have listened and we have made the onboarding to SageMaker Unified Studio a one-click step from existing services like Athena console, from S3 Tables, from Amazon Redshift console. From those pages with a single click, you can go to SageMaker Unified Studio. We will auto-create a default SageMaker Unified Studio workspace. The entire process takes a minute. You don't have to configure IAM Identity Center anymore. It uses the IAM roles you were already using in those consoles, and you'll land in a much more sophisticated interface which allows you to use notebook capabilities, code editors, and so on on the same data sets.

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1400.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1400)

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1420.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1420)

All right, so let's move into a very exciting feature. This is an industry first.  Serverless storage entirely eliminates local disk provisioning for Spark workloads. With this you can achieve up to 20 percent reduced costs as per our benchmarks. Here's how it works. Spark workers have  local disks, and these local disks have shuffle in them. When these disks run out of capacity, your jobs fail. If the disks are constrained, your jobs slow down. Often what happens is certain tasks become stragglers, so some workers keep running for a long time and all workers have to be retained when some workers are working because the disks have shuffled data in them. This makes Spark scaling inherently inefficient.

Now with serverless storage, these problems are entirely eliminated. Serverless storage offloads shuffle to a high-performance storage layer, which means you never run out of disk space. You don't face any job failures. Your jobs never slow down. And as the shuffle is outside the cluster, Spark can be far more elastic, scaling out and scaling in as per the needs of a stage. The combination of the compute efficiencies that serverless storage brings, along with the fact that you don't have to provision this anymore, is an absolute game changer in this space. You can save up to 20 percent as per our benchmarks and maybe even more depending on the shape of your workloads.

[![Thumbnail 1490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1490.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1490)

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1500.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1500)

### Enterprise Security and Governance: Fine-Grained Access Control with Trusted Identity Propagation

All right, so let's now move to the  topic of security and governance, a very important topic for enterprise customers. Of course, the main concern is data access control, right? In the  customers want consistent data access control across the various formats they want to use. Of course, Apache Iceberg is very important, but

so is Delta Lake and Hudi, and of course traditional formats like Parquet, CSV, and JSON. For most use cases, coarse-grained access control is what is needed, where you want to secure certain S3 locations, secure certain tables, and ensure that users get access to them. For some use cases, you also need fine-grained access control where you have column, row, and cell-level security as well.

[![Thumbnail 1550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1550.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1550)

[![Thumbnail 1570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1570.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1570)

We have made many launches this year focused on security and governance. Let's see how all of this comes together. The first focus is coarse-grained access for S3 locations. For this, the recommendation  is to use S3 Access Grants, where you can have simple SQL-like permissions for S3 buckets, prefixes, as well as objects.  When you submit a job, the engine simply looks up S3 Access Grants, gets scoped-down credentials, and uses those credentials to talk to S3. This has been in EMR for a while from version 6.15 plus. It is now also in Glue with version 5.0 and it works consistently across all table formats.

[![Thumbnail 1590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1590.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1590)

Now let's look at coarse-grained access for tables in your catalog.  This is definitely the most common scenario where you don't want to deal with fine-grained access control overhead, but you want to grant users access to certain tables. This has no performance overhead because it's just filtering of metadata, and the expectation is full support, which means read and write support for all use cases. It's a fairly easy implementation.

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1620.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1620)

 The first step is that the admin will go to Lake Formation and will grant permissions for specific users for specific tables. When a job is submitted, it talks to the Glue catalog, which redirects to Lake Formation and gets scoped-down credentials back. Those credentials are used to access S3. This is available in EMR from version 7.9 plus, as well as in Glue from version 5.0 and above, and it supports Iceberg, Delta, as well as Hudi tables.

[![Thumbnail 1660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1660.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1660)

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1680.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1680)

Now let's look at fine-grained access control where you want column, row, as well as cell-level security. Enterprise customers want audit  trails so that you can have compliance reporting, and of course this should work with granular identity-based access controls as well.  The admin will set up fine-grained permissions in Lake Formation for specific tables, rows, and columns or cells as needed. At the engine level, you have to enable the fine-grained access control, and when the user submits a job, it communicates with Lake Formation, gets scoped-down credentials, and accesses S3 as well as the catalog tables.

Note that from version 7.7 onwards on EMR we support reads on all deployments. Glue is supported from version 5.0 onwards. I'm very excited to announce that we support writes now as well. Version 7.12, which is almost the most recent launch, supports writes consistently on all EMR deployments as well as in Glue 5.1 with Iceberg, Delta, as well as Hudi. This is a very unique architecture where we separate the system and user drivers. The system driver is where all of the filtering happens and the user driver is where your Spark code runs. This isolation is what creates the security model that ensures that you cannot run Spark code to bypass the controls.

[![Thumbnail 1750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1750.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1750)

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1780.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1780)

Now let's move to a very interesting and very important topic for enterprise customers: trusted identity propagation.  The expectation of enterprise customers is that the user will authenticate against established identity providers like Active Directory and Okta, and those identities have to be pushed down to the various services. Of course, the expectation is end-to-end tracing of all users' actions. This is now possible with IAM Identity Center.  You will enable single sign-on with IAM Identity Center, which allows end-to-end data access and traceability. You have fine-grained permissions and access based on the user's identity. We support this from both interactive sessions and jobs from SageMaker, Unified Studio, and on the specific EMR versions mentioned from version 7.11. It is supported on all EMR deployments and this works in Glue 5 as well.

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1810.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1810)

Let me show you how this works. I have two users here, Charlie and Elle. Charlie wants to access a specific S3 table, and Elle wants to access another S3 table. When they log into SageMaker Studio, they will authenticate with IAM Identity Center, which federates the identity from Active Directory, Okta, and so on. That entity and authorization is then pushed down to EMR when they submit a job or start an interactive session, and that will talk to Lake Formation and can give them access. I hope this was useful in understanding how trusted identity propagation as well as fine-grained access control delivers enterprise security. 

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1880.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1880)

### Netflix's Journey to EMR: Proof-of-Concept Results and Migration Roadmap Through 2027

Now I would like to invite Anjali Norwood from Netflix to share their story of evaluating EMR. Thank you, Neil. I'm here to share our Netflix story of proof-of-concept with Spark on EMR. Stick around with me to find out whether this has a happy ending. Was this a drama?  Was this a story of collaboration and teamwork, or was there fighting and action? Stick around and you'll find out. I'm Anjali. I lead the Big Data Analytics Platform at Netflix. My team is responsible for management of data in our warehouse, which is S3-based, Iceberg table maintenance, and governance and security around the engines to access the data. Spark is one of the big workhorses. Other engines include Trino, Druid, solutions around Snowflake, and new editions of other tools, along with orchestration technologies that we have built in-house.

[![Thumbnail 1930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1930.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1930)

Netflix is first and foremost an entertainment company.  However, almost all of our decisions are data-driven. When you log on to your Netflix account on TV or cell phone, you start seeing rows of content. This recommended content is just for you. That decision is very much data-driven. Which rows you see, in what order you see them, which cover art you see—this is all personalized and data-driven. These are user-facing decisions. Then there are decisions we make internally, like when we run Spark on EC2, which instance type should we use? Is R7 ready for adoption, or should we stay on R5 for a little while? All these decisions are also made with data.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1950.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1950)

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/1980.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=1980)

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2020.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2020)

That is our streaming video-on-demand, our legacy business.  But now we have started offering ad plans in some countries. We have started carrying live content. Anybody watching NFL on Christmas Day? I am. Games, Netflix originals—these are consumer experiences. We will also start carrying podcasts.  This is all to say that all of these new business initiatives result in more and more data in our warehouse and more and more of a need to get insights out of that data. This is a bit of a Spark story. This started more than seven years ago. 

Netflix decided to do storage and compute decoupling. That was a big bet we took, and it paid off. Since then, we have been operating Spark on Hadoop. We cut a fork from open-source Spark, hyper-optimized it, and customized it for Netflix scale, performance, and the variety of use cases that emerge. We operate multiple Hadoop clusters, and some statistics are there just to give you an idea of scale. Compared to what was shared earlier, this might not look like a lot, but we have 11,000 unique workflows, 250,000 jobs, and more than an exabyte of data in our warehouse. That is some pretty good scale that we operate at. We have about 8,000 to 8,500 R7 nodes.

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2090.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2090)

What do we use Spark for? It is a variety of things: getting insights out of data, ETL, which is a very popular engine for ETL, large-scale analytics, recommendations, and more.  Then you would ask, if we have this nice platform and we have been operating it for all this time, why would we even think of something else? It turns out things are changing a little bit for Netflix.

When we started out, our warehouse was open by default with no security. Anybody could access any piece of data. Netflix is a very transparent culture, and we still are. However, the addition of new businesses like ads means that some of the data is sensitive and has to be protected. Our current platform operates on Hadoop and Yarn, and it's difficult to get the level of security and isolation that we need.

Adopting new technologies is hard because of the scale and the number of jobs and workflows we run. A Scala migration can take a long time. A Python version change takes a long time. A Spark migration takes a long time. It's a lot of work with significant operational overhead and cost to our teams. We call that undifferentiated heavy lifting—work that is important but is not moving the needle for business. We would like to see how we can offload this work and reduce it for our teams.

[![Thumbnail 2200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2200.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2200)

Limited support for specialized hardware is another challenge. With AI use cases emerging, we need GPU support, which is harder to do with our current setup. Our environment is functional, but it's a little bit difficult to use and debug. Ease of use is not where we would like it to be. This is where we started thinking about Amazon EMR. This journey started in Q4 of 2024, where we said, let's check out EMR. 

It turns out that EMR provides isolation and improved security. It provides frequent releases every 90 days, every quarter. Along with that comes new Iceberg, new Python, Scala, and all the nice support. It reduces operational overhead. Things scale up and down. It supports GPUs and integrates very well with the rest of the AWS services like S3, IAM, and other things.

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2240.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2240)

So what did we test?  EMR comes in various flavors. We started with testing EMR on EC2 for a couple of reasons. One is that it is very close to how we operate our current system. While we want managed services, we also want to get there slowly. We still want to be able to tune the knobs, change the configs, and make sure things work for our users.

We tested feature compatibility, which was important because we have customized our Spark for Netflix users. We tested performance, starting with TPCDS checks, and then we decided to bring in our user workflows. These are representative user workflows. We literally reached out and said, give us the workflows that represent your team the best. They fall in various categories: SQL, PySpark, Scala, and Java.

Then we tested for scale. We basically mimicked our production runs on EMR to see if EMR holds up and how the resource consumption is. The second part of scale testing was saturation testing. We overload the system, use up memory, throw all memory-heavy, IO-heavy, CPU-heavy workloads, and see how it holds up. We also looked for operational complexity because as platform providers, my team has to know how to operate this platform, and this has to be easy for them. And of course, cost is important, so we checked that as well.

[![Thumbnail 2340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2340.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2340)

[![Thumbnail 2360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2360.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2360)

Here are some numbers.  If you look at the P90 column, you'll see that PySpark by far saw the most gains, followed by SQL and Scala. I would like to remind you that our platform is hyper-optimized, so seeing these kinds of gains is amazing. Here is another slide that talks about resource consumption.  Again, PySpark shines with the most vCore gains at P90, followed by SQL and then Scala. The consumption of vCPU is directly correlated to cost, so the less resources you use, the better.

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2390.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2390)

 It's a bit of an Eiffel chart. We have a pretty long document detailing our POC and evaluation. It's kind of hard to talk through it all, but some salient points I want to touch upon include futureproofing. We are looking to be on a platform that would just work for us and will grow with us as our business grows.

We see AWS investing heavily with a strong roadmap and continuous innovation in this area. This was important for us. Security and isolation is really great with EMR. We found the cost structure to be competitive and transparent. User experience is good, and notebook integrations are supported.

Our operational burden will go down because this is a managed service. Performance and scale worked out well. I shared performance results, and scale results were also equivalent or better than our current platform. This platform is pretty extensible through bootstrap actions. It is not going to be as flexible as a do-it-yourself approach, but that is to be expected. Taking that into account, we still found this platform to be pretty nimble.

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2490.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2490)

For observability and debugging, we are bringing our own observability rather than using CloudWatch. The rest of the integrations and the overall story look good. We have a little bit of work to do here, but the story still looks good.  Going back to the slide shown earlier, we checked out three of the four boxes and find them to be really great: price, performance, ease of use, and security. AI acceleration is something we have not tried yet. This is something we will try next.

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2510.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2510)

 This is how this story ends, but maybe the new story begins. We have decided to move Netflix Spark workflows gradually to EMR. In the first half of 2026, we have some work to do. We are paying off tech debt and building new control planes, job routing infrastructure, and so on. In the second half of 2026, we will be migrating our own platform workflows, dog fooding and checking things out. We will fine-tune based on our learnings, and in 2027 we will be migrating user workflows.

We plan to test EMR Serverless during the POC. We touched upon it and saw early positive results, but did not have the time to do a deep dive. AI acceleration, as I mentioned, and adding cost attribution are areas we will explore. I want to thank the EMR account team: Wedi, Manju, and Kartik. It was one team with the EMR product team: June, Geo, Pinhuk, and Neil. It has been like an extension of the Netflix team. It never felt like they were outsiders. We were working together, and it was a great experience.

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1e8a53e7eee38aba/2620.jpg)](https://www.youtube.com/watch?v=KckSvlJdV_o&t=2620)

Big thank you to the Netflix Spark team, Anurag and team, DSI Security, and other BA teams. It was a huge effort that went on for three quarters. At the end of Q2 this year, we finished our POC, and then there were more follow-ups because we have custom patches and customizations that we needed to talk through. All of that is coming together. I am super excited for our journey  with AWS EMR. The best is yet to come. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
