---
title: 'AWS re:Invent 2025 - Extend Kiro with MCP support for richer context (DVT213)'
published: true
description: 'In this video, Arundeep and Nathan from Kiro explore context engineering for AI agents, introducing Model Context Protocol (MCP) as a solution to RAG''s limitations. They demonstrate how MCP enables agents to dynamically access tools and data through a standardized protocol, closing the feedback loop so agents can observe outcomes and iterate on solutions. The team showcases MCP''s practical application across Kiro''s entire software development lifecycle—from ideation to deployment—and debuts Kiro Powers, which optimizes context usage by loading MCP tools just-in-time. A live demo illustrates an agent autonomously verifying bugs, creating GitHub issues, proposing fixes, and testing implementations using Chrome and GitHub MCP servers, highlighting how remote MCP with OAuth streamlines production deployments.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/0.jpg'
series: ''
canonical_url: null
id: 3088931
date: '2025-12-06T12:55:09Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Extend Kiro with MCP support for richer context (DVT213)**

> In this video, Arundeep and Nathan from Kiro explore context engineering for AI agents, introducing Model Context Protocol (MCP) as a solution to RAG's limitations. They demonstrate how MCP enables agents to dynamically access tools and data through a standardized protocol, closing the feedback loop so agents can observe outcomes and iterate on solutions. The team showcases MCP's practical application across Kiro's entire software development lifecycle—from ideation to deployment—and debuts Kiro Powers, which optimizes context usage by loading MCP tools just-in-time. A live demo illustrates an agent autonomously verifying bugs, creating GitHub issues, proposing fixes, and testing implementations using Chrome and GitHub MCP servers, highlighting how remote MCP with OAuth streamlines production deployments.

{% youtube https://www.youtube.com/watch?v=s4sRgzNBlbU %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/0.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=0)

### Introduction to Context Engineering: Beyond Prompt Engineering and RAG

 Good afternoon everyone. Welcome to day three of re:Invent. I hope you're having a good re:Invent, and welcome to DVT213. I'm Arundeep. I'm a Senior Solution Architect Manager at Kiro, and I'm joined by Nathan today. I'm the Senior Manager of Software for the team that built the Kiro IDE. We're both founding members of the Kiro team, and we want to take you on a journey that we have been personally on, one that fundamentally changed how we build with agents.

The biggest factor today in whether you'll be happy with the results that your agent is producing is how well you're feeding the agent, right? The primary way to accomplish that is by providing information, what we refer to in the AI industry as context. But here's what's often misunderstood. It's not about having more information, it's about having responsive information, information that reflects what actually is happening right now. Critically, it is about agents being able to observe the outcomes of their actions and input.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/70.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=70)

We've got a packed agenda today, so let's dive right in. Before we do that, quick show of hands.  How many of you have used or are familiar with Kiro here? Great. And how many of you are currently building or using AI agents in your development workflow? That's awesome. So there's one hidden question in here. How many of you have hit context window limitations or struggled with agents getting the right information? Just wanted to make sure that we are presenting in the right place.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/100.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=100)

 We'll go ahead with the agenda. We'll cover four key areas. First, what context engineering actually means and why it matters. Second, we'll take a look at RAG, the current approach and its limitations. Then I'll introduce you to Model Context Protocol, MCP, what it really means, which is transforming how agents are accessing information today. We'll also talk about how you can leverage MCP with Kiro. Next, we'll take you through our journey with Kiro and how we built Kiro with MCP. And finally, we'll see a live demo of this in action. We'll take the agentic leap of faith with Nathan, who's going to do the demo. They asked us not to do a live demo, but YOLO, right?

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/140.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=140)

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/150.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=150)

 But first, let's start with the foundation. What is context engineering? Now here's the evolution that we have seen with AI engineering.  We started with prompt engineering, crafting the perfect long prompt to get good responses. Then we started adding RAG to give models access to knowledge bases, and then agents, giving models tools and autonomy to take actions. But now we're in the final step, which is context engineering, which is what separates prototype from production. It's not just about having tools or knowledge, it's about managing the entire information environment the agent actually operates in. That's what this session is about, how to engineer context for production systems that actually deliver ROI. Notice how each step adds more complexity. Context engineering is how you manage that complexity at scale.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/190.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=190)

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/200.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=200)

 Now you may have heard the term context is king, context is everything. So let me explain what we mean by context is everything in the messages.  When you're using Claude or any other LLM for that matter in a conversation, there is an array of messages that represents the entire conversation. It could be system prompts. It could be user messages. It could be tool calls, tool results, assistant responses. All of this goes into the messages array. That array is the context.

