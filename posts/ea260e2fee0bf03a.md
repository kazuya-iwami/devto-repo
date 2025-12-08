---
title: 'AWS re:Invent 2025 - Amazon Nova Forge: Build your own frontier models using Amazon Nova (AIM3325)'
published: true
description: 'In this video, Mark Andrews and Karan Bhandarkar introduce Nova Forge, AWS''s solution for building custom foundation models. They explain the three training stages (pre-training, mid-training, post-training) and how Nova Forge provides checkpoint access at each stage, enabling organizations to inject proprietary data without starting from scratch. Key benefits include blending Amazon Nova Curator training data to prevent catastrophic forgetting, SageMaker HyperPod recipes for simplified training, and the ability to bring custom environments for reinforcement learning. Rosa Català from Reddit shares results showing 26 percentage point precision improvement and 25% reduction in missed threats for content moderation. Additional case studies from Appian, Nimbus Therapeutics, Nomura Research Institute, and Sony Group demonstrate performance gains across diverse industries while maintaining cost and latency advantages.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/0.jpg'
series: ''
canonical_url: null
id: 3088388
date: '2025-12-06T07:49:56Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Amazon Nova Forge: Build your own frontier models using Amazon Nova (AIM3325)**

> In this video, Mark Andrews and Karan Bhandarkar introduce Nova Forge, AWS's solution for building custom foundation models. They explain the three training stages (pre-training, mid-training, post-training) and how Nova Forge provides checkpoint access at each stage, enabling organizations to inject proprietary data without starting from scratch. Key benefits include blending Amazon Nova Curator training data to prevent catastrophic forgetting, SageMaker HyperPod recipes for simplified training, and the ability to bring custom environments for reinforcement learning. Rosa Català from Reddit shares results showing 26 percentage point precision improvement and 25% reduction in missed threats for content moderation. Additional case studies from Appian, Nimbus Therapeutics, Nomura Research Institute, and Sony Group demonstrate performance gains across diverse industries while maintaining cost and latency advantages.

{% youtube https://www.youtube.com/watch?v=osY67gy-BT4 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/0.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=0)

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/20.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=20)

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/30.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=30)

### Introduction to Nova Forge: Setting the Stage for Foundation Model Development

 Alright, good afternoon, folks. This is the first session of Wednesday afternoon, so we're excited to talk about Nova Forge today. My name is Mark Andrews. We also have Karan Bhandarkar, who's a Principal Product Manager on the team, and very excited to introduce Rosa Català from  Reddit. So, without further ado, why don't we get started? There will be a reveal, a superhero reveal later, so that's just to give a quick heads up.  So don't be surprised.

Okay, so just a quick agenda. We're going to talk about Nova Forge today. To start, we'll tell you about how foundation models are actually constructed. This will hopefully give everybody a warm-up with respect to how foundation models are actually built, so we'll go through some visuals and help you understand that. Then Karan will lead into building your frontier model with Nova Forge specifically. We'll introduce again Rosa to talk about the Reddit experience. We're very excited about the Reddit results, and Rosa will go into those in detail. And then Karan will actually wrap up with additional customer stories.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/70.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=70)

### The Knowledge Gap: Why Foundation Models Need Your Intellectual Property

 So first off, foundation models today are of course super capable. It's been incredible with respect to all of the applications that have been built over the last several years. However, one of the big deficits in foundation models today is many of you will have intellectual property that you own that these foundation models will not be aware of. So essentially there's a chasm here. There is a divide where essentially bridging the gap between what the foundation model is aware of and your organizational knowledge hasn't been solved. You do have RAG as one example, but that's merely essentially a search and retrieval experience. The intelligence of your intellectual property is not baked into the model itself.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/120.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=120)

So just to recap,  we have RAG approaches which essentially allow you to retrieve context to provide intelligent answers to models. You can also adapt models with LoRA adapters. This is like fine-tuning, just as one example. This allows you to enhance certain capabilities of models, but it is limited. Then there's the expansion experience, which is down here on the bottom left. Expansion is essentially like continued pre-training. This is a great approach, and some of you may have done this with open weights models. However, you may also be aware of the risk of catastrophic forgetting, which is a real issue which we have solved through Nova Forge, and Karan will talk more about that later.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/190.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=190)

The other thing you can do, of course, is building your own model. However, this is enormously time-consuming and costly, both in terms of acquiring the data to actually train a foundation model from scratch, plus the GPU hours required. So essentially, Nova Forge gives you the ability to fast-track that in terms of developing your model from scratch while significantly lowering the cost of developing that model. So we're excited again. Matt Garman yesterday announced Nova Forge at the keynote.  We're super excited about this. This is, we believe, one of the most amazing experiences we can give you all as customers to be able to develop your own foundation models, leveraging the power of your intellectual property at each of your companies.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/210.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=210)

### Understanding Foundation Model Construction: From Pre-Training to Fine-Tuning

