---
title: 'AWS re:Invent 2025 - Agentic data engineering with AWS Analytics MCP Servers (ANT335)'
published: true
description: 'In this video, Harshida Patel and Ram Nottath demonstrate how AWS Analytics MCP Servers enable agentic data engineering to address productivity challenges like context switching and manual workflows. They introduce Model Context Protocol (MCP) as a universal integration layer connecting AI agents with AWS services including Glue, Athena, Redshift, EMR, and DataZone. Through a live demo using Kiro, they show agent "Alex" autonomously discovering datasets, creating ETL jobs, monitoring execution, cataloging data, validating results, and generating documentation—tasks that traditionally require weeks. The session covers MCP server configuration, security controls, human-in-the-loop patterns, and scaling strategies using SageMaker Unified Studio and Bedrock AgentCore for enterprise deployment.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/0.jpg'
series: ''
canonical_url: null
id: 3093354
date: '2025-12-08T23:17:29Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Agentic data engineering with AWS Analytics MCP Servers (ANT335)**

> In this video, Harshida Patel and Ram Nottath demonstrate how AWS Analytics MCP Servers enable agentic data engineering to address productivity challenges like context switching and manual workflows. They introduce Model Context Protocol (MCP) as a universal integration layer connecting AI agents with AWS services including Glue, Athena, Redshift, EMR, and DataZone. Through a live demo using Kiro, they show agent "Alex" autonomously discovering datasets, creating ETL jobs, monitoring execution, cataloging data, validating results, and generating documentation—tasks that traditionally require weeks. The session covers MCP server configuration, security controls, human-in-the-loop patterns, and scaling strategies using SageMaker Unified Studio and Bedrock AgentCore for enterprise deployment.

{% youtube https://www.youtube.com/watch?v=5MldQWiCKX4 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/0.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=0)

### Introduction: From Cooking to Data Engineering with Agentic AI

 Hi, good morning. How's everyone today? Good. Excellent. So first of all, thank you so much for joining us today. Before we start the session, think about that you are a chef and you want to prepare a dish for your friend because it's his birthday. As a chef, you have the end goal in mind, which is the dish that your friend likes. But through the course of preparation, there are so many steps involved. You have to get good quality food, the spices, the temperature needs to be right, and you have to put the ingredients in the right order, and finally, there is a dish that your friend likes.

So when we think about data engineering, it's nothing different. You have the end goal in mind to build a scalable, reliable data pipeline with quality that serves your business use case, so you can make the decision at the right time. But with data pipelines, there are so many steps involved. You need to catalog the data, you need to profile the data, and then you have to build the jobs. You have to orchestrate it, you have to deploy it in production, and then provide it to the end users for reporting or to serve them. But what if through each of those steps, there is an agent which works as a pair programmer for you?

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/120.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=120)

So this is our session today to walk you through Agentic Data Engineering using AWS Analytics MCP Servers. I'm Harshida Patel, a Principal Specialist Solutions Architect from AWS, and joining me today is Ram Nottath, who is a Principal Solutions Architect with Data and AI.  So this is our journey for this session. We'll start off with the data engineers' pain points and the challenges they run into, followed by using Agentic AI as the solution with Model Context Protocol. Then we'll do a demo, share with you best practices, and how you can deploy this in production at scale and share resources that you can walk away with.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/150.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=150)

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/170.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=170)

### AnyCompany's Challenge: Data Engineers Struggling with Productivity Bottlenecks

 So let's get started with Meet AnyCompany, which is a fictitious retailer. They want to transform their business by transforming the customer experience by tapping into the data. They have a very clear vision and a mission that they want to put data to work and make an impact.  So they start off with a solid data strategy where data is the foundation. They want to build scalable analytical pipelines with quality to serve their growing use cases, but their main goal is to provide this insight into the hands of the decision maker at the right time so they can make a data-driven decision. So this is their vision and their mission. But when we check the reality, it's completely different.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/200.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=200)

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/210.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=210)

[![Thumbnail 220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/220.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=220)

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/230.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=230)

 A marketing team reaches out to the data engineers and says, this is a new promotional analysis that we want to build and get insight into.  Can you build a data pipeline for this? They say it's going to take two to three weeks. The operational team wants to get near real-time insight into the inventory.  The data engineer shares our data pipeline is already lagging three to five days. The executives want to automate  the Monday morning report so they can make the decisions at the right time, but it's still a manual refresh.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/240.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=240)

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/260.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=260)

So this is impacting  the marketing team, the operations teams, the executives. So the bottom line, it's impacting the revenue because the decisions cannot be made on time. When you further deep dive and ask the data engineers, what is the cause for this productivity or what is the cause for these delays,  I would love to ask you a question. Does any of this resonate with any of you? Context switching starting with number one. So when the data engineers get the requirement, they want to embed a new feature within the product, so they search through the code, they try to understand the code. There is no documentation.

Once they understand the requirement, they go and do web search to figure out the best practices to write the code, figure out how this particular feature in AWS really works. Then they start developing the code, writing things from scratch, and while they are writing and focusing on the development, there is a data analyst who sends an alert that the key performance indicators for today are completely off.