The problem with long-running agents today is that this array keeps growing. Every tool call adds more messages. Every response adds more messages. This is different from single-shot LLM calls. With agents, you're running in a loop. Call LLM, get tool calls, execute tools, add results to messages. The context continues to grow. That is why context engineering isn't optional for agents. It's the core challenge.

[![Thumbnail 250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/250.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=250)

 This is a famous quote by Andrej, where context engineering is the delicate art and science of filling the context window with just the right information for the next step. I would take this one step further. It's not just about filling, it's pruning, summarizing, and reloading context at the right time for the agents. But we spoke about what is context engineering, but let's set the stage on what are the different types of contexts.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/270.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=270)

### Understanding the Four Types of Context in AI Systems

 So let's break this down. First is the system context. That is your foundation, the role definition, behavioral instructions, what sets the agent's personality and capabilities. Think of this as the agent's job description manual. It's always present in every interaction. The second is retrieved context. This is where RAG comes in, semantic search over indexed knowledge base. Here's how it works, right? You've embedded your documentation, your code base, whatever knowledge that you have in data sources that the agent wants to access.

When the user asks a question, the system searches for semantically similar content and injects that into the prompt. It's automated, which is great, but it's also passive. The system decides what's relevant based on vector similarity, not based on the actual need. We'll dive deeper into the limitations of this approach in just a second.

The third type is the conversational context, which includes the previous messages in the conversation and the memory of what's being discussed. This is your message history: every user message, every agent response, every tool call and result. This builds up over time, and here's the challenge. In long autonomous workflows, this can grow to thousands of messages.

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/360.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=360)

We missed one more, which is the tool context, and that's where context is loaded dynamically. This could include MCP and API connections. Now RAG became the widespread architecture for most of 2024,  giving agents access to knowledge. The pattern is simple: embed your documents, store them in a vector database, and when a user asks a question, retrieve the most similar chunks and inject them into the prompt.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/370.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=370)

 But what does RAG essentially mean? First is retrieval. When a user asks a question, the system fetches relevant content from your external knowledge or data sources, which is based on vector databases, and this is based on semantic similarity. We embed the query and search for similar embeddings in our vector database.

The second is augmentation. We take the retrieved context and add it to the user's original prompt. This is the augmented prompt: the user's question plus the retrieved context. Finally, generation. The foundation model generates a response based on the augmented prompt, and it now has access to both the user's question and the relevant background information.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/410.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=410)

### RAG's Strengths and the Context Conundrum

 It's elegant, it's automated, and it works, but for certain use cases. First is improved content quality. Foundation models are trained on data up to a certain cutoff date. They don't know about your internal documentation, your company policies, your product specifications, so RAG solves this problem. It grounds the model response based on actual content in your data, which dramatically reduces hallucinations, another common term that we have all heard of.

Second is contextual chatbots and Q&A. Imagine you're building a customer support chatbot or HR chatbot that automatically references all of your policies. You want that information to be up to date. Third is personalized search. RAG can incorporate user context, their search history, their role, their preferences into the retrieval process, which can influence the way in which the responses come through from the agent.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/480.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=480)

Fourth is real-time data summarization. This one's interesting because it's actually a hybrid approach where you retrieve and summarize transactional data from databases. For example, you could say summarize all transactions over $100 from last week. But the major limitation is that  there's no feedback loop. The model can't say I need more information or I need different information. It gets one shot at retrieval each time in a session, and that's it.

This is fundamentally a passive system. Before the model even starts to think about reasoning, we're guessing what will be relevant based on similarity and not an actual need, and this is what I'd like to call the context conundrum. Retrieve too little and you miss out on information. The agent says, I don't have enough context to answer that. You retrieve too much and you overwhelm the context window with noise. You're paying for tokens that aren't relevant, so the agent gets confused.

You also have a semantic gap. Semantic similarity is not the actual need in itself, and sometimes the retrieved context may be outdated. Your documentation may have changed, and your vector database has to keep up with the up-to-date documentation. You also end up with a multi-hop problem as well, where you're unable to follow the chain of dependencies.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/550.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=550)

### Model Context Protocol (MCP): The USB of AI Integrations

