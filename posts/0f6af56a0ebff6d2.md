---
title: 'AWS re:Invent 2025 - Create hyper-personalized voice interactions with Amazon Nova Sonic (AIM374)'
published: true
description: 'In this video, Veerdhawal Pande and Ankur Gandhe introduce Amazon Nova 2 Sonic, a speech-to-speech foundation model for real-time conversational AI. They demonstrate new capabilities including support for seven languages with masculine and feminine voices, language switching within sessions, asynchronous task completion, cross-modal text and speech input/output, and turn-taking controllability. The presentation highlights significant improvements in speech understanding accuracy (50% better on alphanumerics), state-of-the-art ASR performance, and enhanced tool calling capabilities. Ankur explains the architectural advantages of the unified speech-to-speech model over traditional cascade systems. Key use cases include customer service automation, voice assistants, and education apps. Integration options through LiveKit, Pipecat, Amazon Connect, and telephony partners like Twilio are covered. Amma Pandekar from Cisco demonstrates an AI receptionist application for a tire chain, showcasing real-world implementation with appointment booking and multi-modal dialogue capabilities.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/0.jpg'
series: ''
canonical_url: null
id: 3087938
date: '2025-12-06T01:46:20Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Create hyper-personalized voice interactions with Amazon Nova Sonic (AIM374)**

> In this video, Veerdhawal Pande and Ankur Gandhe introduce Amazon Nova 2 Sonic, a speech-to-speech foundation model for real-time conversational AI. They demonstrate new capabilities including support for seven languages with masculine and feminine voices, language switching within sessions, asynchronous task completion, cross-modal text and speech input/output, and turn-taking controllability. The presentation highlights significant improvements in speech understanding accuracy (50% better on alphanumerics), state-of-the-art ASR performance, and enhanced tool calling capabilities. Ankur explains the architectural advantages of the unified speech-to-speech model over traditional cascade systems. Key use cases include customer service automation, voice assistants, and education apps. Integration options through LiveKit, Pipecat, Amazon Connect, and telephony partners like Twilio are covered. Amma Pandekar from Cisco demonstrates an AI receptionist application for a tire chain, showcasing real-world implementation with appointment booking and multi-modal dialogue capabilities.

{% youtube https://www.youtube.com/watch?v=o92D2x2Zfw8 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/0.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=0)

### Introduction to Amazon Nova 2 Sonic: A Speech-to-Speech Foundation Model for Real-Time Conversational AI

 Before we get started, let me introduce myself. I'm Veerdhawal Pande, Principal Product Manager for Amazon Nova Sonic as part of Amazon General Intelligence, and I'm Ankur Gandhe. I'm a Principal Scientist also working in AGI and focusing on the Nova Sonic models.

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/30.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=30)

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/40.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=40)

 Today we are gathered here to talk about Amazon Nova 2 Sonic, our speech-to-speech foundation model for real-time humanlike conversational AI.  Before we dive deeper, let's quickly go through the different topics that we intend to cover. First, we'll be talking about the product itself and the core features that we have enabled, key benchmarks, speech-to-speech architecture, and major use cases that our customers are using Nova Sonic for. Later on we'll cover developer tools which make it easy for you to integrate Amazon Nova Sonic, and then we have a special guest today who will give us a demo and speak to the product they are building with Amazon Nova Sonic. After that we'll open the floor for question answering.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/80.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=80)

With that, let's get started  with Amazon Nova 2 Sonic. At Amazon, we are at the forefront of building voice-enabled technologies for conversational AI, whether it's for Alexa or other services such as Amazon Connect for customer service call automation. Yesterday we heard Matt Garman speak in his keynote about Amazon Nova 2 Sonic. We launched this model which enables real-time humanlike conversational AI. It's our second generation of model which went into general availability yesterday on December 2nd. It supports speech and text input and output modalities, and it's backed by a bidirectional streaming API as part of Bedrock to enable low latency conversations. It's available in four regions: IAD, PDX, ARN, and NRT with more regions coming soon.

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/160.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=160)

### Core Conversational AI Features and What's New in Nova 2 Sonic

The most important thing here is that this model not only enables smooth conversational AI, but it also offers today's industry's most competitive quality and price performance offering. Let's look deeper into some of the conversational AI features. Accurate speech understanding is the foremost and most fundamental thing  for any kind of conversational AI application. We are proud to say that Amazon Nova 2 Sonic delivers best-in-class speech understanding with robustness to different speaking styles, accents, and even noise in the background.

