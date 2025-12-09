---
title: 'AWS re:Invent 2025 - Accelerating AI: How Sun Life reduces latency and enhances engagement (IND3320)'
published: true
description: 'In this video, Sun Life and AWS Professional Services demonstrate how they built generative AI solutions that maintain user engagement during complex workflows. The team presents three key implementations: a conversational IVR system in Amazon Connect using multi-agent orchestration with Model Context Protocol (MCP) to eliminate traditional phone menus, a Content Assist platform for generating and scoring marketing content with deterministic agents, and an architecture leveraging AWS AppSync for real-time streaming to transform 30-60 second AI processing times into transparent, interactive experiences. The session includes detailed technical architectures showing integration of Amazon Bedrock, Lambda, MCP servers on EKS, and cross-account AppSync configurations with private APIs, along with live demonstrations of streaming content generation for their fictional company PurrfectFrame Studios.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/0.jpg'
series: ''
canonical_url: null
id: 3092939
date: '2025-12-08T18:43:00Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Accelerating AI: How Sun Life reduces latency and enhances engagement (IND3320)**

> In this video, Sun Life and AWS Professional Services demonstrate how they built generative AI solutions that maintain user engagement during complex workflows. The team presents three key implementations: a conversational IVR system in Amazon Connect using multi-agent orchestration with Model Context Protocol (MCP) to eliminate traditional phone menus, a Content Assist platform for generating and scoring marketing content with deterministic agents, and an architecture leveraging AWS AppSync for real-time streaming to transform 30-60 second AI processing times into transparent, interactive experiences. The session includes detailed technical architectures showing integration of Amazon Bedrock, Lambda, MCP servers on EKS, and cross-account AppSync configurations with private APIs, along with live demonstrations of streaming content generation for their fictional company PurrfectFrame Studios.

{% youtube https://www.youtube.com/watch?v=ag2EbZu5gUE %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/0.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=0)

### Introduction: Sun Life's Journey in Accelerating AI with AWS

 Welcome to IND3320, Accelerating AI: How Sun Life Reduces Latency and Enhances Engagement. My name is Salman Moghal. I'm a Principal Consultant with AWS Professional Services. I'm joined here today with three wonderful leaders, great colleagues from Sun Life who will share their real-world journey building generative AI applications, solutions, and architectures that keep users engaged during complex workflows.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/40.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=40)

Quick introductions from Sun Life. We have Robert Chin. He's a Product Owner for GenAI Patterns.  To me, he's the OG who actually started the whole thing and democratized generative AI at Sun Life. He has built numerous generative AI technology patterns since early 2023. Dennis Volovic is the Director and Domain Enterprise Architect for Contact Center. He's building very innovative multi-agent collaborative workflows within Amazon Connect, which he'll go over today. And then we have Walter Kurzatz. He's a Senior Principal Engineer and he's the brain behind designing and operationalizing generative AI patterns at Sun Life in a very tightly regulated and secure environment.

Interestingly enough, this is the same team who recently built an omni-channel generative AI solution, code name Iris, using Amazon Connect and Bedrock. The solution recently won some CIO awards. So the point is that if you want to come and talk to this team afterwards about how they actually took this solution to production, please do so. We have a lot of content. I know this is the last presentation before you can leave, so please stick around and we have a lot of content to cover.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/120.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=120)

 All right, our agenda for today. I will quickly explain the evolution of Enterprise AI and how this evolution has caused some interesting and unique challenges. Dennis will walk us through how he and his team are building a very innovative agentic concierge-like system within Amazon Connect. They are accelerating AI adoption within Amazon Contact Center. Rob will then show us their Content Assist, which is the content generation and scoring platform within Sun Life. And finally, Walter will deep dive into architectures and demos.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/170.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=170)

### The Evolution of Enterprise AI and the Challenge of User Engagement

So let's talk about Enterprise AI and how this evolution has caused some interesting new challenges. We have seen the advent, or dawn, of RAG systems, RAG workflows.  These are Retrieval Augmented Generation systems. These are single-step solutions. The reason why I say single-step is because there's one round trip to your Bedrock or large language model to generate content, and there's a lot of human oversight involved. We're now seeing deterministic agents that are automating multiple steps or multitask workflows, and they're still very heavily being developed and used in SMBs and enterprises. We're also seeing autonomous agents. These are multi-step agentic workflows with a large variety of tool sets. They can truly mimic human reasoning and capabilities. And now we're starting to see virtual worker agentic virtual workers that can truly collaborate between teams across your organization.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/240.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=240)

While this evolution has brought incredible business value and has unlocked a lot of potential, this has also caused a lot of interesting challenges that we'll talk about. So here's one.  On the left-hand side, you see a simple Q&A chatbot. It works perfectly well. You get instant answers. Users are typically very happy with these instant responses. But on the right-hand side, you see a more complex workflow. This is the Content Assist that Rob will go over, and it's toward the right-hand side of the spectrum that we just saw in the previous slide. These are deterministic workflows moving toward autonomy.

Analyzing guidelines, generating drafts, and then refining the output, these intelligent steps take time, sometimes 30 seconds, 60 seconds, or longer. So what is the problem here? It's the response, right? Users expect immediate response. When they see these spinners or three-dot animations, they think the system is not working, whereas behind the scenes AI is working perfectly well, but it just takes time to respond. So it's not a technical problem, it's more of a user experience problem.

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/300.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=300)

 And undoubtedly, in 2025, it has been the year for Model Context Protocol.