That is where MCP comes in. It's an open standard created by Anthropic.  That's important because it's not proprietary. It's not locked to one vendor. Second, it's a client-server protocol connecting AI to data and tools. Third, this is where it gets practical. MCP already has predefined SDKs and built-in integrations. You connect to Google Drive, Slack, Jira, you name it. You don't have to build everything from scratch. There's a growing ecosystem of MCPs.

MCPs consist of three core primitives, which are resources, prompts, and tools. We'll dive into each one of these in detail in the next slide on the request flow. So how does the MCP request flow work? The user has a query. Let's say I would want to see what is the status of my deployment, and the host, in our case we'll talk about Kiro being the host, which consists of the agent and the combination of the LLM, is going to request available tools.

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/650.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=650)

The MCP client is going to make the call for list_tools, and the MCP server is going to list the tools, which will probably give the list of all specific tools related to the deployment question. The LLM is going to make the choice and select the tools and identify the parameters. Let's say there's a specific tool called get deployment status, so there will be a parameter that will be required, and let's say deployment ID is one of the parameters. It could be in the conversation which gets sent back, and we call the tool with the specific name and parameters, and that is probably going to create an API call, and we are going to get an API response back. That formatted response is what is sent back to the user, which is integrated  from the agent. So the MCP protocol is everything that happens from the host to the MCP server, and then this custom logic that the MCP server or the providers that is defining the MCP is going to make.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/670.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=670)

But why is it important? You may have heard that MCP is like the  USB of AI integrations. Remember before USB, every device had its own connector, and after USB came in, that was standardized into a single port. So MCP does the same thing for AI. You build one MCP for each data source. That server works with any agent that speaks MCP. So the M times N, where one agent with 10 data sources, is going to become 11 plus 10, right? So that's 11 integrations instead of 50 odd. And add a new agent, it automatically works with it. MCP provides consistent implementation patterns whether you're exposing a file system or a database or an API. Your approach is going to be consistent. This means faster development, easier maintenance, better reliability, and simpler onboarding.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/720.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=720)

 But how do I deploy MCPs for production? MCP supports two transport mechanisms, which is the STDIO, which is the local MCP, where the MCP client and server runs as a subprocess in the same machine communicating to standard input and output, and each agent spawns off its own subprocess with its own MCP server instance. It's isolated, it's simple, it's fast, no network overhead, no authentication complexity. But notice that this doesn't scale to multiple users or distributed systems, and that's where HTTP comes in. Streamable HTTP is for production deployments. Multiple agents can connect to my MCP clients. The keyword here is streamable, so it's based on SSE, which is the server-side events for real-time bidirectional communication. The agent can make a request and the server can stream back results as soon as they become available.

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/790.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=790)

### Closing the Feedback Loop: How MCP Enables Agents to Observe and Iterate

Now up until now we've talked a little bit about how we get the agent more context and access to more tools, but what we haven't answered to a full extent is why does this matter for you and how does that enable  the agent. So one thing that MCP can do that's really interesting that it can leverage these prompts and these tools is it lets the agent for the first time close the feedback loop and check its work in ways that weren't possible before. So up until now, and in the early days of agentic development, what you saw was an agent would understand your code base. It would propose a solution, but it wouldn't be able to check its work, so you would have one shot to get the best quality result, but that may not be what you were looking for.

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/830.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=830)

So this is what the feedback loop looks like in action.  The first couple steps is very similar to what we had before. The agent was able to reason what you were asking for with your user prompt and propose a solution. Then it was able to act. So the agent might edit code. It might launch a debug server if you're working on web development or you're working on an API or infrastructure. But what really changes with MCP is for the first time the agent is able to observe, so the agent can read terminal output, it can retrieve service logs or client logs, it can inspect breakpoints and code, it can view rendered applications if you're working for a website or even make internal API requests to the server that's running. And then what really becomes powerful is that it's able to iterate on a solution. So based on the information it got from its observation through MCP tools, it can refine its work and to iteratively get towards the right approach, much in the same way that an engineer would traditionally.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/890.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=890)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/900.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=900)

 So where this really starts to get more interesting is with remote MCP. So with remote MCP you can make a connection to remote transport servers,  and this lets you have a secure connection to external resources that's connected through HTTPS.

It streamlines the authentication process because you can have an OAuth connection. You aren't worrying about having locally managed credentials that you might accidentally commit into your config and expose. We have that secure environment variable expansion to make sure that the credentials remain protected. And the biggest thing that you get with Remote MCP is that servers are now hosted. It doesn't require local setup.

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/970.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=970)