Just to give a quick recap,  Nova is a family of models essentially. So we have the models that were introduced last year, Micro, Lite, and Pro. We have, of course, the exciting models that were announced yesterday. We have Nova 2 Lite, which is a super capable model. We have Nova 2 Pro, which is our most capable model in early access, and Nova 2 Omni, which is a multimodal understanding model in preview as well. So these models have all been demonstrating super exciting performance results with respect to industry benchmarks, comparable to many of the other frontier models in the industry, like the latest and greatest models. So we are super excited to see that this is the foundation that essentially you can start with.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/270.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=270)

You will be building on some of the most capable models in the industry, adding your foundational data to that, and enhancing the capability of your very own model to basically produce value for your business needs. So, okay, let's talk about how do we actually build a foundation model.  This hopefully gives you a kind of precursor into how this works from scratch. So essentially we start with an empty model. This is essentially, think of this as architectural scaffolding. This is essentially the structure of the model architecturally. It doesn't contain any data.

Then we introduce pre-training data, and this is extensive amounts of data to essentially help the model understand the world. It helps it reason. I like to use the example of Grey's Anatomy. That's the medical textbook, not the TV show, just to be clear.

Think about all of the foundational knowledge in Gray's Anatomy. It's one of the most elementary medical textbooks in all of history. It's been updated for the last 150 years. It's incredible foundational data to be able to train a medical model as an example. Now then, let's say you have a company that's specializing in dermatology. This is where the mid-training phase comes in, and you may want to enhance the model's understanding specifically for skin disorders and skin treatments and medical protocols for treatment and so on. And this is where the mid-training can come in here.

And then the very last stage is essentially fine-tuning. This is where you enhance the model. Like this might be your treatment protocols with respect to your company. Maybe you've developed some intellectual property around treating procedures and having an industry-leading procedure, and this is where you can introduce that signal so that your model is essentially super-tuned to the way that your business wants to operate. So this gives you kind of a relatively holistic understanding using this example.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/370.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=370)

### The Three Stages of Model Training: Pre-Training, Mid-Training, and Post-Training Explained

And then just going into a little bit more detail, again, starting with pre-training,  we essentially leverage a lot of web content, long-form context texts, like, for example, like the Gray's Anatomy medical textbook example. And essentially this is a lot of unsupervised learning. So the model will learn essentially its core world knowledge, basic understanding of the world and how to understand it.

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/400.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=400)

Then at mid-training, it takes the pre-trained model that we actually just prepared beforehand.  Again, leveraging the example I gave, this is where the dermatology example comes in where you give it specific enhanced information about the dermatology context. You feed that into the model. This gives the model much more competence in that dermatology domain, which is, you know, the analogy is your business domain.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/420.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=420)

And then the last step is essentially  post-training. There are two main parts to this. I like to talk about, or I like to think about this in essentially dog training. If you think about dog training, there are typically two steps to it. There's, you know, you go to the dog obedience school, which is kind of SFT, the Supervised Fine-Tuning. This essentially is where the dog learns kind of rote experiences, like basic experience about how to behave and whatnot. But it's not really a real-world situation, you know, obedience school for dogs is not necessarily the real world.

So the next thing we want to do is think about, okay, well, in the real world, how do we actually help the dog actually understand, reinforce good behavior. Like if we bring the dog to the park for the first time, there's loads of distractions, there are dogs, there's lots of humans, there are balls flying around, you know, we want the dog to essentially behave properly. So this is where Reinforcement Learning comes in, and this is where essentially we reward good behaviors. And, you know, an example might be a treat for good behavior. If they do something that we don't like, like they run after another dog's ball or something, this is where we reinforce it with no. And this is kind of just the example of how we essentially build that super-capable fine-tuned model for your business case.

### Access to Checkpoints: Democratizing Frontier AI Through Flexible Entry Points

All right, so with that foundation, I'm going to pass on to Karan, who's going to talk about the exciting details about how Nova Forge works on the inside. Thank you, Karan. All right, thanks Mark. All right. So this is the part where we actually start with a bit of a pop quiz to decide how much you've been following so far. So everybody ready for an exam? No, I'm just kidding. I think we're just getting into the fun stuff now.

So, okay, so Mark gave a really good overview as to what it means to build a foundation model, right? At Amazon we build Nova. The steps that he described is how Nova was built. Now we're going to talk about why the approach that we're taking with Nova Forge is such a game changer and why we believe it is democratizing access to frontier AI for every organization.

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/540.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=540)

 In this segment, we're going to talk through the five key benefits of Nova Forge. I'll talk about the whys of each one. I'll give you the examples of why you should care. We'll get into some of the details as well. And then towards the end of the session, we can sort of meet on the side for any further questions that you might have as well.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/560.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=560)

Okay, so first,  access to checkpoints across all phases of model development. Now, what does that even mean and why should you care, right? So going back to Mark's earlier example with the slides, these models are essentially trained in three high-level stages, right? What we call pre-training, mid-training, and post-training. What we're giving you with Nova Forge is access to every stage of training. We don't want you to start from scratch. We don't want you to start all the way from the left. We don't even want you to start all the way from the beginning of pre-training.