That acted as a catalyst, speeding up the creation of these agentic systems. It's often compared to the USB-C port for agentic AI systems, providing a standardized way to connect to your tools and data sources behind the scene. Suddenly, with the help of MCP, building these autonomous agents became very easy for enterprises, but there's always a catch, right?

The catch is that while you are able to build these things very rapidly, it takes time to execute agentic workflows. This really amplified the challenge that we just saw, and as more and more enterprises adopt intelligent agents through MCP, keeping users engaged in your system is extremely critical. So how do we solve this challenge? How do we keep users engaged during these long-running AI workflows?

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/360.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=360)

### AWS AppSync as the Solution for Real-Time Streaming in AI Workflows

 Well, you probably have guessed and you have dealt with it at some point. Streaming or real-time updates is the way to go. If you've been keeping up with the plethora of re:Invent announcements, there was one just recently made around Amazon API Gateway now supporting streaming natively. You can also build your WebSocket servers if you want to go down that daring path, but scaling those servers is challenging. So for enterprises, managed services that are secure and provide necessary capabilities out of the box are super critical, and that's why AppSync is a better option.

AppSync offers three key capabilities. First is flexible security through private APIs, so your traffic never leaves your VPC and is always secure internally. Number two is the request handling, so it provides you with flexible request handling where you could stream these real-time updates to the front end. You can also use resolvers, and for those who don't know what AppSync is, we can talk later. You can change your request and response resolvers using these capabilities. And then third is the flexible evolution of your API model. So as you can imagine, as you're adding more and more data points and connections to your backend, your consumers are consuming your APIs. GraphQL has some capabilities built in where the changes in the backend don't really impact your consumer.

Another key capability that enables streaming is GraphQL subscription. It happens over a managed WebSocket, so AppSync provides all these capabilities in a managed way, in a more secure way. Talking a little bit more about security, for highly regulated customers like Sun Life, security is critical. AppSync provides this private API accessibility only from your VPC through PrivateLink. With controls at every level, you can use IAM policies, you can use endpoint policies, security groups, and whatnot. That's what Sun Life uses behind the scene.

Another key capability of AppSync is Resource Access Manager sharing, or RAM. You can share APIs across multiple accounts in a highly regulated environment where you have multiple accounts to deal with. Typically, you are only allowed to get into the cloud through a foundational or a network account, so you can share your workload APIs into this foundational account and then access it through that VPC endpoint. Walter will dive deeper into this way deep in his section.

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/540.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=540)

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/560.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=560)

All right, so a couple of patterns.  For a simple Q&A bot, AppSync calls Bedrock using its built-in capability. These are data sources, so there's a Bedrock data source. You could bring your context, your prompt, call AppSync, and it will work, right? This is for simplistic workflows and works quite well. But for more complex workflows that require a longer time to execute, you need a different pattern, and  this is the async pattern for AppSync, which we talked about. This is applicable to the right-hand side of the spectrum for the AI longer-running workflows.

The user makes a request, an AppSync mutation or query, to start the backend workflow. At the same time, the application subscribes. AppSync invokes the Lambda function, which orchestrates between your MCP server, between Bedrock, and the MCP server provides your standard set of tools, APIs, and databases to your orchestrator. Then it starts streaming. As your AI orchestrator is going through those multi-step processes, you can now start streaming that data back to your front end or to your consumer of the API.

So that 32 seconds turns into a more engaging, transparent, and explainable experience. So this pattern is applicable to any long-running workflows, and this is AI orchestration through UX.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/630.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=630)

### Multi-Agent Orchestration in Contact Centers with Amazon Connect

Sun Life also leveraged multi-agent orchestration in their contact center  so that they removed dial tones like press 1 for this, press 2 for this, with a very smart concierge system that understands user utterances better than your traditional built-in NLP intent. Dennis will talk about this in a second, and the concierge routes these requests to a specialized sub-agent. It can be an authentication agent to verify or to authenticate your users. It can be an IT support agent that can handle your password reset requests, and it can be any specialized sets of sub-agents that can handle claims, your benefits type of requests. So the key is keeping this response time in check when building these multiple agents, and what you're seeing here is the example of agentic virtual workers. This is multi-agent coordination delivering complete self-service.

So to recap, MCP enables agentic workflows. AppSync accelerates AI through real-time streaming. Contact Center or Amazon Connect accelerates through voice and conversational concierge systems. Sun Life uses both, so better UX, faster AI, engaged users. And to dive deeper into the self-service contact use case, I'm going to invite Dennis Volovic to the stage. Hit the green button.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/720.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=720)

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/750.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=750)

### Dennis Volovic on Transforming IVR Experiences with Conversational AI

All right, good afternoon. My name is Dennis Volovic,  and I'm a domain enterprise architect for contact centers at Sun Life. It's been a good re:Invent so far. I'm trying to catch up on all the announcements. I'm looking forward to getting back and experimenting with the 29 new features announced for Amazon Connect. Before I get started, for those that may not be familiar with Sun Life, we're an international financial services organization, and our purpose is to help clients achieve lifetime financial security and live healthier lives. 

Picture this. You call customer service and within seconds you're having a real conversation. No menus, no press 1 for this, press 2 for that, no repeating your account number three times, just natural dialogue that actually solves your problems. It sounds too good to be true. I'll show you how you can transform from menus to a conversational IVR and create concierge-like experiences that move beyond keywords to natural dialogue that understands context, intent, and nuance. From there we'll get to digital assistants that will empower your customers to resolve issues independently through AI-guided interactions. And with conversational digital assistants, you can start to balance the friction typically faced with tasks like authentication.

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/810.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=810)

