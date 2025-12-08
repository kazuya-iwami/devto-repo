---
title: 'AWS re:Invent 2025 - Building autonomous AI at scale with Amazon Bedrock (AIM390)'
published: true
description: 'In this video, Stephanie Zhao and Sean Eichenberger from AWS, along with Kay Zhu from Genspark, explore building autonomous AI at scale with Amazon Bedrock. They discuss the evolution from AI as a tool in 2024 to AI as a coworker in 2025, highlighting trends like multimodal content creation, agentic knowledge systems, and autonomous workflow execution. The session covers four key challenges in production deployment: performance optimization through model selection and function calling, cost optimization via prompt caching and tiering, reliability through multi-region deployment and inferencing service tiers, and security with Amazon Bedrock guardrails. Genspark demonstrates their Super Agent achieving $50M ARR in five months using a mixture of agent architecture with 150+ tools, 30+ AI models, and 80%+ prompt caching hit rate on Bedrock, reducing costs by 10x while maintaining low latency.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/0.jpg'
series: ''
canonical_url: null
id: 3087951
date: '2025-12-06T02:12:37Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Building autonomous AI at scale with Amazon Bedrock (AIM390)**

> In this video, Stephanie Zhao and Sean Eichenberger from AWS, along with Kay Zhu from Genspark, explore building autonomous AI at scale with Amazon Bedrock. They discuss the evolution from AI as a tool in 2024 to AI as a coworker in 2025, highlighting trends like multimodal content creation, agentic knowledge systems, and autonomous workflow execution. The session covers four key challenges in production deployment: performance optimization through model selection and function calling, cost optimization via prompt caching and tiering, reliability through multi-region deployment and inferencing service tiers, and security with Amazon Bedrock guardrails. Genspark demonstrates their Super Agent achieving $50M ARR in five months using a mixture of agent architecture with 150+ tools, 30+ AI models, and 80%+ prompt caching hit rate on Bedrock, reducing costs by 10x while maintaining low latency.

{% youtube https://www.youtube.com/watch?v=cPru70ADRg8 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/0.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=0)

### Introduction: Building Autonomous AI at Scale with Amazon Bedrock

 Hello, everyone, and good afternoon. Welcome to AWS re:Invent Day 2 and welcome to our session. Before we officially kick it off, I want to start with a quick, easy warm-up question. In the year of 2025, has anyone here not heard of or used a web coding agent or coding assistant? Please raise your hand. Great. The same thing in the year of 2025—has anyone here not heard of or used any agent, either for your personal life or at work? Maybe just booking your entire trip or generating your PowerPoint slide for your presentation tomorrow morning? Please just quickly show your hand. Great, not that many. So everyone is in the right session: Building Autonomous AI at Scale with Amazon Bedrock.

[![Thumbnail 60](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/60.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=60)

 My name is Stephanie Zhao, and I work at AWS on the worldwide Bedrock go-to-market team. Joining me today are our Bedrock service PM Sean Eichenberger and also our customer Kay Zhu, the cofounder and CTO from Genspark. Over the next roughly 60 minutes, we're going to cover seven major topics. First, understanding the autonomous AI landscape from 2024 to 2025 and why it matters. Then, understanding and identifying the four key reasons why prototypes fail to reach production. After that, we'll dive into performance optimization, cost optimization, reliability and scalability, security and guardrails. By the end, we'll hear from a real-world example from Genspark.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/80.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=80)

 So first, let's look at where we are today in this AI journey, as 2025 is fundamentally different from 2024. The year of 2024 is basically what I call the foundation year, where AI served as a tool or as a system. Bedrock has served over 100,000 customers since its launch in 2023. Here are some of the top use cases that define the trend of 2024. The first one is content generation, from a piece of internal documentation to external-facing emails or blog posts. But please notice that it's always AI-suggested, brainstormed, and drafted, but human-edited, decided, and approved.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/120.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=120)

 The second use case is customer support. We have seen huge adoption of customer support, from first-line query handling and FAQs to ticket routing. But it always requires a clear escalation path and handoffs to human agents. The third one is data analysis, where we leverage generative AI for automated insights, anomaly detection, and pattern recognition. But again, AI identifies the patterns, while humans decide what they mean and what actions to take. Number four is search enhancement, which improves the user experience for enterprise applications through semantic search and smarter ranking algorithms. But fundamentally, it's an improvement of the existing system.

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/130.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=130)

 Let's look at what are some of the common patterns of the top use cases. Number one is human in the loop—for every AI decision, humans verify and approve it. Second is request and response mode, where each interaction is discrete and stateless. Your system does not have the background or knowledge across prior sessions. The third is single-task focus, where your AI tools are hyper-specialized. You have one tool for customer sentiment analysis and another tool for image generation, and they operate in silos. Last but not least is supervised execution, which is where the fundamental limitation lies—limited autonomous initiative. Overall, it's reactive, not proactive.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/270.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=270)

