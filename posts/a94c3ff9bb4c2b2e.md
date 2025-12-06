---
title: 'AWS re:Invent 2025 - AI agents for cloud ops: Automating infrastructure management (AIM340)'
published: true
description: 'In this video, AWS Solutions Architects Omar Tabbakha and Shawn Abdi demonstrate building AI agents for cloud security operations using Strands SDK and Amazon Bedrock. They showcase a multi-agent architecture that automates security incident response by integrating DynamoDB for business context, Amazon Security Lake with OCSF schema for normalized log analysis, and Cloud Control MCP server for infrastructure remediation. The demo walks through creating three specialized agents: a metadata agent retrieving account criticality and compliance data, a Security Lake agent converting natural language to SQL queries via Amazon Athena, and a supervisor agent orchestrating multiple sub-agents. They emphasize the importance of system prompts, human-in-the-loop for critical operations, and demonstrate how their solution reduced investigation time from hours to seconds by automatically correlating CloudTrail events, VPC Flow Logs, and Security Hub findings. The session concludes with deployment guidance using Amazon Bedrock AgentCore Runtime for production environments.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - AI agents for cloud ops: Automating infrastructure management (AIM340)**

> In this video, AWS Solutions Architects Omar Tabbakha and Shawn Abdi demonstrate building AI agents for cloud security operations using Strands SDK and Amazon Bedrock. They showcase a multi-agent architecture that automates security incident response by integrating DynamoDB for business context, Amazon Security Lake with OCSF schema for normalized log analysis, and Cloud Control MCP server for infrastructure remediation. The demo walks through creating three specialized agents: a metadata agent retrieving account criticality and compliance data, a Security Lake agent converting natural language to SQL queries via Amazon Athena, and a supervisor agent orchestrating multiple sub-agents. They emphasize the importance of system prompts, human-in-the-loop for critical operations, and demonstrate how their solution reduced investigation time from hours to seconds by automatically correlating CloudTrail events, VPC Flow Logs, and Security Hub findings. The session concludes with deployment guidance using Amazon Bedrock AgentCore Runtime for production environments.

{% youtube https://www.youtube.com/watch?v=JRu2GqRe9sg %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/0.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=0)

### Introduction: AI Agents for Cloud Operations and Security Automation

 Good afternoon everyone. Thank you for coming to our code talk today. We're going to be talking about AI agents for cloud operations and how you could automate infrastructure management. One of the key topics we're going to discuss today is how you could operate security operations. The way we formatted today's call is we're going to start off talking about the core components of what an agent is, the type of integrations, tools, and some fundamental concepts. From there we're going to jump into the AWS console and walk through a real demo and talk through the code and look at some of the outputs that come out of this agentic application that we develop.

My name is Omar Tabbakha. I'm a Solutions Architect here at AWS. Hi everybody, my name is Shawn Abdi. I'm also a Solutions Architect here at AWS. So before we get started, by show of hands, how many of you have built out an agent before? It could be as simple as a weather application agent. We have one hand, two hands. Okay, so a decent amount of you have built out agents. My guess is AWS, maybe some other framework that you use, but it looks like some of us have already built out an agent. Awesome, so let's go and get started with that. Let's jump right in.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/70.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=70)

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/80.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=80)

### The 2 AM Security Incident: Why Manual Processes Fall Short

 So let's imagine that it's 2:00 a.m. and you're a security engineer that's just received a call and now you're thrust into a high pressure situation regarding a security incident.  You log in and your first task is to go through mountains and mountains of different logs, all in different formats, whether that's JSON or plain text. That could be stuff like VPC flow logs, firewall logs, API logs, CloudTrail logs, even third party vendors that you might use for your on-premise or cloud environment, nothing native to AWS.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/100.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=100)

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/130.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=130)

 So hours pass by as you try to piece together these different fragments. Like I mentioned, you might have something like GuardDuty alerts, CloudTrail logs, DynamoDB logs, Security Hub findings, and you're still trying to find out how all these different resources talk to each other. You know, they're from separate systems speaking a different language and are really not meant to communicate with each other. So the clock is ticking. You try to identify a suspicious IP, see if it was really a security incident or if we  have a false positive on our hands, and you've just spent hours and hours searching, trying to figure out what's going on.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/140.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=140)

At this point you're starting to get exhausted. You've done all the grunt work that something like  AI might be able to handle. You've spent time searching through these resources with different tools because they're in different formats like we mentioned, whether that's JSON, plain text, or some other standardized schema, and you're figuring out what do I need to do now. So what if we can take this problem and use some sort of specialized intelligence system like some agents that can comb through all of these logs, comb through these different tools, and be able to provide you some insights that are actionable on your current risk, giving you a more precise path to tackle this issue.

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/180.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=180)

And that gets us to why AI for security operations. So the reality today is that  everything is moving super fast paced. Attacks are becoming automated. People are trying to spend hours identifying what's going on, and traditional approaches with this manual process just doesn't seem to work for everybody. So AI has become the game changer in the sense that we can now analyze vast and complex data sets in seconds. You can detect the patterns and anomalies that a human might miss for that error prone manual process, and you can automate the repetitive tasks like triage and log correlation. And then you use these to surface key insights to have something to focus on real threats and not the grunt work that you might see from a typical security engineer's work on a day to day basis.

