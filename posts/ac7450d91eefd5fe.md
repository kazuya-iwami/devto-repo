---
title: 'AWS re:Invent 2025 - Drive Supply Chain Innovation Using AWS Cloud and AI Solutions (IND3307)'
published: true
description: 'In this video, Deborah Simeone hosts supply chain leaders from Sysco and McDonald''s to discuss their AWS-powered transformations. Samir Patel explains how Sysco, the world''s largest foodservice distributor serving 730,000 customers, modernized their 25-year-old warehouse management system (SWIMS) across 110 locations, migrating from Oracle on-premises to AWS cloud with zero rollbacks in one year. They achieved 50x better RTO/RPO, reduced deployment time from weeks to 8 hours, and cut costs by half using services like RDS, Fargate, and Lambda. Sysco is now implementing AI solutions including Root Ripple for incident resolution, Router IQ for delivery optimization using Cloud LLM and LangChain, and AR-driven inventory optimization with SageMaker. Stephanie Koenig describes McDonald''s challenge of tracking 1.2 billion boxes annually across 120 countries with 10,000+ suppliers they don''t own. McDonald''s built MCD Track using Amazon Supply Chain solution, creating a three-layer data lake architecture that normalizes data from suppliers, distribution centers, and 45,000 restaurants. They leverage rule-based models, machine learning, and LLMs via Bedrock for sales forecasting, promotional impact analysis, and traceability to support regulatory compliance and sustainability goals.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/0.jpg'
series: ''
canonical_url: null
id: 3088842
date: '2025-12-06T12:14:25Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Drive Supply Chain Innovation Using AWS Cloud and AI Solutions (IND3307)**

> In this video, Deborah Simeone hosts supply chain leaders from Sysco and McDonald's to discuss their AWS-powered transformations. Samir Patel explains how Sysco, the world's largest foodservice distributor serving 730,000 customers, modernized their 25-year-old warehouse management system (SWIMS) across 110 locations, migrating from Oracle on-premises to AWS cloud with zero rollbacks in one year. They achieved 50x better RTO/RPO, reduced deployment time from weeks to 8 hours, and cut costs by half using services like RDS, Fargate, and Lambda. Sysco is now implementing AI solutions including Root Ripple for incident resolution, Router IQ for delivery optimization using Cloud LLM and LangChain, and AR-driven inventory optimization with SageMaker. Stephanie Koenig describes McDonald's challenge of tracking 1.2 billion boxes annually across 120 countries with 10,000+ suppliers they don't own. McDonald's built MCD Track using Amazon Supply Chain solution, creating a three-layer data lake architecture that normalizes data from suppliers, distribution centers, and 45,000 restaurants. They leverage rule-based models, machine learning, and LLMs via Bedrock for sales forecasting, promotional impact analysis, and traceability to support regulatory compliance and sustainability goals.

{% youtube https://www.youtube.com/watch?v=VRIXSkiPRT8 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/0.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=0)

### Introduction: The Invisible Backbone of Restaurant Hospitality at Scale

 Welcome to Driving Supply Chain Innovation with AWS Cloud and AI Solutions. My name is Deborah Simeone. I'm the global head for restaurants at AWS, so I have an interesting role where I spend my days talking to restaurant operators of all sizes all around the world. The one thing that fascinates me most about hospitality is doing it at scale. When you think about it, if you walk into a McDonald's anywhere in the world, no matter where you're from, you're going to get the same experience, and there's something amazing about that. That type of experience doesn't just happen. It's engineered with the invisible backbone of the restaurant industry, which is the restaurant supply chain.

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/50.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=50)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/90.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=90)

 Today I'm joined by two leading supply chain experts in the restaurant space. First we have Samir Patel. He is the senior director of supply chain engineering at Sysco, which is the world's largest foodservice distributor. Then we have Stephanie Koenig, senior director at McDonald's. McDonald's needs no introduction, but when you think about the level of hospitality at scale I was describing, you're going to be surprised at some of the complexity in supply chain that McDonald's deals with on a daily basis. Sysco and McDonald's are on opposite ends of the supply chain spectrum at AWS, but what we find is that they're using AWS in unique ways to serve their customers and transform the ways that they're meeting the  needs of restaurants and diners.

