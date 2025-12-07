---
title: 'AWS re:Invent 2025 - Introducing the new Amazon SageMaker notebooks for analytics and ML (ANT212)'
published: true
description: 'In this video, Dayanand Rangegowda and Bryan Liles introduce Amazon SageMaker Unified Studio and SageMaker notebooks, demonstrating how simplicity drives innovation. They showcase one-click onboarding that reduces setup from days to minutes, seamless data access through IAM roles, and a serverless polyglot notebook supporting SQL, Python, and Spark. Key features include SparkConnect integration with Amazon Athena for instant scaling from gigabytes to terabytes without cluster configuration, SageMaker Data Agent for AI-powered code generation and debugging, and unified access to AWS services. Through Maya''s data engineer journey, they illustrate transforming raw taxi data into processed datasets using interactive visualizations, DuckDB-powered charting, and context-aware AI assistance that understands entire notebook workflows.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/0.jpg'
series: ''
canonical_url: null
id: 3088552
date: '2025-12-06T09:31:13Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Introducing the new Amazon SageMaker notebooks for analytics and ML (ANT212)**

> In this video, Dayanand Rangegowda and Bryan Liles introduce Amazon SageMaker Unified Studio and SageMaker notebooks, demonstrating how simplicity drives innovation. They showcase one-click onboarding that reduces setup from days to minutes, seamless data access through IAM roles, and a serverless polyglot notebook supporting SQL, Python, and Spark. Key features include SparkConnect integration with Amazon Athena for instant scaling from gigabytes to terabytes without cluster configuration, SageMaker Data Agent for AI-powered code generation and debugging, and unified access to AWS services. Through Maya's data engineer journey, they illustrate transforming raw taxi data into processed datasets using interactive visualizations, DuckDB-powered charting, and context-aware AI assistance that understands entire notebook workflows.

{% youtube https://www.youtube.com/watch?v=203JsHlad2U %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/0.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=0)

### Introduction: The Innovation of Simplicity and Maya's Challenge

 Let me start again. I'm Dayanand Rangegowda. I lead the engineering team behind Amazon SageMaker Unified Studio, and I have with me Bryan Liles. Bryan is a Senior Principal Engineer, and he provides technical leadership to both Unified Studio as well as the data processing suite of services that includes EMR, Glue, Athena, among others.

Every year when we meet to discuss the roadmap, we always talk about what new innovations we want to deliver to customers. However, this year we asked a different question: What if simplicity itself is the innovation? It was a shift in how we thought about innovation. Remove complexity so that everything just works.

In today's talk, we're going to discuss SageMaker notebooks that we recently launched, as well as all the innovations that we delivered using the same principles. Today's hero of the story is Maya. Maya is a data engineer of a growing enterprise, and she has very simple goals.

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/110.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=110)

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/120.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=120)

She's tasked with building data pipelines, ETL pipelines to process customer data. She's expected to collaborate with her colleagues who are part of her team. And then she knows that she's going to  work with AWS services, so she wants to be super productive. However, the path seems rougher than she expected.  She cannot get started right away.

It's the paradox of choices that she encounters. She has access to a lot of powerful AWS services, but she cannot easily get started yet. The choice means when she opens her laptop, she looks at a lot of AWS services that are purpose-built. They are so powerful, each is going to do a specific job really, really well. However, at the same time, it means Maya has to select the services that work for a task.

She has to understand how to configure and how to integrate the services. Most of the time, a single service is not good enough. Further, she needs to learn and understand all about the services, so this slows her down. Further, she can't easily collaborate with her teammates. She needs to prepare the data, and expects the data scientist in her team to pick it up from a place where they manage the data transition or handoff in a manual fashion.

