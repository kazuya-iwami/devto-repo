---
title: 'AWS re:Invent 2025 - Amazon Nova Forge: Build your own frontier models using Amazon Nova (AIM3325)'
published: true
description: 'In this video, Mark Andrews and Karan Bhandarkar introduce Amazon Nova Forge, a service enabling organizations to build custom foundation models using their proprietary data. The session explains how foundation models are constructed through pre-training, mid-training, and post-training phases. Nova Forge provides access to checkpoints at each stage, allowing customers to inject their data at the optimal point while blending it with Amazon Nova''s curated training data to prevent catastrophic forgetting. Key features include SageMaker Hyperpod recipes for simplified training, support for custom reinforcement learning environments, and configurable safety controls. Rosa Català from Reddit shares their success using Nova Forge for content moderation, achieving 26 percentage point precision improvement and 25% reduction in missed threats. Additional customer examples from Nimbus Therapeutics, Nomura Research Institute, and Sony Group demonstrate performance gains across drug discovery, financial services, and legal research domains while maintaining cost and latency benefits.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/20.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Amazon Nova Forge: Build your own frontier models using Amazon Nova (AIM3325)**

> In this video, Mark Andrews and Karan Bhandarkar introduce Amazon Nova Forge, a service enabling organizations to build custom foundation models using their proprietary data. The session explains how foundation models are constructed through pre-training, mid-training, and post-training phases. Nova Forge provides access to checkpoints at each stage, allowing customers to inject their data at the optimal point while blending it with Amazon Nova's curated training data to prevent catastrophic forgetting. Key features include SageMaker Hyperpod recipes for simplified training, support for custom reinforcement learning environments, and configurable safety controls. Rosa Català from Reddit shares their success using Nova Forge for content moderation, achieving 26 percentage point precision improvement and 25% reduction in missed threats. Additional customer examples from Nimbus Therapeutics, Nomura Research Institute, and Sony Group demonstrate performance gains across drug discovery, financial services, and legal research domains while maintaining cost and latency benefits.

{% youtube https://www.youtube.com/watch?v=osY67gy-BT4 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/20.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=20)

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/30.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=30)

### Introduction to Nova Forge: Bridging the Gap Between Foundation Models and Organizational Knowledge

Good afternoon, folks. This is the first session of Wednesday afternoon, so we're excited to talk about Nova Forge today. My name is Mark Andrews. We also have Karan Bhandarkar, who's a principal product manager on the team, and we're very excited to introduce Rosa Català from Reddit.  Without further ado, let's get started. There will be a superhero reveal later, so just a quick heads up. Don't be surprised. 

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/70.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=70)

Here's a quick agenda for what we're going to cover today. We'll start by telling you about how foundation models are actually constructed, which will give everyone a warm-up with respect to how foundation models are actually built. We'll go through some visuals to help you understand that. Then Karan will lead into building your frontier model with Nova Forge specifically. We'll introduce Rosa to talk about the Reddit experience. We're very excited about the Reddit results, and Rosa will go into those in detail. Finally, Karan will wrap up with additional customer stories. 

Foundation models today are super capable, and it's been incredible to see all of the applications that have been built over the last several years. However, one of the big deficits in foundation models today is that many of you will have intellectual property that you own that these foundation models will not be aware of. There's essentially a chasm here, a divide between what the foundation model is aware of and your organizational knowledge that hasn't been solved. You do have RAG as one example, but that's merely a search and retrieval experience. The intelligence of your intellectual property is not baked into the model itself.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/120.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=120)

### Current Approaches and the Nova Forge Advantage: From RAG to Custom Foundation Models

To recap, we have RAG approaches which allow you to retrieve context to provide intelligent answers to models. You can also adapt models with LoRA adapters, which is like fine-tuning as one example. This allows you to enhance certain capabilities of models, but it is limited. Then there's the expansion experience, which is down here on the bottom left. Expansion is essentially like continued pre-training. This is a great approach, and some of you may have done this with open weights models. However, you may also be aware of the risk of catastrophic forgetting, which is a real issue that we have solved through Nova Forge.  Karan will talk more about that later.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/190.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=190)

The other thing you can do, of course, is build your own model. However, this is enormously time-consuming and costly, both in terms of acquiring the data to actually train a foundation model from scratch and the GPU hours required. Nova Forge gives you the ability to fast-track that in terms of developing your model from scratch while significantly lowering the cost of developing that model. We're excited that Matt Garman announced Nova Forge at the keynote yesterday.  We're super excited about this because we believe it's one of the most amazing experiences we can give you all as customers to be able to develop your own foundation models, leveraging the power of your intellectual property at each of your companies.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/210.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=210)