Whether you're in supply chain today or you're in operations or you're in tech or product, I promise you're going to learn something today from Stephanie and Samir about how to build something, how to scale something, and how to achieve operational efficiency with excellence when you've got high stakes and massive scale. I want to show you a little bit about what the scale of the restaurant industry is that we're talking about. McDonald's operates in over 120 countries, and what that means for McDonald's is that they're dealing with regional differences in the suppliers, the regulatory environment, and even the logistics networks that they operate in.

Then you have Sysco, which is serving three quarters of a million restaurant customers every single day, making sure they have what they need when they need it. What's even more complex about Sysco and McDonald's is that they're dealing with fresh food. When you're dealing with fresh food, time and temperature are everything. Both these organizations have a lot in common. The biggest thing that you'll learn today they have in common is that they've used AI to unlock the potential of their supply chains, and that's the transformation that you're going to hear about today.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/170.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=170)

You're going to learn about real solutions that are in production right now. They're  making intelligent decisions that are impacting millions of meals even just today. I'm going to go ahead and hand it over to Samir now. He's going to walk you through some of Sysco's journey and how they got to a place where they're transforming their operations on behalf of their customers using AWS. Welcome, Samir.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/210.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=210)

### Sysco's Legacy Challenge: Managing 56 Years of Growth Across 110 Warehouse Locations

 Thank you, Deborah, for the warm introduction. Today I want to share an amazing transformation journey that Sysco has gone through. We have accomplished a lot as part of the transformation. I also want to talk about our strategy, how we went about it, and AWS, which is a foundational thing that really helped us achieve our goals. I'll also talk about some of the AI vision and use cases that we have and a lot of lessons we learned. Our journey was very difficult. It wasn't easy, but I'll use simple terms to explain to you how we went about it.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/280.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=280)

Show me your hand if you've heard about Sysco or have seen our iconic truck. All right, almost many people. Show your hand again if you're actively going through some sort of modernization or transformation. Okay, so you are in the right session. I will first start with Sysco. Sysco, by and large, is the number one food distribution company in the world.  I want to start also with one fun fact. Sysco is a 56-year-old company, and also by S&P ranking, we are also 56. Our numbers are shown on the screen, right? We have 730,000 customers. We have 500,000 SKUs.

Sysco is an $81 billion company, a great example of a legacy company that's showing growth and innovation. We are always looking for ways to innovate and deliver better solutions to our customers at the right price with the right products. We have a presence in 90 different countries and 340 plus distribution centers, which tells you how complex we are. But for today's conversation, I want to focus on supply chain.

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/340.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=340)

Let me double click into  that. Every solution we have, we try to put an S in front of it. Our warehouse management system, we call it SWIMS. There are two key objectives as part of our warehouse management system, but before that, I want to say that Sysco has grown by merger and acquisition. We have been growing for the last 56 years. There is growth that way, so we want to always optimize and innovate when it comes to our warehouse operations. We want to give our warehouse workforce tools and processes where they can focus on operational efficiency. We also want to have better workflow in our warehouse so that translates into delivering products and orders to our customers faster, and at the same time, we are optimizing our warehouse operations.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/410.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=410)

Now if I can go further into our warehouse management system, we have about 110 locations where we have  deployed this purpose-built custom solution. As I said, it's SWIMS. It's 25 to 30 years of innovation and enhancement that we have done, and as you can see, the scale is in multiple countries. We have 15,000 warehouse associates using it, and we are processing over 2 billion pieces per year. We also have different business units and geographical locations. We have our regional distribution centers and central warehouses, so there's different complexity and different kinds of flavors that it brings.

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/460.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=460)

Now I want to talk about some of the challenges. As we have built this over the last 25 years,  the complexity grew. We have over 1.5 million lines of code. We have a lot of integration with systems in the warehouse, as well as different ERP systems. Some of the technology that we had is the Oracle suite products, so we had UI in Oracle Forms. Our database is Oracle. A lot of logic is in PL/SQL. We had ProC, so you name it. There's a lot of legacy technology that was preventing us from innovating. Even if you have to upgrade, it was taking a lot of time.