[![Thumbnail 220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/220.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=220)

So this is the problem that we set out to solve for Maya. We built Amazon SageMaker Unified Studio. How many of you have heard about SageMaker  Unified Studio? Can I get a show of hands? Oh, not bad, cool.

SageMaker Unified Studio tries to bring SQL analytics, data processing, machine learning, and generative AI services all under one umbrella. We hand-selected a curated set of tools that customers loved, and we integrated them into SageMaker Unified Studio. So this changes everything for Maya. She just has one place to log in and get access to all the services, and she just has one place to collaborate with her teammates.

Even her teammates are going to log into the same project that Maya is working with. It becomes very easy for Maya to collaborate. So this is really good for Maya. I mean, she's very happy. It solves a lot of her problems, but still, many problems remain.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/290.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=290)

### One-Click Onboarding and Seamless Data Access in SageMaker Unified Studio

And Maya started wishing for more.  Unified Studio had lots of powerful tools all in one place, but Maya started thinking it takes multiple days,

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/330.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=330)

sometimes even weeks, depending on the complexity of the setup to get started with work. So she wished, what if setup was instantaneous? Further, all the data and the resources that she had already  accessed didn't carry forward seamlessly. So Maya again started wishing for what if all my data and permissions to all the data and resources carried forward seamlessly?

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/390.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=390)

So let's see how we addressed this. On November 21st, we launched one-click onboarding in SageMaker Unified Studio to help Maya go from days to minutes. There are multiple entry points for Maya from AWS Console, from S3, from Athena, from SageMaker, and Redshift. With one click, by supplying the IAM role that Maya already has access to for her data, the studio is set up. It provisions all the resources under the hood, sets up the tools, and takes Maya directly to the  builder portal. The builder portal is a completely revised, revamped, streamlined experience, and we have completely revamped the information architecture as well.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/420.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=420)

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/450.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=450)

This was great for Maya, but the problem for Maya was seamless data access. Let's see how we solved or addressed that problem. I talked about the role  when Maya set up the studio during one-click setup. The role had access to all of Maya's data and resources. So when we provision studio resources or all the tools, the tools are provisioned using the role that Maya supplies so that she can access all the data and resources from all the tools within Unified Studio. Maya also has access to all the third-party data sources. She can query and access  and load them right in.

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/460.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=460)

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/470.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=470)

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/490.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=490)

Not just that, you can connect to any data sources  or compute sources if you have access credentials by creating connections and using connections from Unified Studio. And  we have Data Explorer that provides a consistent view of all the resources that Maya has. So she can access the catalog, S3 files, all at one place, including connections. It doesn't matter which tool you go to, you have a consistent view from all the places. 

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/530.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=530)

### SageMaker Notebook: A Serverless, Polyglot Solution with Rich Visualizations

So Maya's wishes didn't end. She continued to think, what if I had the most modern and lightweight notebook that feels native? Maya is a power user of notebooks. She's a Spark expert because she's a data engineer, and she knows that she's going to spend a lot of her time on notebooks. So she wishes for a lightweight notebook that provides amazing visualization and feels a lot more native. 

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/540.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=540)

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/560.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=560)

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/580.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=580)

Let's see what we did to address this for Maya. We delivered SageMaker notebook  on November 21st. SageMaker notebook is a serverless notebook that is built from the ground up, and it is available on Unified Studio. It is serverless, like I said, and  it is also polyglot, which means it supports multiple languages. Customers can write SQL queries, Python code, or Spark code, and also interact with charts, all from the notebook. And we provide great inline visualizations. 

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/590.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=590)

Maya can query the data and get the visualization of the data right in the notebook, and she can also do interactive charting. Once she loads the data,  we optimally store the data in the backend, and Maya can interactively create charts. In the backend, we convert the interactive charting requests to blazing fast queries. By the way, in the backend, we use DuckDB for doing that.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/620.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=620)

And if Maya wants more powerful instances, she can switch to more powerful instances.  She can provision more memory, more CPU, or even GPU for that matter, no problems. This basically changed the way Maya could actually do the data engineering and write the Spark code or Python code.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/670.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=670)

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/680.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=680)

So I will hand it over to Brian, who is going to provide a demo on one-click onboarding and seamless data access in the notebook. Brian, take it away. All right, you can hold the mic. I'll just take it. All right, so hello, I'm Brian and we tried to get Maya to come do this presentation, but there's a couple of things. One, Maya is only two dimensional and Maya's not real, so you get me instead. And what we want to talk about here is  the first introduction that Maya would have seen of using the notebook inside of SageMaker Unified Studio. And because  you all can see that I'm 6'2 and the laptop is way down here, I pre-recorded the video because I want to make this as great an experience as I can. So let's go ahead and get started.