As I mentioned, it supports bidirectional streaming API with low user-perceived latency. This is critical for us to not only process streaming audio at speed, but also generate responses which seem natural and fluid in conversations. The model is capable of handling fluid dialogues not only when it comes back with its responses, but even when users interrupt at will, take pauses, or hesitate. These nonverbal cues tell the model to let users finish their thought before starting to respond.

Another key aspect of this model is its ability to detect not just what the user said, but how they expressed it—the tonality, the voice-based sentiment—and adapt its own responses based on that. This unlocks the ability to empathize with users when they express frustration or elate when they express happiness and mirror the overall sentiment of the user. However, any conversational application is incomplete for enterprise use cases unless it can actually be used to make and complete tasks. That's why the model also delivers tool calling, knowledge grounding with responsible AI, and privacy compliance. What this means is you can generate responses backed by a knowledge repository to be factually correct or invoke other tools to accomplish tasks such as making a reservation or upgrading your membership.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/280.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=280)

Now, most of what I spoke about until now was also part of Nova Sonic One. So what's new?  Let's take a look at what are the new conversational AI capabilities that we have unlocked with this launch of Nova 2 Sonic. First, additional languages. We initially started off with English back in April, added a few more languages in the July and August timeline, but now I'm proud to say we have seven languages, each with its own masculine and feminine voice supported by Nova 2 Sonic. We will dive into more details with each of those.

Next, we have language switching, the ability to not just speak in one language, but switch languages at will within the same session, and the AI can come back and respond in the user's preferred language. Another cool feature is asynchronous task completion. As we just talked about tool calling, APIs can take anywhere from 8 to 10 seconds or even longer. Instead of blocking on those tool responses, the Nova 2 Sonic model enables conversations fluidly where the user can switch topics and continue to talk or even invoke other tools while the previous tool call is working in the background.

Next, we have enabled cross-modal input output. This was a key ask from customers who started to adopt Nova Sonic earlier, where they would like the same model to be able to handle text and speech input and keep the conversational context together. This unlocks new features which we'll dive deeper into. And then of course, turn taking controllability. This refers to the ability for the developer to configure the duration of the pause that the user can take before the AI determines it's an end of a turn. Let's dive deeper into each one of these.

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/390.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=390)

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/420.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=420)

### Deep Dive into New Capabilities: Multilingual Support, Language Switching, Asynchronous Tasks, and Turn-Taking Control

As I mentioned, we have now launched 7 languages: English with all different variants  US, GB, India, and Australian. Indian and Australian English are the new additions with Nova 2 Sonic. We also have Spanish, French, Italian, German, Hindi, and Portuguese. Now, as I mentioned, each of these languages not only depicts the ability to understand these different languages and sub-dialects, but also the ability to respond in expressive native sounding voices. Let's take a moment  to listen to one of these examples.

Let me play what we have here for US English masculine voice, Matthew. Welcome to Any Bank. I'm your AI assistant and I'm excited to help you with anything you need. Want to check your accounts, learn about loan options, or explore some investment ideas? Just let me know. I'm here to make your banking experience easier and a whole lot more enjoyable. Okay, let's try some new voices here. Welcome to Any Bank. I'm your AI assistant and I'm excited to help you with anything you need. Want to check your accounts, learn about loan options, or explore some investment ideas? Just let me know. I'm here to make your banking experience easier and a whole lot more enjoyable. That was Australian English.

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/540.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=540)

Any Bank may apka swagat hai. Main apka AI assistant hoon aur apki madad karne mein mujhe khushi hogi. Chahe aap apne account dekna chahte hain, kisi loan ke bare mein janna chahte hain. That was male for Hindi. And then lastly, let me try one for Portuguese. Bem-vindo ao Any Bank. Sou seu assistente de IA e estou muito feliz em ajudá-lo com qualquer coisa que você precise. Quer verificar suas contas, aprender sobre opções de empréstimo ou explorar algumas ideias de investimento? Apenas me avise. Estou aqui para tornar sua experiência bancária mais fácil e muito mais agradável. Welcome to Any Bank. I'm your AI assistant and I'm excited to help you with anything you need. Want to check your accounts, learn about loan options, or explore some investment ideas? Just let me know. I'm here to make your banking experience easier and a whole lot more enjoyable. Okay, lesson learned. Let's take  a moment.

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/570.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=570)