To give a quick recap, Nova is a family of models. We have the models that were introduced last year: Micro, Lite, and Pro. We have, of course, the exciting models that were announced yesterday. We have Nova Lite, which is a super capable model. We have Nova Pro, which is our most capable model as early access, and Omni, which is a multimodal understanding model in preview as well.  These models have all been demonstrating super exciting performance results with respect to industry benchmarks, comparable to many of the other frontier models in the industry like the latest and greatest models. We are super excited to see that this is the foundation that you can start with. You will be building on some of the most capable models in the industry, adding your foundational data to that, and enhancing the capability of your very own model to basically produce value for your business needs.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/270.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=270)

### Building Foundation Models from Scratch: Pre-training, Mid-training, and Post-training Explained

Let's talk about how we actually build a foundation model. This hopefully gives you a precursor into how this works from scratch.  Essentially, we start with an empty model. Think of this as architectural scaffolding. This is essentially the structure of the model architecturally. It doesn't contain any data. Then we introduce pre-training data, and this is extensive amounts of data to essentially help the model understand the world, help it reason. I like to use the example of Grey's Anatomy, the medical textbook, not the TV show, just to be clear. Think about all of the foundational knowledge in Grey's Anatomy.

It is one of the most elementary medical textbooks in all of history. It has been updated for the last 150 years, and it is incredible foundational data to train a medical model as an example. Now, let's say you have a company specializing in dermatology. This is where the mid-training phase comes in, and you may want to enhance the model's understanding specifically for skin disorders, skin treatments, and medical protocols for treatment. This is where mid-training can come in. Then the very last stage is essentially fine-tuning, where you enhance the model. This might be your treatment protocols with respect to your company. Maybe you have developed some intellectual property around treating procedures and have an industry-leading procedure, and this is where you can introduce that signal so that your model is essentially super-tuned to the way your business wants to operate. This gives you a relatively holistic understanding using this example.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/370.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=370)

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/400.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=400)

Going into a little more detail, starting with pre-training, we essentially leverage a lot of web content,  long-form context texts like Grey's Anatomy medical textbook. This is a lot of unsupervised learning, so the model will learn its core world knowledge and basic understanding of the world and how to understand it. Then at mid-training, we take the pre-trained model that we prepared beforehand.  Again, leveraging the example I gave, this is where the dermatology example comes in, where you give it specific enhanced information about the dermatology context. You feed that into the model, which gives the model much more competence in that dermatology domain, which is your business domain.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/420.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=420)

Then the last step is essentially post-training.  There are two main parts to this. I like to think about this in terms of dog training. If you think about dog training, there are typically two steps to it. There is the dog obedience school, which is kind of supervised fine-tuning. This is essentially where the dog learns rote experiences and basic understanding about how to behave. However, obedience school for dogs is not necessarily the real world. So the next thing we want to do is think about how we actually help the dog understand and reinforce good behavior in the real world. If we bring the dog to the park for the first time, there are loads of distractions, other dogs, lots of humans, and balls flying around. We want the dog to behave properly. This is where reinforcement learning comes in, and this is where we reward good behaviors. An example might be a treat for good behavior. If they do something we do not like, like running after another dog's ball, this is where we reinforce it with no. This is how we essentially build that super-capable fine-tuned model for your business case.

### Access to Checkpoints Across All Training Phases: Democratizing Frontier AI Development

With that foundation, I am going to pass on to Karen, who is going to talk about the exciting details about how Nova works and how NovaForge works on the inside. Thanks, Karen. All right, thanks Mark. This is the part where we actually start with a bit of a pop quiz to decide how much you have been following so far. So everybody ready for an exam? No, I am just kidding. I think we are just getting into the fun stuff now. Mark gave a really good overview of what it means to build a foundation model. The steps that he described is how Nova was built. Now we are going to talk about why the approach that we are taking with NovaForge is such a game changer and why we believe it is democratizing access to frontier AI for every organization.

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/540.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=540)

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/560.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=560)

 In this segment, we are going to talk through the five key benefits of NovaForge. I will talk about the whys of each one and give you examples of why you should care. We will get into some of the details as well, and then towards the end of the session, we can meet on the side for any further questions that you might have. So first,  access to checkpoints across all phases of model development. What does that even mean and why should you care? Going back to Mark's earlier example with the slides, these models are essentially trained in three high-level stages: pre-training, mid-training, and post-training. What we are giving you with NovaForge is access to every stage of training. We do not want you to start from scratch. We do not want you to start all the way from the left or even from the beginning of pre-training. What we are saying is that depending on the kind of data you have and the volume of data you have, we are creating a paradigm that essentially opens up the training for you to inject the data you have in the right place for the model to learn sufficiently from it. So if you have instruction-response pairs for fine-tuning where you have a lot of samples of what an input and a right output looks like, you can start all the way at the end in post-training and do just fine-tuning.