So what you're going to notice here is this is one of the landing screens for the SageMaker Unified Studio, but I actually want to talk about what we're going to do here. Are there any data engineers in this room, or are we all developers? All right, there's a few data engineers. So when we get a new piece of juicy data, what's the first thing that we do? Well, first thing we're going to do is we're going to explore the data. And this is different than what developers do. You know, developers just dive in. Data engineers look at their data, they hold their data, they explore their data, they get comfortable with their data. So that's what we're going to do here in SageMaker Unified Studio.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/750.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=750)

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/760.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=760)

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/770.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=770)

So when we get started, we're going to choose a product, and because Maya's new, we're just going to choose the taxi product. And that leads you to a home page, and from here we can now select  the tool that we're going to use. And in this case, we're going to use notebooks. And you'll notice that notebooks have samples or pre-made notebooks, and because  Maya's new, one of her teammates created her a pre-made notebook. So we will go ahead and select this notebook. Now, inside of this notebook are a few  different things. There are three main areas, and what we have is metadata, cells, and the data agent.

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/810.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=810)

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/820.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=820)

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/830.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=830)

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/840.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=840)

So let me pause it here so I can actually talk about this. We understand that everyone here has seen notebooks before, and we understand that notebooks are sometimes a lot of information. So what we did is we broke down the notebooks into sections where you can actually see data that you have access to, the work that you're doing, or maybe you need to talk to someone else, and we'll talk about that later. So let's go ahead and get started with what's on the left side, and on the left side is all the things that you can use in conjunction with your notebook.  So we have shared and local files that could be local to the project or an S3. We have data in catalogs,  and in this data, in this demo we're going to use S3 Tables because I like S3 Tables. And then also, things that we're going to see is I can also see all the connections I have to S3 data.  There's something called variables, which we will talk about later. I find this pretty neat. We can talk about the local computes that we can use with our notebook, and because it's Python,  guess what? We can configure our Python.

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/850.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=850)

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/860.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=860)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/880.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=880)

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/890.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=890)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/900.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=900)

So let's hop right into our exploration. So for those of you who aren't data engineers, the first thing we're going to do is check our  schema and our metadata. And one thing you're going to notice here is Dia mentioned that this is a polyglot notebook. And because we're polyglot and it's 2025  and I'm an anti-Python person, I started with SQL, so you're going to see a lot of SQL up front and then see some Python at the end. And there's going to be very good reasons for both of those. So we're just going to start by querying our notebook, and we're just going to look at the schema and the metadata. And notice some of the rich output. You get  some distributions for the data that's coming out. And notice I'm not doing this in real time. I ran this beforehand, but I assure you this is not a very big file and it's in S3 Tables, and it's actually  not very slow. So we're just trying to get familiar with what the data looks like here. And wait a second, hold on.  As in any data, it's not clean. So we have to go through and figure out, well, what does that actually mean?

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/910.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=910)

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/920.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=920)

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/940.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=940)

We're going to look at some columns and see what the distribution of the data is over those columns.  Some things we might find, and I'll show this in a second, is we might find that this is in Parquet.  Notice instead of having just a table output here, I actually did a chart output. I show this because sometimes you need different modalities for different types of data, so we should use those modalities whenever they're applicable. You'll notice that this is not a very big piece of data. It's only about 1.2  gigabytes, so it's only in about eight Parquet files, and we are just going to continue looking at this.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/950.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=950)

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/960.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=960)

Now we're going to look at the distribution of data.  I know a lot of you who are data engineers are familiar with the taxicab dataset. I was not, and what I found out is this data is not clean. There are negative numbers  in here. As a data engineer, we want to go validate, and notice that I haven't left SageMaker Unified Studio notebooks to be able to do any of this, but I can actually see looking at my data that some of this is not correct. What I'm going to do here is a few more queries just to really dive into what the data looks like and make the decisions on what I plan to do next.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/990.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=990)

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1000.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1000)

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1020.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1020)

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1030.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1030)