So traditionally, what you had as a challenge for setting up MCP servers locally is a lot of them required very specific environments, like having Python and UV installed or having Docker, or you had to have a lot of complex business logic that was deployed to every user that was developing applications for you. So with Remote MCP, these limitations are no longer a factor. And for configuration, Remote MCP looks much in the same way that it does for Local  MCP.

So with Local MCP, you would have a command that runs locally. So with that Atlassian MCP server as an example, that is one that is running UVX, which is a Python library, so it runs with these local arguments. But with Remote MCP, you now set a URL and optionally authentication mechanisms. So there's a couple different types. You can either have header-based authentication that has an access token, or you can use OAuth to authenticate to that remote service.

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1010.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1010)

### Leveraging MCP Across the Software Development Life Cycle at Kiro

So what does this mean for the Kiro team? We were able to leverage  MCP through all phases of our product software development life cycle. We wanted to really look at how across the entire arc of software development, where were phases in that software development life cycle that we could leverage MCP to speed up the team. Because the first focus for agentic development was really on the implementation side and on accelerating the hands-on keyboard or the code authorship phase. But what we found very quickly is that even early on in the agentic development industry, the implementation side and the hands-on keyboard was largely a solved problem, but that's only a small sliver of what we do as software engineers.

So what we wanted to do was see all those phases, how we could leverage MCP to help us deliver software faster to customers. So that really started with the ideation phase where you can do things like retrieve backlog information, work with the product, look at designs. Maybe you store them in Figma and the agent can automatically pull those and analyze the requirements that come with a product manager and come up with a proposal of how we can solve the things that were outlined in the product backlog.

And then we go into the implementation phase, which is what most of the team is very familiar with with agentic development. But with MCP, not only can the agent author code, but it can go check information to get the latest information for reference docs for library information. Here at re:Invent, we've launched a lot of new products that I'm sure people here have a little bit of context on what was launched, but you don't have context on everything. And so having an agent that can go and retrieve that information just in time and present that to you really helps developers get up to speed faster.

And in addition to that, not only can it write code, but it can also do things like control dev servers, launch debug windows, interact with applications, and test the implementation as it's writing it. So once the code change has been completed, then you can go into the testing phase. For us, agent quality and speed and reliability is very important, so we have a benchmarking system that we evaluate every agent change against.

So we're able to have the agent make a change locally to its own source code and the tools that it has available and the information it uses to implement a change, and then it's able to run a benchmarking suite against the changes that it made. We even use it for regression analysis, so when we have a release candidate that we're testing, that we have software that's staged to deploy, we distribute that to an internal group of testers. And then we will have an agent locally that's running that is analyzing all of the service requests that are happening against that release candidate and determining if there's any regression in either quality, speed, or user experience.

Additionally, we have fault and acceptance testing that's doing things like fault injection in the client and verifying that that behavior is handled appropriately.

Then we go to the deployment and support phase of the software life cycle. I think there's a lot of innovation here in the industry and in our team as well. What we'll do is if we get any user reports that there's a defect or a regression or undesirable behavior, we can have an agent that's running locally that's retrieving our service logs and live telemetry so that we can get enough information to diagnose and root cause and then locally recreate failures. It's able to connect to production systems and have all the information at its fingertips that it would need in order to solve a problem. Things that would have taken engineers 10, 15, 30 minutes, maybe hours to diagnose, we have an agent that has all the tools at its disposal to automatically gather that information to make support much faster than it was before.

[![Thumbnail 1270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1270.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1270)

### Live Demo: Verifying Bugs and Creating GitHub Issues with MCP Tools

 So what we learned today is context is everything. The quality of the results that you get from the agent is directly related to the quality of the context that you provided. What is really useful is to close the feedback loop, and that true agentic behavior requires an agent not only to implement a change but also to observe the outcome and adjust. You can extend the agent reach by adding MCP tools that let the agent act outside of the editor and use remote MCP to integrate with external systems. What we wanted to do today was leave plenty of time for demos because really it's hard to understand exactly how this will impact your ability to build software until you see it yourself, so we have some time for a demo here.

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1340.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1340)

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1380.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1380)

 What I will be working on, one thing that we really like to do with spec driven development and with developing Kiro ourselves is build small simple applications. We do use it for developing the application itself and for enterprise production scale systems, but really to get a feel for how it's working, it's useful to build these simple apps to test functionality. What I'm going to look at right now is this 2048 game, which is a simple sliding block game where you try to combine blocks in order to reach 2048, and I will open up Kiro, which hopefully many of you are familiar with, and we'll first  look at our MCP configuration.

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1400.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1400)