If you want to do some reinforcement learning, you can start all the way at the end from the post-training model checkpoint and do reinforcement learning on top of that as well.

If the amount of instruction-response pairs data you have is substantial, you can go one step further up. You're thinking, "OK, I have a lot of data, even though it's for fine-tuning, so I really want to learn from this." So you're going to start fine-tuning from the mid-training checkpoint. Now, you might have some unstructured data. The unstructured data might not be a lot, but one of the high-value aspects that we've seen as these reasoning models have come out in the industry this year is chain-of-thought data, where it's really high-value data because you're actually showing these models how to think in your particular domain or think for your business-specific use cases.

That kind of data—intermediate volumes of unstructured data which may not be chain-of-thought data or anything else—if you want to refine the behavior on something like tool use, you can introduce that kind of unstructured data at the mid-training point. But if you have a vast amount of data, you're operating in an industry, a business, an area where either there's just no way the model has seen this data because it's your proprietary data, or it could be in a domain where these models have been trained to some extent because there's a lot of public data on it as well. But these models are general-purpose models, so they just haven't been optimized for that particular domain.

So if you have that kind of scenario, then you can start all the way from the beginning with a pre-training checkpoint. What we give you is a checkpoint that's at the end of Nova's pre-training, Nova's mid-training, and Nova's post-training, so that you can introduce your data in those stages without having to start all the way from scratch.

### Understanding Learning Rates and Training Stages: Why Pre-training Checkpoints Matter

Now you might ask me, "Hey, Mark just showed me a slide, and it said that the pre-training and mid-training stage at the end of it is what's called a base model. Now, I do get a base model from open-weights models. So why should I really care about your pre-training checkpoint? Aren't you just giving me smoke and mirrors?" You might have that question, and that's a good question to ask. So I think we need to get a little bit into the detail of what the training process actually looks like.

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/770.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=770)

The way these models are trained is you toss a bunch of data at them, right? But there's an aspect here that's important to understand, which is the learning rate. The analogy that I really like to use for such examples is think about when you go into school or college or any of those kinds of things. First, there's a warm-up phase. This is your first day in school, your first day in college. You're essentially getting the syllabus, the curriculum, the textbooks for the first time in your hand. You're looking at what this even is, who the professor even is,  how should I approach the rest of the semester? Those are the things that are going through your mind on the first day.

Imagine if I taught you the most important content on that particular day. You don't want to do that. There's no way you're going to learn any of it. So that's what we call the warm-up phase. This is when you actually activate the learning rate. This is more of a visual representation. Different models have different learning rates. Some do exponential, some do constant learning rates, but at the end of the day, there's an aspect or a concept of learning rate.

The learning rate is essentially the rest of your college semester. Now this is the aspect where you're learning. Every day you get some new information, you learn from it, you move on to the next day, learn from it, and this is how the models are trained. So the entire journey of pre-training or retraining is essentially the pre-training with this learning rate because it's not a sprint—you really want to learn everything from it.

At the end of the semester, essentially what happens is what you call annealing. Now, annealing—the analogy I like to use for that—it's like the morning before an exam. You're pulling out your notes, you're pulling out your flashcards, you're trying to focus on all the most important things because you're about to walk into an exam. That's the process of annealing.

In model terms, what we do is whatever is the most important data, what is the most high-value data, it sees it in this stage of training and annealing. The reason it's called annealing and you see the learning rate drops is because at the end of this stage is when we tell the model, "All right, you're good to walk into the exam, you're done learning."

But now this is where it gets interesting. Base models are available today, and if you see where they are, that's the learning wave that you're operating with. So if you have high volumes of data, you really want to be coming in there. That's the benefit of starting with a pre-trained model checkpoint. That's why this is important to you. And then the post-training—we've talked about it in the past—fine-tuning and all those other aspects, LoRA-based adaptation techniques are popular in the industry, but at that point you're doing more post-training and refining the behavior. So if you have differentiated proprietary data, that's essentially where you want to introduce it.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/900.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=900)

### Solving Catastrophic Forgetting: Blending Amazon Nova Data with Your Training Data

Now there's another aspect to this. If you continue with the same analogy, you get access to blend—you get the ability  to blend Amazon Nova-curated training data with your training data in all those stages.

Now, why is this important? Catastrophic forgetting is a term that's said a lot in the industry. Let me explain what that means, whether it's even real, and how Nova Forge helps you solve for it.