Contact centers are where AI can really shine and transform IVR experiences. The question is, where do you start? We've been working with Amazon  Connect now for several years, and as we're moving through the journey from touch tone to deterministic agents, we're testing and learning thoughtfully and responsibly to ensure we focus on the right outcomes and experiences for our customers. The solution options I'll share helped us improve service quality while progressively enhancing the customer experience on the IVR.

We'll start by talking about Amazon Connect and Amazon Lex. This is how you enable your IVR for speech recognition and natural language understanding. Then I'll introduce services like Bedrock and EKS that will augment the Connect experience with large language models and Model Context Protocol, which will give us more contextual awareness, multi-turn memory, and dynamic dialogues. You'll come to see that there is a path to deterministic agents, and along the way there's value that can be delivered for your customers.

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/880.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=880)

### Building Voice-Powered Experiences with Amazon Connect and Amazon Lex

All right, we'll start by covering a voice-powered experience. Now this is a very nice alternative.  Your customers will be able to appreciate expressing themselves verbally. The solution uses Amazon Lex. I know there were a lot of announcements around Connect AI agents, but Lex does remain the scaffolding that sits between Connect and those AI agents, so it's still an important concept to try and understand, and it's also a key enabler for Nova Sonic.

For those of you that are unfamiliar with Amazon Connect, that's AWS's customer service platform, and Amazon Lex is a builder for conversational interfaces. Now what is a Lex bot? A bot is made up of intents. These are the actions that your customer wants to take, for example, asking about their claim status or what their coverage amount is. Utterances are the way that your customers will express that question, such as "what am I covered for?"

So then how does Lex respond? Lex could respond directly from the intent. For answers that never change, you can put those right in your intent and Lex will respond. It can also respond with a slot. A slot is Lex's way of collecting more information. In that coverage example I shared, Lex might present a slot and say, "did you mean medical coverage or dental coverage?" Finally, if you wanted to do some customization, you can introduce a Lambda. The Lambda will let you do things like attach to APIs, so this will give you an opportunity to create more personalized responses. Going back to that dental coverage example, you'll want to personalize that response so that your customer knows exactly how much coverage they have before they go and visit the dentist. One last point about the Lambda is you can only have one per Lex bot.

Now let's turn our attention to Amazon Connect. Amazon Connect is where you build the IVR experience. For those that haven't seen it, there's a contact flow designer. It's like a canvas where you drop blocks on there, you connect arrows, and that essentially becomes the journey that your customers have on the IVR. If we walk through the flow here that you're seeing, the way we manage the journey in this architecture is through Connect session attributes and contact attributes. When Lex returns something, we use those session attributes, and together we use the results of that to navigate the user across the IVR.

You can see what's happening here. I've got three Lex bots. When a customer calls in, you may want to present the menu bot so that your customer can express their reason for calling. As a result of calling the menu bot, it may be that your customer wants to know what their coverage amount is, so that could then lead us to showing the self-service bot and resolving that inquiry. Another valid outcome from a menu bot could be that we're going to have to put them into a queue so that they can talk to an agent. Then once they're in the queue waiting, we may want to surface the authentication bot and try to collect some more information from them while they're sitting in the queue.

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/1110.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=1110)

Now, you've got lots of options here. You could collapse all the Lex bots into one. These really do become architecture decisions for you on how you want to develop that customer experience. Again, this solution has nice benefits. It does eliminate the menu frustration. It introduces natural language understanding, and there is a structured nature to Lex, so you have very clear outcomes that your customers will get.  With that solution, it does deliver value, but it does leave some challenges when you want to get to a more conversational experience.

### Augmenting Lex with Large Language Models and Model Context Protocol

There's been recent advances in Generative AI that come with some compelling solution options, and we'll get into those. One of the things is as you scale those highly configured experiences that you've built in Lex, it's really nice to be able to augment Lex with some large language models and prompts. In the previous diagram you saw the integration, how there was Lex, the Lambda, and then all your API calls. That feels very tied to your Lex implementation and very use case specific. Salman spoke earlier about what's changed, and he mentioned Model Context Protocol. Now those large language models that we've augmented Lex with, suddenly we can use that to discover tools on an MCP server. The last challenge is more around user experience and how do you manage the state. That's a lot easier to solve with large language models. The orchestration frameworks like LangChain and Strands, they've got hooks to be able to implement session management and conversation history a lot easier.

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/1180.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=1180)

I'll walk you through the solution architecture and I'll share how we can level up the previous  design and get it to be more conversational. In this architecture, Lex is still the gateway. When we spoke about Lex in the previous architecture, I talked about how it's designed and how there's intents and how there's utterances. The idea is that when the customer says something, Lex tries to match it to an intent and then respond.

What I didn't talk about is what happens if Lex doesn't understand. There's another intent called a fallback intent, and that's essentially Lex's catch-all, which allows you to create responses such as "I'm sorry, I didn't understand that. Could you please rephrase your question?" Now, why do I mention that? The reason is that the Lex bot in this architecture has no intents. All we have is the fallback intent. So what's happening is that when the customer is talking, it's going through Lex, it's falling back, and it's going right to the Lambda. That's where the next big change comes in this approach.