What we're saying is, depending on the kind of data that you have and the volume of data that you have, we'll open the training. We are creating, giving you a paradigm that essentially opens up the training for you to inject the data that you have in the right place for the model to learn sufficiently enough from it.

If you have instruction-response pairs for fine-tuning where you have a lot of samples of what an input and a right output look like, you can start all the way at the end in post-training and do just fine-tuning. If you want to do some more reinforcement learning, you can start all the way at the end from the post-training model checkpoint and do reinforcement learning on top of that as well.

If the amount of instruction-response pairs data you have is a lot, you can go one step further up. You're like, okay, I have a lot of data, even though it's for fine-tuning, so I really want it to learn from this. So I'm going to start fine-tuning from the mid-training checkpoint. Now, you might have some unstructured data. The unstructured data might not be a lot, but one of the high-value aspects that we've seen as these reasoning models have come out in the industry this year is you might have chain-of-thought kind of data, where it's really high-value data because you're actually showing these models how to think in your particular domain or think for your business-specific use cases.

So that kind of data, intermediate volumes of unstructured data which may not be chain-of-thought data or anything else, like if you want to refine the behavior on something like tool use, you can introduce that kind of unstructured data at the mid-training point. But if you have vast amounts of data, you're operating in an industry, a business, an area where either there's just no way the model has seen this data just because it's your proprietary data, or it could be in a domain where these models have been trained on to some extent because there's a lot of public data on it as well. But these models are general-purpose models, so they just haven't been optimized for that particular domain.

So if you have that kind of a scenario, then you can start all the way from the beginning of like a pre-training checkpoint. So what we give you is a checkpoint that's at the end of Amazon Nova's pre-training, Amazon Nova's mid-training, and Amazon Nova's post-training, so that you can introduce your data in those stages without having to start all the way from scratch.

### Learning Rates and Data Blending: Solving Catastrophic Forgetting with Nova's Curated Data

Now you might ask me, hey, Mark just showed me a slide, and it said that the pre-training and mid-training stage at the end of it is what's called a base model. Now, I do get a base model from open-weights models. So why should I really care about your pre-training checkpoint, right? Aren't you just giving me, you know, is this all smoke and mirrors? You might have that question, and that's a good question to ask.

So there, I think we need to get a little bit into the detail of what the training process actually looks like. So the way these training, you know, we've talked about toss a bunch of data, these models are trained, right? But there's an aspect here that's important to understand, which is the learning rate. The analogy that I really like to use for such examples is think about when you go into school or college or any of those kinds of things, right?

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/770.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=770)

First, there's a warm-up phase. This is your first day in school, first day in college. You're essentially getting the syllabus, the curriculum, the textbooks for the first time in your hand. You're looking at what this even is, who the professor even is,  how should I approach the rest of the semester? Those are the things that are going through your mind on the first day. Imagine if I taught you the most important content on that particular day. You don't want to do that. There's no way you're going to learn any of it. So that's what we call the warm-up phase. This is when you actually activate the learning rate.

This is more of a visual representation. Different models have different, what we're going to get into, learning rates. Some do exponential, some do constant learning rates, but at the end of the day, there's an aspect or a concept of learning rate. The learning rate is essentially the rest of your college semester. Now this is the aspect where you're learning, right? Every day you get some new information, you learn from it, you move on, go to the next day, learn from it, and this is how the models are trained.

So the entire journey of pre-training or mid-training is essentially with this learning rate because it's not a sprint, you really want to learn everything from it. At the end of the semester, essentially what happens is what you call annealing. Now, annealing, the analogy I like to use for that, it's like the morning before an exam. Right, you're pulling out your notes, you're pulling out your flashcards, you're trying to focus on all the most important things because you're about to just now walk into an exam, right? That's the process of annealing.

In the model terms, what we do is all the, whatever is the most important data, what is the most high-value data, it sees it in this stage of training and annealing, and the reason it's called annealing, and you see the learning rate drops, is because at the end of the stage is when we tell the model, all right, you're good to walk into the exam, you're done learning. But now this is where it gets interesting, right? Base models are available today, if you see where they are. That's the learning wave that you're operating with. So if you have high volumes of data, you really want to be coming in there.

So that's the benefit of starting with a pre-trained model checkpoint. Like that's why this is important to you. And then the post-training, you know, we've talked about it in the past, fine-tuning and all those other aspects, LoRA-based adaptation techniques are popular in the industry, but at that point you're doing more post-training and refining the behavior. So if you have differentiated proprietary data, that's essentially where you want to introduce it.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/900.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=900)

Now there's another aspect to this, right? So if you continue with the same analogy, you get access to blend, you get the ability  to blend Amazon Nova Curator training data with your training data in all those stages, right?

Now why is this important? Catastrophic forgetting is a term that's said a lot in the industry, so let's get a little bit into what that means, if it's even real, and how Nova Forge helps you solve for it.