Let's take an example using the analogy we were just discussing with an exam the morning before. You're about to walk into an exam in three hours. I come up to you and say, "I know you were preparing for this biology exam, but actually it's a biology plus math exam combined together. Here's a math textbook. Learn everything from it because half your grade is based on that." That's not going to be a great feeling for you, is it? That's essentially what happens when you're doing pre-training or mid-training, where you're introducing a ton of new data late in the training stage.

The problem is you're going to try and learn as much as you can from this new content. The risk is you're going to start forgetting some of the things from the previous content that you weren't learning from. What we're giving you with the ability to blend Nova's data with your training data is the biology and the math textbook as you walk in, so that you can refer to both materials. As you're learning the new content, you still have an eye on the previous content that you learned. With these two textbooks in front of you, you're able to connect the dots between those two materials better as well.

For foundation model development, if you have a lot of proprietary data, let's say you're bringing in data for the healthcare domain, you don't want to forget everything else. These models have been trained to reason. You want these models to reason in your domain as well. You don't want to lose out on all the foundational skills just because you're introducing your proprietary data. That's the value of Nova's data. On the pre-training stage and mid-training stage, you get the ability to use Nova's data, mix it with your data, so that you can mitigate such aspects. At least the general purpose foundational capabilities, you're not sacrificing that just to be able to build your own foundation model.

The same thing applies on the post-training side as well. An example I like to give is on the fine-tuning side. Is everyone here familiar with instruction following to some extent? Instruction following is an aspect where you ask a model something and it follows your instructions. That's fundamental for any large language model out there today, regardless of what kind of use case you're using it for. You want it to follow your instructions. That's what you're doing at the end of the day.

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1080.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1080)

When you introduce new data in fine-tuning, yes, you want it to learn what the right output is for a given input. But you don't want it to forget how to just answer questions and answers, right? You don't want it to forget the basics even on the fine-tuning side of things. That's why we provide the ability to use Nova's data across every stage of training, because it's important not to forget the foundational capabilities with the new information that you learn in each stage. 

### Amazon Stores Foundational AI Team Results: 10% Performance Boost in Shopping Domain

I just threw a lot of theory and concepts at you. You might be thinking, is this person just making stuff up? Where's the proof? Let me give you an example. Internally, the Amazon Stores Foundational AI team, which sits within the Stores organization at Amazon, is one of our internal customers. We took this approach to them to see if we had anything here. The Stores Foundational AI team naturally works with a ton of use cases in the shopping domain. No foundation model out there has been optimized for a vast variety of use cases in the shopping domain. They set out to create a foundation model that addresses all use cases as widely as possible in the shopping domain. They don't care about other domains. All they want is for this model to be really good at all the tasks in this particular domain.

We tried all approaches that were out there, all the way from simple fine-tuning to continued pre-training and fine-tuning and all those approaches with open weights models as well. Then we took Nova Forge to them. What they found, and this is what I like to emphasize, is the two key dimensions. When they took this approach of starting with a checkpoint from Nova, and the checkpoint is the stages that I mentioned, they started from a pre-training checkpoint because they have a ton of data. Not only were they able to improve the performance by about 10 percent on their current production model baseline.

The MMLU benchmark, for those who are not familiar, is Massive Multitask Language Understanding. So across a wide variety of tasks and understanding the language of that, just understanding the language. There's a benchmark called Shopping-domain MMLU for multitask language understanding in the shopping domain. They were able to boost the performance by 10 percent on what they currently had with similar approaches with other models. Not only that, they were also able to boost the performance on just standard language understanding. This is the example of when we talk about instruction following and maintaining the foundational capabilities that I was talking about. They were able to not worry about regressing, but they were actually able to even boost the accuracy further on the general purpose aspects because you don't want these models to forget about things like instruction following.

