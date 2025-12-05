---
title: 'AWS re:Invent 2025 - Tracing the Untraceable: Full-Stack Observability for LLMs and Agents (AIM212)'
published: true
description: 'In this video, the speaker discusses LLM observability using groundcover''s approach, which combines eBPF kernel-level monitoring with a bring-your-own-cloud architecture. The session covers key challenges in monitoring modern LLM applications including token usage, response latency, error rates, and hallucination detection across multi-turn agents and retrieval pipelines. A live demo showcases how groundcover automatically instruments Bedrock APIs without code changes, capturing full request/response payloads, prompts, and token consumption. The platform enables real-time monitoring of model performance, P90 latency analysis, SLA breach detection, and cost management while keeping sensitive data within the customer''s AWS environment. The speaker emphasizes zero-instrumentation deployment and immediate production coverage.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Tracing the Untraceable: Full-Stack Observability for LLMs and Agents (AIM212)**

> In this video, the speaker discusses LLM observability using groundcover's approach, which combines eBPF kernel-level monitoring with a bring-your-own-cloud architecture. The session covers key challenges in monitoring modern LLM applications including token usage, response latency, error rates, and hallucination detection across multi-turn agents and retrieval pipelines. A live demo showcases how groundcover automatically instruments Bedrock APIs without code changes, capturing full request/response payloads, prompts, and token consumption. The platform enables real-time monitoring of model performance, P90 latency analysis, SLA breach detection, and cost management while keeping sensitive data within the customer's AWS environment. The speaker emphasizes zero-instrumentation deployment and immediate production coverage.

{% youtube https://www.youtube.com/watch?v=nCsdKZgwYO8 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/0.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=0)

### Why LLM Observability Matters: Challenges in Modern AI Workflows

 Hey everybody. Thanks for joining in. What we're going to talk about today is probably something that is on the minds of many people around here. We're going to talk about LLMs and observability together, which is probably a very interesting topic for a lot of you, moving into Bedrock, using OpenAI, using Anthropic, using whatever. We're going to focus on how we do it right. So let's start with why LLM observability even matters, right?

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/30.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=30)

I think that probably 99%  of you use LLMs in some form, but why does observability and monitoring them matter? We started, just maybe a couple of years ago, with simple flows of an agent or a model that I query and get a response. Everything was simple. Now we have a ton of complications. Multi-turn agents, multiple steps with different functionalities, using different agents and different models to get to a final outcome. We have retrieval pipelines that enrich, embed, and create a data pipeline up to the actual model at the end. Any silent failure on any of these will screw up the entire process.

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/90.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=90)

We have tool-augmented flows. Our workflows now use scripts, APIs, and different tools that basically comprise the entire workflow of what we're trying to do. Something goes wrong, something has high latency, and the entire workflow suddenly doesn't work. Now you want to observe a lot of different things that you're already thinking about if you're using  LLMs today. The most common things we hear about is token usage. All of these pricing models are connected somehow to token usage and on-demand capacity that you're querying. One million tokens of context just keeps growing. You want to monitor that. You want to know how much your users are using, and you have no control of that because eventually your developers are the ones making choices about how many tokens are being inputted and outputted.

Response latency is another critical concern. AI has gone into production quicker than we would imagine. The value was so high that we just rushed into production. Eventually, your APIs depend on it today. If you're serving a customer-facing API and using AI in the middle, any latency there can impact your overall latency suddenly just because you wanted that value. Same for error rates. If a model suddenly replies with an error, which happens due to structural issues or anything that can happen, and you don't know about it, your API can suddenly go wrong. Before, it was all a box you could control. Now it's getting more complex.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/170.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=170)

The fourth thing, and maybe the most sensitive one, is LLM responses. How do I detect hallucinations? How do I know that the response is even correct? Any of you that tried to query an LLM 50 times for the same thing got 50 different results. Eventually, we want to figure out how to monitor and understand what's going on.  Now the challenges are that the speed in which your stack is changing has never been as fast as now. We've implemented AI into production, skipping security, skipping privacy, skipping anything in your companies to get value into production and to your customers. Anything that prohibited that value was put aside. But right now, you're introducing a new tool every month, from cloud code to anything that is creating code into your production to actual AI used in your workflows to get value to your customers.