Deployment is another thing because even though we have 110 warehouse locations, it's one code base, but we were deploying into all locations one by one. It's a single tenant system where deployment happens, and every time we have to deploy a new release, it's time consuming. We can only deploy during the downtime or the weekend maintenance window, and it takes two to four hours. You can imagine if we have to roll out to all 110 sites, it takes so many weeks. There are a lot of challenges that we faced, and it's hard to innovate on top of that because now everybody's talking about AI and agentic AI. If you want to do something, that's also another challenge.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/550.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=550)

What we did is we looked at how we could transform.  We had a vision, a strategy, and an action plan. Let me start with why we want to modernize. One of the main things from the business side is that we want to have a more intuitive experience for our warehouse associates that can help translate into better productivity. As we have new associates, we've gone through the COVID time where we had labor shortages and so many other issues, so there are challenges with the warehouse. There are new associates, and how do we train them faster? How do we make it more intuitive so they can finish their job very quickly?

At the same time, we want to technically use hardware and software which is cloud native so that we can pick up the speed deployment-wise and upgrade-wise, and also develop new capabilities and deliver them to our customers.

There are several complexities to consider. Another important aspect of our warehouse operations is that we have associates working in different climate zones, including dry, freezer, and cooler areas. Imagine they have to pick up orders from all different aisles and then pack them as complete orders. So there's a lot of complexity in our warehouse environment.

Additionally, the users in our warehouse are using different devices. There is a browser-based application that 30% of the associates are using, but the remaining 70% of operations go through forklifts. They have RF guns and wearable devices, so they use different sorts of mobile devices to finish the warehouse workflow. Within four hours of warehouse operations, typically you have different shipments coming in that we put away. We also have cutoff times, and then we pick up the orders and ship them to our trucks for delivery. There's a lot of things happening, including returns coming in, so everything has to happen efficiently. That's something we wanted to modernize.

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/680.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=680)

### The Strangler Pattern Approach: Sysco's Cloud Migration Strategy and AWS Architecture

 So how we wanted to do it and how we did it involved focusing on the user experience to begin with. Yes, we wanted to use cloud-native solutions and modern stack. All that is great, but starting with user experience was key. The browser behavior now uses React with cross-browser responsive UI. That's number one. The mobile apps, which were Windows CE, are now moving to Android, and the new applications that we have built are Android-based.

As I mentioned about single tenant deployments, instead of just focusing on single tenant, we wanted to have regional-based deployment where, instead of just deploying one at a time, we can have more releases going to the region. That way it's multi-tenant, and these are some of the objectives. We also wanted to optimize costs because some of the things I mentioned, like we had Oracle stack, so there was a lot of cost involved with that as well as the upgrade costs. Also, because of the old legacy technology, it was very hard to find talent, so there is cost in different ways. There's human cost, hardware cost, and software cost. So we wanted to go beyond that.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/760.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=760)

 And why AWS? But before I go there, I want to talk about an important aspect or approach we took. Those who are going through modernization or have done it may have heard about sidecar approach and strangler pattern. Sidecar approach is where you have the old system running and then you build a new system on the side, and you have both running in parallel. Then slowly you build capability in the new system and retire the old one. But in our case, that was not a viable solution because there are a lot of things that need to be implemented in the sidecar, and you cannot get the business value faster, and it's a cost issue also.

So what we did is we built a strangler pattern, where strangler pattern is where you keep one code base but also build the modern stack slowly and then shrink your old stack and build a new one, and both can coexist. A lot of those things, plus cost considerations, meant we wanted to innovate faster and deliver faster with blue-green deployment and canary deployment. There are a lot of aspects to how we can go about it. Also, the database I mentioned, we were using Oracle Enterprise Edition in the private data center. So looking at different clouds and different options and different strategies, AWS was a natural choice for us.

We are able to use RDS and Standard Edition, and I'll touch upon some of the benefits in the following slide. But those are some of the things, as well as the cost, because there is a dual run cost. You have the old stack running and you have the modernization stack running. How do we optimize the cost and innovate faster? Because as we start deploying the modern stack, there's a lot of cool technology or AI-driven technology that we want to benefit from. So considering a lot of that, we decided that we wanted to have AWS as our foundation.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/890.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=890)

 Going further, this is very high level, like our cloud architecture. So what we built with the modern stack and moving from our on-premises to AWS, it's not just a lift and shift, but we wanted to build the architecture which is very resilient. In a single region, we are

