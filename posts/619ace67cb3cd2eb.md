---
title: 'AWS re:Invent 2025 - Enterprise AI: Now, Next, Future (ANT202)'
published: true
description: 'In this video, Chris Hillman from Teradata and a representative from NVIDIA present their collaborative solution for an automotive manufacturer facing long R&D cycles. They demonstrate how vector embeddings using NVIDIA''s Nemotron services combined with Teradata''s native vector store enable semantic search of specification documents while processing terabytes of real-time sensor data from test tracks. The system uses AWS Bedrock for private generative AI, allowing junior engineers to access insights previously available only to senior engineers. The Nemotron models achieve 50% more accurate results than open source options, parsing 30 pages per second per GPU. They discuss future development toward AI agents using frameworks like Crew and LangChain, emphasizing the importance of guardrails models and MCP protocols. The solution addresses tool overload through task-specific MCP servers and supports BYOM (Bring Your Own Model) via ONNX format for hybrid cloud and on-premises environments.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Enterprise AI: Now, Next, Future (ANT202)**

> In this video, Chris Hillman from Teradata and a representative from NVIDIA present their collaborative solution for an automotive manufacturer facing long R&D cycles. They demonstrate how vector embeddings using NVIDIA's Nemotron services combined with Teradata's native vector store enable semantic search of specification documents while processing terabytes of real-time sensor data from test tracks. The system uses AWS Bedrock for private generative AI, allowing junior engineers to access insights previously available only to senior engineers. The Nemotron models achieve 50% more accurate results than open source options, parsing 30 pages per second per GPU. They discuss future development toward AI agents using frameworks like Crew and LangChain, emphasizing the importance of guardrails models and MCP protocols. The solution addresses tool overload through task-specific MCP servers and supports BYOM (Bring Your Own Model) via ONNX format for hybrid cloud and on-premises environments.

{% youtube https://www.youtube.com/watch?v=ffy7G4lK94M %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/0.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=0)

### Accelerating Automotive R&D: A Case Study in AI-Powered Engineering Efficiency

 Thanks for taking the time. I need to apologize for being fluffy—it's cold in here and I was wearing an AWS jacket, so I'm all covered in fluff. That's not my normal appearance, but anyway, I'm Chris Hillman, Global AI Lead for Teradata. I'm joined by someone from a small company you might have heard of called NVIDIA. I lead strategic product partnerships and I'm really excited to work with the Teradata team. We're very excited to show you what we built together. Chris, why don't you kick it off and we'll go from there.

Sure. I have a big team and we work in the Americas, EMEA, and APJ. We've been doing great work recently with some of the biggest companies because of the nature of our platform and the huge data sets involved. I've picked out one case to talk to you about today. It's actually related to automotive manufacturing, but hopefully you'll see this is a pattern that's really applicable across all industries, and you'll be able to see why that's going to be relevant to you.

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/90.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=90)

We're going to go through what the problem was, what we did, and then Nima is going to talk about how we did it. After that, we'll discuss ideas for how we're going to build this in the future. The case is for an automotive manufacturer.  The issue they have is that development cycles are really long. Traditional manufacturers are stuck in maybe a four to five year cycle, and there's so much going on—so many different parts to put together, literally and figuratively.

One of the issues they're trying to fix is R&D efficiency. Newer manufacturers, particularly some of the Chinese manufacturers, have gone down to a two-year development cycle. They're bringing new products to market much quicker. The R&D side of this is hugely complicated because there's the engineering side, things like efficiency and safety. But also, one of the issues they have is the level of expertise needed to do this work. An automotive engineer takes a long time to reach the point where they can actually make decisions, because everything has to be super safe, obviously. That's particularly what they wanted to focus on: can we speed up that R&D process using AI? They particularly wanted to make this a generative AI project. Could those junior engineers still be as productive as the more senior ones?

The starting point for all of this was the specification documents. A huge amount of documentation is created when they're developing these new models. That would normally be searched manually, and it contains everything—tolerances, benchmarks, and all these kinds of things, expected levels of performance. By taking those documents and turning them into vector embeddings, which is where we use the Nemotron services that Nima is going to talk more about in a minute, we're taking a complex data type and turning it into a vector of numbers.