### Three Defining Trends of 2025: From AI Tools to AI Coworkers

 Now let's look at what's happening in 2025. In 2025, AI stopped being a tool and became a coworker. Here are three distinct trends that define this transformation in 2025. Trend number one is that we evolved from single-format content generation to a unified multimodal content creation workflow. In 2024, you probably still needed three different models, tools, and platforms for your text, image, and video generation. There was still no guarantee of consistent branding across all of them.

In 2025, a single agent can execute the entire creation workflow with consistent branding across all formats. Let's take a global marketing campaign as an example. Your AI agent, not a human creative director, creates video spots and generates social media imagery that matches your video perfectly. It writes marketing copy with consistent brand voice across all content formats. By the end, it adapts everything into 12 additional regional markets with different languages and cultural contexts. What is different and critical here is that your AI agents understand the context across all formats.

Trend number two involves evolving from retrieval augmented generation to the agentic knowledge system in 2025. Retrieval augmented generation in 2024 is fundamentally passive. You ask a question to your system, it retrieves relevant documents from the knowledge base, synthesizes the insights, and gets back to you. It's useful, but it's reactive. In 2025, the agentic retrieval augmented generation is able to monitor information sources proactively, synthesize insights across domains, predict patterns, and proactively send relevant information to key stakeholders.

Let's take a legal research assistant example. Your legal research agent proactively monitors all law case changes and synthesizes insights across different law domains. It flags any potential precedent conflicts even before you're aware of them and generates legal arguments with all citations. What is different and critical here is that it continuously builds your knowledge graph, predicts what additional information is needed, and preps for the research proactively.

Trend number three involves evolving from conversational interface to fully autonomous workflow execution. The chatbot in 2024 manages scripted conversation within certain boundaries. It's great for customer support and frequently asked questions, but it's inadequate for complex and multi-step problems. In 2025, the autonomous agent can execute the entire workflow, coordinate across multiple backend systems, and make decisions based on your organizational policy end to end.

Let's take the most popular example, the customer service agent. When your customer calls in for a complex problem, your agent coordinates with your inventory system to locate the right product and interfaces with your shipping system to manage and order the delivery. It communicates with your billing system for any charges and refunds as needed. By the end, it proactively communicates with your customer for each step. Everything is proactive, not reactive.

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/540.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=540)

What is driving this transformation? We see the evolution from AI as a tool to AI as a co-worker. This is not philosophical; it's definitely technical.  The foundation model's capability has improved across multiple key aspects, enabling everything we just discussed. Here are some examples. The first is context, which has exploded from the early days of only 32K tokens to 1 million and now up to 200 million tokens and even more. The difference here is basically moving from analyzing one chapter of your legal document to processing the entire 500 pages of your contract without losing any details.

The second capability is planning. Models now gain autonomous planning capability where they can automatically decompose any complex goals into actionable subtasks independently. The third is tool integration. Tool integration has evolved from fixed and hardcoded APIs to a more dynamic context where tool search and selection occur automatically.