Thank you. That was quite a lot of expressive voices. Coming to language switching, it's not only important to be able to speak expressively and with native sounding voices in a given language, but also the ability to switch within the same session and speak different languages with the same voice. And that's what we have unlocked with the polyglot voices and language switching feature. Let's take a moment to see how the user was able to interact in two different languages and the voice stayed the same while responding expressively in either.  Hey, how are you? Hey there, I'm doing great. Thanks for asking. How about you? What can I help you with today? I want to buy shoes. That's an excellent idea. Good shoes make everything better. You'll want to think about the style you need and make sure they fit comfortably. Happy shopping.

Ich will Turnschuhe kaufen. Das ist eine ausgezeichnete Idee. Gute Schuhe machen alles besser. Sie müssen über den Stil nachdenken, den Sie benötigen, und sicherstellen, dass sie bequem sitzen. Viel Spaß beim Einkaufen.

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/640.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=640)

As you saw, the user was able to switch between two different languages within the same session, and the voice was able to adapt to  either and respond back. I mentioned earlier a new feature called asynchronous task completion. As you will see in this demo, the user invokes a request which triggers a tool call in the background. While that tool call is happening, the user is able to switch the context and the topic and have a conversation fluidly while the AI comes back and responds to the previous tool call.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/670.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=670)

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/680.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=680)

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/690.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=690)

While you are looking into that, can you also let me  know the closest AnyBank Home Lending branch? And maybe also let me know what will happen with my escrow account.  All right, Osmond, let me give you the info you asked for. First, the branch. Your closest AnyBank Home Lending branch is at 240 Greenwich Street, New York, New  York, 10007. They're open Monday through Friday from 9:00 a.m. to 6:00 p.m. Their main phone number is 212-232-1420.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/710.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=710)

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/720.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=720)

Now, regarding your escrow account after the transfer, I'll let you know that after I share the refinance status details, which I'm pulling up right now.  OK, Osmond, here's the update on your refinance and your escrow account. Your refinance application for your Palm Beach property is moving along. It's a 15-year fixed at 6.125%,  and the good news is it's still pending, but we do need some documents. As you see, the user was asking about refinancing while the tool was looking up details on that. He was able to switch topics and ask about escrow and other things, and the model did not lose the context when the tool call came back. It was able to weave the response back and generate an effective response.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/750.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=750)

Next feature that we want to highlight here is cross-modal speech and text input output. As you can imagine, a lot of enterprises would like to start off with a text bot and then add the voice interface to their text bot. When users use this kind of application from the mobile phone or the browser, they want to have the convenience of typing in their text input and with the press of a microphone button switch to voice. That's the convenience the cross-modal speech and text input enables.  The demo here shows how the user was able to enter the date in text format. The model picks up that date and comes back with a speech response, after which the user continues talking through their voice.

One key aspect of this feature is it's not only unlocking the cross-modal text input output for speech and text, but it also enables new features. Customers would like to use Sonic to make outbound calls, and when they do so they encounter IVR applications where they have to navigate DTMF tones. Some customers when making inputs maybe want to punch in their credit card information with their phone, which also means the model has to interpret the DTMF tones. Now with this technology, we are able to convert those into metadata, a text metadata that the model can interpret and therefore give the convenience to either use voice or keyboard input to enter their information.

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/860.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=860)

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/870.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/880.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=880)

Another application of this feature that customers have used is the initial greeting. You could now give a text prompt and generate a welcome greeting at the very beginning of the session to welcome or start the conversation.  So Osmond, can you confirm your date of birth for me?  OK, Osmond, I've got January 1st, 1990. Is that correct? No, no, it's not.  Got it. Thanks for clarifying. So, to confirm, what's your correct date of birth? OK, so I've got January 1st, 1980 as your date of birth. Is that right? Yes.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/890.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=890)

OK. Lastly, we talk about turn taking controllability.  We have experienced that there are customers who would like very quick turn taking, for example, a gaming application, or certain cohorts of customers like the elderly.

They're non-native speakers who are learning a new language, or certain cohorts of customers like the elderly, who may take a little longer to complete their thought or pause in between. What our customers asked for was the ability to configure the duration of the pause that the user can take before the AI determines the end of the turn. With that in mind, we've launched the turn taking controllability feature, which gives you three sensitivity settings. The highest sensitivity, which is the default setting, allows you to take a short pause of 1.5 seconds, and the lowest sensitivity can go up to 2 seconds, which allows users to take a longer pause and be able to complete their thought.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/950.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=950)

