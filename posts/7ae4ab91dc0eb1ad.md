---
title: 'AWS re:Invent 2025 - Fixing AI’s Confidently Wrong Problem in the Enterprise (AIM269)'
published: true
description: 'In this video, the speaker addresses AI''s critical flaw of being "confidently wrong," which prevents business users from trusting AI systems with their data. Drawing from PromptQL''s approach, they demonstrate a solution where AI explicitly indicates what it knows (blue links to wiki entries) versus what it doesn''t know (red links for assumptions). When AI encounters unknown concepts like "FY," it makes transparent assumptions and allows experts to join conversations to clarify, creating a learning loop that captures tribal knowledge. The system maintains a collaborative wiki that both AI and humans update, enabling continuous improvement. This approach allows users to trust AI even at 50% accuracy because it admits uncertainty. The speaker emphasizes that traditional text-to-SQL products fail because they require technical verification, whereas their system scales to handle 100,000 tables and 10,000 metrics by establishing this trust-and-learn product loop first.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/0.jpg'
series: ''
canonical_url: null
id: 3088049
date: '2025-12-06T03:43:54Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Fixing AI’s Confidently Wrong Problem in the Enterprise (AIM269)**

> In this video, the speaker addresses AI's critical flaw of being "confidently wrong," which prevents business users from trusting AI systems with their data. Drawing from PromptQL's approach, they demonstrate a solution where AI explicitly indicates what it knows (blue links to wiki entries) versus what it doesn't know (red links for assumptions). When AI encounters unknown concepts like "FY," it makes transparent assumptions and allows experts to join conversations to clarify, creating a learning loop that captures tribal knowledge. The system maintains a collaborative wiki that both AI and humans update, enabling continuous improvement. This approach allows users to trust AI even at 50% accuracy because it admits uncertainty. The speaker emphasizes that traditional text-to-SQL products fail because they require technical verification, whereas their system scales to handle 100,000 tables and 10,000 metrics by establishing this trust-and-learn product loop first.

{% youtube https://www.youtube.com/watch?v=HFOdQbWhg5c %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/0.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=0)

### The Core Problem: AI's Confident Wrongness and the Trust Gap

 I'm going to talk about a big challenge that we've seen over the last year that you've read about. I don't know how many of you saw the MIT 95% Pilots Failed report, but it was all over the news a few months ago. One of our key takes on the problem is that the big issue we're not really solving for is this problem of AI being confidently wrong. What do I mean by that?

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/40.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=40)

AI has been around for  223 years now, right? We've all been using it. I don't think ever in the history of any new technology has gotten close to a billion users in such a short period of time. It's kind of insane. But if you think about the science fiction reality of us as technologists going to our business people and saying, "Here's AI. Go use AI and make decisions. Ask questions on your data and say things like make decisions that will improve the business, make better staffing decisions, make better pricing decisions, make decisions that will improve the business in a meaningful way, in a way that you couldn't have done before." That's what AI can give you. It's finally accessible.

The big challenge there is that you're still not able to use AI connected to your data in a way that you can trust. Almost all of us would have tried some kind of AI on data project, right? You take an LLM, you connect it to a database, whatever connectivity technology you use, and you can make it work. But when you are on the business side and you ask a question that has loaded impact, you know for a fact that you're not going to trust what it says. The reason is because you know that it's going to be hard for even a human to answer those questions, but at least the human would tell you that they don't know.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/140.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=140)

 If an AI tells you something, it's just extremely confident, and you have no idea whether you trust that confidence or not. If you think about this ability that AI has of just being confident all the time and its impact on being confidently wrong, that is what really hurts adoption as you start to use it. It goes from a fun project to I can't actually trust this to do anything useful. Is this a better models problem or is this something that we can do something about? Can we solve this problem ourselves?

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/160.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=160)

I'm going to share with you our learnings as we've built an AI product on how we tackle this problem, and hopefully some of those learnings are useful to you as you think about your projects. That gives you some ideas on how you can tackle this problem.  How many of you listen to Dwarkesh Patel's podcast? For those of you who don't, you should listen to it. It's got a bunch of really fun stuff. I think the most recent one is by Andre Karpathy and then by Ilya, who is the co-founder at OpenAI, talking about a bunch of topics around learning and continuous learning and utility of AI.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/190.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=190)

 This is something that came out from his blog post a while ago, and I really like this quote: the reason why a human being is useful in your business is not because they have 500 points of IQ or because they win the IMO gold medal every time you talk to them. It's not their raw intelligence. The raw intelligence kind of was passed by LLMs a while ago. It's not the raw intelligence that makes them useful. It's this ability that they have of picking up context on the fly, of looking at a failure, thinking about it, and fixing that failure. It's this ability of incrementally improving that you can trust the human to do.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/270.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=270)