Because I'm so confident in my typing, I'm actually going to move over here as well.  One thing that we're going to notice is that you can't have total negative amounts. I mean, I would love that world where I could pay a taxi  cab a negative amount. I wrote this big piece of SQL to actually figure out what that all looked like and look through the mean, standard deviation, and look at the quantiles. You notice that that SQL statement was 68 lines, and this is where I'm talking about where we might want to move back to Python.  I was able to do the same thing in about three lines of Python, but that did not matter with SageMaker Unified Studio in the notebooks because I can actually,  depending on the cell, move to where I need to be. It's all the same compute. It uses the same connections, and I think that it'll be very helpful for people who are trying to explore data in multiple ways.

Some paradigms are only available in Python, like if you want to do plotting and things like that, but everybody knows how to do SQL and they can get started. What this does is it puts us at a place now where Maya knows that she has this data, it has 42 million rows in it, and some of it's not correct. In our next demo, which I will do in a second, we're going to move further onto that. Back to you, Daya.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1100.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1100)

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1150.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1150)

### SparkConnect Integration: Seamless Scalability with Amazon Athena

Literally hands full. Thank you, Bryan. That was awesome. I mean, it was so simple for Maya to set up her studio as well as access her notebooks and start writing the code instantly. She didn't have to do anything, right? It is just the beginning.  Real work happens when this needs to work in production, where the data will scale and Maya has to analyze bigger data. In this segment of the talk, we're going to talk about all the innovations that we delivered to help Maya scale. Maya's wishes continue. Maya is a Spark expert, like I said, and she wishes for better connectivity with the Spark packet. She has access to an outdated way of connecting to Spark. Now she wishes for a more modern way to interact with Spark.  When she wants to scale from one gigabyte to one terabyte, she wants everything to happen seamlessly for her. She doesn't want to configure or tune the clusters.

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1170.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1170)

Let's take a look at how we address Maya's wishes.  I want to give you all a brief intro about Spark interactivity, and I'm going to start by telling a story of making coffee. Running Spark jobs and accessing Spark cluster is a lot like making coffee. In earlier days, accessing Spark means you had to get closer to the Spark cluster or Spark backend. It's like you're going to the kitchen yourself,

grinding the coffee beans, brewing the coffee, and making it on your own. Of course, he had full control, but it was tedious and messy. Then came Livy. Livy is a broker that sits between the Spark client and the Spark backend. It's much better. It's like Maya getting an opportunity to call a coffee shop and place the order. Of course, there was somebody there to receive the phone call. But if Maya wanted to make some changes to her order, then she had to call again. And if Maya wanted to get the status of the order, she had to call again. So the communication was not seamless. It happened in chunks.

Then came SparkConnect. SparkConnect made it all very seamless. It is like a modern mobile app where you can place an order, track the status of the order, make changes to the order, and when the coffee is ready, you can pick it up. So all customers, including Maya, deserve better Spark interactivity like SparkConnect. Let me explain what we did. We took Amazon Athena, which is already serverless and already performing, and made an integrated Spark connector. We made it even better.

[![Thumbnail 1300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1300.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1300)

[![Thumbnail 1320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1320.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1320)

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1340.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1340)

 Amazon Athena Spark starts in seconds, scales in seconds, and customers pay only for what they use. There are no idle clusters, and it scales seamlessly. And with SparkConnect,  customers like Maya can scale seamlessly from one gigabyte to one terabyte without configuring the cluster or without rewriting any of the code. So it's exciting when all three things come together: the notebook,  SparkConnect, and Amazon Athena.

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1400.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1400)

Now the notebook comes integrated with SparkConnect, and it can access the Athena Spark backend using SparkConnect. SparkConnect is smart, so it enables customers to mix and match Python and SQL code along with Spark code. Python and SQL code execute on the notebook compute locally, whereas the Spark plan is shipped to the Spark backend. And with SparkConnect, the interactivity is very seamless. So this changes everything for Maya. She gets amazing Spark connectivity,  and she can scale seamlessly. She doesn't have to deal with the clusters. She gets a great interactive Spark experience all from the notebook.

### SageMaker Data Agent: AI-Powered Productivity Booster for Code Generation and Debugging