Let's take a quick look at how this plays out without actually cutting off the user's speech input.  In the first example, the user says: "I'm planning to visit San Diego next month for a vacation with the whole family. We have 2 kids with us, so what places would you recommend us to visit in San Diego?" The AI responds: "San Diego in December is wonderful for family trips with mild weather and many fun activities." Let's compare this with the other one. The user says: "I'm planning to visit San Diego next month for a vacation with the whole family. We have 2 kids with us, so what places would you recommend us to visit in San Diego? Oh, by the way, what will the weather be like there in December?" The AI responds: "For your family trip to San Diego next month, here are some top recommendations. Must visit spots include the San Diego Zoo, Legoland California, and SeaWorld San Diego."

Basically, it's showing more robustness to a longer pause. The response was more relevant to the user's query. They were really inquiring about the weather conditions and ideas for places to visit next month in December, but in the previous example they were cut off early, so the response they got was not as relevant. This gives you more flexibility in how you can tailor our turn taking behavior of the AI to suit different customer needs.

### Performance Benchmarks: Speech Understanding, Reasoning, Tool Calling, and Conversation Quality

With that, let me call upon Ankur, who's going to walk us through some of the key benchmarks. Thank you, Veerdhawal. I hope you're all excited about the new features that we're launching with Nova 2 Sonic. But it's not just the new features; we're also improving the core capabilities of the model. One feedback that we got from our customers was that the ASR, or speech understanding, of Sonic was really great, but there was still room for improvement. There was room to improve in terms of noisy conditions and especially on things like dates and numbers or alphanumerics, which our customers use often to get date of birth or account numbers and things like that.

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1090.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1090)

 With Nova 2 Sonic, we have improved on both. Compared to all the other models that we have evaluated, Nova 2 Sonic is state of the art in terms of ASR recognition. On alphanumerics, we've improved over Sonic 1 by about 50%. It's not just the ASR. We've also improved reasoning, and compared to all the other speech to speech models that we compared against, Sonic has one of the best speech to reasoning capabilities.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1120.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1120)

Voice agents are not just for chit chatting or asking questions.  They also need to take some action based upon whatever the user is going to ask the system to do, which is why tool calling is a critical functionality that we measure Sonic against. This includes both simple tasks like having a few tools to call, versus complex tasks where you might want to call multiple tools to accomplish a task, and the asynchronous tool calling functionality that Veerdhawal was just mentioning. Based on the key use cases that our customers had with Sonic 1, we improved our model with better training and better evaluations. Across multiple benchmarks, we showed that Nova 2 Sonic has significantly improved over Sonic 1.

For a great experience with Sonic, it's important to also evaluate the overall conversation quality. For every single language that we evaluate on, we made sure that the quality of conversation was better than everybody else. We took human evaluators to compare our voice and our conversations against similar models, and Sonic won every single time. Sonic is not just highly capable in doing speech understanding and doing tasks and functionality; it also sounds more expressive and natural, providing overall a great experience for anybody who is using Sonic or listening to Sonic.

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1200.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1200)

### Why Build Nova Sonic? The Unified Speech-to-Speech Architecture Advantage Over Traditional Cascade Systems

 Now, I want to take a step back to ask a question which many people might be asking.

Why did we build Nova Sonic? If you have interacted with Alexa, Siri, or even more recent customer service calls, you have probably already spoken to a voice agent that already exists in the world. So why did we build Nova Sonic?