Even though the human is not as smart as Deep Seek V3, which just won the IMO gold, somehow the human is still more trustworthy.  This ability to continuously absorb tribal knowledge and improve is critical. But this is obvious. We all know that we want AI to continuously improve, learn tribal knowledge, and do whatever. The key point that I want to share is you can only teach something when it says it doesn't know. That's the critical thing that's missing in AI products. That confidence, being confidently wrong, hurts you. Even if AI got really good at learning continuously, you can only teach something when it says it doesn't know.

The problem is that if AI doesn't tell you when it's wrong, you can't teach it. And if you can't teach it, you can't use it. That's the core product loop that's missing with AI products.

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/330.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=330)

### PromptQL's Solution: Teaching AI to Admit What It Doesn't Know

Let me talk about what we do at PromptQL and show you a demonstration of how we approach this problem.  Broadly, we're yet another chat with your data product. It's fascinatingly amazing and new and differentiated compared to everybody else here. But the idea is to say that if users are interacting with data, they typically talk to multiple people to get some kind of insight. When you want to solve a problem or make a decision as a business user, you talk to an analyst, you talk to a scientist, you talk to an engineer. They talk to each other, they talk to data, something happens, some complexity ensues, and you get the result at the end of the day.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/370.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=370)

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/380.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=380)

Most often when you ask a question, you'll probably get the result like two weeks later, by which time the question is not very important.  The promise of AI is that we'll have this cool AI thing that sits in the middle that users will use.  The AI will do everything that humans do. The AI will do data engineering, data analysis, and data science, and it'll compose all of it magically and give you an answer. By the way, this might have sounded like rocket science a year ago, but today you can see very clearly that it's already possible. You can take cloud code and make it write a pipeline for you, then make it write a SQL query for you, and then make it write a data science program for you. It's already within reach. It's already possible.

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/430.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=430)

The thing that's missing, though, is that you can't actually expose it to end users in a way that they can trust it. If you look at all of these chat with data products, ultimately they all turn out to be text-to-SQL products where you need the technical person to look at the SQL to verify it. So you can never give it to a non-technical person because a non-technical person can't read SQL.  How do you trust it? You can't trust this AI system.

Our operating model is to get AI to admit that it doesn't know so that experts—the red boxes—can teach it. So the AI gets better. But most importantly, because the AI can tell you that it doesn't know, even on day zero when it's only fifty percent accurate, it's fine. Users can trust it because if I can ask you a question and you tell me that you don't know, that's great. I'll ask you two questions and you tell me half the time you don't know, that's totally fine. I can still use your product.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/480.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=480)

I'm going to demo how we do this in product.  This is the core loop that I'd like you to watch out for. The first part is to tell you what it knows and what it doesn't know. That's where the cycle begins in the product. The second piece of the cycle that's critical is that once it tells you that it doesn't know, you can ask it to fix its stuff. And then once you do, it learns. These are the three pieces that I'd like you to anchor on.

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/530.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=530)

I'll do a recorded demo and then I'll do a live demo because live demos are fun and dangerous and who knows if they'll work. But let's say, for example, you ask a question: "What was GM broken down by region yesterday?"  What our system does is first create a plan to solve the problem. But you notice that it highlights in blue links what it knows and where that knowledge comes from. Internally, if you as a user go click on that blue link, that is actually a wiki that powers the AI internally. This wiki contains concepts like gross margin that are pretty flexible. Let me roll that up for you.

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/580.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=580)

The gross margin concept, for example, says this is what gross margin is, this is how you calculate it, these are what our targets are for this quarter, these are what expected range of values could look like.  And this is how you calculate it. That's a wiki entry that backs this. And just like a wiki, there are multiple concepts that are connected to each other. That's the core piece of how it works. But what happens is this is fine—we all have this today. You have some kind of knowledge base, you refer to citations on the knowledge base, and you make it work.

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/610.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=610)

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/630.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=630)

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/640.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=640)

That's straightforward. What's interesting is when it doesn't work. So you ask, "What was GM last FY?"  The first thing the AI does is tell you that FY is a new concept, something not known in this domain. That's a red link that the AI will make an assumption about . It makes an assumption and tells the user what that assumption is. Then the user sees the plan and looks at the answer . When I look at this answer and the value, I think it's fine. But I also don't really know what the fiscal year is. This is the most common thing you'll notice when business users use the product. They'll say, "Thank you for telling me that you don't know something, but true facts, even I don't know. I know we have a fiscal year, but I don't actually know what the exact period is because we're a global MNC. Is it different in different regions? I have no idea."

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/670.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=670)

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/680.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=680)

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/690.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=690)

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/700.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=700)