Let's take an example. Let's go back to the analogy that we were just doing with an exam the morning before. You're about to walk into an exam in three hours. I come up to you and say, hey, I know you were preparing for this biology exam, but actually it's a biology plus math exam combined together. Here's a math textbook. Learn everything from it because half your grade is based on that. That's going to be a great feeling for you, isn't it? That's essentially what happens when you're doing pre-training or mid-training, where you're introducing a ton of new data late in the stage of the training.

Now the problem is you're going to try and learn as much as you can from this new content. The risk there is you're going to start forgetting some of the things from the previous content that you were learning from. Essentially what we're giving you with the ability to blend Nova's data with your training data is we're giving you the biology and the math textbook as you walk in, so that you can refer to both those materials. As you're learning the new content, you still have an eye on the previous content that you learned, but you're also now with these two textbooks in front of you able to connect the dots between those two materials better too.

What that translates into for foundation model development is if you have a lot of proprietary data, let's say you're bringing in data for the healthcare domain, you don't want to forget everything else. These models have been trained to reason. You want these models to reason in your domain as well. You don't want to lose out on all the foundational skills only because you're introducing your proprietary data. That's the value of Nova's data. On the pre-training stage and mid-training stage, you get the ability to use Nova's data, mix it with your data, so that you can mitigate such aspects so that at least the general purpose foundational capabilities, you're not sacrificing that just to be able to build your own foundation model.

The same thing applies on the post-training side as well. An example that I give a lot is on the fine-tuning side. Is everyone here familiar with instruction following to some extent? Sort of familiar? Instruction following is an aspect where you ask a model something and it follows your instructions, which is fundamental for any large language model that's out there today. It doesn't matter what kind of use case you're using it for, you want it to follow your instructions. That's at the end of the day what you're doing.

When you introduce new data in fine-tuning, a lot of data, yes, you want it to learn what is the right output for a given input. But you don't want it to forget how to just answer questions and answers. You don't want it to forget the basics even on the fine-tuning side of things. That's why we provide the ability to use Nova's curated data across every stage of training, because it's important not to forget the foundational capabilities with the new information that you learn in each stage.

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1080.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1080)

### Proof of Concept: Amazon Stores Foundational AI Achieves 10% Performance Boost

All right, so I just threw a lot of  theory and concepts at you. You might be thinking, all right, is this guy just making stuff up? Where's the proof? I'll give you an example. Internally, Stores Foundational AI, this is the team that sits within the Stores organization at Amazon, they're one of our internal customers as well. We took this approach to them to see, do we have anything here?

The Stores Foundational AI team naturally works on a ton of use cases in the shopping domain. No foundation model out there has been optimized for a vast variety of use cases in the shopping domain. So they set out to create a foundation model that addresses as many use cases as possible, as wide as they can go in the shopping domain. They don't care about other domains. All they want is for this model to be really good at all the tasks in this particular domain.

We tried all approaches that were out there, all the way from simple fine-tuning to continued pre-training and fine-tuning and all those approaches that are there with open weights models as well. And then we took Nova Forge to them. What they found, and this is what I emphasize, is the two key dimensions. When they took this approach of starting with a checkpoint from Nova, and the checkpoint is the stages that I mentioned, they started from a pre-training checkpoint because they have a ton of data, obviously.

Not only were they able to improve the performance by about 10% on their current production model baseline. The MMLU benchmark for those who are not familiar is multitask language understanding. Across a wide variety of tasks, understanding the language of that, just understanding the language. There's a benchmark called shopping domain MMLU for multitasks in the shopping domain and understanding. They were able to boost the performance by 10% on what they currently had with similar approaches with other models.

Not only that, they were also able to boost the performance on just standard language understanding. This is the example of when we talk about the instruction following and maintaining the foundational capabilities that I was talking about. They were able to not worry about regressing, but they were actually able to even boost the accuracy further, even on the general purpose aspects because you don't want these models to forget about things like instruction following.