In the past, we had to configure which API to use for wider data and which different API to use for stock prices. But now, models can dynamically discover which tools are available and determine the best combinations to solve these problems most effectively. They can orchestrate this tool chain without preprogramming any possible combinations. The last but not least is reasoning depth, where the model evolved from prior, more black box statistical pattern matching to now being more transparent and explainable with chain of thought reasoning. You can literally see each logical step from the model.

### Specialized Agents and the Rise of Super Agents

We see all these model improvements serve as the technical foundation for technical advancement. But organizations don't just deploy foundation models. They deploy specialized agents for different businesses. Let's look at some of the top adopted specialized agents in 2025. The first one is the coding agent, which offers key capabilities such as pull request review and code writing. You can describe your desired functionality in natural language, and the agent generates working code with error handling and testing.

Document agents handle multi-format extraction, automatically extracting structured data regardless of input format. They also handle automated data entry, which is a table stake for many industries such as healthcare, financial services, and insurance. Customer service agents offer real-time customer sentiment analysis and 24/7 multilingual support, as well as automatic issue resolution. The last but not least, sales and marketing agents conduct A/B testing at scale and offer hyper personalization and predictive analytics.

All these specialized agents are great because they're optimized for specific domains, but they cannot work across different domains. That's where the general purpose agent becomes essential, or as you may have heard it called, the super agent. Fundamentally, a super agent is the orchestration layer sitting on top of the specialized agents. Here are a couple of the core capabilities the super agent brings.

The first one is multi-agent orchestration, where the super agent coordinates across multiple specialized agents to solve complex and cross-domain problems. Each specialized agent does their own part of the work, while the super agent ensures they all work in harmony with proper sequencing and not in silos. The second one is unified context management, where the super agent shares your knowledge estates across all coordinated specialized agents to ensure there's no information loss during handoffs between specialized agents.

The third is task planning and delegation, where the super agent handles intelligent routing of subtasks across the appropriate specialized agents. Last but not least is conflict resolution and decision arbitration. Sometimes your specialized agents might disagree with each other and generate conflicting recommendations. That's where the super agent arbitrates based on your organizational policy, priority hierarchy, and risk parameters.

If we want to make this architecture more concrete, you can think of the super agent as your enterprise AI chief of staff, who manages multiple different specialists. One does meeting scheduling, another works on document prep, another handles follow-up and task monitoring, and then another handles executive briefing. They all work together. We now have a pretty comprehensive review of the autonomous AI landscape. We know autonomous AI is great and it's here, but enterprises are still struggling. Let's hear from Sean about why this happened and how to avoid it.

### Performance Optimization: Model Selection and Function Calling Best Practices

Thanks, Stephanie. Yeah, enterprises continue to struggle to bring agents to the market. There are a few core struggles that we see emerge.

One is performance failures, which often stem from either wrong model choice or developing for a really narrow set of inputs that just don't capture the breadth of what you'd see in a production environment. We also see lots of cost overruns. Model selection plays a role here, as well as some of the prompting optimizations that you can put in place. Whether they're there or not, they can have a big impact.

Reliability and scalability issues we often see when there's not enough done to optimize for failure modes, and there's no robust error handling and no robust mechanisms for failover to other models should there be availability concerns. And lastly, we see privacy challenges. Enterprises need to be able to control the inputs and outputs. They need to ensure that PII isn't leaking. They need to ensure that the model is behaving in a way that's compliant with the regulations that they face.

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1000.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1000)

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1010.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1010)

So we start by tackling  a few of the ways that you can address some of these issues. First, we're going to talk a little bit about performance optimization. When we're talking about performance in  this specific context, I'm really referring to speed, which is also called latency. Specifically, time to first token, time to last token, and end-to-end latency. Which latency is important to you just depends on your use case. Another aspect of this is intelligence. Is the model capable enough of handling the nuance that your use case has? Is it calling the right functions?

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1050.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1050)

Oftentimes what we see  is that customers start their journey by choosing a model before even assessing the use case and the match for the model. So one of the big important things you can do to optimize your performance is select a model based on the use case, not based on the model itself and any sort of acclaim or brand recognition that's around it. The guidance is you want to start small and then work your way up as needed.