Maya's wishes do not end. She continues to ask for more. When Maya is working with the notebook, she needs to write a lot of boilerplate code. And she needs to write code optimally. Although she's a Spark expert, she wants code assistance. Further, when things go wrong, Maya has to go understand what went wrong and figure out how to fix the error. And if Maya is working in adjacent domains or with new libraries, she needs to look up and understand new ways to write code. Of course, all of us are in this situation where we go look up the internet to write the code.

Next, when Maya wants to solve a complex problem, she needs to design a workflow consisting of different steps, and then write code for each step, and then keep all the steps and track them in her head. So it's a pretty arduous task for Maya to keep track of all the steps of the workflow. And when she's working in the context of a notebook in a workspace, she needs to understand the semantics of the data, how data is integrated, and how they are connected.

in different fields of the data. To write optimal and effective code and debug code when things go wrong. Syntax errors are fine. You can get away with them, but semantic errors are hard to fix. So she wishes for a productivity booster.

[![Thumbnail 1530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1530.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1530)

[![Thumbnail 1550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1550.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1550)

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1560.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1560)

[![Thumbnail 1570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1570.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1570)

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1580.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1580)

So how did we solve the problem?  We launched SageMaker Data Agent. SageMaker Data Agent sits along with the notebook and it is always ready to go. It understands the complete context of Maya's world.  All the workspaces, the notebook shelves, everything. It has the knowledge of all the information. Maya can generate code,  including boilerplate code using the context information that the agent has. If there are any errors, no problems. She gets a fix with AI button.  She can fix the code. And then she can express her complex requirements.  The data agent goes and comes back with a plan consisting of different steps.

For example, Maya is a data engineer where she can express her requirements or needs to build a machine learning model. Now the agent goes and comes back with a plan to ingest the data, to transform the data, perform feature engineering, even do hyperparameter tuning and run different experiments to select the optimal model, and then deploy the model. This is cool. This is a game changer for customers such as Maya. It turbocharges the productivity.

[![Thumbnail 1630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1630.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1630)

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1640.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1640)

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1650.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1650)

[![Thumbnail 1670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1670.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1670)

So let's see what all this means for our customers. Let's summarize, a recap,  some of the deliveries that we did. First one is one-click onboarding wherein it takes just over a minute to set up the studio instance,  which gives a clutter-free builder portal with completely revamped information architecture. And Maya's data permissions  seamlessly carry forward with the IAM role that Maya provides during studio setup. And then SageMaker Notebook, serverless, built from the ground up, all integrated with the intelligent assistant. And then we have Spark Connect.  Wherein we have made Amazon Athena even better with Spark Connect. These changes represent significant transformation that we delivered this year to simplify the experience of customers such as Maya.

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1710.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1710)

### Demo: From Raw to Processed Data Using Spark and S3 Tables

I will hand it over to Brian to show everything. Brian, take it away. Alright, so I'm Brian. My surrogate back. Just a little disclaimer, I'm not going to show you everything. We want to go home today. There's a lot of features in this,  but I do want to set the stage for the next bit of demo work that we're going to do here. As a data engineer I learned once you look at your data you've got to make it available for people. So some people subscribe to the medallion method where you have bronze, silver, gold data. Some people also subscribe to the raw, processed, and curated data, and actually those are the words I'm going to stick to today.

[![Thumbnail 1760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1760.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1760)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1790.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1790)

So what we're going to do in this next demo is we're going to see how Maya goes from this raw data, create some processed data that we could actually use for production means, and then also create some curated data. We might demo a little bit of other things too as  we are going through this. Now this time we're going to start from the notebook screen. This time Maya pre-created a notebook that has a little bit of information in here and what we're going to do first is we're just going to remove some of that data. We know there's negative data in here and we're just going to go ahead and remove it and say we don't want to see that anymore. 

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1800.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1800)

And why would we do that? Well, as we are sharing data with others, we don't want to share raw unprocessed data with people. What we actually want to do is do a little bit of a pass of,  performing a little bit of data quality and a little bit of cleanup work for the data.

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1810.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1810)

After the data cleanup work, the next thing we want to do is something that I think is  pretty important. There's information inside of our data that we want to free up for others. In this case, what I want to do is work with some taxi data where we have start and stop times, dates, and locations. Wouldn't it be interesting if we could put some more columns on this data? So this is what we're doing here. We're actually just going to add a few more columns to the data and create a new data frame.

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1850.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1850)

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1860.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1860)