So they start debugging. They start looking at the data, and they finally figure out after hours of effort that there is a source data quality issue. We missed an entry in the dimension, so when you're joining to the fact, it's not able to relay the key performance indicator metrics. So again, data quality is more reactive, it's not proactive. They go back once the data is fixed, they go back to do the development, but then there is an alert that the data pipeline today is very slow and it's not performing. So again, the data engineer switches the focus, figures out what the performance issues are, and then tries to go to the web search to find out how to optimize this pipeline. This is where the time gets lost.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/370.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=370)

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/400.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=400)

So all of these things are piling up. Then one fine day, Sarah Chen, who is the CTO of data engineering, sends an email that we are missing  to provide the data insights to the marketing team, the operations team, as well as the executive team. Sarah heard from one of her friends at another retailer that they are able to see productivity gains while building the data pipeline using agentic AI. So Sarah asked the team whether we continue to do something which is not working or radically shift the direction.  So let's work backwards from what the ask is and see what is possible.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/410.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=410)

### Traditional vs. Agentic Data Engineering: Understanding the Agentic Loop

Let's use this use case of traditional  data engineering versus agentic data engineering. So let's go back to when the developer or the data engineer got the requirement: find the retail customer data set, process it, check the demographics, validate it, and upload it to a data warehouse for reporting. So what does a data engineer do? If the data set is cataloged, they would go to the catalog, they would search the location, and they would be able to find out the column and the corresponding data type. Once they have identified it, they start writing the code from scratch, they deploy it, they run it. If there is an error, they fix it. Then they write a SQL query to validate that data set just to look at whether there are any null values or whether there are fifty states in the data set. Then they write a Python code to load this into a data warehouse.

Versus with agentic data engineering, what if for each of the steps, instead of writing and typing and going through the console, it's converted into natural language? So first, find the retail customer data set. Once you find it, create a Glue job, deploy it. If there are errors, you fix it, you validate the data set, and then you finally load it to a data warehouse. So the manual process of clicking through is converted through agentic data engineering by asking questions in natural language and having the agent do the work on your behalf.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/520.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=520)

So how is this possible? This is where the agentic loop comes into play, where you have an agent  where we exactly ask the same question: find the retail customer data set, process it, validate it, and upload it to a data warehouse. This is your question, and your question becomes the goal for the agent. It has cognitive insight. So as a data engineer, once we get the requirement, we try to understand it, we try to plan it, we try to take action. Then once the action is performed, we try to make decisions. So all of this is the reasoning. And while the large language model takes your request and parses it out into multiple steps, it also needs information, so it's able to identify that it needs more data to get more context to figure out what the next course of action should be.

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/580.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=580)

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/590.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=590)

So the agents have access to the tools where they can execute not only code but also retrieve more  information. And this context goes back to the large language model where it's able to see and perceive from the new  information it got, and then figures out what next step it has to take. So this loop of reasoning as well as taking the action until your goal is met is what the agentic loop is.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/620.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=620)

Now let's dive into the building blocks of Agentic AI. The agent is at the center, and the agent has memory. It has short-term and long-term memory. The short-term memory is for  the context of your session. The long-term memory, think of it as episodic, what happened in the past, like your personal diary. It can also have semantic memory, where it has the context of your domain or domain-level knowledge.

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/640.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=640)

The agent has an arm where it has access to tools,  and the tools are something that you control, whether you want to have a function for the agent to access your data, your AWS services, and this could also be external services. Tools are the actions that the agent can take. It's calling a function. The function can be to retrieve the data, run a code, or invoke an API. So that's the action.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/670.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=670)

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/700.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=700)

The agent has a brain, so we talked about reasoning. The next  step is it identifies multiple steps that it needs to take to solve a complex problem. It takes a step, it gets the insight and the results from the tool, then it reflects on what should be the next course of action. So that planning, reflection, chain of thoughts, as well as self-critiquing. The main thing is action, and this is also what you control, whether you want all the actions to be on autopilot  or you want the human in the loop for some of the actions.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/720.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=720)

An agent, along with the memory, the planning, the reasoning, along with the tool, is what automates and independently works to perform the task that you provide, which is the goal for the agent.  And for an agent, give it a persona. For our talk today, we'll give it the persona of a data engineer. The goal is to build a data pipeline and provide the instructions precisely so it can think better.

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/740.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=740)

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/750.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=750)

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/770.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=770)

### The Integration Challenge: Why AI Agents Need Standardized Tool Access

Now let's summarize all of this.  We talked about the agentic loop, we talked about the building blocks, but now let's put a simple example of a workflow.  So you, as a data engineer, are asking a question to build the pipeline in natural language. Your interaction is with the agentic assistant, and then the agentic assistant is already integrating with the tools, your data sources. So along with this question, the tool descriptions are  sent to the large language model.

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/810.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=810)

The large language model, as it has parsed out your complex question into multiple steps, decides whether it wants a specific tool or to do a function call, so it makes that request back to the agentic assistant so that it can get the context. The agentic assistant invokes the function to retrieve the information from your data source. It gets the results, and these results are sent back to the large language model. Now the large language model gets  that information of the results in context. It's going to reflect, it's going to figure out what is going to be the next course of action.