[![Thumbnail 1210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1210.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1210)

Now we've talked a bit about pre-training,  mid-training, what Nova Forge has to offer for you there. A little bit on the fine tuning side of things as well, because you can use Nova's data there too.

### Reinforcement Learning with Custom Environments: From Reward Signals to Multi-turn Orchestration

Now, on the reinforcement learning side of things, the reinforcement learning progress this year is what's really been key for these foundation models to get really powerful on general purpose tasks. So when we say we want to give you the flexibility and control and democratize AI, we want you to be able to introduce not just your data, but also your environments to train these models.

On the left hand side, what I have is what is today commonly referred to as how we do reinforcement learning—just the industry standard way. For customers who are just getting started or still doing light reinforcement learning, the way it works is we are essentially trying to reinforce good and bad behavior. You don't need a lot of good data or bad data, but as long as you have a way to curate some form of reward signals—rewards being thumbs up, thumbs down, this is good, this is bad, this is a 2 out of 10, this is an 8 out of 10—you can start with reinforcement learning.

There are a few different approaches or ways to go about it. It can be as simple as starting with a regex pattern, because you might want to do some pattern matching and give some 2 out of 10, 8 out of 10 kind of scores in training. You might want to take another step further and introduce or use some sort of Python functions and write some custom code to say, OK, if these situations happen, weight this much; if these situations happen, weight that much.

Or if it's not something that's deterministic in that way, you can take the LLM as a judge approach. You can go on what we call verified and non-verified reward side of things. You can also do non-verified rewards, but it's not as simple as if this text matches this text, then give it a 2. If this text matches this text, give it an 8 out of 10. It's a little more subjective than that. You're getting into the nuances of if I ask for a summarization and the text is 4 sentences long, that's a 2 out of 10. But if the paragraph is 8 sentences long, the summary, then that's an 8 out of 10. So you're trying to get into the nuances of that.

In this industry standard, you do have a lot of options to do reinforcement learning today that are really good. But when we talk to customers, we realized that that's just not enough. At the end of the day, all of this is in a managed environment. You need to upload your regex code, you need to upload your Python code, and the training environment is all managed for you.

But imagine you're a lab where you're doing drug discovery as an example. It's not as simple as uploading Python code to verify if the molecular properties that are being generated by this model are accurate or not. It's just not that simple. These scientists who are in labs have, let's take as an example, some sort of simulations or some sort of tools or chemistry toolkits that they have in their own internal environment, in their own internal labs that they need to somehow now replicate in the form of Python functions. That's just never going to happen.

So what we're also introducing as part of Nova Forge is we want you to be able to use your own proprietary environments, not just data, to train these models as well. There are two ways to go about it. You can bring your own endpoint. So the simulation example that I mentioned, if you want to make an API call into your environment, get the reward calculated score there, send it back, you can do the training loops that way.

Or you can go something even more complicated, especially in the world of agents. It's not as simple as one hop. It's not rewarding things on one step at a time. It's rewarding one function based on that output, the next output, and the next output. If you're doing coding related tasks, it's not input and output. If you're doing robotics, it's not input and output. It's pick up this package and then move to the next package and drop it off somewhere else. There are multiple steps involved in this process as well.

So Nova Forge, what we're introducing, is also the ability to bring your own orchestrator. What this lets you do is not only plug in your real world environments. So if you have a robotic simulation environment, you can plug that in for sure. But we'll also let you use your own orchestrator, so you can actually control the rollout.

Now, what does the rollout mean? When the output of the model comes into your environment, you decide what happens in your environment, send the score back, and then you can actually do reinforcement learning across multiple turns by being able to use your own orchestrator as well. So across all the avenues that you have—pre-training, mid-training, post-training—we're giving you as many keys as possible to essentially help you build your own foundation model using Nova as a base.

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1480.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1480)

Now, you might say, cool story, wow, is this really valuable, which would be a good question.  So the example I like to use is from Cosign AI. They were our design partners. They're a start-up based out of London. You should check out their work. They're doing some really great stuff pushing the boundaries on frontier AI for the coding space specifically.

They were the ones who helped us design this because of the aspects that I mentioned. They have an internal tool calling harness that they use in their production environment to sort of generate code. Now they already have these tools to generate the code.

So why not use those tools for training as well? The tools already know what's good and what's bad. We set up this multi-pattern experience working with them on what such a thing would look like so that in the training loop itself through an API-based approach they can orchestrate the rollout on their end. The call goes into their environment, they can call a specific tool, take the next step, call a different tool, take the next step, call three different tools, do some sort of nested learning, and then send the reward signal back that's used for training.

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1560.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1560)

We want you to use your own data for training, and we want to be able to use your own environments for training as well. This scales across industries. Whether you're doing manufacturing, robotics, or science experiments in the lab, across every industry there's a scenario where if you're able to use your simulation environments and your environments in the training loop itself, you can go a lot further.  That's the training process, but Amazon Nova affords us a lot more than just access to checkpoints and data.

### SageMaker Hyperpod Recipes and Responsible AI: Optimized Training with Safety Controls

The fourth aspect that we talk about is SageMaker Hyperpod recipes. What are SageMaker Hyperpod recipes? The recipes are essentially an abstraction on the training code itself because we don't want you to have to be an expert in ML or AI to be able to do this. We want to democratize AI and democratize access to frontier AI. We offer config files that sit on top of training code so that you can configure every aspect of training through these config files that make it really easy to operate in these environments. Those are the push-button recipes, and the other aspect is these recipes are optimized to maximize performance lift with Nova.