highly available, and in a multi-region setup, we are active-passive. We have a strangled pattern, as I said, so we have the old stack and the new stack both running together. The old stack, from the compute perspective, uses EC2 instances in multiple availability zones, which gives us the resiliency. Also, in the DR space, we have Oracle RDS database, and we are taking snapshots and putting them into the second region. So whenever the region has some issues, we can always start that second region and get that going.

All the new stack we have uses serverless, so Fargate as an example, and we are using Lambda functions. There are a lot of key services that we are using out of AWS that gives us this architecture. Also, we built proactive monitoring, so we have an enterprise monitoring tool, Datadog, with very native integration as well. Even during the recent AWS outage, this architecture was proven to work, and we did not have any issues. There's a lot of advantage that we got out of this architecture. It's a well-architected framework also, so we embraced a lot of good practices as part of that.

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1000.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1000)

But if I fast forward now,  I want to talk about the benefits. In a record time, I talked about the complexity and number of challenges that we had. We were able to migrate all 110 sites with zero rollbacks in a year. This is like a historical moment for us. Also, on the right-hand and left side, you can see the RTO, RPO, service level objective, recovery point objective, and recovery time objective. In the same region, we have very impressive numbers when it comes to high availability, and also when it comes to DR. Compared to our on-premises infrastructure, we are able to have 50 times better numbers. That's because of AWS.

Also, the rollout I mentioned about 2 to 4 hours deployment speed. Now we are able to deploy all sites within a day, which is 8 hours. As part of the migration, it was not just the lift and shift. We were able to make a lot of security improvements, so security in transit, security at rest, database level encryption, HTTPS wherever possible. Also, the password rotation, using Amazon Certificate Manager, Secrets Manager. Those are some of the AWS services that really helped us to achieve a lot of security and cyber improvements. Even the AMIs we have, we call Golden AMIs in Sysco, which are created by Sysco putting all the right hooks, so we use that AMI as part of deployments also.

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1110.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1110)

Another thing is super proactive monitoring. So we have hooks  into ServiceNow, we have hooks into Datadog, and we built a lot of features as part of that where we can see how the modern stack is used, how we can troubleshoot better. There's a lot of monitoring that we built. I want to also touch upon the savings because as you modernize and start deploying it, you have to worry about cost. Amazon AWS has a very great program called Migration Acceleration Program, which is MAP credit. We tapped into that. Our AWS team really helped us achieve that, and that was really great to have for dual run cost.

But we didn't only stop there. We are also parking the infrastructure, and all the infrastructure that we have is very much scriptable. We can create it, use it, and remove it in non-production environments. In production also, the RIs, savings plans, there are a lot of things on top of that we leverage, which gives us the cost benefit. We are running at half of the cost compared to our on-premises. Even the support cost I was mentioning about, we moved from Oracle Enterprise Edition to Standard Edition using AWS RDS, so that helps us save a lot. We got a lot of capabilities out of that, more capability at low cost. Those are some of the benefits that we realized.