Most customers will actually find that a smaller model than what they're using is perfectly sufficient for their use case. So if you think of something like a summarization use case, which might be used within an agentic workflow, you can go with a very small model, which is going to save you cost and save you latency. That's certainly something to consider.

As reasoning capabilities become standard, many customers tend to flock to it and utilize it even when it isn't necessarily appropriate. And so you're going to end up paying a latency tax and also an increased number of tokens just to use reasoning even if it doesn't necessarily enhance your accuracy. With all that said, you want to develop a set of evaluations that really measure how impactful reasoning is, how impactful the size of the model is, and test it against a wide variety of inputs.

You want to make sure that the inputs are reflective of what you're going to see in production and not just a narrow subset of what you think might be best case scenario. This will pay dividends down the line as long as you can establish up front a framework for evaluating models that's specific to your organization. You can rinse and repeat over and over again as you add new use cases and new agents.

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1200.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1200)

If you're not able to find something off the shelf, you obviously have the option to  fine-tune a model on your own data and bring that in. So Bedrock offers a custom model import.

[![Thumbnail 1240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1240.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1240)

You need to think about the trade-offs here though. There is upfront time and cost that goes into fine-tuning, but on the other hand you are going to get an efficient, well-aligned model for your use case. So let's say that you've now got your model and you're deep into the process of creating  the agents that you're going to be shipping. It's really important to optimize your function calling. Your function calling is the core of all of your agentic workflows, and there are really five categories that I recommend using to orient yourself around how to optimize.

One is you want to define clear contracts. Specifically, you want to be explicit about the schema and intent, and you want to be very descriptive with the parameters that you're giving your functions. For example, if I have a get weather function, which takes two parameters—one is a temperature unit which could be Celsius or Fahrenheit, and the other is a city—you don't want the parameters to just be named something like parameter one or parameter two. You don't want them to be some abbreviation. The model needs to have completely unambiguous meaning, so you're better off having the wording be very explicit like user city and temperature unit to make sure that the model is going to properly call the function.

In addition, where possible, you're going to want to constrain your arguments. So if in the example we just talked about, you've only got two options for temperature unit—Fahrenheit and Celsius—make it into an enum and make it more explicit for the model so that it has a better chance of calling with the correct arguments. You also want to design for natural language. You want to align the names of the functions with the customer intent. Let's say I have a customer that wants to summarize emails, which is an agent use case I'm working on. Well, I want to have the function name be something like summarize emails. I don't want it to be reflective of maybe an existing backend system that orchestrates across multiple functions like something like stream messages or summarize content.

You don't want to have functions that are not semantically similar to what the query is that the customer is sending in. The other factor here is that you want to have, wherever possible, a single function orchestrating on behalf of the model rather than have the model do all the orchestration of many individual micro functions. This reduces the ambiguity and reduces the chance for error. You want to guide the invocation logic and you want to give a very high signal prompt. The high signal prompt should be guiding when exactly and what are the very narrow parameters under which you want to call a function. That's going to improve your odds of calling the function and making sure that it's called with the right parameters as well.

Additionally, you want to optimize context, and what we mean by that is you want to keep just the bare minimum context that you need in order to actually execute on the task at hand.

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1540.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1540)

Lastly, you want to instrument everything. You want to make sure that you are logging your function calls, logging the parameters, the failures, and the corrective actions that the model might have taken. You want to use all of that to inform how you might tweak your prompts in the future to deliver a better experience in the long run. 

### Context Management and Cost Optimization Strategies

Beyond just the function calling, there are more generally some optimizations that you can make around context management that are going to help you with latency and costs. One of the key strategies that you can use is compression. Oftentimes you don't need all of the prompts that came before in previous turns. You can compress them, you can summarize them, and you can strip out unnecessary portions of the context like verbose language that's already in the prompt.

