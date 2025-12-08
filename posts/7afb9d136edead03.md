---
title: 'AWS re:Invent 2025-How enterprises are deploying PolyAI''s voice agents at scale to reduce handling..'
published: true
description: 'In this video, Michael from PolyAI explains how their voice AI agents help enterprises save labor hours equivalent to 1,000+ FTE. He presents a PG&E case study showing 67% call resolution and 25% reduction in customer effort by migrating 7,000 intents to their Gen AI solution, handling 16 million calls annually. The presentation covers three key pillars: AI agent capabilities (ensemble speech-to-text models with NVIDIA, multimodal engagement), platform features (observability, simultaneous drafts, Amazon QuickSight dashboards, Bedrock-powered analytics), and operating model (forward-deployed engineers, solution consultants). Built on AWS with Gen AI competency partnership, PolyAI operates 2,000 AI agents across 100+ enterprise customers, emphasizing that enterprise-scale deployment requires optimized speech recognition, architectural flexibility, and comprehensive collaboration tools beyond just intelligent LLMs.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025-How enterprises are deploying PolyAI's voice agents at scale to reduce handling..**

> In this video, Michael from PolyAI explains how their voice AI agents help enterprises save labor hours equivalent to 1,000+ FTE. He presents a PG&E case study showing 67% call resolution and 25% reduction in customer effort by migrating 7,000 intents to their Gen AI solution, handling 16 million calls annually. The presentation covers three key pillars: AI agent capabilities (ensemble speech-to-text models with NVIDIA, multimodal engagement), platform features (observability, simultaneous drafts, Amazon QuickSight dashboards, Bedrock-powered analytics), and operating model (forward-deployed engineers, solution consultants). Built on AWS with Gen AI competency partnership, PolyAI operates 2,000 AI agents across 100+ enterprise customers, emphasizing that enterprise-scale deployment requires optimized speech recognition, architectural flexibility, and comprehensive collaboration tools beyond just intelligent LLMs.

{% youtube https://www.youtube.com/watch?v=zOGMWM5HQ_0 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/0.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=0)

### PolyAI's Enterprise-Scale Voice AI: A PG&E Case Study

 Mike Check, cool. Hi everyone. So yeah, I'm Michael. I'm VP of Strategic Alliances here at PolyAI, and thanks for spending a bit of your morning with us. The title of the talk is how enterprises are deploying PolyAI's voice agents to reduce handling time by over 75%. I'm going to suggest flipping that a little bit. I mean, handling time, everyone talks about that. The objective really is to save labor hours, right? And so what I'm going to talk about today is how PolyAI, what our enterprises are sort of looking at to do, to have an AI agent that can truly take on the work of about 1,000 FTE. What does that actually look like, right?

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/50.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=50)

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/70.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=70)

 So I'll talk a bit about us. I'll go through one of our Fortune 500 customer examples, specifically gas and electric, and how we're deploying an AI agent with them. And I'll talk about a few of the capabilities we see as important to have these AI agents operating at truly enterprise scale. So a little bit about PolyAI. We were started in 2017,  working with large language models and transformer-based architectures, specifically for enterprise customer service as the use case, right? Our three co-founders, Sean, Eddie, and Nicola, were researchers at the University of Cambridge studying dialogue systems and speech recognition systems. They really wanted to find a way to apply that invention of the transformer model, but in a way that could touch the daily lives of everyday people, and they found that in customer service and contact centers.

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/130.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=130)

Fast forward to today, we have about 2,000 individual AI agents now running in production for over 100 enterprise logos. We've raised over 120 million in venture capital from investors such as Nvidia. We've been recognized in this year's Magic Quadrant in conversational AI platforms, and we're now about 280 to 290 people spread out across London, San Francisco, and New York, right?  To talk quickly about us and AWS, so we're built on AWS. We're available on the marketplace. We're one of the Gen AI competency partners as well as other competencies, and we have multiple live deployments already with Amazon Connect across the US, UK, and Europe. We also do work with other contact center platforms. We work with our friends at Zendesk as well. So yeah, just a little bit about us and PolyAI.

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/160.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=160)

 One of the things that we hear time and time again from industry analysts and from our investors is we really stand out in terms of the enterprise scale deployments that we have running in production. So I'll talk about one today, which is our deployment with PG&E, right? For a utility company like PG&E, there's no interaction that's more important than an outage or a billing conversation, right? Before PolyAI came around, what that looked like for the customer is maybe over an hour of wait time during very unpredictable periods, during very sensitive periods. These are periods where actually the utility needs to get things right. There's a lot of regulation around communication to the customer, and they were struggling with a legacy Nuance IVR that was very difficult for them to make changes to and adapt, right?