[![Thumbnail 1210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1210.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1210)

Now we've talked a bit about pre-training,  mid-training, what Nova Forge has to offer for you there. A little bit on the fine-tuning side of things as well, because you can use Nova's data there too.

### Reinforcement Learning Revolution: Bringing Your Own Environments and Orchestrators

Now, on the reinforcement learning side of things, the reinforcement learning progress in this year is what's really been key for these foundation models to get really, really powerful on general purpose tasks. So when we say we want to give you the flexibility and control and democratize AI, we want you to be able to introduce not just your data, but also your environments to train these models. On the left-hand side, what I have is what is today commonly referred to as how we do reinforcement learning, just the industry standard way.

For customers who are just getting started or still doing light reinforcement learning, the way it works is we are essentially trying to reinforce good and bad behavior. You don't need a lot of good data or bad data, but as long as you have a way to curate some form of reward signals, and rewards being thumbs up, thumbs down, this is good, this is bad, this is a 2 out of 10, this is an 8 out of 10, you can start with something like reinforcement learning. There's a few different approaches or ways to go about it.

It can be as simple as starting with a regex pattern because you might want to do some pattern matching and give some 2 out of 10, 8 out of 10 kind of scores in training. You might want to take another step further and introduce or use some sort of Python functions and write some custom code to sort of say, okay, if these situations happen, weight it this much, if these situations happen, weight it that much. Or if it's not something that's deterministic in that way, you can take the LLM as a judge approach.

So you can go on what we call verified and non-verified reward side of things. You can also do non-verified rewards, but it's not as simple as if this text matches this text, then give it a 2, if this text matches this text, give it an 8 out of 10. It's a little more subjective than that. You're getting into the nuances of if I ask for a summarization and the text is 4 sentences long, that's a 2 out of 10, but if the sentence is 8 or if the paragraph is 8 sentences long, the summary, then that's an 8 out of 10. So you're trying to get into the nuances of that.

In this industry standard, you do have a lot of options to do reinforcement learning today that are really good. But when we talk to customers, we realized that that's just not enough. At the end of the day, all of this is in a managed environment. You need to upload your regex code, you need to upload your Python code, and the training environment is all managed for you.

But imagine you're a lab where you're doing drug discovery as an example. It's not as simple as I'm going to upload Python code to verify if the molecular properties that are being generated by this model are accurate or not. It's just not that simple. These scientists who are in labs, they have, let's take as an example, some sort of simulations or some sort of tools or chemistry toolkits that they will have in their own internal environment, in their own internal labs that they need to somehow now replicate in the form of Python functions. That's just never going to happen.

So what we're also introducing as part of Nova Forge is we want you to be able to use your own proprietary environments, not just data, to train these models as well. There's two ways to go about it. You can bring your own endpoint. So the simulation example that I mentioned, if you want to just make an API call into your environment, get the reward calculated score there, send it back, you can do the training loops that way, or you can go something even more complicated.

Especially in the world of agents, it's not as simple as one hop. It's not rewarding things on one step at a time. It's rewarding one function based on that output, the next output, and the next output. If you're doing coding-related tasks, it's not input and output. If you're doing robotics, it's not input and output, it's pick up this package and then move to the next package and drop it off somewhere else. There's multiple steps involved in this process as well.

So Nova Forge, what we're introducing is also the ability to bring your own orchestrator. What this lets you do is not only plug in your real-world environments, so if you have a robotic simulation environment, you can plug that in for sure, but we'll also let you use your own orchestrator, so you can actually control the rollout. Now, what does the rollout mean? When the output of the model comes into your environment, you decide what happens in your environment, send the score back, and then you can actually do reinforcement learning across multiple turns by being able to just use your own orchestrator as well.

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1480.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1480)

So as you can see across all the avenues that you have, pre-training, mid-training, post-training, we're giving you as many keys as possible to essentially help you build your own foundation model using Nova as a base. Now, you might say again, cool story, but is this really valuable, which would be a good question.  So the example I like to use is from Cosine AI. They were our design partners. They're a startup based out of London. You should check out their work. They're doing some really great stuff, pushing the boundaries on frontier AI for the coding space specifically.

They were the ones who helped us design this because of the aspects that I mentioned. They have an internal tool calling harness that they use in their production environment to sort of generate code. Now they already have these tools to generate the code, so why not use those tools

to do the training as well, because the tools already know what's good and what's bad. So we set up this multi-turn experience working with them on what such a thing would look like so that in the training loop itself through an API-based approach they can orchestrate the rollout on their end. So the call goes into their environment. They can call a specific tool, take the next step, call a different tool, take the next step, call three different tools, do some sort of nested learning, and then send the reward signal back that's used for training. So not only do we want you to use your own data for training, we want to be able to use your own environments for training as well. And this scales across industries, right?

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1560.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1560)

### SageMaker HyperPod Recipes: Optimized Training with Safety and Responsibility Controls

So this is an example in the coding space, but whether you're doing manufacturing, whether you're doing robotics, whether you're doing science experiments in the lab, across every industry, there's a scenario where if you're able to use your simulation environments, your environments in the training loop itself, you can go a lot further. Now  that's the training process, but Nova affords us a lot more than just access to checkpoints and data.

The fourth aspect that we talk about is the SageMaker HyperPod recipes. Now what are SageMaker HyperPod recipes? The recipes are essentially an abstraction on the training code itself because we don't want you to have to be an expert in ML or AI to be able to do this, right? We want to democratize AI. We want to democratize access to frontier AI. So what we do is we offer config files that sit on top of training code so that you can then actually configure every aspect of training and the training code itself through these config files that sit on top that make it really easy to operate in these environments. Those are the push button recipes.