[![Thumbnail 1210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1210.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1210)

[![Thumbnail 1230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1230.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1230)

### Building the Smart Supply Chain: AI Use Cases from Root Ripple to Router IQ

So what's next?  As part of modernization, we have a modern stack that we are actively deploying. We have both the legacy or old stack and the modern stack running side by side in production, and we are actively deploying to different sites. We now  have a vision which is more about getting into AI. We want to have a connected experience to our warehouse, and it's not just agentic, but there are automation use cases, vision use cases, and robotic use cases. We're combining all of that and building the smart supply chain.

[![Thumbnail 1270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1270.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1270)

This is a blueprint, but I want to take a little bit of time and talk about three specific use cases that are either in the early stage or in a further stage. Let me touch upon that quickly. The first one  is a very technical use case. We call it Root Ripple. Imagine 110 warehouses where issues happen. How do we go about reactive resolution to proactive solution? There is data coming from ServiceNow, data coming from incidents, we have Datadog, and we have runbooks.

This is a solution that we implemented using GitHub Copilot just to begin with, and it is basically looking at all the data. It has an LLM running, and then basically it can find if there's a pattern, if this issue has happened before. It can give you a resolution, or it at least navigates how we can go about solving the problem. This is something that we deployed using Kubernetes, but we were able to save a lot of time. We can reduce the MTTD by 30 minutes. Even the recovery time was reduced in half. It was 6 hours, and now it's reduced to 3 hours.

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1360.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1360)

Importantly, the productivity of our associates who are troubleshooting drastically improved. Now they can use that 10 hours to do bigger and better things. This solution is not just for our warehouse, but we can take this and apply it everywhere. That's the power of it. I have another interesting AI use case  which is more about the transportation space. By the way, this is a hackathon winner. I have one of my team members sitting here. He's the pioneer of that.

Before I talk about the Router IQ, who is a router and why are there a lot of manual things happening? Routers are the human beings working in the warehouse. Sysco has this concept called cutoff time where every day, around 4 or 5 o'clock depending upon the site, we stop taking the orders. Depending upon what are the orders we need to deliver for the next delivery date, there's a person or set of people sitting in a warehouse. They look at all the orders for the day that have to be delivered and then look at the customer location and figure out what should be delivered, what is the optimal route, and how do we pack this order together so it's efficient and delivered on time. Every customer also has a time when the order should be delivered.

Combining all of that, a person has to spend time figuring out what is the optimal route. The AI solution was a POC, but we are actually going about implementing it. I'm going to tap into my cheat sheet here. We still want human in the loop. We want the router to be the final decider. Even though AI can say this is how the optimization works and this is how it should be delivered, we want the final say to be done by the router.

For the optimization engine, we're going to be using Cloud LLM and then LangChain as a framework, plus we have the hybrid VRP, and that's for the shortest path engine. The third thing is how do we go about deploying? We either use ECS or EKS cluster. For the data streaming and getting data from different systems, we are using Kafka pipelines. Those are some of the technologies that we're going to tap into, but there are benefits. That's improving efficiency of our warehouse workers as well as delivering products to our customers faster. Those are some of the key advantages that we can see from this. And then I have one more use case.

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1510.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1510)

This is more about your traditional AI and machine learning use case where  we have AR-driven inventory optimization. At Sysco, as I talked about, we have about 500 SKUs, so many different items that customers can order. When customers are ordering through our e-commerce channel, which we call Shop, they can look at the items, but sometimes what might happen is it may show an item is there, but when we try to fulfill after the cutoff time, the item may not be there, so there may be a stock out. That can lead to unhappy customers, or we have to substitute items which customers may not like. With this AI and machine learning use case, and that's something we want to build using SageMaker, we can really help transform how we can have AI-driven inventory. Some of the key benefits listed here are mostly talking about the initial MVP, but we want to go further and improve how we can do this. Those are some of the use cases I wanted to share.

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1580.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1580)

I want to close it off by telling you some of the lessons learned.  As part of the transformation journey, a very important thing is there will be challenges. We are full of challenges, but what we did is we navigated through them. Have your team and empower them. I think you need to have a strong engineering team and a great relationship with your business partner and product team. There will be different things happening every single time. You have to swim against the tide, but if you are intentional about it, you can definitely get there. Automation is key. We try to automate everything, so getting migration done, getting database backup and snapshots, infrastructure as code, deployment pipelines, even the disaster recovery testing, even going from one availability zone to another. There are a lot of things when it comes to running your workload on the cloud, and automation is the key.

One quick example I can share is we recently upgraded the Rails release, and it was something we did in the middle of the day with zero downtime. That's the power of automation, so definitely do that. Cost-wise, I touched upon some of the aspects in the benefits, so that's one thing you need to worry about. Manage your cost because you don't want modernization to take a lot of money and time and then have that get deprioritized. You want to optimize the cost as you go. Innovation is important. AWS is a really strategic partner, so definitely tap into that and work with them closely. Work with the business partners and the core teams around your department. Think about innovation because it's not about just going from where you are to the cloud and just modernizing the stack, but what's next. Always think about it and be strategic about it. That's it for today.

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1710.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1710)

 I know there is no Q&A here, but I'll be spending about an hour in the expo hall from 2 to 3 p.m. If you have any questions, please stop by, or even if you have your story to share with me, I'll be there. With that, I will hand it over to Stephanie, and she'll talk about her journey at McDonald's. Thank you.