### Understanding Agentic AI vs Traditional LLMs and the Role of Amazon Security Lake

So let's start off with what's a typical LLM versus agentic AI. On the left hand side here you can see that we have our usual LLM and what this does is it takes the knowledge that it's already been trained on, providing a simple, concise answer without doing many iterations or any thought process or reasoning. So for example, you ask it the question, when did the French Revolution take place, and it'll respond back to you with the simple answer of 1789.

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/260.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=260)

Now on the right hand side we have agentic AI and this is where things get interesting.  The agent is able to think on its own. So let's go ahead and check a weather website for the forecast. Let's take a look and dive in and see what the forecast looks like for the next week. We've got some rain, but it's also sunny and warm next Friday, and then we can evaluate and reiterate until it comes to a proper answer for us with its reasoning capabilities to say next Friday is the ideal day for Mount Fuji hiking. And the difference here again, as I mentioned, rather than just having a concise answer on knowledge that it's already been trained on, this can think for itself and iterate until it has a valuable response that you can use for your triage or to get further information from the agent by asking follow-up prompts.

[![Thumbnail 310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/310.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=310)

So the key thing that I wanted to mention to  start us off is that it's very important that your data is the key source for all of this agent building. You want to make sure that you have the right data sources, that you have everything centralized in a location that's easy to read and formatted specifically for your security use case. This is where Amazon Security Lake comes into play.

For those of you that are not familiar, this is basically a service that we have that allows you to centralize all of those different tools that I mentioned, all the different logs, whether it's JSON, plain text, or something else, into a data lake in your AWS account. By doing that, you're able to normalize that data from the different schemas into an easy, ready-to-use format called OCSF, the Open Cybersecurity Schema Framework. That way you can use a singular tool or a singular method of approach for your log analysis without having to worry about these data silos and how they are disparate to begin with. Now with that, I'm going to go ahead and hand it off to Omar to do a brief overview of our solution before we dive into the console.

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/380.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=380)

### Multi-Agent Architecture: Building a Team of Specialized Security Agents

Awesome, thank you for that, Sean. So let's cover our  solution. The first thing I want to talk about today is that we're going to introduce a multi-agent application. This means we're going to have a team of different specialized agents. The way I like to introduce this is to think of a team that you have today. Usually you have a data scientist, you have a computer scientist, you might have a sales team, marketing team, and each team or person is specialized in a specific skill and they're able to come up with skilled outputs based on the knowledge they have. We take that same exact paradigm to the agentic world.

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/430.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=430)

For example, in this case,  we have these agents. I'm going to call them sub-agents because they're one of the agents of many that are going to work together to help us find the different security events and also automate the cloud operations of remediating those problems. This first sub-agent is going to be able to tap into DynamoDB and pull real business context from our environments.

Another big one here is going to be an agent that's able to use Security Lake. A key component here is if you have servers on-premise, other cloud providers, or maybe you're working with different SaaS providers as well, we can centralize and normalize all our data in a central security data lake with one specific schema. That way our agent can tap in and access these logs and provide that information to the overall system.

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/460.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=460)

Another big one is cloud operations. Does anyone know here what MCP is? Has anyone heard that term? Great. So that stands for Model Context Protocol.  A lot of times in today's talk we're going to build out custom functions that you could use as tools. Again, tools are essentially APIs or data sources that large language models can use as additional context to provide answers. With cloud operations we could do different things. We have a Cloud Control MCP server that lets you do different things like, let's say, add block rules to specific IP addresses that might be malicious. Let's say you want to isolate EC2 instances or you want to take forensic snapshots of your EBS volumes. We could automate our cloud operations.

Obviously one key concern is, hey, do we really want to hand the keys to our production environment to an agent? That's a big question on everyone's mind and that's where we look into different things like guardrails and also implementing human in the loop. We could actually take a summary of all the findings and ask the user, hey, is this the correct approach? Even the best agentic applications today have around a low 90% success rate. So for key critical operations, you always want to have a human in the loop within that process.

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/530.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=530)

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/540.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=540)

And of course, incident response playbooks. Some security events might have non-sensitive data, and we're going to approach those a different way than an account that's going to have the  more highly critical environments. So here we could have these agents work together and we could have a supervisor agent up front. This supervisor  agent is going to be able to use its team of sub-specialists to actually coordinate different tasks and get different answers and synthesize them into a final response.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/550.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=550)

 This could work in two different ways. The first one could be approached like an event-driven architecture where we could take GuardDuty findings. GuardDuty is our threat detection service. We could take GuardDuty findings and automatically feed that to our agentic system. Let's say for example that someone opened a public S3 bucket. This will trigger some sort of GuardDuty finding and send that directly to our agent, and our agent's able to look at that and understand, okay, what account is this coming from? Let me look at the logs. Who did this? What steps do I need to do to remediate this? And it could do different things, things like either raise alerts to the security teams that that bucket belongs to based on the business context, or we could interact directly with the agent. If you want to dive deeper into the events or do more advanced things, we could interact with the agent. So let's see what that actually looks like here.

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/610.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=610)