In our use case, we started off with retail data customer dataset which is on S3. So it's going to request, find me the list of, find me the location of retail customer dataset. The tool it's going to call, whether it's List S3 or whether if they are using DataZone, it will invoke an API for DataZone. The agent calls that tool, performs that function. The results are returned, and that information that, hey, this is the location of the S3 dataset, is sent back to the large language model.

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/860.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=860)

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/890.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=890)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/900.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=900)

But the next step, we need to create a Glue job. So the large language model  is going to call another set of functions. So this entire loop is dynamic in nature. This loop is going to continue until it has all the information to meet your goal and provide a final comprehensive response to the end user. So we saw that we have agents, but the agents just with large language model is not enough. It needs to have the context. It needs to have the information. It needs to perform an action,  so the integration of the AI agents with the tools and the data sources becomes important. 

So why is this a little bit challenging to integrate the data sources and the tool with an AI agent?

How do we integrate the data sources and the tools with an AI agent? Let's consider this use case. You have an AI agent. When you need to talk to a data warehouse, it uses SQL. When it needs to talk to, say, a Glue catalog or it needs to do data processing, it's going to use API. What if there are specification changes? Then they have to update the specification to adhere to the API. What if they need to add another tool or remove a tool? This might be simple if you have one agent, but when you start creating n number of agents, think about the tight coupling and the integration problem. This is all custom integration. This is n times m custom integration problem.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/950.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=950)

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/960.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=960)

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/980.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=980)

### Model Context Protocol: The Universal Language for Agentic AI Applications

So how do we simplify this? How  do we make it seamless for the AI agents to discover what are the data sources and what are the tools which are available? This is where Model Context Protocol comes into play.  It's a universal language. So in this example, you want to add a tool, you add a function. If you want to remove a function, you remove the function from MCP. If there is an API change, you make that change in one single place. Then you can integrate one agent or you can integrate a number of agents.  This makes the integration standardized integration.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/990.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=990)

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1010.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1010)

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1030.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1030)

So what is Model Context Protocol?  It's USB-C for agentic AI applications, and this was developed by Anthropic. So let's look at the architecture for MCP. There is MCP hosts, the one with which you are integrating. There is an application  you can use, an application of your choice. It could be Amazon SageMaker Unified Studio, Kiro, Claude Desktop, and those applications need to be MCP compliant. So there is the client side, and these clients are integrating or communicating with the MCP servers.  And this integration from the client to the server is known as the transport layer or communication model.

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1060.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1060)

There are two deployments of servers. When the server is locally deployed, where it's using the compute of where your host is, that is the local deployment of MCP server, and the communication it uses is standard input output. If the MCP server is remote, it's going to use HTTP. Now the server is the one  that is integrating with your data sources, your data, which is in your control. So the way the MCP server integrates with the data sources, it could be SQL, it could be API. It's completely independent depending upon the services that it provides.

So let's take an example here. Say you're working with Kiro. I'm going to use Kiro because it was announced even yesterday with a lot of features. You ask your question, the same question, right? We are going to repeat throughout the talk. Find me the retail customer dataset, process it, validate it, and then load it into the data warehouse. The large language model, think about it, it's going to break it down into multiple steps. For the first step, because of this integration of MCP client, it also has the information of the tools that it can use. So the first step is find the dataset retail customer data. The tool it wants to use is list S3, or if there is a data catalog, go through and search through the data catalog. So the client sends that request to the server. The server is the one that is running the function to integrate with your service. Then the response is sent back from the server to the client and to the large language model, enriching the context, and it figures out the next step.

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1150.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1150)

So what are the components on the server side? There are three components. Tools,  consider it, not consider, it is the functions that the client can call. For example, create a Glue crawler or create a Glue job, for example. Resources are read only, get me the list of all the Glue tables. And then you have prompts, which are blueprints or structured prompt templates. If you want to take a picture, I'll go slide back. Okay.

[![Thumbnail 1190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1190.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1190)

So now we talked about how agentic AI, which is the reasoning and the action and the loop and the integration with the tools, is going to help  work independently to serve a data engineer to perform the tasks.

### AWS Analytics MCP Servers: Addressing Context Switching and Best Practices

In a typical analytics stack, you could be using multiple services like AWS Glue, Amazon EMR, Amazon Athena, or Amazon Redshift. The AWS Analytics MCP servers address the first problem that developers face, which is context switching. These services cover multiple AWS services simultaneously. The second challenge they address is getting the latest and greatest information on features and best practices. When a data engineer asks a question like "Build me a code," that code will be of high quality because it is aware of best practices. When you're trying to build a workflow, process a Glue job, and then load data into a data warehouse, it's like workflow orchestration. The MCP servers have the domain knowledge of all these services to work with effectively.

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1260.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1260)

[![Thumbnail 1270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1270.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1270)

[![Thumbnail 1280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1280.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1280)

The MCP servers work with AWS services when you are using Amazon Glue  for data processing, Amazon EMR for your data processing, or Amazon Athena for interactive queries.  There is a data processing MCP server, and the questions a data engineer can ask include creating a Glue crawler. These are only a few examples  from the list of questions. We'll also share with you a QR code where you can find the entire list of MCP servers along with the tools that each one supports. The LLM is the one which decides which tool to invoke. For example, if you call for a Glue crawler, it knows that there is a function associated with Glue, and it's going to call that function. But when you start asking questions about your dataset, such as "From the sales data, identify the top 10 products," the LLM is aware that this is a query, so it's going to invoke the function which is associated with Amazon Athena. Again, best practices are integrated with all of these workflows.

[![Thumbnail 1330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1330.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1330)

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1340.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1340)

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1360.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1360)

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1370.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1370)

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1380.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1380)

