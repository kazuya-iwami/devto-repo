---
title: 'AWS re:Invent 2025 - LPL Financial and Incedo: Accelerating Advisor Success with AI & Cloud (ANT102)'
published: true
description: 'In this video, Nitesh Ambastha from LPL Financial discusses modernizing wealth management through cloud and AI. He explains how AI can reduce advisers'' administrative burden from 60-70% of their time, enabling better client relationships. LPL aims to leapfrog competitors by implementing agentic AI directly without legacy constraints, targeting operational workflows like account opening and KYC-AML processes first. Key challenges include building a modern data ecosystem through M&A integrations (Prudential Financial, Atria Wealth, Commonwealth Financial), ensuring security and trust, and talent transformation toward ML engineering skills. Only 6-7% of AI use cases reach production, making proper sequencing critical—prioritizing Data Foundation migration to AWS, democratizing AI access, and establishing security before scaling solutions.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7d2ed668ad3dff7b/0.jpg'
series: ''
canonical_url: null
id: 3088031
date: '2025-12-06T03:26:07Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - LPL Financial and Incedo: Accelerating Advisor Success with AI & Cloud (ANT102)**

> In this video, Nitesh Ambastha from LPL Financial discusses modernizing wealth management through cloud and AI. He explains how AI can reduce advisers' administrative burden from 60-70% of their time, enabling better client relationships. LPL aims to leapfrog competitors by implementing agentic AI directly without legacy constraints, targeting operational workflows like account opening and KYC-AML processes first. Key challenges include building a modern data ecosystem through M&A integrations (Prudential Financial, Atria Wealth, Commonwealth Financial), ensuring security and trust, and talent transformation toward ML engineering skills. Only 6-7% of AI use cases reach production, making proper sequencing critical—prioritizing Data Foundation migration to AWS, democratizing AI access, and establishing security before scaling solutions.