[![Thumbnail 1240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1240.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1240)

Typical voice assistants are built in a cascade system. They have a speech-to-text model that converts speech to text. That text is then fed into a text reasoning model that performs all the reasoning, agentic workflows, and tool calling, and it generates a text response which is then sent  to a text-to-speech model. I spent most of my career working on Alexa trying to improve this entire pipeline. These systems work great. In the past decade, they have made major improvements in terms of architecture and training to improve the reasoning abilities and to improve the accuracy of these models into the high 90s. So why build Nova Sonic?

[![Thumbnail 1290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1290.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1290)

We realized that these traditional voice AI systems have a constraint: they are fragmented. Each model is designed to do a different task. Because each component is working on its own, it creates a bottleneck. The systems cannot talk to each other without this text. Without this common brain, you are unable to carry over context beyond what is feasible by the limitations of the system. 

[![Thumbnail 1300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1300.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1300)

We have tried to address this. We have tried to build orchestration around this for something like error recovery.  We have tried to build error recovery systems that detect when the automatic speech recognition was wrong and then ask clarification questions. We have tried to build personalization that feeds into each and every single component of this pipeline so that we get better speech recognition, better pronunciations, and better understanding. We have also tried to add more contextualization by carrying context through these different models.

The problem is that this requires a lot of orchestration designed by engineers and scientists, and a lot of handholding and building feature by feature. It is not scalable. We also realized that even though we are building all these things, there are a lot of edge cases that could not be solved just due to the limitations of this architecture. When foundation models came out, we saw something that could change this. We could get rid of the constraints that were in this cascade system and truly build a conversational model that was like a human assistant.

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1360.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1360)

That is what Nova Sonic is. It is a foundational speech-to-speech model that understands and generates speech at the same time,  preserving tone, prosody, and conversational flow in a single dialogue, resulting in a more natural interaction. We did this by training on millions of hours of data, learning from human-human interactions and human-agent interactions. The model learns on its own how to act in different conditions, different scenarios, and different acoustic conditions. Because our training data is tailored towards specific use cases, the model naturally learns how to do things like context carryover, how to do error handling, and all these improvements would not have been possible without this architectural change.

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1410.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1410)

The numbers showing earlier on speech understanding accuracy, on tool calling, and on reasoning are a result of this unified architecture. Now customers do not need to build this complex orchestration trying to do everything.  With this unified architecture as its backbone, Nova Sonic is able to naturally carry context of the entire conversation. Things like personalization through retrieval-augmented generation or through prompting or even through document upload can happen easily, and it is available to all the components of the model, not just a single one.

Another piece of feedback that we always get with voice assistants is that high latency is very annoying to somebody who is used to talking to human assistants. With this unified model, it minimizes the time that it takes to understand what the user said, reason about it, and then respond. This significantly reduces the user's perceived latency. The foundation model has been trained on millions of hours of data, so it has learned naturally how to understand the nuances of speech, something that would not be possible to encode in just text or some other binary functions.

When you talk, it is not just about what you are talking about, but also how you are talking—the tone and the intonation of your voice. With Nova Sonic, it learns to adapt its responses based on the prosody and the emotion of the user. We also built in this moral responsibility so that the entire conversation in both voice and in text stays consistent. With every generation of the speech-to-speech model, all the abilities I was talking about will continue to improve, which is why now is the time to build with these models and give your customers a much better experience.

I think I've spoken enough about the benefits. I'll head back to V, so he can talk about real customer use cases and how customers are actually using Sonic. Thank you.

### Customer Use Cases and Developer Integration Tools: From Amazon Connect to Telephony Partners

So as you can imagine, there are a lot of different use cases that have emerged, but the three most primary use cases are self-service voice-first customer service automation on interactions, voice-enabled personal assistants, and education and language learning apps, which lend themselves very well for the application of this kind of speech-to-speech technology. We have key customers who are building applications and productionizing workloads with this, including Crescendo, iFrame, Rujum, Cisco, Ring, and Amazon Connect. Here's a testimonial from the CTO of Rujum highlighting the accuracy of understanding speech, the ability to handle dialogues fluently with low latency, and overall deliver a better human-like customer experience.

[![Thumbnail 1520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1520.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1520)



One thing that we learned very quickly after the launch of Sonic 1.0 was that it's not just enough to deploy a model in Bedrock and provide bidirectional streaming APIs. There's more nuance to building these kind of real-time applications, and that required developers in particular to handle all the input output signals and manage the session life cycle. This turned out to be a last mile problem where really deploying this and scaling to their applications took much effort on their behalf.

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1580.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1580)



Therefore, we have partnered with a couple of different teams here which I want to walk through who've made this integration seamless. Let's start with LiveKit and Pipecat. As I mentioned, these are frameworks available for conversational AI which handle all of the input output signaling. They abstract these so that it's easier for developers to directly integrate their mobile apps and browser applications with Sonic on Bedrock while also managing the session life cycle. Typically Bedrock supports 8 minutes of a session lifetime, but with these frameworks developers would like to extend this and carry forward the conversational context to extend the overall cycle.

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1650.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1650)