[![Thumbnail 1610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1610.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1610)

The Hyperpod recipes are designed to make it super easy  to essentially execute training runs where all you have to do is select a recipe. A recipe is something like, I want to do pre-training, mid-training, or fine-tuning. You select a particular recipe for mid-training or fine-tuning, you specify the training data in the config file or in the recipe, and you hit run. It's as simple as that.

We know that customers of different types prefer different experiences. If you prefer a CLI-based experience and want to be in the notebook working in the details, you can do that. We make it easy to do that with these recipes. Or if you want to execute a large number of experiments at scale just to quickly get started, the CLI experience is great if you want to orchestrate things, commit code back to repositories, and maintain pipelines. But even if you just want to execute these runs very quickly to try a few different things, we make it super easy for you to do this.

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1680.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1680)

[![Thumbnail 1700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1700.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1700)

An illustration here is all you have to do is select the checkpoint that you want to start with, enable the data mixing with Nova data, and these recipes already come preconfigured with Nova's data ratios to essentially help you quickly get started. It's not just the training part. We want to make the end-to-end lifecycle super easy.  In the same UI, once the training run completes, you can trigger evaluation runs, you can trigger model deployment to an endpoint, or you can even continue customization with the model that you just trained. If you fine-tune a model and want to continue fine-tuning that same particular model, you can do that too, all from the UI. 

I said these recipes are optimized to maximize performance with Nova. What does that mean? This is also another big differentiator compared to what's available out there with the open models. Checkpoints are available, especially from mid-training and post-training, but what we're giving you is these recipes are built on top of the code that was used to train Nova itself. This is the same code that was used in building the Nova Foundation models as well.

When you use your proprietary data with these recipes, these recipes are already optimized to maximize the learning from the data that you're now introducing. These recipes are also refined in research collaboration with partners. Once we make these available, we provide you with smart defaults so you don't have to spend too much time optimizing the recipes themselves.

[![Thumbnail 1750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1750.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1750)

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1770.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1770)

I love giving examples, and I'm going to do that in this situation as well.  Consider a scenario where you're trying to build a foundation model that's an expert in a domain-specific language. An example would be if your company has developed some proprietary coding language and you want this model to be really good at that. In that particular example, we  actually partnered closely with the Computer Science and AI Lab at MIT to optimize the recipe for something like that. They were trying to build foundation models that are really good at metamaterials design, but at the end of the day, the metamaterials are nothing but just proprietary code.

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1790.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1790)

Let's take Appian as an example.  Appian was a customer in beta. They have their own proprietary language for process automation. Their current production model was Sonnet 3.5. They've been experimenting with Sonnet 4, but what they found is that by fine-tuning Nova, not because of the optimizations that they've made, they're actually projecting exceeding the performance of Sonnet 4, but while benefiting from the cost and latency aspects of Nova, that makes it cheaper and faster to essentially achieve the same kind of accuracy results.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1820.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1820)

Last  but not the least, it's not just the training, it's not just the experience, but it's also the safety and the responsibility aspects of these model trainings. We want you to be able to configure these to meet your business needs as well.

So the training data that I mentioned that you get from Nova to involve in training, you do get access to the responsible AI and safety data that we make available that we use for Nova's training as well as part of it. You want to evaluate your model on Nova's responsible AI dimensions, you can do that as well. We provide the same runtime controls that ensure that Nova is, we prioritize safety and responsible AI for Nova, so you get the same runtime controls to ensure you can do the same with your model as well.

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1860.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1860)

And it's not just the controls we allow you to configure  the controls to meet your business specific requirements as well. Say you're a security firm trying to push the boundaries of what you can do for cyber testing. You want to make sure that if you're testing for cyber attacks, you're able to do that. You want these models to be able to produce that. The general purpose foundation models that are out there will just block you because they're trying to protect general purpose use cases.

Or if you're in media entertainment, you might own some proprietary characters. These models would block you from generating that because generally they don't want you to block it. But if you're a company that has a lot of proprietary characters, you want the model to be able to generate them. You don't want the models to be blocked. So even on the responsible AI and safety aspects, we want you to be able to adapt these models to your business specific needs.

### Reddit's Journey: Rethinking Online Safety with Nova Forge for Democratized Content Moderation

Next up, I talked a lot about the things and I try to give examples as much as possible. Rosa Català, who's been an incredible partner through the entire beta process, will tell you why this mattered for Reddit. Hi everyone. I'm Rosa Català. I'm a distinguished engineer at Reddit. And today I would like to share what has been our experience in rethinking online safety by building our own foundation model using Nova Forge.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/1950.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=1950)

So let me explain why we are here, why we were interested in partnering with the Nova team for our content moderation and safety operations.  One of the main goals that we have is to democratize online safety across the entire platform. The problem that we encounter is the coverage lag and the scalability in getting to that point. The best way of thinking about it is in traditional or legacy ML models, how they learn. They learn on history. Therefore, you're always playing catch up when you are facing a new emerging trend in the platform, new communities, new slang.