[![Thumbnail 1390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1390.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1390)

When you're using Amazon Redshift, which is a data warehousing service,  there is a Redshift MCP server. This Redshift MCP server is read-only and provides you with information. You can say "List all the endpoints in my account," or if you have two endpoints  and you want to query across those two endpoints for customer information, you have the ability to do so, but it's read-only. If you are using DataZone, there's a DataZone MCP server where you can list and subscribe to data assets. When you are using Amazon Managed Streaming for Apache Kafka,  think about an admin persona who wants to create a cluster, set the retention log, or get insight into  resource monitoring. The MCP server for MSK is designed more from an admin persona perspective. When you are using OpenSearch, the OpenSearch  MCP server allows you to list indexes, but you can also ask questions about your dataset. Again, this is only a small list  of the MCP servers and the corresponding natural language questions you can ask.

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1400.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1400)

Now, we've walked through the theory part. Let's now dive deep into the action and  see how to make it possible. I'd like to hand over to Ram. Thank you so much for joining us.

### Architecture Overview: Configuring MCP Servers in SageMaker Unified Studio and Kiro

Thank you, Harshida, for that amazing overview of the agentic loop and the capabilities of the various analytics MCP servers. Just a quick check, am I audible? All good? Alright. As a solutions architect, I help multiple customers implement data and AI solutions. When I talk to them, there is one thing that is consistent, one requirement that is consistent across all customers, which is the need for data engineering. I'm pretty sure we all are here because of that, right? Now, the AnyCompany Retailer that we were talking about is not any different either. They have the same challenge.

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1470.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1470)

Let's take a look at how we can implement a solution which will help you do this data engineering much faster. Here is an architecture that can help you with that. This can actually give you  state-of-the-art data engineering with agentic capabilities that Harshida was mentioning earlier. Now, if you look here, we can see the various MCP servers coming together to help with the tasks that a data engineer would like to do. There are multiple tasks, whether it's governance, data processing, or SQL querying. There are multiple things that a data engineer would want to do, and these servers come together to help the data engineer with those tasks. Let's take a look at it a bit more in detail.

Let's start from the left. Here the user is interacting with the MCP host, the one that Harshida was mentioning earlier, and that MCP host could be any tool of your choice. It could be SageMaker Unified Studio or Kiro or Cloud desktop, any of those MCP compliant MCP hosts. Once you have an MCP host, the next thing that you do is configure whichever MCP servers that you want to work with. When you configure that, it makes a connection between the host and the server using the MCP client.

Now the intelligence part of this whole agent lies below, which is the large language model, and in this case we are using Amazon Bedrock's large language models. When the user asks a question, saying this is a task that I want to do, the LLM interprets what the user is looking for. The LLM also understands through the MCP host and the client that these are all the tools that I have at my disposal and how can I best deliver the result to the user. If you remember what Harshida was mentioning earlier, there's always a loop or it's an iterative process because we may not get the answer right at the first attempt since there are multiple steps involved in the task.

So what the LLM does is it uses the first tool, gets the data or does the first task, gets the result, and then identifies what is the next tool which can solve or continue solving the problem that the user has asked. That iterative process finally takes you to the final result that you want to get to. Now let's take a look at how we can configure MCP server in some of the tools so that you can get going with some of these activities.

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1620.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1620)

 Here is a screenshot of how you would do this in SageMaker Unified Studio. If you're familiar with SageMaker Unified Studio, this is the notebook experience there. On the left hand side, you will see the Amazon Q icon, and once you click on that, there is a tool icon on the top of the right corner. Once you click on that, it will give you a pop-up to provide the details about the MCP servers that you want to configure. All you need to do is provide the name of the MCP server and a couple of configurations, and you're done. In this particular case, I'm configuring the Data Processing MCP Server with this simple step. Now you have the capability to interact with your various data processing services in natural language.

[![Thumbnail 1670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1670.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1670)

 Now let's take a look at how you would do the same thing in the case of another tool, Kiro. It's not any different, just that in the case of Kiro, there's an MCP JSON file. All you would do is have a block of JSON configuration that you need to add for each MCP server that you want to bring in. So you might be bringing in Redshift MCP Server or DataZone MCP Server or Data Processing MCP Server. For each one of those MCP servers, you will have a JSON block that you need to add. You might see some of the auto-approved list and things like that. We will cover that later in the session, but this is how easy you can go and configure the MCP in your MCP host application that you have.

[![Thumbnail 1720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1720.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1720)

[![Thumbnail 1730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1730.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1730)

### Meet Alex: Live Demo of the Data Engineering Agent in Action

 Now, let's get a bit more specific into the task that we have at hand for AnyCompany Retailer. Let's meet Alex,  the data engineering agent. Harshida mentioned the task that we have for AnyCompany. We want to start with the help of Alex. We want to start with data discovery, and we want to make sure that Alex is able to go and find out what is the customer behavior and customer dimension data files.

[![Thumbnail 1750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1750.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1750)

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1770.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1770)

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1780.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1780)

 Then we want Alex to go ahead and create an ETL job. In that job, the activity that we want Alex to perform is to join the two datasets based on customer ID and aggregate the data based on state. Once that is done, of course we created the job, but we want Alex to go ahead and run that job.  It's not just about running. We want Alex to help us with monitoring the job and fix any issues if there is anything coming up.  Then we want to catalog the output.

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1790.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1790)

After cataloging, we want to do some data validation because at the end of the day, anything that we do needs to be validated, so we need to do some data validation.  Once everything is done, if you remember the marketing team, the executives are really looking for the data in their data warehouse, so we want the data to be loaded to the data warehouse. This is what the data engineering team at AnyCompany Retailer has at hand. There's a task, so let's take a look.

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1810.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1810)