Once everything's been converted into vectors of numbers, we can store them because Teradata has a native vector store. We can store them and then they can be searched semantically, which is the key thing. Semantic search means you're searching on the meaning, not on older methods like TFIDF. Some of my data scientists call that brutal methods, but it used to work, just not nearly as well as what works now. So we take the documentation, put it in the vector store, and then at massive scale you can search this stuff for meaning and similarity.

That's half of the story. The other half is that they have a very sophisticated test track that they run these cars around during the testing phase. That test track is measuring hundreds of things on sensors with data streaming off the car. You've got things like brake temperature, battery drain, battery load, and many vibration measurements. Hundreds of data points are coming in, and in real time, you're talking about terabytes worth of data for every run these cars make.

That data needs to be processed, and time series signal processing is an established technique that's been around for a while. The process of smoothing, resampling, and similar operations can now be done directly in the database. At the end of the day, you end up with a system that has unstructured data and structured data in exactly the same place, and that's where the real magic happens.

Using a large language model, you can query that in real time and get answers that combine both sets of data. This is where our partnership with AWS was key, because the research and development data is extremely sensitive—it's as sensitive as you can get because it cannot get out to competitors. We were using the cloud, but there's no way you could use a public cloud service because that would mean sending this data back and forth. Using AWS Bedrock, we implemented what someone termed "private gen AI"—everything stays within your network and you're not sending anything out elsewhere. AWS was key for that capability.

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/430.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=430)

Given that infrastructure, you can now ask questions like what anomalies were there in this run, and the system can look and say the temperature on the brake disc was too high at a particular point. You can then ask why that happened, and it can look at all the other sensors and specifications and determine, for example, that you were going too fast or the brake pads are worn or the disc is worn because it's looking at the vibration simultaneously. So given all of that together, we can answer questions like which parameter is responsible for the worst fuel consumption when comparing simulated results with this specification compared to the previous specification. 

The system can actually answer that it's because the vehicle speed threshold for the 7th to 8th gear upshift in the gear shift map has been increased by 5 kilometers. Given that insight, junior engineers can now access the same information that senior engineers knew, and they can begin to work on these problems and be far more efficient with the way they work. Looking more closely at how we did that, I'm going to hand over to Nima to talk about the Nematron service.

### Technical Architecture: NVIDIA Nemotron Models and Enterprise Vector Store Implementation

Thank you, Chris. The Teradata Enterprise Vector Store is powered by NVIDIA's Nematron models, and this one in particular is composed of two specific workflows. The workflow on top is the end-to-end RAG solution—retrieval augmented generation—which is the ability to generate embeddings, vectorize them, store them, retrieve them from a vector database, re-rank the results, and run them through a reasoning engine if you want before outputting to a customer via an API or whatever interface you use. You can also put guardrails in front of it.

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/490.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=490)

The lower workflow is the extraction engine, which is the ability to take unstructured data in any format and parse it through, then decouple and split text-based embeddings from image-based embeddings.  In this particular workflow, we have a PDF parser, and these are all composable as microservices. You can swap out any of these components for whichever component you'd like to use. If you have a better PDF parser, or a multilingual or multimodal one, or a newer version comes out, you can swap these out in situ and the overall pipeline will continue to operate.

In this example, the way they've set it up is that documents as they come through the PDF parser get split between infographics on top versus text-only on the bottom. Different modules and different retrievers are able to deconstruct images, charts, graphs, and bar charts into a text-based format so you can store those directly into the embeddings. Therefore, every bit of metadata or document data is transcribed and available in a text-based format. What's really powerful about this is that the current Nematron models and Nematron OCR will generate about 50 percent more accurate results compared to other open source options. In its current configuration, it will roughly parse through about 30 pages per second per GPU.

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/640.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=640)

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/650.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=650)

You can transcribe about 1.3 million pages of documents per week per GPU, and that will scale linearly. All of these components are completely configurable. Over time, as we build better versions of the Neotron Nano, Super, and Ultra models and the retriever, reranker, and guardrails, they can be swapped out in situ by the customer directly by end users. This enables the ability to generate better responses. So over time, the quality, throughput, and speed will all improve from the current baseline.  With that, let me turn it back to Chris and we'll move on to the next topic. Thanks, thanks, Nima. What I haven't shown here is the time series processing of the sensor data, but that's all happening in the database, so that's not part of this final system. That's kind of preprocessing that goes along with it. 

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/710.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=710)