For all you SQL lovers out there, I did not use SQL in this part, and I actually did not talk about where this data is being stored. I might have mentioned it earlier, it's in S3 Tables, but notice that I'm using it like it could have been in S3 directly or it could have been anywhere in the AWS Glue Data Catalog. I think this is a really powerful feature here.  I will pause here to show what I'm doing now.  What ends up happening is we created this new data frame and now we want to persist this data frame back to S3 Tables and Iceberg.

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1900.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1900)

What this code is actually doing here is it's looking at the data, building up the columns, and we're going to store this back to Iceberg. Once we store this back to Iceberg, you're going to notice that if it was in S3 directly or it was in the Glue Data Catalog directly, it still would have been fine. Notice that I created this new Iceberg table here with not a lot of effort.  Those of you who are paying real close attention, you're going to notice that this time I did speed it up. It did take like 30 seconds and I didn't think you all wanted to hear me tell jokes for 30 seconds.

[![Thumbnail 1910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1910.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1910)

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1920.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1920)

 Now I'm actually verifying that my table was created, and just note one thing here as I'm talking. When I create it, whenever I have a data  set and I create data and I create some data frames or I do some local data, that's my data, or maybe someone who has access to the notebook can see it. But now when I put it in S3 Tables, now it can be available outside of this notebook experience. I think that's a very important thing as a data engineer or even a data scientist to be able to make your data available in the context that people want to be able to see it.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1950.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1950)

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1960.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1960)

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/1970.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=1970)

### Data Agent in Action: Context-Aware Code Generation and Error Fixing

So we transformed our data and now I'm going back to the SQL folks. I'm actually creating a query here and what I want to do  is get a little bit more data sciencey with my data here. I want to show you a  pretty neat feature, a pretty neat feature of notebooks. Notice I did this query and did a little bit of aggregation, just trying to see like when people are traveling.  Dayanand mentioned that we have Data Agent which is available in the notebook. I'm going to be very honest with you all. I wrote no more of the code in this demo. The rest of this code in this demo, the rest of this session that we're going to talk about is showing the powers of Data Agent and the context that it has over the data that you're having here. So everything that I'm not writing outside of this right column is generated by the agent, and I just want to show you the integration that we have here.

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2010.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2010)

[![Thumbnail 2040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2040.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2040)

One thing you'll notice is that I was talking about variables earlier.  Every time we create something, when we create an output, it gets saved to a variable. Because it gets saved to a variable, what I can do is I can actually refer to that variable when I'm talking to or when I'm conversing with the agent. What I'm actually asking the agent is I'm saying, hey agent, you have some data over here. Why don't you just tell me when most of these trips occurred? Now I'm thinking like a fledgling  or a neophyte or a new data engineer, you know, let's build up some confidence here.

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2050.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2050)

Our Data Agent is like, I got that boss, and it starts writing some code.  Notice that I'm just accepting it, and the reason why I'm accepting it is because it is a demo. But it actually knows the notebook, so it's writing code in the context of the notebook. You'll notice some other things. If it knows that I'm using SQL, it actually knows my preferences here. Now the Data Agent wrote some code and just like every other people or other people who write code, sometimes it doesn't work right. Not only can the code be written, it can write code for you, it can also debug code. Notice I have an error here and I scrolled up and I actually saw what it was pretty instantly and I was like, nope, Data Agent is going to do it and it didn't include pandas.

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2100.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2100)

 Now I am exploring my data and

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2120.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2120)

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2130.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2130)

notice I wrote no code. I just used Matplotlib, and it is actually going through. Now, as a human, I can use my human intuition to determine what this data looks like and whether it's doing the right thing for me. So we're going to keep on moving on, and I noticed that  I thought it was only going to do one visualization. It actually created a way more robust example of showing me when trips were being taken than I thought I was going to see. 

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2170.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2170)