The models have been trained on past data, and they remain blind until it's too late. So what is the solution to this? The solution is hyper-personalization in the way we protect and we support our communities with our safety capabilities. But what that means is that we will need to train 100,000 models to serve our communities, more than that, in fact. Because on top of serving every community in the platform, that's democratizing safety at Reddit, it will require also that you make these models really performant, so you will have also to invest on fine-tuning, on making these models very performant for specific tasks.

So we have a goal, we have a constraint. What is the role of Nova Forge in our journey to try to get at the democratized online safety that we are striving for? What Nova Forge unlocks for us is breaking this trade-off between specialization and scalability. How we can inject our rich graph-based context that we have at Reddit, about what users intend, what moderators need, and how communities react.

At every point of the training and creation of your own frontier model, exactly as Mark and Karen showed, we can start using different levers to create a model that is an expert but generalized to new events.

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2160.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2160)

So how do we do that? Let me explain where we start. At Reddit, we are in a situation of maturity and safety. We have moved past keywords and simple rules. We use proprietary and sophisticated models to protect our platform. The problems are not the models or even their performance.  The problem is how we use them. Because all those models are going to require a rich context awareness that they don't have, because most sophisticated state-of-the-art AI models have not seen our proprietary data. They don't know about the reactions and the interactions between users, moderators, communities, and participants. But we have this data.

That is one gap that we have, and we call it the knowledge gap. A model native to Reddit, or a model that speaks Reddit, is not available yet. The second gap is whether we can reduce the dependency on fine-tuning for more performance models. The importance of doing this is twofold. We need to avoid the very hard operational wall that is to scale all the nuance that we present to the models. The second, and I think this is the most critical from a safety standpoint, is that we want to be able to protect our users at all times.

If a new threat emerges, we detect it, we need to gather data, we need to train or fine-tune a new model or adapter, and then we deploy it. It can take days to weeks, and in that time our users are exposed. At Reddit, we have adapters we call shields that protect our users from spam, from harassment, from violent content, and many more. Even if I want to also put the context of how that will play out with the norms and rules of our site, this makes it incredibly complex.

[![Thumbnail 2300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2300.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2300)

So then we call in the superheroes we call Nova.  And we say, look, let's start with measuring performance. Can we use Nova while maintaining the performance that we need to protect our platform with the highest level standards? What we need is to start with measurements and evaluations that have ground truth. We have data that explains how moderation works, how moderators in particular and safety teams at Reddit work, and how communities react to different types of content.

We also have this ground truth that optimizes for the reality of Reddit, not for an external general benchmark. Once we have these evaluations and measurements, we go to the refinement stage. We didn't start back with pre-training; we started with supervised fine-tuning. The supervised fine-tuning was using what we call incremental learning. We said, look, content moderation is incredibly complex and it is not a simple zero and one. So what we did is teach the model incrementally, the same way you teach in a classroom. You teach the model the fundamentals, and then you start graduating the model as it learns, increasing the model complexity gradually, moving into more complex ones.

This is our first checkpoint. The second and third point for us was: can we now cover a knowledge gap? The knowledge gap involves thinking about pre-training, starting with a pre-training checkpoint and asking whether Reddit data can be mixed with Nova data. We see that the model learns to become more capable of understanding, reducing the intensity and frequency of labels and fine-tuning compute that you need afterwards.

This is how we started collaborating and partnering with the Nova team. It's important for us to recognize that Reddit is about humans at all levels. Humans interact in conversations, and humans interact by indicating content they like and content they don't like. We have tons of labels that are not just explicit labels but implicit labels, which is where unsupervised learning becomes so scalable for Reddit.

In terms of the fine-tuning work, what we are learning with Reddit and Nova is that we can scale our human-in-the-loop operations. When we extract answers from Nova, we don't aim for safety detection for complex generation. What we are looking for is classification and representation of the data with what we call uncertainty awareness. This allows us to rank all the responses of the model and prioritize what our AI Ops needs to review to provide feedback to the model, reinforce the model, and use post-alignment in the most efficient way.

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2510.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2510)

Let's talk about results. In the first stop of the journey,  on the left, you have the supervised fine-tuning. We are looking at how the model has been fine-tuned with this curriculum supervised fine-tuning and uncertainty awareness. We compare that against our Reddit proprietary ground truth. What you can see is that we were able to reduce the number of pipelines while maintaining or increasing performance.

Across the number of pipelines that we reduced, we were able to increase precision by 26 percentage points and recall by 16 percent. That's remarkable. But we're not only doing that. We are also reducing the amount of inference. We're making it fast and cheap with this uncertainty awareness that I mentioned, and we're also reducing the number of fine-tuning compute and fine-tuning pipelines that you have to call in inference.

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2560.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2560)