[![Thumbnail 1760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1760.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1760)

### McDonald's Global Scale: Feeding 1% of the World's Population Daily

Hello. As Deborah mentioned, my name is Stephanie Koenig, and I am the head of supply chain globally, at least the tech side, at McDonald's. Today I wanted to talk to you a little bit about some of the great innovative work that we're doing here at McDonald's.  I know Deborah mentioned that we all know McDonald's quite well, but a lot of people aren't familiar with maybe some of the detail or the complexity of it. For anyone who isn't familiar with McDonald's, we're a burger company that makes the Big Mac, and we also invented the Chicken McNugget. We've got over 45,000 restaurants in roughly 120 countries around the world, and every day of the year we serve approximately 1% of the world's population. To keep all of this running, we have over 2.2 million people working in our restaurants and thousands of developers keeping all of our technology on the cutting edge. As you can imagine, we don't do anything small.

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1810.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1810)

To help explain our scale, as I mentioned before, we're in 120 countries  and we work to source much of our food locally. This means that we have suppliers around the world, both big and small, who help us feed 1% of the world's population. To give you some perspective on the scale, this is what it looks like from a world map perspective. And I have some more fun facts to help bring this into perspective.

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1830.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1830)

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1850.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1850)

 So in the US annually, we source over 2 billion cage-free eggs. They flow through our supply chain every single day. This is enough eggs to go end to end around the world three times. And if you wanted to make an omelet, it's a big enough omelet to fill 40  Olympic-size swimming pools. That's a heck of an omelet.

[![Thumbnail 1870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1870.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1870)

For those of you not into breakfast, let's talk about burgers. Since we don't release global burger numbers and AI is kind of part of the conversation here, I thought I'd use AI to help me get some estimates about our burger claims.  AI came up with an estimate of 2.36 billion burgers sold globally each year. That's roughly 75 burgers sold per second, or 6.4 million sold daily worldwide. To make this many burgers, it takes an awful lot of raw ingredients: beef, cheese, onions, and buns.

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1900.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1900)

So some of the key challenges facing our industry, the QSR industry as a whole,  is regulatory reporting. There are a lot of regulations like FISMA and EUDR that are out there that we're required to comply with. We need to ensure food safety and that our standards are supported worldwide. We need to meet and report on our global commitments around sustainability. And most importantly, we need to provide supply chain visibility, which is knowing where our inventory is within the system.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1950.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1950)

Actually, our supply chain visibility supports the other three: regulatory reporting, food safety, and sustainability. So as I was saying, of all of these, visibility is the foundation that enables all of our other priorities. Enhanced visibility allows us  to verify sustainability sourcing locations, improve our regulatory reporting with accurate data, and quickly identify the origin of food quality issues.

[![Thumbnail 1990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/1990.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=1990)

Beyond compliance, visibility drives better investment decisions. It optimizes our supply demand accuracy and accelerates our real-time data-driven decision making. Ultimately, achieving this requires traceability of raw materials across a network of more than 10,000 suppliers. With hundreds of potential use cases, we decided to focus on the following  first.

As I mentioned before, traceability is kind of the key to visibility. We also wanted to focus on our sales and operations planning, as this makes a really big impact on how we could successfully support the demand at our restaurants. We also wanted to look at the impact of marketing campaigns and promotions on the supply chain, as they greatly affect how our supply chain functions and the demand that's put on our system.

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2020.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2020)

[![Thumbnail 2040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2040.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2040)

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2050.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2050)

Since traceability is our biggest challenge and  traceability is the foundation that enables all of our other priority use cases, it means at McDonald's we needed to track the location of 1.2 billion boxes annually. This means I need to track almost enough one-foot boxes to fit side by side to go to the moon.  That's a lot of boxes. And although McDonald's is not on the moon yet, although I'm sure we'll have aspirations to get there sometime,  this is a lot of tracking.

### MCD Track: Solving Visibility Across 10,000 Suppliers in 120 Countries