We use the standard MCP client SDK, and effectively we turn our Lambda into an MCP client that is capable of calling an MCP server hosted in Amazon EKS. When the process starts, the Lambda will do a fetch. It'll do a resources call on the MCP server, and it will pull back the configuration elements it needs. It'll pull back the prompt that it needs to drive the orchestration. You'll also see in the diagram that I have Amazon Bedrock. That's where we get our large language models. With the prompt that we just pulled, we can suddenly pass that over to the large language model, and the large language model can then discover the tools that it needs and then start orchestrating the authentication flow.

Another thing on this that's new is there's a DynamoDB, and we use that for managing the session and conversation history. We could have just as easily used Amazon S3. Something that's not reflected in here is that LangChain and LangGraph, that's our orchestration framework, and as a result, the persistence layer is implemented using checkpointers that are provided with that.

Now let's look at the MCP server. When we first started doing this, the agent core wasn't available for us to use at the time, and one of the things we were thinking for expediency was let's try to convert a Lambda into an MCP server. But due to the stateless nature of Lambdas and the fact that they die off quickly, we thought not, because MCP protocols operate asynchronously and they are stateful. If we were to use a Lambda, we would have to build our own persistence layer again in DynamoDB or S3 so that we could rewrite those session IDs. So our choice was to containerize because that gives us that permanent long-running mechanism. The other thing is that with the MCP protocol using session stickiness, it all kind of works better that way for us.

Another thing about the MCP server: we use FastMCP and multi-threading to scale the MCP server container, and then Amazon EKS takes care of horizontal scaling when our workloads increase. In the MCP server, you'll see that we've got a couple of servers. We've got one for authentication, so that's where we keep our prompts up there, and then we have several tools like a user ID tool so that we can do a validation of the user ID and an OTP tool so that we can send a one-time passcode to somebody's phone and then validate that. We also have a knowledge base MCP server as well.

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/1420.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=1420)

### Creating Deterministic Agents Through Prompt Engineering and Multi-Agent Orchestration

So let's talk about how this flow is being orchestrated. Before diving into the prompt engineering,  let's talk about what makes a deterministic agent. With our authentication scenario, we need the same predictable output for given input every single time. We can't have the LLM decide a user ID is good one day and then not another, or worse, let someone bypass it altogether. So the agent must operate on a set of predefined rules and make large language model outputs more deterministic by lowering the temperature, structured JSON outputs, and leveraging MCP tools for standard responses.

I mentioned earlier about creating a concierge experience, and to do this we make use of prompts to deliver a multi-agent orchestration. The concierge is also deterministic. It needs to properly understand customer intent and know when to route to a human or hand off to another agent. The concierge prompt has its own persona definition. It's got its own instructions and confidence logic. It needs to know when to route or ask clarifying questions. There's also retry logic.

Once the concierge has done the job of understanding the intent, it may determine that to fulfill it, it needs to call an authentication service. Here we do the multi-agent orchestration and introduce the authentication agent.

This too has its own persona definition and its own step-by-step deterministic workflows. We've all been through authentication. There's a clear sequence of steps. You've got to resolve an ID and then get on to the next factor like OTP, so we have to make sure that the flow we create with the prompt is very step-by-step and deterministic. And of course, there's the tool discovery.

Another neat thing about the authentication agent is it doesn't operate in a dual mode. The first mode is the agent decides the next step, and then in the other mode, it's when Lex collects input via the slots that I mentioned in the previous slide. And because we have a structured JSON output, that's what enables the seamless switching between the two modes. So when you augment the Connect experience with large language models and MCP servers, the options with prompt engineering open many possibilities, and you can take this in endless directions.