As a first step,  we'll take a look at how to configure the MCP server in one of the tools like Kiro. So I'm in Kiro, and this is the lighter theme of Kiro. Typically you might have seen the darker theme there. So here I have configured multiple MCP servers: data processing, Redshift. And you will see the various tools that are listed, and you can also see the other MCP servers that I have configured. You can either enable or disable them using a right click.

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1850.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1850)

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1860.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1860)

Now, once you click on that little icon over there, that's what will open up that JSON file. So it's a matter of just adding that little block of JSON configuration so that you can add any MCP  server that you want to bring in. So that's the data processing MCP server, and here you have the S3 MCP server. Here is the Redshift one. So these are MCP servers that I've configured. It's a  matter of adding that particular block. Once you're done, that's it. That's all the configuration that you need to do. You are good to go and work with these services using natural language.

[![Thumbnail 1870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1870.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1870)

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1880.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1880)

 Now this is the prompt that we are giving to Alex. Let's not worry about all the things that are written there,  but the major things are: hey Alex, you are a data engineering agent and we need your help in identifying the data set and processing the data. But one thing, human in the loop is important. So after every step, come and ask me if whatever was done in the previous step looks fine so that you can move forward. So that's a confirmation that I'm expecting. We are asking Alex to come back and ask, and then move forward with the six steps that we just looked at.

I want to go ahead and find the data, then create a Glue job. And here we are becoming very specific this time. We want to join based on customer ID, and I want to get the sum of page views and total purchase amount and land the data set in Parquet. And of course the aggregation is based on the state, right? And then run the job, fix any errors, and monitor the job. Then go ahead and create a crawler so that we can catalog them. We can very well put that in the same step itself in the job creation or job execution itself.

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/1970.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=1970)

Now the next one is validating the data by running the queries. And then finally, this time we want Alex to create a notebook so that we can use that to load the data into Redshift. There's a task at hand. Now let's move to step one. So here  we're asking Alex, hey, can you please, these are all the steps that are involved, right? So hey, you are a data engineering agent. These are all the six steps that we want you to go ahead and run, but make sure that you come and ask me after each step.

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2010.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2010)

Now let's take a look. You can see that the very first thing that it is doing is it's picking up an MCP tool to list the S3 buckets. And it is going and analyzing the data that is there in S3, because again in this particular case we have specifically said that hey, go and get the data that is sitting in S3. Now in your case, the data might be sitting in, it could be in Redshift, it could be in other places, or the data might be cataloged in  a data catalog. So you can use a respective MCP server's help to go and scan for the data, find where it is, and get that specific data.

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2020.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2020)

[![Thumbnail 2030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2030.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2030)

[![Thumbnail 2040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2040.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2040)

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2050.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2050)

 So it is able to find the data, but it is still trying to do something. You know why? Because we have asked in the next step, you need to go and join these data sets based  on customer ID and go and do the sum of page views and amount and things like that. So even though it is able to find the data set, it is still going and verifying,  hey, do I have those fields in there? Because if I don't have those fields, I cannot really move on to the next step. So it is thinking ahead and making sure that those fields are available, so  it is able to find the customer ID, customer name, city, state, all of those details.

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2060.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2060)

So there you go. Step one is complete. So you have the data sets,  that's where it is, and then you have the customer ID, product ID, page views, all of those details, all good. Hey, I'm done with step one. Can I proceed to step two? So cool, data discovery is done. Let's move to step two. Let's say yes, proceed to step two.

Okay, so this time, the expectation is Alex will help us create a Glue job. Now to create a Glue job, first and foremost, if you look at the first activity that it is trying to do, it is using another MCP tool there to get the roles. What that really means is Alex knows that if it has to create a job, it needs to understand what is the role that should be used with that job.

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2110.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2110)

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2130.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2130)

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2150.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2150)

The role should have access to the right resources.  It should have access to the three datasets, the AWS Glue job resources, and all of those things so it is able to get all the data. The agent went ahead and created a job.py file, so you can see the .py file here. In a real-world scenario, we should ideally validate some of  these things before allowing it to run, so let's assume that everything is fine here. Right now it is asking, "Hey, can I go ahead and upload this to S3?" We will talk about why it is asking us for permission to upload. For some operations, it went ahead and did that, but for some of the tools, it is coming and asking, "Can I do this?"  We'll talk about that a little bit later.

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2170.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2170)

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2180.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2180)

Here you can see that it is using another operation, which is create job. So it went ahead and created the job, all good, so it looks like the job creation is complete. Step two is done.  It's making sure that there is a good job run to see whether the job creation went successfully. So we have the job ready.  Now we can move on to step three, so looks like job creation is done. Let's get to step three.

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2190.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2190)