These kinds of optimizations are going to save you on tokens, which means that you're going to have fewer tokens that need to be processed before your output starts to generate. This results in faster time to first token and lower costs because fewer tokens means lower costs. You're also going to reduce the chances that stale context is going to misdirect the model to call the wrong function.

Another strategy you can use is prioritizing context. Over time, some context is going to get stale, and you can just remove that context and call without the context from many turns ago. There are multiple libraries that help with the prioritization and summarization, including open source options like LangChain and LlamaIndex.

You can also focus on dynamically budgeting your prompts. Using the token counting APIs, you can call them as you're structuring your prompts to check where you are in the overall context window and start adjusting based on that. Lastly, for any repeated context that you're going to use across model calls, such as a system prompt defining all the behaviors that you want your agent to exhibit, if that's going to be a fixed portion of your prompt every single turn, you can benefit from caching, which will save you on latency and cost.

[![Thumbnail 1740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1740.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1740)

As an aside, if you have a use case that's more customer facing, there's a perceived latency benefit that you can get with streaming. So I'm going to talk a little bit about  cost optimization, and that brings us right back to model selection. As I said earlier, a lot of customers will select a model that's either too large or unnecessarily costly for their use case. So start with the use case and not the model. It will pay dividends over time. Scale the model only as needed and make sure that you're evaluating at every stage, even in production.

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1780.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1780)

The other side of this is that you can trade off your latency to get some cost savings.  If you have a less latency sensitive use case, there are different tiers, and I'll let Stephanie talk a little bit more about this in a few minutes, but there are different tiers like flex tier that would give you some savings as long as you're willing to trade off on latency.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1820.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1820)

As I mentioned, prompt caching works when you have an exact match  to an existing prefix in your prompt, such as a system prompt. As long as it matches syntactically and semantically, you will get a cache hit and save on cost. The reason you can save on cost is because there's less compute associated with actual processing of those tokens. You're pulling the intermediate attention states from memory rather than having to recompute them every time. You're also getting a latency benefit because you don't have to spend as much time on the prefill aspect of LLM calls. You can skip forward straight to the generation part.

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1880.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1880)

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1900.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1900)

### Reliability, Scalability, and Security: Multi-Region Deployment and Guardrails

Now you have optimized your performance and your cost. Let's make sure your AI system stays up and scales peacefully.  We always consider multi-region deployment as fundamental and critical for your enterprise-scale AI systems.  There are three key reasons here. Number one is high availability. Today, Bedrock service is available within 36 global regions, including GovCloud. With the implementation of cross-region inferencing, it has largely improved the availability of the models across the world.

The second reason is latency optimization. The intelligent routing behind the scenes prioritizes not only the availability of each model but also tries to fulfill your inference request from source regions and then seamlessly reroutes to a closer region to minimize latency impact. The third reason is regulatory requirement. Beyond the global cross-inferencing endpoint, we also introduced the US, EU, Japan, Australia, and GovCloud regions in order to meet your data residency requirements.

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/1970.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=1970)

Now let's talk about the inferencing options on Bedrock today.  We recently introduced inferencing service tiers in order to meet your different requirements for your different workloads. There are four different service tiers available. The first one is what we call the Reserve tier. Basically, customers are able to reserve prioritized compute for any mission-critical and production workloads. All your Reserve tier requests will be prioritized among the others. It offers a target 99.5% uptime and gives you the flexibility to provision input and output tokens independently to meet your request shape. It also automatically spills over to on-demand during peak traffic.

The second one is the Priority tier. If you don't want to reserve capacity ahead of time but still want to make sure your critical requests are served as soon as possible at a request level, the Priority tier gives you prioritized compute compared with standard on-demand. The third one is what we call the Standard tier, which is basically the default on-demand inferencing for real-time generation applications. The last one is what we call the Flex tier, which is the cost-efficient way for your less urgent workloads where you're able to take a few seconds up to a few minutes of latency but at much lower cost. Think of use cases such as model evaluation, content generation, summarization, and non-real-time, multi-step generative workflows.

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2070.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2070)

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2080.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2080)