[![Thumbnail 1600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/1600.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=1600)

I hope that you saw that there is a path from touch tone to deterministic agents, and along the way there's a lot of value to be had. Before I transition to the next speaker, I just want to thank you for joining, and I hope I've given you some ideas on how you can accelerate AI and transform your own IVR experiences in your contact centers. I'll transition now to Rob, and he'll talk about Content Assist and  another creative use case for MCP. Thank you.

### Rob Chin Introduces Content Assist: Leveraging GenAI for Content Creation

All right, thanks, Dennis. Never would have thought I'd be up on stage talking about content at re:Invent, but content seems to be everywhere. Social media, email blasts, everybody has their Instagram, their TikTok, their Facebook content coming in. Problem with content is it's difficult to create. Everybody hates doing documentation even, but creating content is really tough, and that's where the whole concept of could we make it easier with GenAI. And that's exactly where we're going in terms of how can we make things easier for different use cases for content as we start moving forward.

As Salman mentioned, I'm Rob Chin, product owner of GenAI Patterns, and I started a journey with Salman and GenAI probably around 2023. Back then, there were no AWS GenAI blogs to learn from. There weren't any GenAI services, and now it's probably pretty hard to find a session that doesn't have any GenAI in there or Bedrock. I remember in 2023 when Bedrock started getting released, a Google search just said that Bedrock was where the Flintstones were from. But now we have Claude, so you can ask Claude if you don't know the Flintstones who lives in the town of Bedrock, and you'll get some interesting answers.

[![Thumbnail 1690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/1690.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=1690)

But let's jump into Content Assist. So Content Assist  just started because GenAI is great at generating content. So could we design a solution that could leverage large language models to help us create, score, and even rewrite content as we move forward? And do we have to limit ourselves? Large language models really don't know what kind of content they're creating, so we could do it for marketing, custom summaries, documentation, even responses to RFPs. You could definitely do it if you just kind of guide it, and that was where Content Assist went.

Our solution is powered by Amazon Bedrock. One of our preferred models, and probably a lot of folks in this room, Claude Anthropic is one of the big models for creating content. And the intent of the solution is really to generate high-quality content through precise prompting. Precise prompting, the power of the prompt, everything comes around the quality of the prompt to generate quality output, and we'll be talking about that a little bit later on.

And prompting in terms of content, who knows content the most than the subject matter experts? So as we go through here, you'll start seeing that creating content is really a partnership between the technology team and the business teams, because I know very little about content. But if we can provide the foundation like Content Assist to move this content creation forward, that's where we want to go. For today's demo, I always wanted my own mock company, so I created PurrfectFrame Studios. It's a company that specializes in creating premium cat videos, which everybody loves, so we'll be using PurrfectFrame Studios as we move forward.

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/1800.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=1800)

What you're seeing here is a preview. We'll do the demo later. It's a preview of content that was created for Instagram for PurrfectFrame Studios, and we'll start going through. So Content Assist definitely wasn't developed overnight.  It was a very iterative process.

We started back in 2024 with a few curious users, and the thing I found about generative AI is that curiosity is really what drives moving forward with ideas. We had folks that were using one-line prompts like "create an Instagram post for PurrfectFrame Studios," and it did really well. They were happy with the results, but over time the content looked very generic. It didn't have that brand tone, that brand voice that everybody was used to in creating content.

Then we started working together—the IT team and professional services from Amazon—and we started creating prompt templates that we could hand to the business so they could start pulling in those brand guidelines and brand voice that they knew they were experts in. We started creating these complex, more precise prompts that were really generating amazing content, so amazing that we started getting a lot of use cases coming in. Business groups were filling in these templates, creating these amazing prompts, so we started amassing a lot of prompts that were being shared, that people were using and putting into an internal tool that we had similar to ChatGPT where you would put the prompt in, put your content, and it would generate it connected to Bedrock.

The key to getting good quality content is to be very clear and descriptive in your prompts, and we'll cover some of these prompting styles as we move forward. But as we started getting more and more of these prompts, we created a UX wrapper around it so that if you're a marketing person wanting to create an Instagram post, you would go in, select the categories that you're wanting, and it would pull the predefined prompt from the back end and create the content.

Later on, we thought about where else we could introduce this. From a front-end perspective, we chose a web UI to start with, but you could easily extend this out into other channels whether it's Slack, Microsoft Teams, or even a Microsoft add-in. The key, I think, with any of these generative AI solutions is to build your solution as modular as possible because you don't know where the next technology platform is going to be. We started small doing things in our tool, moving fast, but then we took the prompt, dropped it into a Lambda, dropped it into a UX, so everything was very modular and nothing was lost.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/1950.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=1950)

So I talked about precise prompting, and you're probably wondering what is this magical prompt that we use to get this quality output. I'm just going to walk you through a little bit in terms of things that we considered. You'll see the brand guidelines we have captured  within an XML structure. Using Claude, the model, the XML structure really keeps things organized so that when you start referring to these areas in your prompt, it keeps Claude very focused. So if you can get things structured in the XML format, in this particular case brand guidelines, we want our marketing post to be professional and caring, plain language.

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/1980.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=1980)

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2000.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2000)

We can talk about the brand examples, good examples, bad examples, which are very  key. A bad example would be "if you have a cat with an attitude problem, no worries, we can handle difficult talent," not exactly the best example. But maybe "your cats need some extra time to warm up and that's perfectly normal." So these examples really help guide the models in creating your content. Other guidelines you  can bring in, and as you can see as you start looking through this content, we talked about prompting being the new development language. You'll notice that everything in here is really business-driven from the content experts, so you're starting to see a shift from technical development more into the business side of things. So the business is driving a lot of the content generation logic now, and we provide the foundation and content assist to enable them.

[![Thumbnail 2040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2040.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2040)

In this case, you can include call to actions, anything that in this case a marketing team would want to drive through. It's very iterative—if you're not getting the right output, look back at your prompt and add more details. In terms of the output format for this particular one, you'll see I ask it for  the actual post, but the other one that I ask for is a reasoning, and the reasoning is very key. Because as these models generate your content, they're using your guidelines, but when you're doing the human-in-the-loop review to understand why did it write a marketing post in a particular way, the LLM will actually go through and say "I wrote it because you told me," and a lot of the times it'll actually be spot on because your guidelines weren't specific enough. And then you can go back in and start tuning it, so be specific.

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2070.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2070)

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2100.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2100)

And then lastly,  telling it step by step exactly what you want the LLM to do as you go through. And if it's starting to drift and not follow exactly what you are expecting, just start adding on to your instructions. So you start pulling all of these sections into your overall prompt, and we're having complex prompts now that are like five to ten pages in length, which is giving really good results, but it does create challenges. Complex prompts take longer to run, which  is expected—to get quality, it takes time.

When we had these complex prompts being run, they could take up to a minute in time. Users are impatient. Nobody likes to wait. They start flipping away, reading their emails, going to different chats, and then sometimes they don't come back. To improve that, we looked at a streaming solution so that we could keep the client engaged, and that's when we introduced AppSync, which Salman touched on and Walter will be explaining. Getting that streaming going as soon as Bedrock starts creating the content, you get that scrolling effect, almost near immediate response.