So the next thing I'm going to do here is I'm actually going to say, hey, I am now a data scientist on the weekends, and what I want you to do is I want you to help me understand where people are taking trips. Notice I did spell the word wrong, that was not in the demo, I did that on accident. But this is taxicab data from New York City, and notice that last sentence I just said, break it down by borough. Now I'm trying to test the limits of what our models understand and what context they can actually bring to the table. It was like, once again, okay boss, I can do that. 

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2180.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2180)

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2190.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2190)

So the first thing we're going to do is we're going to say, hey, I know what New York looks like, and I'm just going to draw bounding boxes around all the boroughs in New York City. I'm like, alright, I'm intrigued.  And then it's like, I'm going to do some more things, and I'm actually going to transform this data and show you a visualization. Once again, I'm like, I'm intrigued. 

[![Thumbnail 2200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2200.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2200)

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2210.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2210)

And it's still going on, and notice one thing that it's doing is it noticed that I like to use the markdown blocks to separate things, so it  started inserting markdown blocks. Our Data Agent actually doesn't just know about the cell that you're working on, it knows about the full context of the current notebook. So any time that you communicate  with it, it's actually taking the full notebook into context.

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2230.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2230)

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2240.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2240)

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2260.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2260)

So we're going through this, and notice that most of this started from a three-sentence conversation. I'm going to keep on going, and it's creating some graphs. I'm like, wow, this is kind of interesting, and what I'm going to see  here is, you know, we inevitably run into errors. The worst thing about running into errors in a notebook is, well, now I have to fix that error, and how do I fix that error? But  the neat part here is that the Data Agent will actually fix it. There is a Fix with AI button, and it will actually work through using the context of your current cell but also using the context of your entire notebook to determine exactly how to make the fix. Like I said, my goal  here was to write no more code other than the prompt, and we're going to actually see what happened.

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2270.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2270)

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2310.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2310)

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2320.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2320)

So now I was thinking, and I didn't actually, I do know what the issue is, and I'm  working to see where I would make the fix. But it was like, alright, I'm going to make this fix for you. There was a missing dataframe that I think that needed to create, and it went ahead and created it. Now we have all these visualizations about all of my trips, and notice that even as a data engineer who is used to moving data from here to there but maybe not as well versed in doing the analysis part, like the more sciencey part, or you know, I don't like doing images because I'm scared of Matplotlib and things like that, you know, I think that this is a great empowering function that allows you to expand where you are  so you can start with I know where I want to go. And this is going to say, instead of taking the yellow brick road, you know, take this road instead. 

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2330.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2330)

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2350.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2350)

[![Thumbnail 2360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2360.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2360)

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2370.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2370)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2380.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2380)

And so as you see here, I'm like, well, now, okay, I believe you. So the things that I wanted to do in this demo were two.  I wanted to create some processed data that I could actually share, and then I wanted to create some curated data that I could share with a smaller subset. Maybe it's actually a smaller dataset. So this is actually what we're doing right now, is we're creating a curated dataset that we  can use or someone else can use for further analysis. And like I said, I am not typing anymore. You know, I'm just trying to go  through, and I'm going through. Now it's not just me working on it, it's a combination of me and the model working on this. The reason why I'm showing this more than once is  because, you know, one time was a neat trick, Brian, but you know, if I do it two or three times, you're like, alright, well, maybe I do believe that this works. Notice here that we're talking in different modes. 

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2400.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2400)

So we're doing some graphs, we're doing some graphics data. We're doing some create new dataframe data, but also we're doing some, hey, let's create a new table data. Now we have this curated dataset that we can actually use and we can use for other purposes. 

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2410.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2410)

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2430.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2430)

Once this creates, we'll go through and actually validate. You always have to check if your tables were created properly,  and the neat part here is that you didn't have to leave this tool, and you won't have to leave this tool to see what your data looks like. If you come from Athena, everyone likes the Athena interface on the left side where you have the ability to actually see all your data. We're providing that here for you.  And you can actually see now we have a new table.