### Live Demo: Threat Detection and Automated Response in Action

 This is a front-end application that Sean and I developed. We're using Streamlit to come up with this application. On the left-hand side, we have example queries that our security engineers could actually look at and use. One example here is going to be the threat detection query. So here we're saying find any failed login attempts exceeding 10 tries in the last hour, analyze the source IPs and affected accounts, and then block the suspicious IPs and create snapshots. This prompt here is going to take advantage of multiple different agents to complete this task, right?

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/640.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=640)

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/650.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=650)

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/660.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=660)

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/670.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=670)

If we hit submit, we could actually see that the agent's going to go, okay,  let me analyze the authentication logs. So it's going to call those specific tools. Those tools are able to look at the different tables we have within our Security Lake.  It finds CloudTrail events and Security Hub findings. It's able to then go ahead and query it to find what are the source IPs. Here it finds three primary source IPs across different regions,  and then we can execute our Cloud Operations agent that could actually go ahead and block it. So here you go, it's showing the different IPs that are being blocked,  and here it shows automated response actions.

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/700.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=700)

So again, this is more of an automated approach. One thing you could do is they can give you the different summaries of what I found, and then it can ask you, do you want me to block these IP addresses? Do you want me to put the limited STP controls within our account? So again, those are things that you want to think through. Some processes you want to give full autonomy to the agent, but other ones that are more critical, you definitely want to think about human in the loop. On the left-hand side again, we have different agents, and we're going to go through  a few core agents in the console today and show you how we actually wrote this. So the way we developed this is, let's go to the next slide.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/710.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=710)

### Building with Strands Agents: Simplifying Agent Development from 350 to 55 Lines of Code

 Okay, the way we developed this is using Strands Agents. This is an open-source SDK, and this is going to help you build out agents with just a few lines of code, right? It's very intuitive, easy to use. When I first started looking at agents and started building them around a year ago, I don't have my Python. I haven't done Python since college, so I was a little rusty. But with Strands, I was able to pick up very quickly, right? You're going to see later today how it abstracts a lot of the complexity of building an agent.

Another core thing is the native tools. Today we're going to build the tools. We're going to define those Python functions and make them tools, but there's also community tools that you could tap into that already exist, so you don't actually have to code them. Also, easy integrations with different MCP servers. AWS has a bunch of different MCP servers that you could take advantage of today, and other providers as well have MCP servers that you can use. You could support any model providers, right? You're not stuck with just Amazon models. If you go to Bedrock, we have over 10 foundation models that you could use. If you want to use OpenAI and Gemini, right, you're not locked to any sort of provider.

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/790.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=790)

One thing I want to talk about is for these sub-agents, depending on the task, we could play around with the different models that's powering that specific agent. So at its core, what Strands is doing is it's implementing this agentic  loop. So the way it works is if you look at the weather application example, let's say again we want to check the weather for tomorrow. Again, the foundation model has no knowledge of what the weather's like next week, right? It wasn't trained on that kind of data.

So what happens here is the prompt is going to be sent to the agent, and the agent has access to the model and the tools. Right here, in this case, we're using a ReAct orchestration where we're actually going to invoke the model with the user prompt, and the model is going to have all the context around the tools it has access to. It's going to be able to do the reasoning like, okay, I need to get the weather for tomorrow. Wait, I have these lists of different tools like get weather, so it's going to pass that response back to the agent, letting it know, hey, execute the specific tool. In this case, it could be a Python function that goes to the weather.com API and grabs that weather for tomorrow.

It's going to return that result and then send it back to the model, and this loop is going to keep on happening until you get your final response, right? It could also prompt the user, right? If some tools require specific inputs and they're not there, we could ask the user, hey, you're asking about weather for tomorrow, but I need a state, right? I can't just give you weather without knowing what location you're talking about. So those are some things we'll talk about as well.

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/870.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=870)

So just a quick visual on the differences between using some sort of SDK like Strands or Q AI.  As you could see, you go from 350 lines of code to 55 for a weather app agent. So for the tool registration, the conversation management, API communication, all that's abstracted away. It's handled automatically. And again, when you go through the code talk, you're going to see there's not that many things you're going to define. Again, our goal is to simplify creating agents. Same with the agent loop, right? Beforehand, I had to build out the iteration logic manually, but now I could literally just call in the agent class and have it do it for me.

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/920.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=920)

So a process that took 4 to 6 hours can now take 15 to 30 minutes. Okay, so let's go ahead and jump into the AWS console before we  and look at our first agent. Go ahead, John.

### Hands-On: Creating the Business Context Metadata Agent with DynamoDB

So before we jump into the console, I want to take a quick look at the first agent that we'll be building out, and this is our business context metadata agent. As you can see here, the agent is going to ask for an account ID based off of your AWS account. That's a 12-digit number that you give it. You say, tell me about so and so account. Our metadata tool that we're going to attach to the agent before we instantiate it will be able to look through a DynamoDB table or a specific API that you might be using that holds all of that information. The information that we're talking about when it comes to business context is going to be something like your business unit, the criticality, a point of contact in case something needs to be reported, the scope of compliance, and so forth, and we'll take a look at that in the console once we open up the DynamoDB table.