{% youtube https://www.youtube.com/watch?v=V9FUUGzTcG0 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/7d2ed668ad3dff7b/0.jpg)](https://www.youtube.com/watch?v=V9FUUGzTcG0&t=0)

### The AI Opportunity: Transforming Adviser Experience and Business Models in Wealth Management

 Good afternoon. I'm very happy to be here with my good friend Nitesh Ambastha, and we're going to be talking about modernizing wealth management using cloud and AI. Wealth management is a very interesting space within financial services and highly profitable, but from a technology perspective, it's fair to say it has been a bit of a laggard. That's an opportunity because with all the changes happening with cloud and now AI, there is a real opportunity to leapfrog. Who better to talk about it than Nitesh, an industry leader and senior leader at LPL Financial, and I know this is something he's very passionate about.

We'll structure this in three parts covering strategy to execution and impact. We'll cover the spectrum by starting with laying out what the opportunity is here. We'll then get to the challenges, and then we'll get to solutions, all in the next 15 minutes. Without further ado, let's start with the opportunity. The hero of the story for wealth managers are advisers, and everybody is trying to make the experience better for advisers. There has been this promise about adviser personalization that hasn't been fully delivered, but with AI, how do you see this playing out? How could AI transform the experience and the proposition for advisers?

I think all of us are trying to figure that out. Personalization is very easy if you have only one client and they don't change their mind, but the hard part with personalization has been that it's an ecosystem. It's a system, not a feature or a product. Systems are messy, and the reason systems are messy is because clients are messy. Clients change their mind, they have changing preferences, and so on. Personalization has been a challenge and continues to be a challenge.

The part about personalization has always been that if you're an adviser with all these clients and you try to personalize it for everyone, everyone will have unique demands of you, and that is hard for an adviser to keep up with. Where AI comes in is it takes the heavy lifting from the adviser's plate. Today, advisers spend 60 to 70 percent of their time preparing for meetings, collecting documents, and doing a lot of work that means they're not spending time with clients. That 60 to 70 percent is your attack space with AI. If you can address that, then advisers can focus their time with customers and use their relationships better. That's the holy grail.

With agentic AI, we can automate a lot of the work that advisers are doing manually today or spending their time doing. AI can take on a lot of the heavy lifting behind personalization. That's very exciting. Let's get to the bigger picture of what that implies from a business model perspective and maybe putting the LPL hat on, where you play in the value chain. Everybody in wealth management is trying to be this financial supermarket. How could AI be a bit of a disruptor and maybe help you leapfrog?

Definitely, for LPL, it's a possibility. We were not advanced in a lot of products and feature capabilities like warehouses, and LPL having 30,000 advisers affecting the financial well-being of about 8 million clients can definitely be that marketplace ecosystem. You mentioned the word leapfrog, and we do have that opportunity. We don't have to go through traditional models of building a banking lending product or traditional ways of creating a securities lending desk. You can go straight to agentic and AI-based models. You can skip a generation. As LPL, we can go straight to those opportunities that existing players will take a long time to change their model for. We don't have anything, so we can go straight to the target segment there.

That's an opportunity segment and opportunity space for us. How does that translate? We have our own margin expansion targets, and boards and management teams are excited about it. The trick is to really figure out those use cases. You can't stop the hype, right? If you try to fight the hype curve, you will lose. So you have to pick and choose those areas where you can actually make a difference.

AI is clearly very good at natural language and text use cases, so you would want to deploy it there first and get some value, and then it becomes a flywheel. That's the direction of travel we are after with AI at LPL. We definitely believe that we can leapfrog a generation and close the gap with competitors in terms of capabilities and features much faster. That's very exciting. You talked about customer experience and business model, and that eventually implies revenue and growth. Let's talk about the third big business KPI, which is cost to serve. You mentioned margin earlier, and cost to serve has also been a bit sticky in this industry. How do you see AI really making a difference to cost to serve?

### Bridging the Gap Between Promise and Reality: Key Challenges in AI Implementation

That's actually an opportunity area where we can attack first. Cost to serve has been on our radar for a while, and we have tried to make incremental progress in that space. But what is now possible is you can look at those end-to-end workflow steps in cost to serve, like account opening workflows, commission processing workflows, or simple money movement and how fast you can do those things. Today there are a lot of exception cases and supervision use cases with so many human beings involved in account opening and account transfer scenarios. Those are ripe areas where you can say we can look at this as a completely inside-out approach instead of incrementally making those workflows better.

You don't have to do the typical workflow systems like our favorite workflow products from the past and say, let's look at this completely differently. Until I saw a demo of this live, I couldn't believe it. An adviser or end user is trying to chat to create an account opening or KYC-AML use case, and on the side you can see what the agents are doing. That experience is fully personalized to that adviser. Each adviser office can do this differently and have a very personalized experience on how they open accounts, how they deal with clients, and how they lift that margin up for them.

That results in us having a significant boost in our margin targets. So that's the first area in 2026 we are going to go after, which is starting with the operational transformation area and then learning from that and going after other business lines. It's fascinating that as you're talking about operations, this is not just about cost, but also about personalization and improving the customer experience. So there are fascinating possibilities. It almost feels like being a child in a toy shop. There is so much you can do, but there's also a reality check that I think it's fair to say, and I know that you're very aware of it, that there is a chasm between the promise and the reality.

So what are some of the challenges that you and your teams are seeing in terms of implementing AI and cloud and realizing this vision? There are a couple of stats I'll give you. One is the number of AI use cases truly in production according to McKinsey survey was about 6 to 7 percent, so we should be aware of that. The second stat is getting to a 60 to 70 percent accurate model and result is very fast, but from there trying to get to 80 percent or 90 percent is extremely hard. Knowing that those things are hard, you solve for the difficult problems first, which is something my team keeps reminding me.

I always say solve for the difficult problems first, and that is creating a data ecosystem that is conducive to this. At LPL we have been on this journey of transforming from a monolith to a data product environment where you can have a modern fabric that is event-driven. You can now connect data that we have on platform versus off-platform data, and now you can do those use cases faster. That is hard work, fixing the data ecosystem and getting it ready, but that in itself is not enough. In a financial services firm you have to think about security. You need to think about trust in the ecosystem because our business is all about trust.

Our clients trust our advisers, and that's how they conduct business. So we have to be extremely prepared for trust and security. The technology landscape is relatively new—two and a half to three years old—and the maturity isn't there yet. You want to start in an area where you feel strong and comfortable, not with the most important segment of your business. So I would say those two factors are critical.

The third one, which is obvious to everyone here, is about talent. You have to do a talent transformation. I see leaders who are embracing AI and doing it themselves, and then there are leaders who are telling their teams to do it but not doing it themselves. When I self-reflect, I was one of those second type of leaders, so I've enrolled in a course myself so I can do it myself. Talent transformation is essential, and there are three or four important issues we should discuss further.

### Building the Foundation: Data Modernization, Strategic Sequencing, and Talent Transformation at LPL Financial

Let's talk about data, because you also mentioned choosing the right sequence, and data is very tricky because it can encompass everything and be very hard to work through. In your case, you've had the added complexity of conducting a series of M&As—at least one every year. What I found very interesting is that you've used that challenge from a data perspective as an opportunity to actually upgrade your data foundation. Talk us through how you've approached this whole data journey, especially with this focus on inorganic growth.

I think that has helped us a lot because many firms face a challenge when they try to upgrade their data ecosystem. Your main business runs on legacy systems, and now you want to build something new or transform to a new platform, but how do you motivate or change so much of your current ecosystem, applications, advisor workstations, and everything else to move to the new platform? We use M&A as an opportunity—think of it as a beachhead approach. When we were integrating with our largest client to date, Prudential Financial, we introduced a new introducing broker dealer concept, which didn't exist at LPL. We got that as an opportunity to put that data flow and all the ecosystem around Prudential integration onto Data Foundation, which is the new AWS-based, cloud-based, modernized platform that we are building.

Then as we did the next M&A, when we acquired Atria Wealth, we put all the TAMP flows, which are new to LPL, onto Data Foundation. What we used to do was a very small, tiny footprint, whereas Atria gave us an opportunity where it was all TAMPs. We took all that flow and put it on Data Foundation. We continued that journey in 2026 where we have announced the acquisition of Commonwealth Financial. Commonwealth will be the big piece now where all of our custody data will be moving onto Data Foundation. This is the real big milestone. We've used those opportunities to transform our journey.

It does come with the complexity of keeping two systems and two sets of ecosystems in sync. That's where your teams come in and help us from Incedo. Thanks for your partnership there. It's complex—that's the hard work you need to do before you could be ready for the AI journey. But we are very glad that we did all that work over the last two or three years, so we are ready for the next phase.

I want to go back to the point you made about sequence. I think that's a very important thing—how you approach the problem. Share some tips with us about how you choose the right things to do. What may seem like the biggest may not be what you focus on. I know that's something you think a lot about. Give us some tips around that.

If I have to give one tip, it is really the sequencing. It's not so much about whether you can come up with the best architecture or what's the best architecture for agentic AI and what technology tools or products you should buy. That's not going to get you there. It's really doing things in the right sequence. The fundamental part, the most important thing for us from a sequencing perspective that we've learned, is the journey to Data Foundation and the journey to cloud. Without those ingredients, you cannot even start. Since we have done or are on our journey to get those two things done, now comes the question of how do you democratize access to data and AI tools to a broader ecosystem. Right now, while we are experimenting with use cases, the thing that we are trying to solve for is how to do that.

How our business lines get engaged, how the product teams get engaged, and how this is democratized for everyone to start working with AI at a pace. Otherwise, if everyone waits for technology and product to give them tools, we will be behind the game. So that's the next priority: the learning journey for the organization and building that momentum, which is what we are focused on. The third thing is security and trust in the solution we build. We address these three things in sequence, and then we can go after what is the best technology out there and whom we should partner with.

I'll come to my last question, which is about talent in the AI age. Paradoxically, the human aspect becomes more important. So finally, people and talent—what are your top three mantras, or what is different that you are expecting from your teams? I know many of your teammates are sitting here, so this is very important for them. Anecdotally, what I heard is that we are the last generation managing only human beings. After this, our next generations will be managing human beings and AI agents. I don't know if that will be true, but that's the promised land.

I think it's not something magical, but something you embrace. It's no longer an approach where you can stay on the sidelines. It's happening. I am taking it on myself to learn, and some of my team members are already experimenting and doing the coding themselves. It has become so easy that you don't have to be a programmer anymore. So I think the mantra is everyone trying to do it themselves. The hiring will change in the future, shifting more towards ML engineering as the main focus, as opposed to software engineering, Python, or what used to be full stack engineering. All our hirings will shift towards ML engineering skill sets.

We are at time, and Gotham is looking at me. Nitesh Ambastha, thank you so much. In 18 minutes, we managed to cover the entire value chain of modernizing wealth management with cloud and AI. I appreciate your insights and thank you to all the audience. Thank you also to AWS for bringing us together here.


----

; This article is entirely auto-generated using Amazon Bedrock.