The rate in which this is changing is higher than the rate that you can observe and instrument your code and application to even track and monitor what you're doing. I'm sure that 50% of the companies here are not even sure what models you're using right now in production just because of the pace things are changing and things are examined. The second thing is the sensitivity of data. AI creates a very interesting situation where user input can be incredibly sensitive. It can be something that doesn't even depend on you. What happens if your application exposes a chatbot to your end user? They can input very sensitive information about their customers, about themselves, that you might never want in your application, but now you cannot control it, and now you're responsible for it.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/270.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=270)

### Groundcover's Approach: eBPF and Bring Your Own Cloud Architecture

So who gets to upload what, where the data goes, who stores it, which third party am I relying on? That's a big part of the challenges around LLM observability. Groundcover does things very differently around observability in general, but it has a lot to do with how  people should think about LLMs. Most of the people currently using Bedrock, for example, with AWS, have similar concerns. The first thing that Groundcover does differently is that we use a bring your own cloud architecture. Basically, instead of being a simple SaaS model where we store the data of all the monitoring telemetry that you send out and present it to you, we deploy and manage an observability backend on your cloud premises in your AWS account. It's fully managed. It's fully hands-off like a SaaS experience, but you get the value of data privacy and data residency.

You also get full control of what's being stored without us processing it, storing it, or removing it from your environment. We also use eBPF, which we're going to talk about in a second, which is a kernel level visibility to instrument your application at the speed you write them in. If you introduce a Bedrock application tomorrow, it's instantly instrumented without you doing anything. The pace at which things are moving is higher than what you can predict. Those two combined—the data collection with eBPF and bringing your own cloud to store it cost effectively and privately—is what allows us to build experiences with high granularity, which we'll show in a second, of how observability can actually look.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/350.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=350)

So what is eBPF? eBPF is basically a kernel  level capability, a superpower that allows us to look into what APIs and microservices are doing without you instrumenting code. With LLMs and AI, it gets even more important. You can literally cover your production now, any AI involved, without modifying your applications, without even redeploying production, without adding an SDK into your code. You want to get to value very quickly, and you don't want to do that.

Second, we get kernel level visibility. We can actually capture the full API requests and responses, their actual payloads, so we can see the prompts, we can see the responses, we can see what's actually being transmitted. You get the visibility to figure out what's happening. We can translate that to real-time insights because suddenly you have access to token usage, to reasoning paths, to LLM calls and error rates to actually correlate them to logs, metrics, and traces collected outside of your environment, right outside of that LLM application.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/410.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=410)

The bring your own cloud piece  is basically what allows us to do it so securely and privately. What it means is that we've found the balance between a managed service and an on-premises deployment. We separate the control plane and the data plane. The data plane that you can see on the left is basically being hosted in a sub-account in your AWS environment. The control plane that manages the infrastructure, scales it, updates it, and provides the SLA that you deserve from an observability company is being run by groundcover on our side. We have no access to your data. We don't store it. We don't process it. You don't have to rely on us for security and privacy of your sensitive and very business critical data.

That allows us to do a few interesting things. First is protect LLM pipelines from data exposure. You can now capture very sensitive data on what your users are doing with your AI without worrying that this data will be tracked or shipped anywhere. All this data is stored in your premises with groundcover, and there's no third party storage, no outbound communication, no egress of this data to anywhere that you need to be concerned about.

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/450.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=450)

The second is that you can actually use this to improve performance  and manage spend, which, with all the value of AI, is becoming a critical issue that everybody wants to look at. You can look at token usage, latency, and throughputs. We're going to see examples of error rates and things that we're going to dive deep into the demo in a second.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/480.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=480)