But what this allows us to do is take, what most people fail to realize is that the context and the business context related to a security incident is just as important as the technical details. You'll be able to sift through all of these things within seconds to be able to identify if this is something that your team should prioritize or if you can just leave it alone. It's low risk and maybe come back to it for a more priority adverse security incident that might have already been happening. So with that, let's go ahead and jump into the console and take a look at what we've got to build up.

Yeah, and real quick before we jump in the console and get to the fun part that we're all excited about, I just wanted to see if there's any questions on what we discussed, make sure we handle those questions before we get into the code. Does anyone have any questions about what we discussed so far on Strands or agents? Great, okay, awesome, awesome. Let's get right to it.

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1050.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1050)

Cool. So here you can see we have the AWS console, just your standard landing page, and what we want to do is go to Amazon SageMaker AI. Now we use Amazon SageMaker AI because there's notebooks available for us to use. If you all are familiar with Jupyter, Jupyter Notebook is an interactive IDE that's available for you all to build out your code, visualize the outputs, and then experiment in real time, and this is super helpful as we build out this agent together in the room step by step.  So just a quick little refresher for you all.

Now the first thing that we want to do for this agent is install all the dependencies. That's things like Boto3, Pandas, and other libraries that you might need. So we'll go ahead and install those dependencies. Once the dependencies are installed, we're going to go ahead and use another import code snippet right here to get the JSON. We're also going to import from Strands the agent and tool functions. We're also going to import Strands models, Bedrock models that we'll be using for the sake of this code talk. So we'll go ahead and run that.

And then now we're getting to the fun part. So this is where we want to start off by making a connection to our data source, and in this case we're using DynamoDB. So we'll create the Boto3 sessions so that we can interact with our AWS resources programmatically. And we are using DynamoDB as the data source. As you can see, the resource is DynamoDB. We're based out of US East 2. Now for this particular code snippet, we defined the region that we're working out of. There are ways to auto-detect that, and you don't have to hard code US East 2. You can have it detect whatever region you're working out of if you need to.

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1140.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1140)

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1150.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1150)

And we want to create a metadata table and define that for the sake of this agent. So we're going to name that metadata_table and we are linking that to a DynamoDB table that we've already pre-created in our console. Yes, zoom in a little bit, yeah. Is that better? Awesome. So the database that we currently have  set up is called SOC Account metadata, and if we switch back over to the console and look up, go to DynamoDB, we can see  that all the things that I was mentioning earlier, the account ID, the scope of compliance, the criticality, and so forth are visible right here.

[![Thumbnail 1160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1160.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1160)

So as you can see we've got the account ID,  account name, compliance scope, and then if we scroll over to the right a little bit more, you can see that we have the criticality, the environment that we're working out of, determining if it's production, development, and so forth to help you prioritize what you want to act on. So this is just to see the type of data that we'll be pulling when we return the business context using the agent.

[![Thumbnail 1190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1190.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1190)

Now if we go back to the code, the first thing we want to do now that we've connected to our DynamoDB  table is define the tool. So we start off with the @tool decorator, and for those of you that may not be super familiar with tools, these are just specialized capabilities that we give our agent that allows it to act beyond its core reasoning abilities. So in this particular case,

we're defining a tool to retrieve account metadata from an AWS account. We'll define that as get_aws_account_metadata. We are giving it the information and instructions to retrieve metadata about an AWS account given an account ID. The argument that we're giving it is an AWS account we need metadata for. The ID will always be 12 digits. It's important to distinguish that here because, as I mentioned, in the case of AWS, it's a 12-digit account number. If you put in a number smaller than that, it'll return a result saying we can't find this account. However granular you want to make that, we'll find that in the argument.

The agent tool is going to return to us a dictionary containing that metadata context like we just looked at, including the scope of compliance, the account name, what environment it belongs to, who we should contact, and if we should prioritize this or maybe move on to something else. As you can see here, the response is going to be based off of the account ID string that we saw on the DynamoDB table, and we'll get that list in a dictionary format once we've instantiated and run the agent. Let's go ahead and run that code, and now we're ready to initialize our agent.

[![Thumbnail 1280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1280.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1280)

 An agent consists of three components. Can anyone tell me what those three components are that make an agent? No worries. An agent is a model, a system prompt, and a tool. You may notice here that there's no model defined here in this agent, and we'll cover that in just a second. We'll name the agent metadata_agent. We'll give it the system prompt: "You are a helpful agent that looks up metadata about an AWS account based on the account ID." As I mentioned, if the account number is not 12 digits or it can't find it, it'll let the user know so that they can try that again. The tool that we're giving the agent is that get_aws_account_metadata that we just created up above in the previous code snippet. Let's go ahead and initialize the agent.

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1340.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1340)

This next piece right here, as I was mentioning, we don't have a model defined, and that's because I was perfectly fine using the default model that Strands provides us, which is Claude Sonnet 4.0. When we run this model config command, we  can verify that. Obviously, if you have a specific need for your use case for a different model, you're more than welcome to change that and put the model that you want to use. Claude Sonnet 4 worked perfectly fine for this example right here.

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1370.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1370)

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1380.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1380)