The second challenge we talked about was this huge library of complex prompts. What are we going to do with them now? We had a UI in front of it. The model was, you know, if this then that, if you're doing this post, choose this prompt. And then thank goodness MCP came around, and that really opened up an opportunity to start having a dynamic library of prompts. The concept that we're looking at is putting all your prompts into the MCP catalog, being able to tell the server what you're wanting to do, and it'll choose the prompts on the back end, and you'll get a pretty deterministic output. In terms of if you put in enough detail as to what each prompt does, it'll go through and start choosing the right tools. Salman did touch on MCP being the enabler for building these deterministic agents, and I think this phased approach that we've taken with Content Assist is taking us further down the road to full autonomy. We're not there yet, but it's getting you further down the road.

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2210.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2210)

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2220.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2220)

### From Tools to Autonomy: Understanding LLMs, MCP, and the Future of Content Assist

When you first started talking about LLMs and MCP, it was a little bit hard for me to understand at first because the whole concept of an LLM being able to think like a human. So I'm going to share a simple analogy that I started through, and  it's a little experiment like you can do in Claude AI if you want to kind of understand the connection between LLMs, tools, and MCP.  Everybody knows as you start moving to agentic, it's very goal oriented. You just want to explain what your problem is, and you want the LLM to solve it. So in this particular example, we want to build an engaging social media marketing post for a particular topic.

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2240.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2240)

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2250.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2250)

So we start with giving it a list of tools, and Salman and Dennis had talked about that. So in this case we have brand guidelines,  content examples, social media post trends, and also some random tools, a hammer, a lawn mower, and of course an instant pot.  So we provided this list of tools to the LLM. It knows what it wants to do, and we asked it which tools would you choose. It actually goes through and tells me it's going to take the guidelines, examples, and the social media trends, and it'll actually tell me it threw out the hammer, lawn mower, and the instant pot. So now we know which tools it's selected.

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2270.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2270)

And then I'll ask it how are you going to use these tools  without any instructions, and it'll actually build me a step by step guide as to what it's going to be doing. I didn't have to instruct it at all. Just based on its knowledge and its thinking capability, it told me step by step, I'm going to start with the guidelines, look at the examples, understand the trends, draft the post. It gave me a lot more details and review, rewrite, and presented back with reasoning.

So with that said, you can see how this starts forming your next generation agentic workflow. If you can start kicking off more activities in the back end, maybe you want to create it, maybe you want to start emailing it to a distribution list, maybe you want to put it onto SharePoint, put it into a workflow, you can start creating these different steps. And the beauty of using MCP is as you start adding more tools, let's say I start building a library of how our social media posts have been performing, and let's say I threw that into a simple SharePoint with descriptions on each post, I could actually have a tool that does a standard RAG approach, pull the latest details, what's been performing well, cats in the snow videos if that's trending well. I want to pull that in as my good examples, and I can start creating dynamic examples in my content creation process.

So huge opportunities with MCP, and like I said, building it modularly allows you to drop it in, and maybe some other groups will be using those tools. The LLM will decide and start exposing them similar to the concierge approach that Dennis said. We're going to have a lot of tools in there, and we really want the concierge to help us understand which ones to use. If you take the hotel example, if a new restaurant pops up, they sell pizza, they're going to offer, hey, you know, Rob, there's a pizza shop there, you want to go there. So we can just dynamically expose that, and the LLM, like a concierge, will be able to help through things.

Problem with coming on a Thursday session is there's so much new releases at re:Invent, so we kind of updated some of our slides. For those that attended the Swami session, we talked about Agent Corp. You probably think there's a lot of opportunities in there for Content Assist, and definitely there is. There's semantic search to help find the right tools, policy and evaluations to make sure that you're choosing these agents that are working or doing a good job.

And episodic memories was the one that's interesting. If something went off the rails in one quarter, let's say you created these cat videos that really flopped, the episodic memories will allow us to when we start creating things, it'll remind us that hey, you know what, last year you had a good idea, it failed, don't go there. So all those things that are coming to agentic core is going to be huge, I think, as we move forward with content assist.

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2460.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2460)

So I talked about how all these things come together, but there's a lot that happens in the back end, and Walter's going to talk about the technology and the architecture behind that. And there's a lot of steps, but I'll pass it off to Walter. Thanks. Thanks Dennis and Robert. That was great for that context.  So in this section, we'll do a deep dive into how we use AWS AppSync to accelerate our solution and reduce latency. We'll explore how real-time streaming transformed our user experience, turning long waits into interactive, transparent progress updates. And then we'll walk through the architecture and key design decisions.

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2520.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2520)

### Walter Kurzatz's Deep Dive: Architecture and Implementation of AppSync Streaming

My name is Walter Kurzatz. I'm a senior principal software engineer at Sun Life. I led the engineering efforts behind our AppSync implementation, working closely with our engineering teams to design and deliver a solution that met our robust enterprise requirements. Today, I'll show you how our architecture leverages AppSync's managed GraphQL service, private APIs, and real-time streaming to support complex, long-running workflows. You'll see how we integrated AppSync with Amazon Bedrock, Lambda, and our Model Context Protocol server to orchestrate multi-step processes. So yeah, later on, Rob will walk you through the demo,  and we'll show you how our solution streams the progress updates in real time. And instead of loading a spinner and uncertainty, you'll see updates as they actually occur.