Next we have Amazon Connect. More recently, as of yesterday in fact, Amazon Connect has now gone into general availability with Nova Sonic to enable customer service call automation. This not only brings the expressivity and fluid dialogue handling that we just talked about of Sonic, but it also unlocks the agentic capabilities of Connect AI agents and different AI tools that they bring for customer service call automation. This integration is now available in general availability in IAD and PDX regions supporting at least three languages: English, Spanish, and Portuguese, with more coming in Q1.

And lastly, not all enterprises have a customer service contact support center. Maybe there are small business owners who would just like to enable an outbound call mechanism to reach out to their customers, or while their shop is closed, they may want to receive phone calls on their behalf and take orders, which is where telephony integration is key. Therefore, we have partnered with the likes of AudioCodes, Vonage, and Twilio, which make it seamless and easy for customers to not only integrate through the telephony channel but also extend through other ways such as SIP telephony, which allows you to connect internet telephones through your browser or mobile app to talk to Sonic and get the same sort of conversational expressivity and dialogue that you get directly integrating to Bedrock.

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1800.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1800)



### Partner Demo: Cisco's AI Receptionist Solution Powered by Nova Sonic for Enterprise Voice Workloads

With that, it's time to see a partner demo. And it's my pleasure to welcome Amma Pandekar, who is the VP of product management at Cisco Calling, who would like to speak about what he's doing with Nova Sonic. Thanks for having me. You folks having a good show so far? It's amazing to be here and we have a great partnership with AWS over the years. I'm Amma Pandekar. I work for Cisco as a lead product manager for our voice platform that powers our calling, our contact center, and so on. Just by a quick show of hands, how many of you deal with voice workloads or manage them? Quite a sizable number. But how many of you work with customer service enablement type roles? So what's happening is we're here to talk about how AI is changing the overall voice-enabled workload ecosystem.

Cisco is a big networking powerhouse with networking, security devices, and collaboration all together. We serve the largest financial service houses, including all of the Wall Street banks. The largest development bank in the world is our customer, and we have large retailers including Amazon on our platform. All of the pharmacies and most governments across the world, including the US government, use our services. Healthcare is huge for us as well. We have over 50 million users on our platform and enable about 1.5 billion minutes in the AI assistant that we have built on the Amazon platform.

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1880.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1880)

On the other side, we also have Apex Contact Center, which is a contact center like Amazon Connect that powers customer care scenarios for companies like United Airlines and Maersk. The key is that this is available globally in all markets on the AWS platform. We have 195 markets today across the world with a 99.999% SLA. This has to be super critical for large customers, particularly when it comes to voice. 

I'm really excited to show you what we have built in partnership with the Nomasonic team for an AI receptionist use case. Previously, all of these workloads were hard to deploy and manage. For example, if you have a bank and after 5 o'clock there's nobody there to answer the phone, what are you going to do? You don't have staff in those locations. If you want to call a pharmacy and update your prescription or do a refill after 5 p.m., there's nobody available. That's one case where customers want to do less with more, so they're using tools like AI receptionists for that.

The other scenario we're seeing is cyclical demand during a short period of time. Tax season is a typical example. You want people to be able to answer questions, answer customer calls, book appointments using the tool handling we discussed, and even transfer calls to people who might be able to assist with all the information. How many of you have started a call and had to repeat your information three times over when you call customer service? For me, it starts with my last name, which is complex, so I have to go with P for Pennsylvania and A for blood, and by the end of it, I'm so tired. Things like multimodal input give you the ability to text it or put it on a website, so you don't have to specify or call things out.

[![Thumbnail 1990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/1990.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=1990)

Lastly, what these tools are doing is helping customers prioritize their employee time for high-value customer interactions so that you can provide really good customer service.  Let me show you a demo of how this works. But before that, very quickly, if you call any business, we have a receptionist that answers the phone powered using the languages and voices that the platform has. We train it with a knowledge base. For example, if you're a pharmacy, you can take all of your pharmacy information and input that to create a knowledge base. It can answer questions about medicines, products, hours, and more.

Thirdly, it can take actions. For example, you can book an appointment or schedule an appointment. You can look up your data from a CRM system and know your account information. When you need help, you can say, "Hey, I need to book an appointment with the doctor," for example. If that's a complex task, then it transfers to a live person, so it's human assisted. That person already has the knowledge that was told to the AI receptionist, so you don't have to repeat yourself.

I'm going to show this to you in action to make it real. This is a demo we built, and we're actually building it for a customer. I cannot name them, but they're a large tire chain in the US with a lot of tile stores across the West Coast and the Midwest. They have thousands of tire chain stores but don't have staff to manage all of those and answer those calls. What they want to do is automate 50 percent of the questions and answer them and even do the appointment handling in an automated way so that the technicians can focus on actually fixing tires and car problems. That is their goal, and we built this with them in mind.

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2110.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2110)

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2120.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2120)

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2140.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2140)