You'll see in here I have a couple MCP servers that are already installed. In order to configure these, you'll see we have a few in here like I have this AWS docs MCP server, we'll make this a little bit bigger.  We have this AWS docs MCP server. So this is a local MCP server. We do have a more powerful AWS MCP that allows you to configure production services, but this is a simple one that runs locally that just lets you look at documentation. We have a couple of commands that are auto approved. Since this is just documentation, we trust the agent to take actions.

[![Thumbnail 1460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1460.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1460)

We have a Chrome MCP server that lets it run a browser, a fetch tool that lets it retrieve websites from the internet, and then we have a couple of remote MCP servers. One uses header authorization and one uses OAuth. So we have GitHub and Figma here. The Figma one you'll see is not authorized, so I can walk through the authentication flow to see how OAuth gets set up. This is one that it will launch in your browser and you have to trust that Kiro can launch this, and then you'll see  Kiro would like to access your account. With this I don't have to worry about any local credentials. Did I accidentally commit something that I shouldn't? I just give Kiro access.

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1470.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1470)

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1480.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1480)

[![Thumbnail 1490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1490.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1490)

 Authorization is successful, and I can come back in Kiro, and now it's connected, and so that's all that there is to it.  So a very simple MCP server that we can look at is the AWS docs. So at re:Invent,  AWS just announced a new model called Nova 2. Can you use the AWS docs tool to tell me a bit more about it?

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1510.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1510)

[![Thumbnail 1520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1520.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1520)

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1540.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1540)

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1560.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1560)

 And hopefully conference Internet lets us do this  and it does look like so it was able to call this search documentation tool and because I have this trusted in my list of MCP servers it didn't ask me for any approval. This is because this is a tool that I deemed safe and that I have no problem with the agent doing automatically. So we'll look up here a little bit. So there's a couple.  It's asking for information on Nova 2 in here and you can see it got a couple results, and then once it understood where the documentation was located, it went in and read the documentation to get some more information and now we've got a summary of all of the new Nova models. 

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1580.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1580)

I can use this if this was something that it was a framework or a library that I wanted to integrate with. This is really helpful to get me started. But I want to go on to doing some meaningful work now with that Nova server, and if I look at it, I think this will no longer be running, so yeah,  I now have my localhost turned off, so I want a little bit of help from Kiro.

[![Thumbnail 1590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1590.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1590)

[![Thumbnail 1600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1600.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1600)

[![Thumbnail 1610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1610.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1610)

Can you help me run my dev server?  So this is something every project's got a little bit different command, and I always forget it, so I just have Kiro  help run it for me, and this is one that is useful. It's running in a background process and if ever it needs to check on what the output of this server was, it can, but it'll have it running here. 

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1620.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1620)

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1640.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1640)

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1650.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1650)

So with Kiro's help now I have this running now I can look at this 2048 game. So if you haven't seen this before,  what it'll do is combine numbers in order to try to ultimately combine it up to 2048. This is a game that's pretty simple, but in trying to learn it, I wanted to see, okay, can we just create an auto solver here? So I have this auto solver,  and it just does it automatically for me, but we've gotten some reports from users that if I click new game here,  it just keeps going from the last game.

[![Thumbnail 1660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1660.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1660)

So my product manager asked me to create a ticket for that, but that sounds like a lot of work, so I'm just gonna have Kiro help me.  This is our day to day life. Yeah, hey Kiro, yeah, users are reporting that if you have the auto solver running and you try to start a new game, it keeps going with the last game. Can you help me verify this defect using your MCP tools and once confirmed, let's create an issue on GitHub in the AI playground repo.

So what I'm doing, I'm asking Kiro, let's verify it locally because I mean we tested it ourselves but just for completeness we wanted to have the agent test it and then it'll get some information and then it'll draft it into a ticket so that we could track our work. And it'll be a little bit slower here on the conference Wi-Fi, but normally this is a very fast process. Yep. And one of the other things if you have noticed, in this particular prompt, Nathan didn't specify which MCP to use, so Kiro automatically detects the intent and then uses the GitHub MCP.

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1800.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1800)