So I'm sure you're asking yourself, we've gone through a lot of background on McDonald's, why does this matter for this discussion? Well, it matters because it means the data surrounding our supply chain resides in roughly 120 countries around the world at entities we own and many more that we do not. So unlike many of our large international firms, Sysco is a good example, we do not own our supply chain. This means that our suppliers, our distribution centers, our delivery services are all outsourced.

Since we try to source locally for our restaurants to support our local communities and also have the freshest possible food, many of our suppliers are local. And although we have lots of global suppliers, it means that 50% of our data is locally held and some of our data is globally held.

We have incredibly close relationships with both our vendors and suppliers. But as I mentioned before, our data is distributed all over the world. We do own our restaurants, we own their inventory, and we own the decisions on how and with whom we procure food and services. We're also responsible for quality assurance and legal regulatory compliance with all global and local laws.

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2140.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2140)

 So how do we start by solving this visibility challenge? Well, we started by defining some guiding principles. We decided that when working with our over 10,000 partner organizations, we would first get all of our data directly from our source systems. We wanted to make sure that all the data we were getting was not adjusted. All of our master data was going to come from McDonald's own systems, and we would not establish connections through third parties, so peer-to-peer connections were part of our main solution.

To make sure that we were moving into the world in a modern and ever-changing place, we wanted to make sure that we had near real-time data. And as always, security and scalability are a very big concern at McDonald's, so those were a big part of our initial concerns. From an architecture perspective, we wanted to utilize the AWS Well-Architected Framework. We wanted to focus and take a data analytic lens to ensure our consumers could easily use our solution and we could easily leverage AI capabilities. Of course, we wanted to follow McDonald's best practices and standards, and as I mentioned before, security, security, security. Out of all of this, MCD Track was born.

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2230.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2230)

 Now what does MCD Track? Well, it's a solution McDonald's built using the Amazon Supply Chain solution. We took our guiding principles, we partnered with AWS, and we leveraged their technology that they use to get packages to you within two hours. At McDonald's, we're using this technology to build a system to provide traceability globally. We're tracking the delivery of our ingredients that we need to make hot burgers, Chicken McNuggets, and delicious fries for you and the one percent of the world population daily.

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2270.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2270)

 So technically, what is MCD Track? Well, it's a data lake solution comprised of three layers. The first layer is comprised of our data producers. This is where our raw data from over 10,000 suppliers, our distribution centers, and 50,000 restaurants, including McDonald's master data, are all normalized. The second layer we call the refined layer. This is where the near real-time data from producers' multiple data solutions are merged together to gain tracking visibility using our Amazon Supply Chain solution.

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2330.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2330)

The third layer is our insight or curated layer. This layer we use to create reports, provide access to our users, and we leverage it for our AI and ML processing. Now that you understand how MCD Track came to be and a bit about how we manage  the data, let's look at the architecture.

### Layered AI Models: From Rule-Based Data Quality to Agentic Marketing Insights

This diagram depicts the MCD Track solution architecture using the Amazon Supply Chain solution. Amazon Supply Chain solution comes integrated with a wide array of services, including robust data services with canonical data models for supply chains. Amazon Supply Chain solution also provides data governance to assure accuracy and timely data updates. It provides LLM-based data ingestion and mapping of external data. It also utilizes AWS's advanced data and AI capabilities to monitor supply chain and generate recommendations to improve McDonald's overall supply chain. Let's talk a little bit more about how we're leveraging the AI/ML capabilities throughout MCD Track.

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2390.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2390)

 To ensure we're using the right solution for each challenge, we use different types of models to solve different challenges we're facing. When solving a known and well-defined problem, we use rule-based models.

Where data is less structured, we use machine learning and large language model capabilities. And where our desired outcomes are less predictable, we leverage AI capabilities. These models are layered on top of one another to solve a lot of our more complex problems.

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2420.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2420)

 So how did we do this? Well, we began by looking at our base inventory data, or our raw data. Our challenge was to validate the quality of this data and ensure it was aligned with the other data being pulled into our new MCD Track solution. Since this type of problem was well-defined and had a known solve, we applied rule-based models to tackle this challenge. Using data quality intelligence, root cause analysis, and simple remediation responses, we're now ensuring quality, reliable data.

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2460.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2460)