The third and most important thing is that you can actually monitor LLM responses with the context that allows you to do  something with it. Maybe I want to look at response payload. I want to see which tools the LLM used or chose to use in the actual application. I want to see a history of the path that the LLM chose to eventually do stuff, and I want to figure out what variance my users are getting from my deployed AI. What's actually working and what's not.

All of this can be deployed in minutes. Install the sensor. It has auto-discovery. You don't have to tell it that you just instrumented Bedrock into your application. It will auto-detect it, and suddenly you will see Bedrock APIs going out of your microservices. They will be monitored and correlated to all the other insights that you collect like log management, infrastructure monitoring, and all the things you're used to seeing.

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/500.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=500)

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/520.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=520)

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/550.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=550)

### Live Demo: Monitoring Bedrock Applications with Real-Time Insights

Jumping into a quick demo just to show you how that might look.  So with groundcover, basically the first thing that you're going to see is all of your deployments and what's running inside the cluster.  You can also see a dependency map of who's communicating to who. In this case, specifically, I can filter by Bedrock and see the shop service, which is a regular microservice running  on my Kubernetes cluster, communicating with the Bedrock service through Bedrock traces that I could actually observe.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/560.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=560)

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/570.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=570)

The second thing that we can look at is actually see our API calls. In this  case, focused on Bedrock. I can see which models I'm using in general, what's the latency, what's the throughput, how many of these are being used. Just to emphasize, anything that you're seeing here, you didn't do anything to get. The sensor just collects it once you deploy a daemon set on your EKS cluster, and you're good to go.  AI information and context is being collected immediately.

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/580.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=580)

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/600.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=600)

Let's look at a cool example. This is the tracer screen in groundcover.  Basically the  eBPF sensor will collect any trace that flows through the system.

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/610.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=610)

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/620.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=620)

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/630.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=630)

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/650.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=650)

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/660.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=660)

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/670.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=670)

 We can focus, for example, on anything that is bedrock related. I want to see what's flowing through the cluster. And I can immediately see flows  that are even wider than Bedrock. In this specific example, I can see a service map of a client communicating to another service, and then I can see Bedrock being executed  in the middle at some point, where AI needs to be involved. The cool thing about it is that I can actually see the full eBPF context of what's happening. I can see the request model that's used, the response model, the tokens that are being used. I can see the chat. I can see the prompt and the response from the server. I can see the raw  request in case I want to figure out exactly what was transmitted, and I can even see correlation to things that you care about like logs. In this case,  shop service was telling me that it's about to create an AI greeting. In this case, it created some kind of greeting with the AI service, and  I can correlate that with the logs and even with the infrastructure monitoring tools.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/690.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=690)

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/700.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=700)

How much is that AI being called from in front of Bedrock? What is the actual latency? What is the CPU of the container? Anything that I want to take a look at. And all of this was correlated at the edge by the eBPF sensor without you thinking about it, instrumenting, or notifying the sensor what to look  at, because eventually you're moving really quick. The other thing we can look at is errors. In this case, this is an example of a Bedrock and Anthropic model that failed,  and I can see the actual prompt and the response that in this case indicates some kind of IAM problem where I couldn't eventually use the relevant model or approach the relevant Bedrock service. Troubleshooting that without observability would require someone from DevOps to figure out what's going on. In this case, a developer that is experimenting and trying to deploy this to production can now see what's going on.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/730.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=730)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/740.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=740)

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/750.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=750)

Another interesting example that we can look at is basically trying to explore this data in ways that can help you  build business context on top of that. We can take a look at our traces in this case, for example, take a look at P90 latency  of any model that we use. So in this case, I can actually take a look over a specific period of time. I can look at which models are  the high latency models that I want to take a look at. In this case, Anthropic is one of the high latency ones. There are a few faster ones that are maybe operating better. Now I can correlate that with token usage. I can correlate that with value, with specific APIs that trigger these models, and I can make decisions.

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/770.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=770)

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/780.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=780)

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/800.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=800)

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/810.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=810)

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/820.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=820)