[![Thumbnail 1390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1390.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1390)

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1400.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1400)

Now that we've got all the necessary resources compiled and ready to go, we can go ahead and send a message to our agent to get a response with that dictionary. Tell me about account 679851713709. Maybe we didn't run everything.  Yeah, we need to run the blocks.  There we go. If that ever happens, just make sure you ran all the previous code snippets beforehand. As you can see here, we've got the account name, it's Production-Main,  the ID that we gave the agent in the first place. You can see the business unit that it belongs to. You can also see that it is a production environment with critical criticality,  and then if we want to let the security contact know, security@company.com, and the incident priority, for example, highest priority.

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1420.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1420)

This is the type of context that you might spend hours trying to gather from the information that you have stored from your data sources. The agent was able to pull that in just a matter of seconds given an account ID. With that,  I'm going to hand it back off to Omar to show how we use that business context and how we use the data that we have for log analysis.

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1440.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1440)

### Building the Security Lake Agent: Natural Language to SQL Query Translation

Thank you, Sean. One key thing that I would like to bring up when we're talking about agents is if you're new to building agents, always start off with a more simple use case, something that's a little more straightforward so you could actually get the hang of defining different tools,  understanding what works and what doesn't work. In this case, we start off with the DynamoDB. Obviously, the business context agent is simple, but it's also a key component of our overall architecture. Let's look at the next agent right here that we're going to build out on the console, and this one is going to be the Security Lake agent.

[![Thumbnail 1460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1460.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1460)

 This is definitely a core one here. The way this agent's going to work is it's going to use Amazon Athena. This is going to be a serverless interactive query service that actually lets us run SQL queries directly on all the data sources we have in our Security Lake. The way this works is the agent's going to be able to take a specific prompt. It's able to convert natural language. For example, here if you look up at the screen, we're saying show security group changes followed by unusual traffic patterns. Can anyone tell me where do we store security group changes? What sort of logs will that be placed in? Can anyone tell me? CloudTrail. Exactly. That's one of the log sources, CloudTrail. How about unusual traffic patterns?

VPC logs, that's right. So just based on this natural language query that we have here, it's going to understand that I need to create the SQL code to actually query the Security Lake and pull in any sort of security group changes that have unusual traffic patterns. So this is an example of the power of the agent by itself.

[![Thumbnail 1570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1570.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1570)

Down here is sample queries for multi-agents. So here if we're saying correlate unusual API activity with network anomalies and then contain the threats, does anyone know what two agents we need to use for this? So the first one is, how do you correlate the API activity? What agent would you need for that? So hint is we're looking at it right now. What was that? Yes, so one agent is going to be the Security Lake agent, and then for the containing the threats, if anyone remembers the name of that one, let's go back. Yes, the Cloud Ops agent, exactly. So these two are going to work together  to actually handle that query and solve that problem that we have.

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1620.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1620)

Okay, and yeah, let's jump into the actual console and see what this looks like. You want me to drive? I got it, appreciate it. So let's go to demo. Good, okay, so let's go into the Security Lake notebook. So like Sean, I'm going to install the dependencies. We're going to then import different things. The key two things is going to be the agent class. This is what lets us actually define the agent with the different tools, foundation model, and prompt. We're also importing the tool class so we could actually define the Python function as a tool. Sure, zoom in, okay, how's that? Awesome. 

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1650.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1650)

Okay, here I'm just doing some Security Lake configuration. I'm giving the agent permission to access my Athena workgroup that's associated with my Security Lake. Again, my Security Lake has the logs across the board from VPC Flow Logs, Security Hub findings, and also CloudTrail events. Here I'm initializing different AWS clients so I could actually interact with Athena, or not me, the agent can actually interact with Athena, Bedrock to actually access different foundation models,  and then also Glue. Glue helps us grab the schema information so we could build the queries.

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1710.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1710)

Here we're defining a Bedrock model. So I think one of the strongest points here is being able to define the model that you want to use for your agent with just one line of code. So Claude Opus 4.5 probably came out a week or two ago, and I was able just by changing one line of code to replace a different foundation model. And again, one thing you want to think of is depending on the agent and the complexity of its task, you might want to adjust which model you want to use. You want to think about latency, you want to think about cost. So if we think about the metadata agent, it's just going to create a DynamoDB table. It's a pretty simple task, there's not much going on there. So in that case we could use a lightweight model that's faster and cheaper, like Haiku, for example. Something like this that needs to look at a natural language, look up schema, create an actual query, grab the data, and then create the correlations between activities, we want to use something a little more advanced with more capability. In that case we're going to use Claude Opus 4.5. 

Okay, so now we're going to actually define the tools. So now what tools, does anyone know what sort of tools my agent would need access to for the Security Lake agent? There's no wrong answers, there's a hint right here. I'm going to have to pick someone up. How do we query the data? What's the best way to query that? What was that? Athena? Athena, yes, so that's one tool we need. We need to create a tool for our agents to actually use Amazon Athena, and that's what we're doing here.

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1770.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1770)

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1780.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1780)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1790.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1790)