[![Thumbnail 1610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1610.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1610)

And the other aspect is these recipes are optimized to maximize performance lift with Nova. Now what do these things mean? So the HyperPod recipes are quickly covered to make it super easy  to essentially execute training runs where all you have to do is select a recipe. A recipe is, you know, I want to do pre-training or I want to do mid-training or I want to do fine-tuning. You select a particular recipe for mid-training or fine-tuning. You specify the training data in the config file or in the recipe that I just mentioned, and you hit run. It's as simple as that.

We also know that customers of different types prefer different customer experiences. So if you prefer a CLI-based experience, you want to be in the notebook, you want to be in the weeds, you can do that. We make it easy to do that with these recipes. Or if you want to execute a large number of experiments at scale, just in the beginning to quickly get started, right, the CLI experience is great if you want to orchestrate things, commit code back to repositories, maintain pipelines. It's great for that. But even if you just want to execute these runs very quickly just to try a few different things, we make it super easy for you to do this.

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1680.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1680)

So an illustration here is all you have to do is select the checkpoint that you want to start with, enable the data mixing with Nova data, and these recipes already come preconfigured with Nova's data ratios to essentially help you quickly get started. And it's not just the training part. We want to make the end-to-end lifecycle super easy.  So in the same UI once the training run completes, you can trigger evaluation runs, you can trigger model deployment to an endpoint, or you can even continue customization with the model that you just trained. You fine-tune a model and you want to continue fine-tuning that same particular model, you can do that too, all from the UI.

[![Thumbnail 1700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1700.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1700)

 Now, I said these recipes are optimized to maximize performance with Nova, right? Like what does that mean? This is also another big differentiator compared to what's available out there with the open models, right? Yes, checkpoints are available, especially from mid-training and post-training, but what we're giving you, these recipes are built on top of the code that was used to train Nova itself. Like this is the same code that was used in building the Nova Foundation models as well. So what that means is when you use your proprietary data with these recipes, these recipes are already optimized to maximize the learning from the data that you're now introducing.

[![Thumbnail 1750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1750.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1750)

These recipes are also refined in research collaboration with partners. And once we make these available, what I was just showing in the slide before, we do provide you with smart defaults, so you don't have to spend too much time optimizing the recipes themselves. Now I love giving examples. I'm going to do that in this  situation as well. Now what does that mean? Consider a scenario where you're trying to build a foundation model that's an expert in a domain-specific language, right?

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1770.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1770)

So an example would be if your company has developed some proprietary coding language, you want this model to be really good at that. Let's take that as an example. So in that particular example itself, we  actually partnered closely with the Computer Science and AI Lab at MIT to optimize the recipe for something like that. So they were trying to build foundation models that are really good at metamaterials design, but at the end of the day, the metamaterials are nothing but just proprietary code.

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1790.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1790)

And what that results in, let's take Appian as an example. So Appian was a customer  in beta. They have their own proprietary language for process automation. So this is process automation. Their current production model was Sonnet 3.5. They've been experimenting with Sonnet 4, but what they found is that by fine-tuning Nova, because of the optimizations that they've made, they're actually projecting exceeding the performance of Sonnet 4,

but while benefiting from the cost and latency aspects of Nova that makes it cheaper and faster to essentially achieve the same kind of accuracy results.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1820.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1820)

Last  but not least, it's not just the training, it's not just the experience, but it's also the safety and the responsibility aspects of these model trainings. We want you to be able to configure these to meet your business needs as well. So the training data that I mentioned that you get from Nova to involve in training, you do get access to the Responsible AI and safety data that we make available that we use for Nova's training as well as part of it. You want to evaluate your model on Nova's Responsible AI dimensions, you can do that as well. We provide the same runtime controls that ensure that Nova is, we prioritize safety and Responsible AI for Nova, so you get the same runtime controls to ensure you can do the same with your model as well.

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1860.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1860)

And it's not just the controls we allow you to configure  the controls to meet your business-specific requirements as well. So say you're a security firm, you're trying to push the boundaries of what you can do for, say, cyber testing. You want to make sure that if you're testing for cyber attacks, you're able to do that. You want these models to be able to produce that. The general-purpose foundation models that are out there, they just will block you because they're trying to protect general-purpose use cases. Or if you're in media entertainment, you might own some proprietary characters. These models would block you from generating that because generally they don't want you to generate it. But if you're a company that has a lot of proprietary characters, you want the model to be able to generate them. You don't want the models to be blocked. So even on the Responsible AI and safety aspects, we want you to be able to adapt these models to your business-specific needs.

### Reddit's Journey: Rethinking Online Safety Through Hyper-Personalized Foundation Models

Next up, I talked a lot about the things and I tried to give examples as much as possible. Palakam Rosa, who's been an incredible partner through the entire beta process in itself, will tell you why this mattered for Reddit. So, hi everyone. I'm Rosa Català. I'm a Distinguished Engineer at Reddit. And today I would like to share what has been our experience in rethinking online safety by building our own foundation model using Nova Forge.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1950.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1950)