Let me explain this to you.   Welcome to Technology Tires Emergency Service. I can help you in Spanish, English, French, or Hindi for your tire emergency. Speak in your language of preference. How can I assist you today? Hey, I have a tire blowout and I need a complete replacement. I'm sorry to hear about your tire blowout.  I can help you get that sorted right away. To get started, could you please share your phone number with me? This will let me look up your vehicle details and account information.

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2160.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2160)

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2180.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2180)

[![Thumbnail 2200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2200.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2200)

Great, thank you for sharing that. I found your account, Amey.  You have a 2024 BMW X4. Since you mentioned a tire blowout and need a complete replacement, let me present the tire options available for your vehicle. Just a moment while I pull up the options for you. Perfect. I have the tire options for your 2024 BMW X4. Here are your three options for a complete tire  replacement. Option one, Michelin Pilot Sport AS4, premium performance tire with excellent grip and handling. Option 2, Pirelli Cinturato. I'll take the Michelin Pilot, please. Excellent choice. The Michelin Pilot Sport AS4 is a great option.  To schedule your tire replacement, I'll need two more details. One, your current location where you'd like the service performed. Two, your preferred date and time for the appointment. Could you share those details with me?

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2220.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2220)

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2230.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2230)

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2240.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2240)

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2250.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2250)

I'm at 67 Broadway, Las Vegas, Nevada, and I'd like the technician to be here somewhere between 11:00 a.m. and 12 noon.  Just a moment. I'm getting that scheduled for you now.  While you're doing that, do you have any towing services in case you're not able to fix it here? Yes, we do offer towing services if tire repair is not successful,  or your vehicle needs to be moved immediately to avoid a potential hazard. Your appointment is now confirmed.  Your appointment number is tire 260715. A technician will arrive at 67 Broadway, Las Vegas on November 26th between 10:00 a.m. and 10 p.m. to replace the tire on your 2024 BMW X4 with the Michelin Pilot Sport AS4. You'll receive a confirmation call 30 minutes before arrival. Please make sure your vehicle is accessible at that location. Is there anything else I can help you with today?

[![Thumbnail 2280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2280.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2280)

No, that's all. You've been great. Thank you. You're very welcome, Amey. I'm glad I could help.  Your technician will arrive shortly and we'll get that tire replaced quickly. If you need any further assistance, don't hesitate to call us. Have a safe day. What do you think of that? You know, the best thing I like about that demo is that it got my name pronunciation right. Because my name is complex, and I mean all that's simple, but most of the time it gets pronounced as Amy. So you can even train these models for the correct pronunciations and the phonetics to power a great customer experience.

A few things I'll call out that we talked about and used here. One was that we use a multi-modal dialogue so they can enter a phone number, specify an address, or provide a credit card number, all of that very easily. The second thing is that, like bargaining, which is I don't want to wait for all the information before I can talk back, and then the model will recognize that and respond appropriately. That's the second one. So this is actually real, and we are planning to roll this out sometime early next calendar year to our customers.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0f6af56a0ebff6d2/2410.jpg)](https://www.youtube.com/watch?v=o92D2x2Zfw8&t=2410)

You see the power of this tool set and what it can do for customers and essentially revolutionize the voice interactions that you may have with your employees or your customers on a data database. I want to thank Veerdhawal and the Ankur team for letting me be part of this, and then thank you for being at re:Invent. I think it's on to the next questions next, right? Thank you, man. Thank you. All right, that brings us to the end of our session, but I want to point out a few links and open up the floor for questions.  Here's a link to the user guide and our technical report, which has all the benchmarks that we talked about. The user guide has code samples and other details on how you can prompt the model to make it work for different use cases. I also want to point out two other sessions that are related to Sonic that the team is doing to really give you a hands-on experience in building applications. Thank you so much, everyone. Thanks for coming in.


----

; This article is entirely auto-generated using Amazon Bedrock.