Here is the context. So we feed, if you notice, every tool is going to have this metadata description, and that's because we feed this to the model. The model knows I have a tool that can query the Security Lake using Athena, and the only argument I need is a SQL query to do this. And this is the returns, right? I'm going to return the results as a string table or error  message if I don't get anything back from the Security Lake. So here I'm just connecting to Amazon Athena, and I already defined Athena database up top, so we're connecting to Athena.  And we're actually going to grab the unique query execution ID that belongs to Athena, and this is going to go through and actually pull in the results for Amazon Athena. So that's one tool. 

One tool is to actually do the querying. Can anyone tell me what the other tool is going to do? There's a hint on the screen. So the second tool is going to actually list the tables, right, because the agent is not going to know what sort of data sources I have.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1820.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1820)

It could be VPC Flow Logs, it could be CloudTrail, Security Hub, or if you're using other third-party providers or security services, you could have unique tables as well. That's what makes Security Lake so powerful.  You have one normalized, standardized data format, so the agent doesn't have to go in and change up the queries. It has one unified way of approaching it.

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1850.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1850)

Finally, we have get table schema, and this retrieves the schema information from the Glue Data Catalog. The agent needs to grab the schema. It has to look at the column information and how the data is partitioned, and based on that, it can actually create queries that can be executed against Amazon Athena. So we define the three tools over here.  We're going to run that.

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1880.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1880)

Now what we're going to do after we define the tool is go ahead and call the Agent class that we have. Again, for the agent, like Shawn said, there are three core components. We need the Bedrock model that we want to use, or any model. You're not tied to only Bedrock. If you want to use OpenAI or these other foundation models, feel free to do that. Then we define the different tools that this agent has access to. I think one very important component  of agents that a lot of people usually overlook is the system prompt.

The system prompt is essentially the persona, the personality of the agent. In this case, we're letting it know that you're a cybersecurity analyst with access to an AWS Security Lake. I give it important database context, and if you notice here, I also give it a workflow. The thing is, with agents, it could actually go through the agentic loop and figure out the workflow. But in this case, there is one good way to actually generate the actual query, and that is list the tables, get the schema, and then build queries based on the actual data and the schema. That's one workflow that I define.

[![Thumbnail 1940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1940.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1940)

One thing that you want to do when you build agents is you'll notice sometimes it's taking two to three iterations to do something. I could just tell it in the prompt the information, and we can get rid of a lot of the latency and extra tokens that it would usually cost. In terms of outputs, I tell it to give me specific remediation steps. I want data-driven insights and then key findings with the  risk assessments.

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1960.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1960)

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1970.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1970)

That's it. We defined our Security Lake agent with these different tools. The first prompt I want to test out is, what kind of data do I have in my Security Lake? So we run that,  and as you can see, the first thing it does is it's going to list the Security Lake tables. It realizes we have three different tables, so let's get the schema and details so you can get the full picture. 

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1980.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1980)

[![Thumbnail 1990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/1990.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=1990)

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2000.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2000)

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2010.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2010)

It runs, as you see, three different executions, one tool call for every single table, and it grabs that information. It tells us, okay, you have the CloudTrail events, which has  API activity, user role, and authentication information, source API calls. Same with Security Hub findings, where it tells you  everything that is usually involved with these specific findings, vulnerabilities, remediation guidance, compliance status. In the same way with VPC Flow Logs.  Here you go. 

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2020.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2020)

[![Thumbnail 2030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2030.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2030)

[![Thumbnail 2040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2040.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2040)

### The SOC Orchestrator: Coordinating Multiple Agents for Comprehensive Security Analysis

Now that we built that Security Lake agent, let's look at how we could actually build out that supervisor agent so we could use multiple agents together.  The next step is we have this log analysis agent that could actually generate its own SQL queries  based on the different events coming in or interaction with the agents and return results. We also have the metadata agent. So how do we actually set up these two to work together in a multi-agent architecture? 

When I first started, we talked about native tools that are created by the community. Two ones I want to use for this specific agent are going to be file write. This is going to give our agent the ability to actually write files to our computer. Another one is current time. So if you have specific queries like tell me about events that happened yesterday, this is a native tool. Again, you don't have to write any logic for it. It's already built in, and we just need to import it from the Strands tools class. So we import those two.

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2090.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2090)

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2100.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2100)

This metadata agent that Shawn went through was defined in another notebook, so I'm just going to redefine it so this specific notebook has awareness of  the agent. Let's look at how we actually define multiple tools. The way we do this in this example is we define the agents as  tools.

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2130.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2130)

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2140.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2140)

Think back to that agentic loop. The SOC Orchestrator Agent now has access to different agents that you can use to complete tasks. So in this case, all I'm doing is defining that Security Lake agent as one of the tools. The logic was already defined above in terms of the different tools it has access to and its persona, so here I just need to provide the context so our supervisor agent knows what it has access to. Same  with the get AWS account metadata. I'm just going to define the metadata and then put some error handling in case the ID is incorrect. 

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2160.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2160)

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2170.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2170)

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2190.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2190)