[![Thumbnail 2540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2540.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2540)

The complexity of generative AI workflows and lack of scalable streaming infrastructure were some of the hurdles that we had to overcome.  As we advanced our generative AI initiatives, we encountered several significant challenges that shaped our approach. And as our use cases became more complex, the complexity of context increased dramatically, requiring substantially more time to complete. As Rob mentioned, our second challenge was user experience. Our users are accustomed to instant responses, and with complex reasoning and longer wait times, there was a real risk of users abandoning their sessions.

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2580.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2580)

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2600.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2600)

So let's get started with the architecture overview, and then I'll move into a demonstration so you can see AppSync streaming in action.  So in this slide, I'll walk you through the key components and technologies of our AppSync solution. Our UI was written in React JS, and we are leveraging the Apollo TypeScript client libraries to manage our GraphQL subscriptions, queries, and mutations.  Our networking and hosting, we're leveraging OpenShift and Kubernetes for container and orchestration management. In there, we've implemented a secure gateway to access our Lambda functions. We've implemented NGINX to act as our reverse proxy to manage our WebSocket connections. And then finally, we have our MCP server implemented as a container as well.

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2630.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2630)

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2650.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2650)

 We also are leveraging AWS Identity and Access Management. We have implemented an invoke role to allow us to access AppSync within our network account. And we are using STS to actually do an assume role in that network account, which allows us to get temporary credentials to access the AppSync  itself. AWS Resource Access Manager, as Salman alluded to, we implemented that to enable cross-account resource sharing for AppSync. Our AppSync is implemented in two places, our network or foundation account, also in our private workload account.

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2670.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2670)

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2680.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2680)

 And of course, AWS AppSync itself, it's a fully managed GraphQL service, and for our solution, we're leveraging it  for the real-time streaming. AWS Lambda, our serverless compute, we're leveraging it to implement our backend approach, our backend strategy. So we have three Lambda functions. We have our API Lambda, which we're using to do our assume role in our network account. We're using a data source Lambda, which acts as an intermediary processing our GraphQL requests.

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2720.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2720)