### From Augmented Intelligence to Autonomous Agents: Building the Future Infrastructure

Given what we've done, we've obviously been looking and talking a lot about what's coming up and what's in the future, and the answer has to be agents, doesn't it? That's the answer for the future for a lot of us. Looking at how this might work, this is what I would call augmented intelligence. Although this is something you couldn't have done three years ago, you could have done it in different ways and it wouldn't be anywhere near as accurate. Moving on and looking more at automation, how would that work? Building on top of that, I like that definition of an agent: a wrapper for an LLM that provides a frictionless automated experience.  You've probably heard lots of different variations on what agents actually are. But for us, to go from that kind of chatbot to something automated, there has to be a bill of materials around it. There are a lot of other things you have to get involved in to go for full automation.

Obviously, you need the agents themselves and some kind of agent framework, so Crew, LangChain, whatever. There are loads of them. That will typically be running in a Python server. A lot of people are looking at low code applications, and we are obviously ourselves. That's a nice way of bringing in maybe the engineers themselves into this process, because they're not going to be writing Python. You need the large language model itself, and that's what we're talking about with Bedrock in this instance. Guardrails models are just going to be so important, and if you're looking at anything to do with agents at the moment, you really need to research guardrails models.

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/850.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=850)

Every prompt that an agent outputs and every result it gets back needs to be checked because there are lots of scary things that can happen, and hallucinations are the least of the problem. So guardrails models are super important. You need the comms protocols. We've gone for MCP and we've got our own open source version of an MCP server which connects to Teradata. You need the knowledge platform itself, which is the Teradata system, and then other tools because there are always other things you're going to need like web search, file search, or an application that needs controlling. 

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/900.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=900)

If you look at the diagram on the right, the bits that are colored in are the bits that we do, but there's a whole ecosystem around it that needs to be built out if you're going to turn from augmented to automated. Another way of looking at that is here along the bottom you've got the tools that you might need. I always like to think, and I think it was actually Crew AI that came up with this: agents, tools, and tasks. The agents have a job to do. They've got tasks they need to carry out and they need tools to complete those tasks. The kind of tools you've got on the bottom, you need access to the data source, you need access to the external data in the data fabric, and you need the analytics themselves. For us, we have stuff built straight into the database because a lot of these functions that we would use in an AI/ML world aren't set-based SQL, so we need these parallel functions built in that you call from SQL. 

BYOM is a key part of this. It stands for Bring Your Own Model. So if you create a model in pretty much any system, if it can be exported to the format called ONNX, the Open Neural Network Exchange, then we can import it and we can run it.

We can also use other functions like time series processing. When you look at that middle layer, the MCP, the reason there are multiple ones of them is there's a problem with something called tool overload. As you add more tools to that MCP server to prevent the agent from hallucinating and using the right tool for the right job, you risk confusing it because when the LLM creates the query, it might pick up the wrong tool if there are too many of them. Our system has quite a lot because we have such huge capability.

One way around that is to create tasks or specific MCPs for the things they're going to do. If you think you might have a load of DBA tasks, things like reindexing or archiving tables, you could create a specific MCP server and restrict it to just those tools. This takes away some of that tool overload problem. If you're just querying, you might have a query tool and not have all the other stuff around it. The agents themselves sit on a separate layer in the Python server.

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/1010.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=1010)

That's where we're going with this, and it may be that the system will continue being augmented. It may be that the agents are finding all the anomalies and suggesting solutions while somebody else makes the final decision, or it might be that they end up being completely autonomous.  Unstructured data is an untapped goldmine. You need vector stores to work around that, and using services like Neumatron, the throughput you're talking about now is quite incredible to get the PDF shredded.

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/619ace67cb3cd2eb/1050.jpg)](https://www.youtube.com/watch?v=ffy7G4lK94M&t=1050)

Don't forget hybrid environments. A lot of companies we work with are still using on-premises infrastructure as well as cloud. That doesn't matter to us because it's the same software running everywhere. Since a lot of the innovation is happening in the cloud, you still need that kind of service.  Focus on building expert agents. If you scan that QR code, you'll get more information or come see us on the Teradata booth. Thanks very much.


----

; This article is entirely auto-generated using Amazon Bedrock.