While we wait, is there anything new that's gonna happen this week that folks should be excited for? Yeah, if you've used Kiro before, you might notice that there's a new panel here, and one of the things that you will also notice is I am already 19% into my context usage, and I have barely started the conversation. And so if you'll see I've got quite a few MCP tools like I have this GitHub MCP server that has 40 tools in it, Chrome has 26, this is the biggest problem that we hear from customers is I love  MCP tools and the power that it gives me, but it uses all of my context.

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1810.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1810)

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1820.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1820)

It uses all of my context and there's less usable space for me to actually do the interesting work. So what you'll see  here, which was announced earlier, is a product that we're calling Powers. What Powers is, is it combines  steering files and MCP tools and all of the information that an agent needs to leverage popular libraries. You'll see a lot of them here, things like Figma, Netlify, Supabase, Strands, and many more here.

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1840.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1840)

It packages all of the information that you need to leverage these tools into one common configuration.  And then the agent is able to just in time add that information to its context so that it doesn't need to have everything in its system prompt and in its context window all of the time. If you recall back in the conversation, prompt engineering is not context engineering, and prompt engineering is not making sure you have as much information as possible. It's about making sure that you have just the right information at the time for the agent's next action.

### Autonomous Bug Fixing: Agents That Test, Iterate, and Update Issues

So you'll see here in the side very similar to the way that we have command approval with the Chrome MCP server that allows the agent to operate your browser. That is a little bit less safe than things just like retrieving documentation. The agent could do anything in your browser, which obviously there's some unsafe operations that it could do, so this is one that I wanted to accept any command. I see it's trying to run my local server, so I'm going to let it call and it ran into a 404 because it's not running. Oh, there's already another window open. I will quit this, and now it'll fix. Okay, there we go.

[![Thumbnail 1940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1940.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1940)

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1960.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1960)

So it's launched a new window. Now it's going to navigate. Oh, so now it's opened up my project and what it's going to do is it's going to take a snapshot of the  page and that'll give it some information on how it's going to navigate. For simplicity's sake I'm going to approve these just so we're not flipping back and forth between the two windows. So it actually clicked autonomously and then it's going to take another snapshot of the page.  This is pretty cool.

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1970.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1970)

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/1980.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=1980)

So it's going to try this auto-solve, but it is already broken. Let me see.  The game had already been lost, so now it's going to try to start a new game. So you trust it once and  yeah, so with that it gives you the option to run the command but also trust the tool and run it so that allows the agent to have a little bit more autonomy as it's performing actions. Hopefully at home you have a little bit faster Wi-Fi than we have here at the conference, but obviously there's a lot of people here, so it takes a little bit longer than it would normally.

But really what this is unlocking, so there's actions that you can take where the agent might be able to diagnose an issue by just looking at the source code, but this is something where for the first time it's able to see it in a real environment and it's opening up interesting possibilities. So one of the challenges that we ran into as an engineering team is obviously the speed of agentic development is at lightning pace. Okay, now it started clicking, so it started the auto solver and now it's going to see if I click new game, is it going to continue.

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2060.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2060)

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2070.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2070)

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2080.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2080)

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2090.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2090)

So it was able to confirm the bug by clicking new game and that's  still going, it didn't stop, so now it's going to take another snapshot to make sure, okay, is it still progressing. So now it's confirmed that the  behavior is still there. So I'll go ahead and stop this. Now what it's going to do is it's confirmed that the bug exists and now it's going to write an issue for me.  So I'm going to accept that it can run an issue, but I'm not going to trust that because I don't want it to go too crazy. You don't want to create  random issues.

Yeah, so now it's creating the GitHub issue for me, but going back to what I was saying before. What this does allow is really interesting use cases.

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2140.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2140)

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2150.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2150)

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2160.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2160)

For a team that's moving quickly with agentic development, it's always a challenge to write comprehensive regression tests or comprehensive integration tests. So one of the things that we were looking at is Kiro can drive these windows, it can click through things, and it can read documentation. We have pretty thorough documentation on our website already. If I go here, I can go to Kiro.dev/docs, and we have a lot of information here on how specs work, how hooks  work, and examples of using them. So one of the things that we thought of is, hey, this is all the information that a user needs to use  our tools successfully. Is that all the information that an agent needs to use our tools successfully? So we just said go read every page on our documentation site and go make sure that  if I follow the instructions for MCP and if I configure this server, does it work?

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2180.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2180)

And so we just got a report on not only is the tool working as expected, but is the information on the documentation site correct. And if there's ever any contention between reality and the documentation,  we've got to fix one of them. And we can go take a look and see is this something that we need our doc writers to help us update the documentation, or is this actually a defect in the tool. So with very, very low lift from the engineering side, we could get a lot of value by leveraging these systems and their ability to inspect reality.

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2220.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2220)

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2230.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2230)

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2240.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2240)