[![Thumbnail 2740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2740.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2740)

[![Thumbnail 2750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2750.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2750)

And then finally we have our Lambda Orchestrator, which is really putting it all together, connecting to SNS, connecting to Bedrock, connecting to AppSync, and of course it is also our MCP host and client.  We have that we're using SNS for secured pub and sub notification. So in this case, our data source Lambda is publishing to SNS and our Lambda Orchestrator is subscribing to that. We're leveraging AWS Bedrock  for the LLM integration as well as the streaming API which we're using to actually write back to sync our mutations. And then finally our Model Context Protocol,  which we're using to host our system prompts as well as our content scoring tools.

[![Thumbnail 2810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2810.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2810)

So let's walk through step one. In step one, the user, the UI sends a request through our secured gateway to the backend Lambda to obtain temporary credentials. These credentials allow our UI to send requests to the shared AppSync endpoint in our network account. The Lambda function uses the AWS STS to assume an IAM role in the network account. The network account's IAM role then trusts the Lambda's execution role and grants the permissions to invoke AppSync. Just as a note here, we are using SigV4  in the headers. This is required for AppSync. AppSync uses this to authorize and authenticate. At the time that we created this solution, there was no other mechanism for authentication authorization, so we were left with using the SigV4 headers.

[![Thumbnail 2850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2850.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2850)

So when our UI connects to AppSync, especially for real-time subscriptions via WebSocket, it must sign the initial handshake using SigV4 headers, and this signature proves the request is authorized using the temporary credentials obtained via the Lambda and cross-account role assumption.  The UI uses the Apollo library and step two uses the Apollo library to connect to AppSync and subscribe to events. It prepares the WebSocket connection signed with the SigV4 headers, and that is passed as a query parameter in the WebSocket URL. The WebSocket connection is established through our NGINX reverse proxy. The reverse proxy accepts our WebSocket upgrade request and then forwards the WebSocket connection to the AppSync endpoint in our network account.

[![Thumbnail 2880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2880.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2880)

[![Thumbnail 2900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2900.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2900)

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2910.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2910)

 The AppSync network, AppSync in a network account then validates the SigV4 signature, the temporary credentials, and forwards the request to our private AppSync instance. AppSync establishes the subscription and enables the streaming of the real-time  data to the client via the WebSocket connection. And then finally, just a little note here, subscription in GraphQL  is a way for the client to receive real-time updates from the server whenever certain events or data changes occur. So instead of repeatedly polling for a specific event or a data stream, it gets notified instantly when something happens.

[![Thumbnail 2930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2930.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2930)

So in AWS  AppSync, in AWS AppSync, GraphQL subscriptions are defined in your schema and typically use, defined in your schema typically use a NONE data source which allows you to execute custom logic or transformations without connecting to a backend. In a multi-account setup, the subscription requests are securely forwarded to a private AppSync instance which processes events and streams real-time updates back to the client over WebSockets. This approach enables instant event-driven feedback while keeping the backend resources protected.

[![Thumbnail 2970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2970.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2970)

[![Thumbnail 2980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/2980.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=2980)

[![Thumbnail 3000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3000.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3000)

And step  step two, I think one of my slides got messed up here.  And step two, the UI uses the Apollo library to connect to AppSync and subscribe to the events. It prepares the WebSocket connection.  I'll just skip to this next section here.

[![Thumbnail 3020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3020.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3020)

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3030.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3030)

The user submits the GraphQL query through the established WebSocket connection. The request is routed through NGINX reverse proxy, and then forwards the request to the WebSocket. AppSync in the network account receives  the query,  validates the SigV4 signature and temporary credentials, and routes the request to the appropriate API and resolver. In this case, the resolver is getContentResponse.

[![Thumbnail 3060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3060.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3060)

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3080.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3080)

The resolver transforms the GraphQL query into a payload for the data source Lambda, and then invokes the Lambda itself. The data source Lambda receives the payload, processes it, and publishes it to the SNS topic fire and forget.  Finally, the LLM orchestrator Lambda is subscribed to that topic, and when triggered, it receives the payload and invokes the Bedrock LLM streaming API.  The orchestrator Lambda then streams the tokens from Bedrock, buffering them and sending the updates back to AppSync as they are generated. So how does this happen?

[![Thumbnail 3090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3090.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3090)

[![Thumbnail 3130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3130.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3130)

 The mutation sendMessage is defined in the AppSync schema and is mapped to use a resolver, which is again configured as a NONE data source. The resolver acts as a correlation ID mechanism to ensure that the right client receives the mutation before posting the response back to the WebSocket connection. Finally, how does MCP fit into our solution? The user requests the social media content providing basic context. The LLM then connects  to the MCP server via Server-Sent Events transport.

[![Thumbnail 3170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3170.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3170)

The server announces what's available, what prompts are available, what tools are available. The Lambda orchestrator then requests a system prompt which instructs the LLM to execute the scoring tool from a list of available tools after the content has actually been generated. The LLM then generates the social media content based on the system prompt, and finally the LLM invokes the MCP scoring tool as instructed by the system prompt. The Lambda orchestrator streams the scoring results back to the UI. 

### Live Demo and Lessons Learned: Bringing It All Together

I think this next step we'll just walk through a bit of a demo, seeing all this in action. All right, thanks Walter. So now you've seen the crazy technical portion in the back and I'm going to show you the easy stuff that I look at, the front end. So for this demo we talked about our fictional cat videography company that offers premium cat photo and video creation service. We come to your cat. You don't have to bring the cat to us.

[![Thumbnail 3210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3210.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3210)

In this particular promotion we're launching a new winter holiday photo pack for clients with multiple cats, 15% off for each additional cat. It's a great gift idea for that cat loving friend that's so hard to buy for.  So this is Content Assist and I'm going to try this thing. The right hand screen is actually the one that's going to be showing the AppSync streaming and the left hand side is the legacy version that we had working.

[![Thumbnail 3230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3230.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3230)

[![Thumbnail 3240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3240.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3240)

[![Thumbnail 3250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3250.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3250)

[![Thumbnail 3260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3260.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3260)

As you can see we've chosen the menus, social media, generate content, we're targeting the US customers  as well as we want to do let's say Instagram, kick it off, see the dancing dots and you'll see the AppSync version it starts streaming as soon as  you send it and then Bedrock starts creating the content and it starts generating it. You'll see the other side start catching up soon. You'll see it does a post, it does the reasoning,  and it's going to keep going because in the new version we've actually have the scoring engine inside the MCP so it's actually decided to  score the content at the end so you'll see it gives a whole category score, brand alignment, grammar, and these are all different scoring mechanisms that you can put in place depending on what kind of content you're looking for.

[![Thumbnail 3290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3290.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3290)

So that's really totally customizable depending on the content. If you're writing, let's say technical content, you may want to score differently in terms of technical terms complexity so it's totally customizable. Like I said before it all comes down to the prompts and how you design them for your  specific business units. Some of these lessons learned from going through this, I know Walter, I come to you with a lot of crazy ideas and you seem to kind of bring the team together to actually solution them.

If you were to look at the lessons learned, what would you say was the biggest challenge that you had to deal with?

Well, I've put a few bullet points in here, but I would say leveraging AppSync keep alives on server side mutations was a big deal for us. Initially we started using AppSync. It didn't really perform the way we thought it should, but then we realized it was really our implementation of it and how we were using it. Making those connections every single time and doing the handshakes was something that you had to leverage just a pool for, connection pools for that. Without doing that and doing all of that work up each and every time, that can be time consuming.

Always be open to new technologies like I talked about before. We started deploying on just doing our content creation within our Sun Life Fast tool, then we built a solution deployed in a container. Now we're looking at .NET Core, and who knows what's going to release next week. It seems to be a new release from AWS every week, so build it modularly so you can move it around wherever you want. I think that's kind of one of the biggest lessons learned. Build something that's cohesive and solid that you can deploy, right?

[![Thumbnail 3380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/c4bf54fb8ad593fe/3380.jpg)](https://www.youtube.com/watch?v=ag2EbZu5gUE&t=3380)

But as you can see, it was a huge architecture diagram. A lot of folks have to work on it. We kind of pushed the limits on a lot of these  different services and scenarios. As you can see, these are a few folks that were key contributors in terms of designing the solution, every part of those boxes. When you show me that architecture box actually, Walter, as we went through the project meetings, I can kind of look at associating the names to each of these boxes. But when it all came together and actually gave that streaming demo, that was pretty amazing. It's free.

So thank you everybody. I know thank you for staying for the last session. We're standing between you and the party, but we're going to be around. I know we had a lot of content to cover. We'll be around if you have any questions on whether it's MCP, AppSync, content creation. We'll be here. Please complete the survey in the mobile app. Don't leave any stars on the table. It's your last session. You can give us five, but enjoy the party tonight. Hope you enjoyed it. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