And here is the last topic: security and guardrails.  This is what we always emphasize for any in-production workload. That's the reason Bedrock is built from day one with data privacy and security.  As with other core AWS Foundation services, we have been emphasizing that no customer data is used to train the foundation models or shared with model providers. Your fine-tuned models are encrypted and saved in a security container which is only accessible to you. In addition, Bedrock has also achieved over 20 complex standards for you to meet your regulatory compliance.

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2120.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2120)

The Amazon Bedrock guardrails help customers apply safeguards for your generative AI applications. There are six different policies  or safeguards you can apply, including the content filter, the word filter, deny topics, and others. We also expanded the capability so you're able to apply the safeguards not only at your AWS account level, but at your AWS Organizations level across accounts for all model invocations.

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2170.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2170)

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2190.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2190)

### Genspark's Journey: Real-World Implementation of Autonomous AI Workspace

In addition, the guardrails now also apply to any of your coding use cases. You can apply the safeguards to detect any undesirable content within coding elements, and we support 12 major programming languages. Now that you've heard a lot about autonomous AI and about Bedrock, let's hear from Kay Zhu. Thank you, Stephanie.  Hi, everyone. I'm Kay, cofounder and CTO from Genspark. Let me do a very quick quiz here. Has anyone here heard of or used Genspark before? Please raise your hand. Not a lot. So I'll do a very quick introduction of ourselves. 

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2250.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2250)

Genspark was founded in November 2023, and we launched our Super Agent suite this April. After launching our Super Agent, we received tremendous user feedback. We reached $50 million ARR by the end of September, and we are recognized by Forbes and entered the OpenAI trillion token club. Last month, we announced our second round of funding, which is $275 million led by Emergence Capital. The key insight from this journey is that the world is ready for truly autonomous AI agents, and Genspark is real-life proof. We are very proud of reaching this milestone of $50 million ARR in five months. 

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2270.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2270)

We also achieved $1.25 billion in valuation in 20 months. We thank all the users who love and use Genspark. For Genspark, our mission is very simple. We want to enable the three-day workweek for more than one billion knowledge workers.  We are working very hard on that. This is our launch calendar. During the past six months, we launched more than 19 products, almost one new product every week. We work very hard and everything is done almost entirely by AI agents and AI coding tools. More than 80 percent of our code is written by AI tools.

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2330.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2330)

As a software engineer, I'm super excited to bring the cursor of cloud code-like experience for software engineers to regular white-collar workers. That's what Genspark is doing. Last month, Genspark launched our AI Workspace.  For the Genspark AI Workspace, it is composed of three components. One is collecting information, the center is processing information, and the end is generating outputs. Let's talk about each one. For collecting information, Genspark can collect information from both the digital world and the physical world. From the physical world, Genspark has features like AI Meeting Notes and CareFM that gather information from the physical world. For the digital world, it collects data from public data sources and also your private data through your AI Inbox or AI Teams.

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2400.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2400)

For the output, Genspark implements a set of sub-agents specified for different kinds of outputs. For example, AI Slides, AI Sheets, and AI Documents, the standard office suite, and also AI Developer and AI Designer, the standard AI employee set that you can use in Genspark. In the center is the Genspark Super Agent. It coordinates and organizes all the sub-agents and all the Genspark features together to help users finish their work end to end. Let's see it in action.  Imagine I'm in the apparel business.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2410.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2410)

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2420.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2420)

[![Thumbnail 2430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2430.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2430)

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2460.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2460)

This is our order database in Supabase.  It contains more than 50,000 orders in this database. For every order, it has the time, all the detail information, category, and so on.  Now, we can use Genspark to actually analyze it. Let's first start with a very simple analysis. This is Genspark Super Agent. I will ask,  what was the sales share by the subcategory in October 2025? When you input that, let's see it in action. Genspark will use MCP core to connect to the Supabase and issue a SQL query to it. And this is the AI Sheets output. You can see it's totally compatible with Microsoft Excel, and it will generate the table compatible with Microsoft, with all the right Microsoft format and also the right formula. 

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2480.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2480)

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2490.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2490)

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2500.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2500)