And then here again, importing the agent class and we're defining the different tools. In this case, we have two native tools that are native to Strands, and we also have two different tools which are actual agents. Here again, the big key is the personality, the persona, which is a system prompt.  When you start building out agents, that's one thing I highly recommend: try out the different prompts and you're going to see wildly different results. So in this case, I define an investigation workflow.  Get the metadata, use the Security Lake agent to find the findings overview, get the CloudTrail network IP analysis, and then finally I want you to generate a report that gets sent to our security team. I gave it a list of available tools, and then also the output.  I want account context criticality, account summary, the risk assessment based on available data, and also recommended actions. And if there's any data gaps that require manual investigation, sometimes the agent doesn't have all the necessary context or reasoning to actually give you some sort of remediation plan, so it could tell you what sort of data gaps you have within your environment.

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2220.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2220)

So around that,  when you use different native tools like file write, Strands sees it as a sensitive operation to actually write to your computer. Usually you'll ask for consent whether or not you could write to the computer in the middle of the invocation. In this case, agents are autonomous. We don't want it to stop halfway through and let us know that, hey, could I invoke this specific tool? So I'm just going to run this and just say bypass any sort of tool consent. You're allowed to use any tool you want without human loop permission.

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2260.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2260)

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2270.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2270)

So now let's actually use the SOC Orchestrator Agent we just built out. So we have this account ID, it's been compromised. What are the affected resources I should be concerned about? So if we run this,  me and Sean have actually been populating the Security Lake for the last two to three months with almost sixty events a minute. So we have a lot of logs in there, and it's still performing very  well. And it's going to actually go ahead and start querying the different data sources, getting the schema. This probably takes around maybe a minute or two to complete, so I'm going to show you a report that I generated last night.

[![Thumbnail 2290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2290.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2290)

[![Thumbnail 2300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2300.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2300)

So if I come back to home, look at all my reports. Right here, these are different reports I generated. Let's look at the one from,  let's do four hours ago. So this is the report that I generated. It's going to give me the account ID and note based on the business context that this is a  production account, so it's a highly critical environment. It's a P1 priority and it's critical, and it gives you a summary. It says this account has confidential information, PCI information with active compromise. You need immediate containment required. So it gives you all the context, the security contact within that specific AWS account.

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2320.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2320)

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2330.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2330)

[![Thumbnail 2340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2340.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2340)

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2350.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2350)

Here's  the executive summary. Me and Sean are definitely in trouble. We have over eighteen thousand security findings within this account, so definitely a lot of work to do there. Again, we've been blasting the Security Lake with a lot of different events,  and here it breaks it down. You have eight hundred fifty-nine critical within S3 buckets, IAM users, EC2 instances, security groups that have been modified,  and it tells you the risk. Data exposure, IAM user, there's a credential that's been compromised and used. It shows the attack analysis, the source IP,  which user is compromised, and also the VPC traffic.

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2370.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2370)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2380.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2380)

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2390.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2390)

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2400.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2400)

Here we go. Different API operations that are happening. That user is creating security groups, it's deleting security groups, and it's authorizing specific ports within existing security groups.  And it actually runs down through immediate actions that are required. Rotate all the credentials immediately, especially the admin IAM.  And it tells you the things that are critical that need immediate action, the short-term actions, and then some more information on the compliance impact assessment.  And then let's see if it generated the support. 

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2420.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2420)

### Production Deployment with AgentCore Runtime and Closing Remarks

So it's still working through, we can come back here, but that probably takes around another minute. So yeah, let's go ahead and go to the next slide. Now that we have our agent code, we wrote it out, we're confident it's working, and we tested it within Jupyter notebooks  and it's doing what we wanted to achieve, the next big question is how do I run this within a production environment? That's one big key thing that we're seeing.

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2430.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2430)

And with that we have the AgentCore Runtime.  This is a managed service that came out a few months ago and it lets you run your AI agent code so you don't have to worry about the underlying infrastructure. So in this case, you can use the Strands SDK, any sort of foundation model you want, whether it's from OpenAI, Gemini, or one of the handful of models in Bedrock. Same with framework, right? You don't have to use Strands SDK if you're comfortable with LangGraph or CrewAI. If you're already using them, you're open to use that as well.

So what happens here is let's say we have this Python file that I have. What you could do is you could either containerize it in a Docker file or upload the code as is to AgentCore, and they'll do the packaging and host it for you. So what's going to happen here is AgentCore Runtime is going to create a runtime endpoint, and this is the endpoint that was integrated in that front end application that I showed in the beginning of the talk. It's sending all those prompts to the endpoint, and the endpoint is actually able to scale up the amount of compute resources based on demand, right? So when there's a lot of users coming in and using it during actual incidents, it's able to scale up and also scale back down by default.

There's also a lot of cool different integrations whether that's through identity, whether you want to set up observability, and also if you want to integrate different MCP servers, right? We talked about the Cloud Control MCP server to automate cloud operations. If you haven't checked those MCP servers, I strongly recommend you do. It shows you the different range of things that you can give access to your agents. So any questions? Yes.