[![Thumbnail 2470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2470.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2470)

 Once data passes through the supply chain,  you can see here the raw layer, the refined layer, and the curated layer. This combined with our data quality engine, which is comprised of over hundreds of different rules, means the data is considered curated and now used by our AI and machine learning solutions after it goes through our data quality engine.

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2500.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2500)

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2520.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2520)

One example of how we use AI and machine learning models is our sales and operations forecasting.  With the help of IoT, which gives us accurate inventory across our supply chain, plus restaurant sales data mixed with strong AI models, we're able to provide high-value forecasts which will be included in our S&OP processes.  As you can see here, we train the model using SageMaker pipelines. The training data is stored in an S3 bucket and fed into the SageMaker pipeline. The pipeline automates the end-to-end machine learning training process.

The pipeline includes processing, training, and evaluation steps, followed by a register model step. The register model step publishes the model to SageMaker Model Registry. Then downstream workflows retrieve the approved model from the registry for deployment and inference. An inference pipeline consumes the data set from the S3 bucket, processes it, and executes batch transformation jobs to generate predictions. We then can feed these predictions into Amazon Q to support our agentic needs.

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2580.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2580)

 In another example, we're looking at how our market campaigns are impacting our inventory needs. To solve this challenge, we're starting with base data, adding marketing information and outside influential information like economic data, weather, and public sentiment. Because much of this data is not structured, we began using an LLM-based model to extract much of this data into MCD Track. We then layered agentic AI on top to help us understand the unexpected relationships between, let's say, weather and what people choose to order. This helps us gain insights into these types of non-standard relationships.

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2640.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2640)

An interesting fact is many of our restaurant owners say that our ice cream sales don't fluctuate a lot with weather changes or the change of season. Apparently, people who love ice cream eat it all the time.  As I mentioned before, and you can see in this architecture diagram, we're extracting marketing data using the Bedrock LLM models to transform and standardize the data before putting it into our MCD Track data lake. The next step is to take specialized sentiment data and use the combination to predict promotional impact so we can ensure a smooth supply chain. We want to make sure that our customers have exactly what they need when they want it.

[![Thumbnail 2690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2690.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2690)

### Conclusion: A Multi-Year Journey Toward Complete Supply Chain Transformation

As you know, most promotions drive a bell curve demand, and this solution could help us enable to address this type of scenario in conjunction with our existing S&OP forecasting processes.  This brings me to the conclusion of this presentation and a review of our approach. As you heard, we started with the problem. How do we get visibility into our outsourced supply chain that spans 120 countries and has 1.2 billion boxes a year? We needed to provide McDonald's with complete traceability. We outlined the rules of engagement, or guiding principles.

We wanted to make sure that our solution adhered to those. We then aligned our vision with our stakeholders and our vendors and started gathering data to create a comprehensive view of the inventory across our system. Last but not least, this is a multi-year journey. We still have a lot more AI to implement and markets to work with, but as we all know, nothing worth doing happens in a day.

I want to thank you all very much for coming to listen to me today. It's been a pleasure, and I look forward to hearing from you. Thank you very much.

Thank you so much to Samir and Stephanie. We're confident at this point that as you enjoy your next McDonald's meal or meal at your favorite restaurant, you now have a new appreciation for the level of care and sophistication that goes into crafting that experience. I encourage you all to try the new Grinch meal at McDonald's this week and really appreciate the level of planning that goes into that hospitality at scale.

I offer you three ways to continue to engage with this content moving forward. First, check out Samir's demo in our Industries Expo later today. It's going to be awesome, and you can dive deeper with him. Second, we have a multi-agent demo as well in the Industries Expo that we'd love for you to dive into. Third, we're always here to help you innovate at AWS, and we invite you right outside the theater where we can engage with you directly after this presentation and chat about what you're doing and what you're working on and how we might help.

[![Thumbnail 2810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ac7450d91eefd5fe/2810.jpg)](https://www.youtube.com/watch?v=VRIXSkiPRT8&t=2810)

So thank you again so much for your time. Have a  wonderful rest of re:Invent, and we appreciate it.


----

; This article is entirely auto-generated using Amazon Bedrock.