[![Thumbnail 2540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2540.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2540)

Let's try a more complicated example. This is another one. I will ask a more complicated question. For each sales channel, identify  the categories and the SKUs, which are the top performers, defined by the growth rate comparing 2024 to 2025.  For this more complicated one, Genspark will issue more MCP calls, doing all kinds of computation, writing all the formulas,  writing in the Excel format, and this is the result. For every subchannel or categories, we have the top performers. But data itself is not enough for action. So we can ask Genspark to give us the action plan of the analysis. We can ask Genspark to analyze the subcategory and product selection strategy by channel. After another round of analysis, you can see Genspark will create the subchannel analysis, create a new tab of the spreadsheet, and we can have everything together. 

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2550.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2550)

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2580.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2580)

You can see for the mobile app, for the eBay channels, all the action recommendations, and also the cross-channel strategic insight.  The Excel is very good to communicate inside your team. You can directly send this Excel to your operating teams for further action and discussion. But you may not want to show it to your boss or to your clients directly. Now, Genspark all-in-one workspace actually comes to the rescue. So for Genspark, you can just ask it to create a five-page channel strategic deck answering where to play  and how to win. And Genspark will collect all the information together and use the AI Slides sub-agent to generate a boardroom-ready deck for you. This is the deck generated. All the charts and all the graphs are computed in Excel, and the characters and the numbers will be absolutely right. And then you can even have the implementation roadmap with it.

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2620.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2620)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2640.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2640)