We have a checkmark here because we were able to avoid the model-per-task bottleneck that fine-tuning and export models create. We can go back to the idea that we can democratize online safety by being expert but with a fraction of the cost of computing and calling these models. But that's not enough. We went then to the pre-training stage. What we try to do is what I said before: can the model learn more nuanced context by going a step back? That's important because all the implicit labels that we have on Reddit can help us. The question we're trying to solve is: if we mix Reddit data with Nova data, 

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2770.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2770)

because the Nova pre-trained checkpoint is smarter for Reddit use cases, the answer is yes. We were able to ground Nova on Reddit intelligence and obtain what we call a superior model for two reasons. It is able to achieve a 25% reduction in missed threats, which is massive for safety operations. It means that we are a more resilient platform. It also means that if you see the precision and recall trade-off, it becomes more flexible. We have higher sensitivity. What that means is that the model now is not just guessing keywords; it is understanding what content violation represents and means at Reddit. 

I think now we are on a path in which we can combine the power of continuous pre-training and fine-tuning, supervised fine-tuning. What we expect to obtain is increasing benefits, compounding the benefits, leading to this democratized online safety that supports scale to the long tail without drowning us in engineering. I think we can go a step further though, as Karen was detailing just before me. What you can take now is these pre-trained models, these fine-tuning models, and combine them with the post-alignment phase.

This means that now if we want to create hyper-personalization that is already aligned with the policy that we are learning from the ground up already with safety, we can create new, more exciting user recommendations and ads ranking. I think then we can talk about a Reddit Nova Frontier model. That is a journey we are on. We are very grateful to the Nova team for allowing us to understand the capabilities of what they built, and it is really exciting. Karen can talk about other users and other customers that have been already interacting with Nova Forge.

[![Thumbnail 2890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2890.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2890)

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2910.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2910)

### Customer Success Stories Across Industries: From Drug Discovery to Financial Services and Legal Research

Rosa explained how and why Forge is important for Reddit.  I will quickly get through a few other examples as well, just to show you that it is not just about one specific area or one specific domain, but in every domain, in every business operation that you are in, there is value that you can unlock by building a frontier AI model that is purpose-built for your own business. 

Let us take an example of Nimbus Therapeutics. Their use case is to build a drug discovery assistant. They are a small startup and they want to see how AI can help them scale and bring better medicines to patients faster. The use case was to build an LLM that can help them with pharmaceutical R&D and drug discovery and much more. The approach that they tried with Forge was supervised fine-tuning and reinforcement learning. The results that they achieved were similar in theme to what we saw with Reddit as well.

[![Thumbnail 2970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/2970.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=2970)

They were able to converge several of their graph neural network models, which are actually specialized models for scientific tasks, into one model that was built with Nova. Not only were they able to converge those things, they got a model now that outperforms any other model out there, any large language model of any size for their particular use cases, while giving them the latency and the cost benefits of operating at Nova's point. 

Let us take a similar example in a different domain: Financial services. I like this example a lot because it combines two things: not just expertise in financial services, but specializing in a different market as well. Nomura Research Institute was building a financial services LLM that is an expert for the Japanese market specifically. The approach that they took was continued pre-training, followed by the rest of the post-training process, combining their proprietary data with Nova's training data. The reason they were doing this was they were trying to evaluate this as an alternative to what they are currently doing with open weights models.

The results that they saw was that they were able to do this and far exceed the performance that they had with a similar approach with open weights models.

Let's get into a different domain. Let's take the Sony Group. The Sony Group has started to drive AI business transformation across all of their business units. As you can imagine, there's a lot of diverse businesses, a lot of diverse operations in a bunch of industries even within just Sony. They want to build a legal research assistant that can help accelerate some of the processes and help them scale the AI adoption process itself.

[![Thumbnail 3030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/3030.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=3030)

What they found was that through reinforcement learning, they were able to exceed the performance of larger language models. This is why I say it's not about always just doing pre-training—it's about entering the stage that's right for your business-specific need. Through reinforcement learning for this specific use case, they were able to exceed performance of the larger language models while benefiting from Nova's price and latency performance. 

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ea260e2fee0bf03a/3080.jpg)](https://www.youtube.com/watch?v=osY67gy-BT4&t=3080)

In this case as an example, they were able to exceed all of the performance that was out there from the largest models available as well. This was specific for a legal use case.  This is a theme I hope you take away from here: this is not something that's difficult to do. The end goal here is we want to democratize access to frontier AI for each and every business, for each and every organization.

We've tried to make this the easiest and most cost-effective way to do so with the Amazon Nova Foundation models. A lot of customers are already building with this. We hope to see you leave from this session and start building with Nova Forge as well. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