[![Thumbnail 2200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2200.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2200)

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2230.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2230)

This time, what we are hoping for is that Alex will go and run this job and monitor it  and fix any errors if there is anything. This time it called another tool, which is manage AWS Glue jobs,  and this time it is starting the job. Again, think about the way we would work. It is thinking ahead and it is also going through that step-by-step process that we as data engineers would have done, giving us the assistance of what we want to do. Since it started running the job, let's get to the AWS Glue console just to see how things are happening. So let's go to the Glue console, ETL jobs and runs.  You can see that the job is running now.

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2240.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2240)

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2250.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2250)

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2270.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2270)

[![Thumbnail 2280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2280.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2280)

It looks like it is happening, and if you remember what we told it, it's not just about running the job and leaving it out there.  As a data engineer, the agent wants to go ahead and monitor it, so you can see that it is trying to monitor the job.  It found that it's still running, so it put a sleep time of 30 seconds. Now think about it as an end user. What you may want to do is, you may know that this job may take one hour, so you don't want the agent to go and monitor it every 30 seconds.  That is where the human in the loop is again important here. You can guide the agent what to do, what not to do, so you can say, "Hey, go and monitor the job after five minutes."  But this is a smaller dataset, so we let that happen. Okay, go ahead and check every 30 seconds, so that's okay.

[![Thumbnail 2290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2290.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2290)

[![Thumbnail 2300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2300.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2300)

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2310.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2310)

Now we can see that  the job was done, but if you remember, we told it to go and check if there are any errors and fix them. So it found some error in the logs,  and it is going and checking, "Hey, is there anything concerning?" So it went and checked and found that no, there was an error that was fixed,  so it moved forward. It is again going back and validating, "Hey, is everything good?" So it did a list job once again, just list job run once again to make sure that everything is fine. Looks like it succeeded and it took around 83 seconds, and this is what the result is. We have the output saved in Parquet format, perfect. So the task for data aggregation is done.

[![Thumbnail 2360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2360.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2360)

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2370.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2370)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2380.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2380)

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2390.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2390)

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2400.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2400)

Now let's move on to our next one, which is cataloging it. This time we want Alex to help us with cataloging this dataset. For that, we're asking, "Hey Alex, go ahead and create a crawler and catalog it." So this time it is using the MCP tool manage AWS Glue databases to find out which Glue database it can catalog that metadata in.  Once it finds that metadata, the next thing that it is doing is manage Glue crawler and creating a crawler. So here you can see that  it went and created a crawler, and it even started that. So think about how fast it is going. We would have clicked through multiple screens, created the crawler,  started the run, but by the time we explain what it is able to do, you can see that in action. You can already see that the crawler is running. That is  the productivity improvement that we can gain from these agents. So here you can see that the crawler is running. Now if you remember,  we told it in the previous step, "Hey, monitor my job," right?

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2410.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2410)

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2420.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2420)

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2430.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2430)

We did not specify anything when it comes to the Glue Crawler. We did not specify anything about monitoring,  but it understands that whether it's a job that is running for data processing or a crawler that is running for cataloging, it's all the same. The intent is the same, right?  So it is trying to monitor by itself and giving us an idea, hey, is it running fine? You can see that there are some sleep commands and all of those things that it is putting, and it is  validating that, hey, is it running. So you can see that now the crawler is in the stopping state, so it's validating again to see another 15 seconds to see if it got executed, if it is complete.

[![Thumbnail 2450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2450.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2450)

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2460.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2460)

[![Thumbnail 2470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2470.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2470)

So think about the time that we would have spent in a lot of these activities and  what if you are going into an important meeting, starting this pipeline, coming back and seeing the results, right? So a lot of these things can be done by the time you come back.  So here you can see that yes, the task is complete. So you can see the new crawler that got created and this is the database  and that's the table that's cataloged. That's great.

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2480.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2480)

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2490.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2490)

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2500.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2500)

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2510.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2510)

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2520.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2520)

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2530.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2530)

So let's move on. So we have our data cataloging done. Next comes the validation.  So we are requesting Alex, hey Alex, can you help us? Let's, okay, Alex, can you help us  validate this data? And this time we are just saying validate by running queries. We did not say what query, which service to use,  but this is where the intelligence comes into the picture, right? So the LLM has an idea which, there are multiple tools that are available. We have configured Redshift, we have configured  data processing MCP servers, so all of these tools are available now. LLM is able to decide that, hey, for me to go and do this quick validation  for the data that is sitting in S3, Athena seems to be a better choice. So it used that Athena query MCP tool in data processing and went ahead and validated  the data.

[![Thumbnail 2540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2540.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2540)

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2550.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2550)

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2560.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2560)

Now as a data engineer, if we were asked to validate, would we just go with one query or we would validate that from various angles?  We would validate that from multiple angles to make sure that our data is good to go, right? That's exactly what Alex is also doing. So it is trying to validate the data from multiple  angles so that we don't have any challenges in the data that is getting created for the next step. So it looks like all the validations are  done and it gives a good amount of stats as well. So it has various states and it is also making sure that all 50 states, the data from all 50 states are there. This is what is the total page views, this is what is the total amount, so it gives a quick validation of what can be done and making sure that the data is fine.

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2590.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2590)

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2600.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2600)

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2610.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2610)

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2620.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2620)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2630.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2630)