[![Thumbnail 2440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2440.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2440)

So how I like to end this is that you might not want to use the AI piece,  and I think that's a great choice if you choose to make it. But at the same time, I like the idea of having the option to be able to build, have context, and build on top of that context, and then share whatever you're creating somewhere else. So this is actually, I think, one of the biggest pieces of the data notebook. It's not just that I'm working in cells now. It's that I'm working in cells along with the model. I'm working in cells and I can share this, and I can actually have access to multiple data types. It's not that I'm doing, you know, I can work in cells. It's also I'm working in cells and I have variables that I can rename dynamically.

And then there's something else that Daya kind of highlighted, but I did not include in the video. When notebooks have compute attached to them, you can change the size of compute. You can add more cores and you can add more memory. But once you outgrow that and you need to go into the place where you want to get data that is bigger than the memory of a particular machine, and you want to actually have the power of Spark, you can move there, and you don't have to make any of these changes. Daya mentioned that Spark comes up in only a couple of seconds. We were using Spark for the whole entire time, and I know this is not in real time, but there was no delays anywhere in this demo for, hey, we're moving to Spark now, now we have to change how we're operating. No, Spark was seamlessly included in this through Spark Connect.

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2550.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2550)

And you know what? If you choose to say that I want to work with Athena SQL, guess what? You can do that too, cell by cell by cell. So the whole point of this section is we really want to provide a notebook that not only works with you, but it grows you into things that you weren't thinking about doing before. As I'm going to  summarize this section here, it really is work with all your data. The idea is you have data, we want you to be able to work with it. I don't want you to work with it in a whole bunch of tools. I want you to be able to work with it in one place. We can use a data agent. We are used to now using models to write code. Well, let's write code in a data context. The data agent inside of there actually does understand data a little bit better. And then also, now that we have all of our data sets in front of us, it does become easier to create and manage our data sets. So I'll move back over to Daya, and he will bring us home.

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2620.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2620)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2630.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2630)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2640.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2640)

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2650.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2650)

### Transformation Complete: Quantifying the Impact and What's Next

Yeah, thank you, Brian. That was a really cool demo. Let me, oh, sorry. Okay, so everything that we saw demonstrates how Maya's experiences transformed,  transitioning from a collection of AWS services.  And how there are three steps here, right, to simplicity. To begin with, there was a collection of purpose-built AWS services, and Maya had to figure things  out on her own to integrate the services, to access the services, and make them work together. And then we launched the SageMaker Unified Studio.  With this, Maya had an ability to access all the services under one umbrella, all the capabilities. She didn't have to set anything up in the sense she didn't have to integrate them. So everything was integrated at one place.

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2670.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2670)

 And then with all the new releases, the simplicity is taken to the next level, where she gets the always-on notebook that is serverless, instant onboarding, seamless permission carrying forward, and Spark Connect, which is connected to Athena to give a superior interactivity as well as seamless scalability. So it means, let's take a look at the impact. We're trying to quantify the impact here.

Onboarding time is down from hours or days to minutes. With the AI assistance, you can write faster code, better code, and become more productive. The notebook brings everything together, so you don't have to switch across multiple tools. You can spend most of your time in the notebook instead of hopping across multiple tools.

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2760.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2760)

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2770.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2770)

There are no idle compute clusters, so you pay only for your usage. Scaling from gigabytes to terabytes is seamless without writing any single line of code or configuring or fiddling with your clusters. Your overall data pipeline development  can be done in a matter of a few hours instead of days that it used to take previously. There is significant improvement in  the experience for our AWS customers.

At the beginning, we asked this question: Why can't simplicity itself be the innovation? We worked backwards from customers to remove complexity to make sure everything just works. We delivered one-click onboarding, an always-on notebook that is available for you instantaneously with AI assistance built in, and seamless interactivity with the Spark backend and scalable Spark cluster, all with one goal in mind: to make sure creativity takes center stage.

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5ffdbeb9ea159d3d/2820.jpg)](https://www.youtube.com/watch?v=203JsHlad2U&t=2820)

So talking about what's next, it's available today from the AWS console.  We highly recommend you all try it out. Tomorrow at 10:00 a.m. in MGM Grand, there's a chalk talk, so if you want to dive deep, it's a great opportunity for you to do that. We also highly recommend visiting the expo floor. We have a booth there, so you can get demos, try it out, and get hands-on experience.

Finally, we are really excited about what we have built, and we can't wait to see what customers like you all can do with this. Happy to take any questions. We'll take them down.


----

; This article is entirely auto-generated using Amazon Bedrock.