So what I'm going to do is actually play a very short clip of our customer and the solution, and I'll talk a bit about then the impact. Hi, thanks for calling PG&E. I'm Peggy, PG&E's virtual assistant. How can I help? Hi Peggy, I think my bill's due, and can you tell me how much I owe? I can give you balance information over the phone, or I can tell you how to check your account balance online. Which would you prefer? Well, definitely over the phone. Great. Your current balance, including your upcoming payment plan installment, is $29.

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/260.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=260)

 Very short clip. I'll give you the link to the full video in just a little bit, but I want to talk quickly about then the impact, right? That was our customer there, Kristin Punter, calling the solution live and in production. What we helped the customer do there was migrate 7,000 intents that were built into their legacy IVR over to a Gen AI solution, resulting in about 67% call resolution and about a 25% reduction in overall customer effort. Now, during a period of outage, being able to scale that AI agent about 50x within five minutes to deliver uninterrupted service netted about a 22% increase in CSAT.

And just to show you what does a deployment actually look like, right? I'm not going to sit here and say we did that all on day one, right? This is a journey, especially at the enterprise scale, especially in regulated industries. We started with outage notifications.

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/340.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=340)

Once the customer was comfortable with the level of performance, you grow that and add use cases until now you're on track to about 16 million calls per year. Every time you add a use case, there are always unexpected challenges with voice AI. There's different tuning that you need to do around speech recognition. There are different intents, knowledge base, or RAG issues that might pop up that you need to address. So it is a journey, but it's one that we've gone on successfully for multiple Fortune 500 companies. So yeah, this is a little link to where you'll be able to find the full video  talking from our customer about our impact.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/350.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=350)

### Building Enterprise-Grade AI Agents: Speech Recognition, Customer Engagement, and LLM Orchestration

So now then to touch on what does it actually take for an AI agent to do the work and be capable of doing the work of over 1,000 staff.  We view this challenge in about three main lenses. Firstly, it's the application, the AI agent itself. That's about the customer experience, the brand experience. Second, it's the platform itself, so what is it for the developer, what about the contact center manager, what is their experience when you're operating at a scale of over 1,000 staff. And then finally the operating mode, what's the vendor experience, and what's the experience around the continuous improvement of the solution. So I'll talk a little bit about each of these pillars.

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/380.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=380)

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/410.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=410)

 In the first pillar with the AI agents, when we looked at the performance of our solutions in aggregate, we found that about 70% of the time, poor experiences delivered by a generative AI solution was actually down to errors in the speech to text, errors in the transcription, not necessarily that the AI wasn't smart enough to figure out the answer. It was given an incorrect signal from the speech recognition engine.  So for us, what does enterprise grade look like for spoken language understanding? Through our partnership with NVIDIA, we're building, and we have built, an ensemble of speech to text models. So think of a model for US speakers, a model for UK speakers, a model for zip codes in the US that's different to the model for postcodes in the UK that's different to the model for first names in Croatia. So being able to really optimize speech to text is really, really important.

Being able to switch models, boost certain words, create guardrails anywhere in a conversation is also very important. So, you know, it is limiting if you're only able to adapt your speech recognition at very specific points in the solution. Voice activity detection that automatically through machine learning adapts to the caller is also critical. There's nothing more frustrating than that whole interruption game that happens when you're sort of offbeat a little bit with an AI agent. So having our own voice activity detection capabilities there has been very important to scale to enterprise. And then finally with AWS, an architecture that can scale instantly, so auto scaling. And like I mentioned in the utilities, in the outage use case, being able to 50x the call volume within a matter of minutes.

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/490.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=490)

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/520.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=520)

 Secondly, in the AI agent, how do you get customer engagement? It doesn't matter how intelligent the AI agent is if customers are not willing to engage. Certain information is great through a spoken conversation, but other forms of data are also important. We don't always give information by speaking. We might give information by sharing videos, by sending forms, spelling, sharing locations and photos. And so for us enterprise grade customer engagement  is the ability to move into multiple modalities while maintaining voice as the primary. So being able to send forms in the middle of a conversation without ending the phone call, so the phone call is still live, you're sending a form, you're waiting for the customer to complete that form before you continue the conversation. You're giving the customer an ability to share photos, share videos, share location pins. So that's all very important when it comes to voice AI running at scale.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/550.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=550)

 Thirdly, around the AI agents, the LLM layer. What does enterprise grade LLM orchestration look like? So we have our own LLMs. It's called our Raven models trained on top of AWS, and we're currently in the process of surfacing those models through SageMaker through Bedrock. But having LLMs that are trained on your specific use case, so that have been fine-tuned to the style of a contact center conversation, of a customer service conversation, is really important for more natural, lifelike, engaging responses. Being able to trigger function calls, integrations anywhere in a conversation, either synchronously or asynchronously, that is a very important capability when it comes down to solutions that can handle operations at scale.