I do understand. Yeah, so your question is you're using a lot of these services and tools. What's the value of using a security agent? Basically, what's the advantage I'm going to get? Is it automated currently? Okay, yeah, so I guess the complexity of setting up the automation in terms of are you automating remediation of security findings? Yeah, who does pretty much all these things? Yeah, I'm missing something. Yeah, no, it's definitely a good concern. If you want, let's connect after. I'm curious to explore more about the tools, what you're doing, and then how this would fit in the overall. There's some questions I want to ask you. Yeah, appreciate the question.

Any other questions? Yes. For registry specification, is this related to registering tools or what is this related to? Are you currently using that? Are you currently doing a registry for your setup? Because we're trying to allow authentication to the agent. Let's say we need to have a unified approach with this. Yeah, yeah, so I know with AgentCore it's not in this specific slide, but there's also a core component, a feature called AgentCore Identity, and that's one thing where we could unify the different access and authentication for our agents within that area. And if you want we could dive deeper after the talk about how that works at a higher level, yeah.

Yes, we'll go this way and then we'll come back to you. So in the cloud, is it more of ensuring that the agent is up to date with your infrastructure environment like your Terraform? Like I guess like let's say I have some environment sitting in somewhere, you know, and then there's an event that changes the security posture. Like I want to reconcile those two states. Oh, I see what you're saying, so it reflects reality. Yeah, how should I think about that in this environment? Yeah, and that's one thing I recommend for production environment changes at the current state of agents and how they operate. That's one thing where we would look at implementing the human in the loop where you look at the different changes and recommendations and then you're implementing them manually yourself within the environment.

There's still some necessity for human in the loop when it comes to that because you don't want automated remediation happening behind your back. You know what I mean? And then you have to go reconcile that. Yeah, we're getting there. Hopefully maybe next year you could automate the whole stack, but as of now we take those recommendations of what changes need to be made, and you could actually feed the Terraform code to you so you could have the Terraform code. You could review it and then feed it and upload it to your main code repository and then sync that.

Yeah, can you have that agent run through the same CSCD process? You could, then you wouldn't have any drift because the agent would be using Terraform in the same way that you're using. Exactly, yeah, so you could have a, let's say part of the overall solution you could have an agent that specifically is working to developing and working with that specific environment. So that's true, that's also another option. Yeah, I had a question over here. Thank you. Into Security Lake or with other sources besides Security Lake? I'd have to double check on that. Let me talk with you afterwards to see what other sources that you might be trying to use. I know that most of those resources, people have been centralizing them within Security Lake, but is there a reason why you want to keep them separate? Security Lake. I'm sorry, are you? Okay. Marks. Got you. I'd have to double check on that one for you, and I'll get back to you on it. Did you have something to say on that? No, I'll talk to him after. I couldn't hear.

So just to summarize, at this time, should we say that? Yeah, I'd say at this time it's more of not only monitoring but also doing the correlation for you. Like, for example, jumping between the different data sources, running the SQL queries, and actually looking at the correlations between this specific security group being deleted, tied to specific IPs. So a lot of that grunt work of doing that manual process definitely could be automated. And even in terms of like actually automating the infrastructure as code and deploying things, that's something that the Cloud MCP server actually has access to, right? It could write Terraform for you and actually execute it and run it, but again back to the point of at this time like you're saying, I wouldn't feel confident enough with agents to actually perform production changes without me actually looking at what it's doing. Awesome. Should we wrap it up? Yeah, yeah.

[![Thumbnail 2870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2870.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2870)

So hopefully some of  that was useful to the people in the room, and on the left hand side we have our Strands getting started. This is all of the documentation and everything you'll need to build an experiment with Strands if you'd like to test something like this out in your own environment. Highly recommend getting hands on and doing so. And if you're also interested in deploying Strands agents to Amazon Bedrock AgentCore Runtime, please scan the QR code on the right hand side. This gives you the operationalized capabilities to test this in your own AWS environment. Everything that you need to get started is right there.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2910.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2910)

And then one last thing I wanna leave you all with, if you haven't  heard of AWS Skill Builder yet, whether you're new to the cloud or new to AWS or just wanna get some hands-on, more hands-on experiment experience to enable yourself, we've got thousands of free different learning resources, hands-on labs, immersive real life situations where you can get that experience to build these things out. If you're pursuing an AWS certification and need some extra tools to study for that, we also have that available on our site. And if you need to validate some practical skills for some micro credentials, whether that's for yourself or for your team, if you need that, that's also available on AWS Skill Builder.

[![Thumbnail 2950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a94c3ff9bb4c2b2e/2950.jpg)](https://www.youtube.com/watch?v=JRu2GqRe9sg&t=2950)

So with that, we'd like to thank you all so much for joining us today and if you have any questions,  thank you. Great job Omar. If you have any questions, we'll be here for the next 10 minutes. Please feel free to come by. We'd love to chat with you. If we didn't get to one of your questions, mentioned we didn't wanna talk to you, please stick around. We'd love to chat. Hope you all enjoy the rest of re:Invent. Thank you. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