So, let's explain why, why we are here, why  we were interested in partnering with the Nova team for our content moderation and safety operations. So, one of the main goals that we have is to democratize online safety across the entire platform. The problem is, and I think is what we encounter, is the coverage lack and the scalability in getting to that point. The best way of thinking is in traditional or legacy machine learning models, how they learn. They learn on history. Therefore, you're always playing catch-up when you are facing a new emerging trend in the platform, new communities, new slang. The models have been trained on past data, and they remain blind until it's too late.

So, what is the solution to this? The solution to this is hyper-personalization in the way we protect and we support our communities with our safety capabilities. But what does that mean? That means that we will need to train 100,000 models to serve our communities, more than that, in fact, because on top of serving every community in the platform, that's democratizing safety at Reddit. It will require also that you make these models really performant, so you will have also to invest on fine-tuning, on making these models very performant for specific tasks.

Okay, so we have a goal, we have a constraint. What is the role of Nova Forge in our journey to try to get at the democratized online safety that we are striving for? So what Nova Forge unlocks for us is breaking this trade-off between specialization and scalability. How we can inject our rich graph-based context that we have at Reddit about what users intend, what moderators need, and how communities react.

At every point of the training and the creation of your own frontier model, exactly as Mark and Karan showed, we can start using different levers to create a model that is an expert but generalized to new events.

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2160.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2160)

So how do we do that? Let me explain first where we start. At Reddit, we are in a situation of maturity in safety. We have moved past keywords and simple rules. We use proprietary and sophisticated models to protect our platform. The problems are not the models or even their performance.  The problem is how we use them. All those models are going to require a rich context awareness that they don't have because most sophisticated state-of-the-art AI models have not seen our proprietary data. They don't know about the reactions and the interactions between users, moderators, communities, and participants at the end of the day. But we have this data.

So that's one gap that we have, and we call it the Knowledge Gap. A native Reddit model, or a model that speaks Reddit, is not available yet. The second is, can we reduce the dependency on fine-tuning for more performant models? The importance of doing this is twofold. We need to avoid the very hard operational wall that is to scale all these nuances that we present to the models. The second, and I think it is the most critical from a safety standpoint, is that we want to be able to protect our users at all times.

If a new threat emerges and we detect it, we need to gather data. We need to train or fine-tune a new model, an adapter, and then we're going to go and deploy it. It can take days to weeks, and in that time our users are exposed. At Reddit, we have adapters we call shields that protect our users from spam, from harassment, violent content, and many more. Imagine if I want also to put the context of, for example, how that will play out with the norms of the rules of our communities. This makes this incredibly complex.

[![Thumbnail 2300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2300.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2300)

So then we call the superheroes, we call Nova.  And we say, look, let's start with measuring performance. Can we use Nova, maintaining the performance that we are looking for to protect our platform with the most high-level standards? What we need is to start with measurements and evaluations where we have ground truth. We have data that explains how moderation, how moderators in particular, how safety at Reddit, and how communities react to different types of content. Then what we also have is this ground truth that optimizes for the reality of Reddit, not for an external general benchmark.

Once we have these evaluations or measurements, we go to the refinement stage. We didn't start back with pre-training, we started with Supervised Fine-Tuning. Now, the Supervised Fine-Tuning was using what we call incremental learning. So we went and said, look, content moderation is incredibly complex. It's not a simple zero and one. So what we did is to teach the model incrementally. Like the same as you are in a classroom, you teach the model the fundamentals, and then you start increasing, graduating the model as it learns.

Moving from simpler tasks into more complex ones. This was our first checkpoint. The second and third point for us was, can we now cover a knowledge gap? The knowledge gap is thinking about pre-training, starting with a pre-training checkpoint and asking, can Reddit data get mixed with Nova data? We wanted to see if the model learns to become more capable of understanding, thereby reducing the intensity, the frequency of labels, and the fine-tuning compute that you need afterwards. So this is the program and how we started collaborating and partnering with the Nova team.

I want to say that it's important for us to think that Reddit is about humans at all levels. Humans interact in conversations, humans interact by saying this is content that I like and content that I don't like. So we have tons of labels that are not just explicit labels but implicit labels, and that's where unsupervised learning becomes so scalable for Reddit.

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2510.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2510)

In terms of the fine-tuning work, I would say that what we are learning with Reddit and with Nova is that we can scale our human-in-the-loop operations. What that means is that  when we extract answers from Nova, we don't aim in safety detection for complex generation. What we are looking for is the classification and the representation of the data with what we call uncertainty awareness, so we can then rank all the responses of the model and prioritize what our AI Ops needs to review to provide feedback to the model, reinforce the model, and use the post-alignment in the most efficient way.

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2560.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2560)

Let's talk about results. In the first stop of the journey,  on the left, you have the Supervised Fine-Tuning. Here what we are looking at is how the model has been fine-tuned with this Curriculum SFT and this uncertainty awareness, and what we attain. We compare that against our Reddit proprietary ground truth. What you can see is that we were able to reduce the number of pipelines while maintaining or increasing performance. In fact, across the number of pipelines that we reduced, we were able to increase precision by 26 percentage points and recall by 16 percentage points. That's pretty amazing.