This is where it becomes critical . You bring another person into this conversation—an expert or a group of experts—to say, "Can you clarify what fiscal year means?"  This person joins the conversation. This is an actual human being, an expert who joins  and says, "Well, our fiscal year is whatever, whatever, whatever." The AI responds, "Cool, that fills my gap. That fills the assumption that I had," and it redoes the plan and gives you an answer . This was really important for two reasons. First, the user is actually happy because they got an answer that was right. They did something, it wasn't accurate, they brought in an expert, and they got the right answer. This is what I would have done even without AI. It's not surprising; it's natural to me.

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/740.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=740)

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/760.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=760)

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/770.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=770)

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/780.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=780)

But what's really nice is that this entire conversation contains so much tribal knowledge that just got captured. This conversation is the seed of learning. So there's another agent loop  that helps you create a wiki entry and updates the wiki based on what just happened in this conversation. Because it's a wiki, literally like Wikipedia, AI and humans can collaboratively maintain that wiki and keep getting better over time so that you can use it . Then in the future, let's say you ask a question: "What was the GM last fiscal quarter?"  That's the key capability where you can also reference these advanced concepts in the wiki despite what users are saying . This is how you surface it up.

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/790.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=790)

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/810.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=810)

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/820.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=820)

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/840.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=840)

Let me take another example to show you how we think about what goes in the wiki . This is another example of a finance dataset. I can say, "True North Revenue for Q3, also forecast Q4" . You can literally imagine what it's doing in the background. It's going to the True North Revenue wiki article . It's reading that, and the wiki article is literally just a wiki. So it says, "Go to this table, this thing is in Google Drive, it's a PDF you'll have to extract from a PDF, subtract adjustment. What is an adjustment? Go read up about an adjustment, learn what an adjustment is, read up about all of these things" . That's what it's doing for this first step of the plan.

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/850.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=850)

In the next step , what it'll do is try to read what forecast is. Forecast is a wiki entry that says, "Do this data science thing, these are the outliers that you need to think about, whatever," and these are progressively maintained. You can imagine that these are sparse and they get more and more structured, they get better and better as the knowledge accumulates. Whereas initially it's just basic revenue forecast, use this model, then later somebody used it and said, "Nope, Black Friday bad outlier, please factor that in." Maybe a data science expert comes in and teaches that to you. But that powers the ability to create an accurate plan.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/890.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=890)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/900.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=900)

There's another really important piece here because the uncertainty and lack of accuracy  is not just semantics and business concepts, but it's also in the details of fairly specific technical problems .

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/910.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=910)

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/930.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=930)

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/940.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=940)

What we also surface is the fact that the forecasting methodology assumes that something should be consistent.  It should assume that there are no refunds. Now, whether or not that is important to you is something that you can discuss, think about, and bring in an expert. They can say, "Yeah, you know what, this assumption was good," or "This assumption was bad. Let's not do this again." So you teach it, fix it, and the learning loop improves.  The way we think about our product is that for non-technical users, we surface the wiki. For technical experts, we surface it in what we call the confidence analysis of the confidence score.  Users are expected to think about the wiki, and experts are expected to look at the confidence score. That creates the loop.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/950.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=950)

### Scaling Beyond Traditional Limitations Through Continuous Learning

So that is a quick look at what this looks like.  What we have observed, and this is kind of our core pitch, is that whatever AI you are using today is doomed for failure. It will fail. The reason why it will fail is because you have probably not managed to get 100 percent of the tribal knowledge into your AI anyway. How can you? It is not possible if you are a business of any respectable size. It is not even possible to try to embed that tribal knowledge. So you are doomed for failure because not only do you not have knowledge, but you also do not have a system where people can trust what your AI is doing.

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/960.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=960)

 What we want to get you on the path for is to say it will always be inaccurate. Can we tell you that it is inaccurate? And then can we learn? So we put you on this path of actual adoption, and that is what has allowed us to scale massively. It has allowed us to focus on solving interesting technical problems. For example, the fact that our AI system is not restricted by the size of the schema. If you look at things like Databricks Genie or Snowflake Cortex, they have, you know, 25 tables of AI on data. Who has 25 tables? What enterprise has 25 tables that you work on? That is not even a problem. That is not even real.

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/1010.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=1010)

Of course, it is a problem because you cannot squeeze that much in context. So we have focused on solving problems like 100,000 tables and 10,000 metrics. How do you solve for that? But you can only start to solve for that once you fix the product loop. You have to say, whatever the size of your schema is or whatever the size of your context is, unless you have that improvement loop, you cannot even get to those interesting problems. But that has allowed us to scale.  Since we started early this year, we have scaled across a bunch of really interesting use cases where there is a lot of high-velocity decision-making happening.

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7ae4ab91dc0eb1ad/1080.jpg)](https://www.youtube.com/watch?v=HFOdQbWhg5c&t=1080)

Come stop by our booth at 1733 if you want to learn more.  We have got a bunch of tech folks and a bunch of sales folks. If you want to buy this and use this, I will never say no. But also, if you just want to stop by, exchange notes, and learn, please pop by, and we can chat more. Thank you for your time.


----

; This article is entirely auto-generated using Amazon Bedrock.