Cool. So let's move on to the next one. So data validation is complete. Our final step is to go download this data into our Redshift.  For that we're asking Alex, hey, can you go ahead and load this to Redshift or create a notebook to load this to Redshift. So here  you see that at this point in time it immediately used a Redshift MCP server. Until now it has been using other MCP servers. The moment we said, hey,  I want to load this data into Redshift, it used a Redshift MCP server to list the clusters. So it is identifying which cluster because our prompt says  go ahead and load this to analytics Redshift cluster. So it identified the cluster and it is creating a notebook with all the various steps that are involved and you can, you'll be  able to see the notebook that is getting created right away in the Kiro ID itself.

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2640.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2640)

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2650.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2650)

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2660.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2660)

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2670.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2670)

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2680.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2680)

[![Thumbnail 2700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2700.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2700)

Now think about the real challenge that we have. I think when  Harshida was mentioning about the company that, the challenge that any company had, one of the challenges were documentation, right? And we all, we all love to code, we all love to build  the data pipelines, but we all struggle a bit to document it well, right? And that's a challenge that we all have, and that becomes even more challenging when we  acquire a set of data pipelines or we transition over to another team and having to know what is being done. So if you look at this one, you see a very detailed  notebook that has a step by step process on how to load, how to make the connection, what is the step to load, validate the data, all of those details,  and some cleanup at the end, and on the left hand side you can also see a README file so that it by default creates a documentation for you so that that can be passed on to any other team member who is coming into your team. So it's creating that, the README file at this point in time.  So that is how you can use these agents in your day to day work when you do data engineering.

[![Thumbnail 2710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2710.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2710)

### Best Practices and Learnings: Tool Management, Security, and Human-in-the-Loop

Now, with all of these things,  we can say that Alex is able to help us with each one of the activities that we have asked Alex to do. Now in this whole process, AnyCompany has learned and had some good learnings as well, right? So let's talk about that. Let's talk about some of those things.

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2730.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2730)

 One thing that you may want to keep in mind is we talked about configuring multiple services, multiple MCP servers, right? Now each one of these MCP servers come with various tools, and it is the LLM which is deciding which tool to use for a particular task that the user is asking the agent to do. Now, how will the LLM decide which tool to pick? It scans through the various tools and identifies what is the best one to pick. It is always good if you are a data engineer, you have the control to configure, enable or disable the various servers. It is strongly recommended to work with the servers or the number of tools that you feel would be more relevant, because if you have hundreds and thousands of tools that are listed, your LLM will have to scan through that and figure out what is the best one to use, so you're putting a little bit more work on the LLM.

So you may want to, as a data engineer, you might have a good idea of these are all the MCP servers that I want to use. So stick to the tools that you want to use. That's one thing, and it's pretty easy to do that in each one of these IDEs. For example, SageMaker Unified Studio can either add or delete an MCP server with a couple of clicks. In case of Kiro, we saw that it's a right-click and enable or disable. You can go ahead and do that. OpenSearch has a very interesting way of doing that. There's a tool filter that you can provide. You can say that, hey, I want to have MCP servers loaded only for this particular category or this specific task. So you can be very specific in the list of tools that are getting loaded. So something that you may want to keep in mind.

[![Thumbnail 2840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2840.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2840)

 The next one is on the protection of your data and infrastructure, right? So each one of the MCP servers has some good levers that you must be aware of, and that's something that we highly recommend you all to be aware of. For example, Data Processing MCP Server by default doesn't allow you to go ahead and create some of the resources or write the data back and things like that. It is designed like that. So you can go ahead and put allow write to make sure that your MCP server is able to do those activities. Whereas in the case of Redshift MCP Server, it's a read-only mode. So it is intended to consume the data from Redshift at this point in time. And the same is the case with OpenSearch, it is intended to consume the data from OpenSearch.

When it comes to MSK server, the persona is slightly different because that's a control plane more than the data plane. The server is more for a control plane. It is for an admin persona, for example, creating a cluster or changing the cluster configuration and things like that. That's why the modifications are by default on in case of MSK cluster, but you have an option to go ahead and turn that off. So please be aware for each one of these servers there are some good levers that you can use to protect your data and infrastructure, so please make use of that.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/2910.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=2910)

 And the next one is trusting the tools, and this is what I was mentioning earlier. So you might have noticed that in some cases the agent was asking, hey, can I go ahead and run this one? In some cases it went ahead and executed those tools. Why did that happen? That is because I have trusted some of the tools. For example, let's say I want to run a query. If it is going against my production database, I want to see that first. Or if I'm uploading something to my S3 bucket, I want to see what is getting uploaded. So it depends on what is your comfort level, which environment you are working on, right?

So depending on what are the tools that you feel comfortable that the agent can directly go and run versus something that you want a confirmation from your side, you can configure those in these various tools. The left-hand side is SageMaker Unified Studio, so you can say that hey for this particular tool, ask me or always allow. In case of Kiro, that's where that block comes into the picture where you can put a list of auto-approved tools so that it will automatically be executed if the LLM decides to make use of that tool.

[![Thumbnail 3000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3000.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3000)

 Now with all of that, no matter what, human in the loop is important. We cannot emphasize this enough, right, so please understand that the agent is there to help and consider the agent as a peer programmer.

The human in the loop is important, whether it's read versus write decisions, what tool is being used, or even the code that is getting generated or the query that is getting generated. You may want to validate that. Also, MCP servers do have options to prevent some of these runaway queries and things like that, but human in the loop is important there as well. So please keep that in mind.

[![Thumbnail 3040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3040.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3040)

With all that said, we have looked at all of these pillars where we see that context switching, data  quality, error handling, resource management, and best practices—the pillars that any company's data engineers mentioned as the pillars of loss of productivity—have been very well addressed. We're not saying that all of these things will be gone, but they will be improved or reduced. These productivity losses will be reduced with agentic AI capabilities now.

[![Thumbnail 3070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3070.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3070)

Here are some of the results from AnyCompany, so it  is saving a lot of time for sure. It is productive and it is easy to use, with very simple options to configure. And at the end of the day, yes, it is safe. There are a good number of levers to keep everything safe.

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3100.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3100)

### Scaling to Production: Deployment Options with SageMaker Unified Studio and Bedrock AgentCore

Now, yes, it's really good to have a nice POC and pilot. How will we scale it? In the case of data engineering, it's not really about taking it to production. Rather, it is about scaling it and making sure that there is a scalable option out there. How will you deploy these? 

[![Thumbnail 3110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3110.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3110)

If you're just starting with agentic data engineering, I would strongly recommend you to get to SageMaker Unified Studio and notebooks  and try out the new feature that was released, which is a data agent. Here you have an agentic AI assistance agent that is built in SageMaker Unified Studio which can discover the data, which can create the various steps that are involved for the activities that you want to do to generate the SQLs or Python code or troubleshoot any of the errors that you are getting into—pretty much whatever we just saw what Alex was doing. That's already built in SageMaker Unified Studio, so please try that out.

[![Thumbnail 3160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3160.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3160)

[![Thumbnail 3190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3190.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3190)

Now there are a lot of customers who would want to have a lot more control and levers. For example, hey, I have a third-party service or third-party data source, and how do I connect to that? That is where this architecture that we looked at earlier comes into the picture. So it's not just about the  servers that are listed here. Depending on the various data sources that you want to connect, you can bring any of these servers. If you have, let's say, a ten-member team, it is very much okay to have each one of your developers have the respective—let's say all of them are using Kiro—them going and configuring that MCP.json and working with these MCP servers. It could be the same set of MCP servers. It could be a different set of MCP servers. That's perfectly fine. That's a widely accepted pattern  now.

[![Thumbnail 3200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3200.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3200)

There are cases where you may want to have an agent deployed and roll it out to the whole organization. How would you do that? That's where  AgentCore, Bedrock AgentCore comes into the picture. In this session we will not be doing a deep dive on AgentCore per se, but just to give you an example or give you a little bit more details, Bedrock AgentCore has multiple components to it. The runtime helps you to deploy an agent, and the one on the left-hand side, which is the AgentCore memory, helps you to keep track of the conversations that you're having. So that's what AgentCore memory does.

Similarly, if you are working with an agent, we may want to know everything that is happening, so traceability is important. That's where the observability comes into the picture. AgentCore observability will give you those details, and AgentCore identity will help you to do the user authentication. Now gateway—the crux of it is we talked about the various MCP servers. Now you have an option to go ahead and configure various MCP servers using gateway so that when the LLM gets a request from the user, it can go to gateway, find out out of the whole list of tools what is the tool that it wants to use for the next best action.

So this is an architecture and you can see other tools that are getting added to the gateway as well. You may have some custom tools that you have built in your organization which you want to bring in as well. So AgentCore gateway helps you to do that as well. This is an architecture that you can use if you want to scale it out or if you have a very specific agent, let's say a data analysis agent or data engineering agent that you want to bring or deploy and roll it out to your entire organization.

Now with all that said, going back to Sarah, our CTO, she feels that yes.

[![Thumbnail 3310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3310.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3310)

Everything looks good.  The pilot went very well, the results are good, and at the same time she clearly sees a path forward to deploy this at scale. The recommendation was to move forward and fully roll out the AI capability to all the work streams.

[![Thumbnail 3340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3340.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3340)

Now we really hope that you will be able to take these learnings and implement some of these things in your day-to-day activities in your organization, and we want to leave you with some resources. Here are some of the resources. The first one is on the MCP, the list of MCP servers.  We have a couple of blogs on data processing and Redshift MCP blogs, and the one on the bottom left is the data agent, the SageMaker Unified Studio AI agent, and we have a QR code for the agent code as well.

[![Thumbnail 3370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3370.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3370)

[![Thumbnail 3380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/9e5bbd8cef5737db/3380.jpg)](https://www.youtube.com/watch?v=5MldQWiCKX4&t=3380)

At Amazon we strongly feel that we should always keep learning and be curious, and here is a way to learn more about agents.  We hope you'll be able to learn more about agents as well. Thank you, and please share what you thought about the session.  Please fill in your survey because we really care about that, and we'll be around to talk to you on any questions that you may have.

Because it's a silent session we may not be able to take the live questions, but we'll be around to talk to you. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