Having dedicated instances for inference, so not sharing instances and having one dedicated to the LLM and to the speech recognition layer, and finally having a level of architectural flexibility. Enterprises rightly are very concerned about where data is being processed and where data is being hosted, and so having a level of architectural flexibility is something that we also offer our customers.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/630.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=630)

### Platform Capabilities and Operating Model: Observability, Analytics, and Forward-Deployed Support

Next, to go into the platform, before you can run an AI agent at the scale of thousands of people,  you need a level of observability. Just to show an example of what we offer to our customers, core metadata is fairly standard. We also offer custom metrics, these are business metrics, context states, so being able to track what is happening in the conversation and the history. A conversation, being able to track exactly the inputs and outputs given to LLMs at every step of a conversation through every function call, and finally transcriptions and recordings.

We also allow the developer to dig into the full LLM request if they need to for debugging and for troubleshooting, and we also provide visualizations of latency. When an AI agent is taking a bit longer to respond, it's important to understand whether that latency is coming from the speech recognition, from the LLM, from the way that you've written your function calls, or from the text-to-speech provider. That's all very important for troubleshooting and debugging.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/690.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=690)

 Next is the collaboration experience. When you're operating with an enterprise, there's never just one person that is looking after the solution. You're going to have IT folk, contact center folk, customer experience folk. There'll be multiple team members that are working on a solution and evaluating a solution concurrently.

Think of this as like a maturity scale. The very first thing, the most basic thing that you need to be able to do, is accommodate different users, different levels of permissioning, different environments, so nothing gets pushed straight into production. It's important to be able to offer that on the AI agent platform that you're working with. Now when you have different users and you have different environments, what you're then going to need is an audit trail. What changes are being made, what version of the agent is live in which environment, who made that change, why did they make that change, being able to append notes. That was sort of stage two for us as we were going on this journey.

Stage three, which we're very excited to be launching, is then not just having an audit trail of a single version of the solution, but now having simultaneous drafts and a merging process. Everyone can be working on the AI agent concurrently, and there is a way to merge different drafts together without conflicting and without destroying functionality and without losing capability. That's been a very, very important change that we're introducing into the platform.

The ultimate goal that we see, the end result here, is then once you have simultaneous drafts, you have the ability to manage conflict and merging, you have audit trails, you have different environments, those are the unlocked steps to having a native developer co-pilot. You don't need to be a developer. You don't necessarily need to be writing Python to build a state-of-the-art voice AI agent. You should be able to prompt a co-pilot, a Cursor-esque, Cloud Code-esque experience. But before you can do that, you need all these other pieces in place, and that's what we're offering to our enterprise customers.

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/830.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=830)

Next, beyond observability is analysis, analytics, insights from the agent.  These are just screenshots of some of the capabilities that we offer. There is an ability to through natural language create the analytics that you want the AI agent to be tracking. This is an ability to implement custom metrics that are important for different types of business operations.

Down the bottom here, yes, we provide dashboards. Actually, we use Amazon QuickSight as our dashboarding solution. But we are also now enabling for the business user a natural language way to talk with their conversational data, to talk with their contact center essentially, and we're unlocking that through a smart analyst type of capability. Here we're leveraging Bedrock and the Anthropic models for this type of use case. I think the new paradigm will be a more conversational, shall I say, natural language way for you to interact with the millions of phone calls that are now being transcribed and flowing through your contact center platform.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/900.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=900)

 And finally, I'll just touch on quickly,

there's a lot of talk around forward-deployed engineering as the model. We've touched on agents, we've touched on platform, and now this is about the operating model. What does it look like when you're having an AI agent take over the work of a thousand people? You need a different type of AI operational layer. It's not just about forward-deployed engineers, although they are very important.

We also offer to our clients our expertise in the architecture component with solution consultants. We have our own product solution managers that are helping the enterprise think about the improvement cycle of the AI agents that have expertise around whether this should be a fix in the speech recognition, in the LLM, in the text-to-speech. We also have our own dialogue designers that are working with enterprises to implement their brand experience into an AI agent.

That's all the content I had prepared for you today. Again, having an AI agent to truly take on the work of a thousand FTE or more of staff, you need to think about a lot of new capabilities from the AI agents, from the platform, and from the operating model that you have with your vendor and with your technology providers.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7afb9d136edead03/990.jpg)](https://www.youtube.com/watch?v=zOGMWM5HQ_0&t=990)

We've got a few minutes left. If you do have a question, I'm happy to answer it on stage now, or I'll be around after this conversation. Do we have any questions from the audience for PolyAI that I could answer? Just  raise your hands. This is your time to get one-on-one questions answered. Don't be shy.


----

; This article is entirely auto-generated using Amazon Bedrock.