But we're not only doing that. What we are also doing is reducing the amount of inference. We're making it fast and cheap with this uncertainty awareness that I mentioned, but we're also reducing the number of fine-tuning compute and fine-tuning pipelines that you then have to call at inference. So checkmark here, we were able to avoid the model-per-task bottleneck that fine-tuning and expert models create, so we can go back to the idea that we can democratize online safety by being very expert but with a fraction of the cost of computing and calling these models.

But that's not enough. We went then to the pre-training stage. In the pre-training stage, what we tried to do is what I said before, can the model learn more nuanced context by going a step back? And why is that important? Because of all the implicit labels that we have at Reddit. The question here that we're trying to solve is, if we mix Reddit data with Nova data,

the Nova pre-trained checkpoint becomes smarter for Reddit use cases, and the answer is yes. We were able to ground Nova on Reddit intelligence and obtain what we call a superior model for two reasons. It is able to achieve a 25% reduction in missed threats, which is massive for safety operations. It means that we are a more resilient platform. It also means, if you see the precision and recall trade-off, it becomes more flexible. We have higher sensitivity. What that means is that the model now is not just guessing keywords but is understanding what content violation represents and means at Reddit.

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2770.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2770)

 I think now we are on a path in which we can combine the power of Continued Pre-Training and Supervised Fine-Tuning. What we expect to obtain from this is increasing benefits, compounding the benefits leading to this democratized online safety that supports and scales to the long tail without drowning us in engineering. I think we can go a step further though. As Karan was detailing just before me, what you can take now is these pre-trained models, these fine-tuned models, and combine them with the post-alignment phase. This means that now if we want to create hyper-personalization that is already aligned with the policy that we are learning from the ground up already with safety, we can create new, more exciting user recommendations and ads ranking.

I think then we can talk about a Reddit Nova Frontier model. That's a journey we're on. We're very grateful to the Nova team to allow us to understand the capabilities of what they built, and it's really, really exciting. I think Karan can talk about other customers that have been also already interacting with Nova Forge. Thank you.

[![Thumbnail 2890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2890.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2890)

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2910.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2910)

### Customer Success Stories: From Drug Discovery to Financial Services and Legal Research

Thank you, Rosa.  Alright, so Rosa explained how and why Forge is important for Reddit.  I'll quickly get through a few other examples as well, just to show you that it's not just about one specific area or one specific domain, but in every domain, in every business operation that you're in, there's value that you can unlock by building a frontier AI model that's purpose-built for your own business.

Let's take an example of Nimbus Therapeutics. Their use case there is to build a drug discovery assistant. They're a small startup, and they want to see how AI can help them scale and bring better medicines to patients faster. The use case was to build an LLM that can help them with pharmaceutical R&D and drug discovery, and much more. The approach that they tried with Forge was Supervised Fine-Tuning and Reinforcement Learning. The results that they achieved were a little similar in theme to what we saw with Reddit as well. They were able to converge several of their graph neural network models that are actually specialized models for scientific tasks into one model that was built with Nova. Not only were they able to converge those things, they got a model now that outperforms any other model out there, any large language model of any size for their particular use cases, while giving them the latency and the cost benefits of operating at Nova's scale.

[![Thumbnail 2970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2970.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2970)

 Let's take a similar example in a different domain, financial services. I like this example a lot because it combines two things, not just expertise in financial services, but specializing in a different market as well. So, a financial services LLM in the Japanese market. Nomura Research Institute was building a financial services LLM that's an expert for the Japanese market specifically. The approach that they took was continued mid-training followed by the rest of the post-training process, combining their proprietary data with Nova's training data. The reason they were doing this was they were trying to evaluate this as an alternative to what they're currently doing with open weights models.

The results that they saw was, yes, they are able to do this. They were able to far exceed the performance that they had with a similar approach with open weights models.

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/3030.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=3030)

Let's get into a different domain. Let's take the Sony Group. The Sony Group has started to drive AI business transformation across all of their business units. As you can imagine,  there's a lot of diverse businesses, a lot of diverse operations in a bunch of industries even within just Sony. So they wanted to build a legal research assistant that can help accelerate some of the processes to help them scale the AI adoption process itself.

What they found was that just through reinforcement learning, and this is why I say it's not always just about doing pre-training, it's about entering the stage that's right for your business-specific need. Just through reinforcement learning for this specific use case, they were able to exceed performance of the larger language models but again benefiting from Nova's price and latency performance. In this case, as an example, they were able to exceed all of the performance of the largest models available as well, and this was specific for a legal use case.

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/3080.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=3080)

 This is a theme I hope you take away from here, where this is not something that's difficult to do. The end goal here is we want to democratize access to frontier AI for each and every business, for each and every organization. We've tried to make this the easiest and most cost-effective way to do so with the Amazon Nova Foundation models. A lot of customers are already building with this. We hope to see you leave from this session and start building with Nova Forge as well. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