My team owns the doc site, so Nathan's going to ping me. And yes, okay, so the agent was able to finish creating this issue, and we can see in GitHub the auto-solver continues running after clicking new game.  And I think if it were me writing this, I probably would have stopped there. But now we have steps to reproduce, we have expected and actual behavior, we have the  test environment and additional context. So this is something that again we see the agents are much more thorough than engineers would like to be.  And it's really valuable context, and this helps delegate a lot of the day-to-day busy work that prevents you from spending time delivering value for customers.

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2260.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2260)

[![Thumbnail 2300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2300.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2300)

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2310.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2310)

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2320.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2320)

So now that that issue is created, great, thank you, Kiro.  Let's go ahead and start fixing this issue. Before we implement a fix, come up with a proposal for how we might address this by inspecting the source. Once you have a proposal, post a comment on GitHub. So we'll have the agent take a look at the source code to try to understand what the problem is, and it also has all the same MCP tools that are at its disposal. So if it  needs to go inspect the actual behavior at all, it can go and do that. But for now it's going to start by just searching the code base.  So it's retrieving the files that it needs, and this is all just back to the regular agentic loop, but it will use MCP tools at its disposal. 

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2330.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2330)

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2350.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2350)

So it's got a root cause for why it believes the game continues, and it has a proposed fix. So now we're going to add a comment to that ticket  that has its findings, and it added that comment. So we should see now it's a comment from me, but also if you want to add an access token that shows that it's from the agent, a lot of times we'll have a convention from the team is if it's coming  from an agent, we'll just say hi Kiro here and have the rest of the comment. That just shows people that it's from an agent, and maybe treat it with a little bit more scrutiny than you might before. But generally we've learned to trust the agent pretty well because it is very thorough in its analysis.

So it's been able to identify the root cause, it's been able to propose a fix, and this is something that we can work with the teams to take a look to see is there anything that we disagree with this proposal, and we can kind of continue the conversation in GitHub. Or if you use Asana or Jira or any other tool, generally speaking, there's an MCP server for any of them. And if there's not, it's very easy to wrap any existing API in an MCP server. So it's able to leverage the existing systems that you have, and that's really the power of MCP and remote MCP. It lets the agent act outside the editor.

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2430.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2430)

[![Thumbnail 2450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2450.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2450)

So we will check back to see the progress that the agent has made. It added the issue comment. Great. Let's get to fixing the issue. So it went ahead and made some changes  to the game. And all we had to do is this one line. We turned auto solving off as soon as it restarted the game, so now it wants to go navigate to the page and reload the window to confirm the behavior.  So I will come back to this.

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2460.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2460)

[![Thumbnail 2470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2470.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2470)

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2480.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2480)

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2490.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2490)

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2510.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2510)

Yeah, so now it's verifying the behavior  again and it's going to click New Game. And now that it clicked New Game it stopped. So we were able to fix  that issue, but what we do see is that all the tiles are still here, so there's a little bit more work to do. So we're going to let it auto approve navigating.  And so this is what is really helpful with closing that feedback loop. It's something that if it's looking at the source itself  it may have had an incomplete solution, but then you're faced with the frustration of nudging it, hey, you didn't quite get to the full solution. And with this it's able to check back against reality and it's able to iterate and refine in pretty much the exact same way that an engineer would. Because you start to have a solution, but often times  what you'll find in software is every solution has unintended consequences, so this is able to allow the agent to further refine.

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2550.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2550)

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2560.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2560)

So it's looking again at the logic and based on the learnings that it made from observing reality, it's able to come up with a little bit more complete solution. So now it's going to test the changes that it made. And in here  it's going to update any pending movements and clear those out. And one thing that I can even do is say, hey, our issue is now out of date.  Can you update it with the latest findings and the problem you observed? Unfortunately, even though the agent is still working and it's navigating, I can interrupt it at any time and I can say, hey, I want you to go do that real quick before we go much further. So I just want to update that issue to make sure I told the product manager it was a one line fix and now it's not, so he's got to get up to date.

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2600.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2600)

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2610.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2610)

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2620.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2620)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2630.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2630)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2640.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2640)

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2650.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2650)

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2660.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2660)