[![Thumbnail 2650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2650.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2650)

Let's push it a little bit further. What if you don't want to present it by yourself? What if you cannot be here? Let's ask Genspark to generate the audio with the slide.  Using the voice your slides feature, Genspark is actually using the AI Pods capability to generate the audio for each slide. Now let's hear the result. Welcome everyone. So this is our executive briefing on channel strategy. We've focused on two fundamental  questions, right? Where to play and how to win. So let's start with where to play. Our performance analysis reveals a really compelling story.  We have two channels, eBay and our mobile app, that are just significantly outperforming all the other channels. This is where our opportunity is. So we're going to double down on what works. And this naturally leads to how to win. We're going to implement a proven winners strategy. Obviously, Genspark can generate a presentation better than I do. So maybe in the future I will just ask Genspark to do the presentation for me.

[![Thumbnail 2680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2680.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2680)

### Genspark's Technical Architecture: Less Control, More Tools, and Prompt Caching Excellence

Let's come to the technology part  and the design principle behind the Genspark AI agent architecture. We have the principle to be less control and more tools. Let's talk about this one by one. For less control, we are actually fully embracing the agentic engine. We actually convert from the traditional rigid workflow to the adaptive agents. That's very important for us because the traditional RAG-based search actually uses a fixed workflow that often breaks on edge cases.

By shifting to an agentic engine that plans, executes, observes, and backtracks in real time, we observe much better results. The agentic approach is more robust to errors because in traditional workflows, all errors accumulate. In agents, errors are recoverable.

Another philosophy we believe in is providing more tools. We have built more than 150 tools that can be composed like Linux pipes, where one tool's output can be another tool's input, as you saw in the demo. All these tools have network effects because each new tool creates exponential combinations with the existing ones.

Some fundamentalists believe you only need to give an agent a large language model, one computer, one mouse, and one keyboard—nothing more. However, we don't believe in this approach. Imagine two software engineers: one with only a brand new Mac and internet connection, and another with a laptop pre-installed with all critical apps, menus, and documents. Obviously, the latter will perform better. That's what we believe in—more tools.

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2820.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2820)

Let me explain the Genspark Agentic engine very quickly.  The Agentic engine has two components: the orchestration layer, which puts everything together and makes it work, and the self-improvement layer, which ensures everything works better and better as users interact with it. For Genspark, the top layer is the orchestration layer. We combined more than 30 AI models in a mixture of agent approach, more than 150 tools, and more than 20 datasets curated in-house.

The bottom layer is the self-improving layer. Evaluation is key. We use large language models as judges to evaluate all agentic executions and attribute session rewards to single steps. This way, we accumulate a lot of data feedback and learning experience. All this learning experience is internalized through reinforcement learning and prompt playbooks. As users interact with Genspark more and more, Genspark's performance gets better and better.

[![Thumbnail 2900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2900.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2900)

Let me discuss the mixture of agent architecture.  In agent work today, some tasks are very demanding and hard—none of the frontier models can answer them perfectly. However, you can ask these demanding questions in parallel to multiple frontier models, record all their outputs and thinking traits, and then use an aggregation layer with an aggregator model to combine everything together. By reading through all outputs and thinking processes to synthesize a better result, we discovered that we can reduce hallucination significantly and achieve much better final results.

[![Thumbnail 2950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2950.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2950)

[![Thumbnail 2980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/2980.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=2980)

Regarding AWS infrastructure, Genspark's image studio uses many GPUs, and by using AWS elastic infrastructure, we save costs by 70% and can scale very easily as we experience growth in the Japanese market.  Amazon Bedrock is also very key to our agent infrastructure.  As Tiffany and Sean mentioned before, Bedrock has very good performance-optimized infrastructure, multi-region deployment, and enterprise-grade guarantees. Bedrock also has very good prompt caching optimization for Genspark.

[![Thumbnail 3010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/3010.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=3010)

I will talk a little bit about prompt caching for Genspark. Prompt caching is actually an understated  problem that is mentioned but not really discussed by a lot of people. Genspark actually achieved more than 80% prompt caching hit rate on Amazon Bedrock, and it helped us reduce costs dramatically—a 10 times reduction in cost—while also maintaining very low latency. The way to achieve that is by applying an append-only context design. Just as shown in the right graph, each time, whether it is a tool calling or a user input, we always append the context to the bottom of the context window. This guarantees that we have a stable context prefix.

[![Thumbnail 3070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/3070.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=3070)

But what if the task is too complicated and actually uses up all the context window? You can use two ways to fix that. One is context compacting, and another one is context isolation.  For context compacting, it is triggered in a garbage collection type of way. When you have used up, for example, 70% or 80% of your context window, you can trigger one round of context compacting. It will summarize all the important information, compress large outputs, and offload them to the file system while maintaining critical decisions and session goals. By this way, you can free up a lot of your context window using the context compacting technique.

[![Thumbnail 3150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/3150.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=3150)

Another way is using context isolation. Just as you saw in the previous demo, Genspark Super Agent actually uses AI Slides and AI Sheets as sub-agents. All these sub-agents take care of all the context details about how to operate on a slide or how to write a formula in the spreadsheet. All these contexts will not pollute the lead agent, and the lead agent will just orchestrate what to do with the sub-agents. Finally, we also thank AWS a lot for the startup program, which helped a lot for Genspark. 

[![Thumbnail 3160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/3160.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=3160)

This is a short summary of my part. We talked about less control, more tools—the design principle  of Genspark Super Agent—the agentic agent, the orchestration layer, the self-improving layer, the mixture of agent architecture, and the prompt caching and context engineering techniques. That is all for my part, and please feel free to try Genspark at Genspark.ai. Thank you.

[![Thumbnail 3190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/3190.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=3190)

[![Thumbnail 3200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/3200.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=3200)

[![Thumbnail 3240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/68acc8daf8f2b6ff/3240.jpg)](https://www.youtube.com/watch?v=cPru70ADRg8&t=3240)

We have talked about the evolution of agents.  We talked about the common struggles that customers have with performance, cost, reliability, scalability, and safeguards.  We have also talked a little bit about how you might optimize for each of these, and then you saw the proof in Genspark and how these optimizations can lead to impressive results. I just want to say thank you very much for taking the time to chat with us this afternoon. 


----

; This article is entirely auto-generated using Amazon Bedrock.