I can also do the same for token usage. I can  suddenly take a look at any token usage that I have inside the platform and say, I want to figure out which models are consuming the most tokens, what  is the actual usage of tokens across my different models, and I'll make decisions about whether I should really use this model, is this equivalent to the quota that I was expecting? What is my team doing? What are they building? So I can figure out what's going on. We can also do more sophisticated stuff that will eventually allow us to  dive into areas where we care about for actual SLA in front of our customers. Say that I'm using Bedrock, but I also want to figure out  anything that is of duration above one second, because that's the SLA that I promise my customers. Anything that any API that crosses one second  kind of affects my SLA that I guarantee to my customers.

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/830.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=830)

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/840.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=840)

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/850.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=850)

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/870.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/880.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=880)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/900.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=900)

First of all, I can see the actual traces that cause this. I can actually take a look at an example,  in this case, a 1.4 second response from Bedrock, and I can see what's going on? What's the actual flow, but also what's the prompt that triggered that? What's the  context? Why is it taking so long? The second thing that I can do is drill down on this specific filter and figure out if there's anything I should care about. Is there any model that is taking care of that?  Is there any specific token usage that might be triggering that specific latency? Am I seeing that high latency only on very big prompts, very big contexts, or very specific workflows? That is very valuable, just getting into this data and correlating it and figuring out what's going on. That's really hard. I can also take a look  at the relevant workloads. Who eventually is the service that is responsible for any of this? What's creating these queries  inside my specific environment? The second thing I could do is set alerts. Now that you have all this data and you care about SLA and you know that if this specific SLA is breached, you want to know, why not just create a monitor of that, that you can now use to alert your teams and alert yourself that something is going  on.

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/930.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=930)

If there's an LLM SLA breach, I might want to know every time that it crosses 1 second, and I want to get notified. Look how easy that was without you doing anything because the traces were there, the contexts were there, and now you just need to define the business value of what you care about. Anything that revolves around that can now be created into dashboards. The things that are repetitive, that you should care about for a longer period of time, that you want to  even share with your customers—that you want people to be aware of when you're using AI—which models and vendors am I using? What's the latency? How is token usage behaving over time? Am I crossing my quota? Am I in a good state?

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/950.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=950)

How is latency behaving over time and who to blame? Which models am I using? Have I introduced  a new model? Is something moving around? Is it related to a version that I want to focus on? Anything that we've seen so far can basically be baked into monitors, dashboards, and things that can be repetitively used to dive into what you're looking at. Baking that into the context of your production, your dependency map, who's communicating with who, is what eventually can drive the information and the value that you need for your business.

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/970.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=970)

Going back a bit to the start where we were, all of this is basically achieved with a very innovative approach  to how we monitor AI. If up until now you had to instrument an SDK into your code and pick and choose what you want to monitor—I've instrumented the new AI applications into my code, I now need to instrument the functions using it and notify my SDK, my OpenTelemetry SDK or whatever I'm using—I need to notify that this is important for me while my developers are running forward. Now I can deploy an eBPF sensor and get the value immediately.

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/980.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=980)

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/edcbdd32d1a264eb/990.jpg)](https://www.youtube.com/watch?v=nCsdKZgwYO8&t=990)

If before I had to redeploy production or test things at staging and dev, now production is running and it's covered in 30 seconds from deployment.  And also I can store logs, metrics, and traces in the same environment under my bring your own cloud, not worry about data residency, not worry about privacy, not worry about data security, and not worry about creating a new subprocessor that might concern my customers and get all this value. 

Thank you for listening. If there are any follow-up questions, first of all, you can find us in the yellow shirt booth right over there. You can't miss us. I'll be right over here for any questions or follow-ups that come up. We have a playground that you can check out at play.groundcover.com where you can see all this demo and play with it yourselves. I'm sure that it will give way to new questions, so feel free to try it out. We have a free tier and we're over there, and I'm happy to answer any questions. Thank you for your time.


----

; This article is entirely auto-generated using Amazon Bedrock.