So we're going to add another comment and we don't trust that, so  now we'll come here and we can see updated analysis. So now it's something that  obviously I can go in and check this just for demo purposes, but if I were an engineer I'm able to stay in my editor and I don't go out of my flow state. And I can even  add some steering information. So I can create an agent steering file and I can say GitHub  updates. And I can say, we'll get rid of these, any time you're working on an issue  from GitHub, be sure to add a comment with the latest  status. When we start looking at an issue, post a  proposed fix. So things like this where we just add steering and we can let the agent take care of it and we can just continue to focus on the implementation and actually fixing the bugs. And all of the project management and the status tracking, that can just happen in the background and the agent can keep us updated.

So that's something that you'll find a workflow that works for your team, but this is tools that let you evolve so that you're able to really focus on continuing to solve the important problems. This is just a basic steering file, right? We've seen very complexly architected steering files for production systems by customers.

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2720.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2720)

### Introducing Kiro Powers: Just-in-Time Context for Production Systems

Even Kiro can help with this. I'll cancel this. Hey, I added a steering file on GitHub, but it's pretty basic.  Can you add information on our workflow of updating the ticket with our latest findings? This is a really important thing that Nathan just showed, right, because you're not just relying on your updates to be made manually. You can take control of the agent and then ask the agent to make the updates on behalf of you, and you'll see how comprehensive it gets.

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2760.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2760)

[![Thumbnail 2780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2780.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2780)

Yeah, so one of Kiro's superpowers is that in its system prompt and in all the tools that we've built into it, it deeply understands all of its own functionality and how to configure it. So even things like  these MCP servers, say for fetch, I'm going to call this busted. And oh, my connection now failed, so we'll close this window and make sure it's not working.  I'm going to say, hey Kiro, help me fix it. So it's able to retrieve its own logs and it'll help you debug it. Things like this are really useful for any of the features that Kiro has. It's able to help debug itself, so it's going to read its MCP file.

[![Thumbnail 2810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2810.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2810)

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2820.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2820)

And this is for simulated errors, right? I imagine if a customer is having production errors. Yeah, so it figured out that that  busted command is probably not right and that it needs UVX, and so now we've already got the MCP server fixed. So things like that, the  secret sauce that we tell customers is just ask the agent, and most of the time it's able to resolve the problem.

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2850.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2850)

[![Thumbnail 2860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2860.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2860)

So that is most of the information that we wanted to cover here with the demo. Again, we have an innovation talk tomorrow that is going to  give a lot more information on Kiro Powers, and we also have a lightning talk, so I'll be presenting a lightning talk on Kiro Powers tomorrow  in case you're not bored of me yet. But we will also have a breakout session at 4 p.m. talking about Powers and how we have integrated with the launch partners that we worked with. We've released our blog post today in the morning, which talks about some of the common Powers that you can install, and we're looking out for more. So these are some of the partners that you can see here. You all are the first ones to actually see Powers in action, so that's the perk of attending our session.

[![Thumbnail 2890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2890.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2890)

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2910.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2910)

In which case, what we wanted to do,  we really wanted to talk about the information and the power of MCP, but also introduce, there are some challenges that we wanted to address with MCP and the adding dynamic context and adding just-in-time context and being thoughtful about what we're including.  And if there's information that's actually distracting the agent, and so that's what we wanted to solve with Powers. So we didn't want to leave you with this talk without hearing about Powers before it's even officially announced in the innovation talk, because everyone has heard about MCP, right? We are thinking what's next, what's beyond, yeah.

So there's a lot of other places around here where you'll find Kiro. It's actually just behind you. There'll be a lot of talks in the Builder Loft throughout the event. We have the House of Kiro that's right by the Expo Center. There's a Kiro kiosk in the AWS Village, and again in the Builder Loft there's many whiteboarding sessions and Kiro meetups as well. I should have asked Kiro to figure out the bug in this slide. Yeah, there is a Y missing in the end for Kiro meetups, so.

[![Thumbnail 2990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ad8eb1fbdee08918/2990.jpg)](https://www.youtube.com/watch?v=s4sRgzNBlbU&t=2990)

But we just wanted to say thank you for everybody that attended the talk, and I think there's always a lot of questions on MCP and how we leverage it internally, and then how customers can leverage it. So we'll be sure to be here after the talk. We'll be here to answer any questions, but just want to say thanks everybody for interest in Kiro and in learning about MCP and how you can leverage it for your production systems. Thank you. 


----

; This article is entirely auto-generated using Amazon Bedrock.
