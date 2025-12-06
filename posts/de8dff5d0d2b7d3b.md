---
title: 'AWS re:Invent 2025 - How Adobe & Salesforce enable sustainability initiatives with AWS CCFT (AIM332)'
published: true
description: 'In this video, AWS sustainability tech team leader Alexis Bateman joins Adobe and Salesforce representatives to discuss how the Customer Carbon Footprint Tool enables sustainability initiatives. AWS''s journey toward net zero carbon by 2040 is explored, including carbon-free energy goals, water positivity targets (53% achieved toward 2030), and component circularity with 23.5 million components recycled since 2022. The session details how AWS calculates Scope 1, 2, and 3 emissions across regions and allocates them to customer accounts. Salesforce''s Eric discusses their 100% renewable energy achievement since 2021 and public cloud transition strategy. Adobe''s Rao shares their net zero by 2050 commitment, including 42% Scope 1-2 reduction and 52% supply chain emission reduction targets by 2030. Both customers emphasize needs for greater data granularity, API accessibility, and product-level carbon footprint visibility to meet stakeholder demands and enable informed sustainability decisions.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - How Adobe & Salesforce enable sustainability initiatives with AWS CCFT (AIM332)**

> In this video, AWS sustainability tech team leader Alexis Bateman joins Adobe and Salesforce representatives to discuss how the Customer Carbon Footprint Tool enables sustainability initiatives. AWS's journey toward net zero carbon by 2040 is explored, including carbon-free energy goals, water positivity targets (53% achieved toward 2030), and component circularity with 23.5 million components recycled since 2022. The session details how AWS calculates Scope 1, 2, and 3 emissions across regions and allocates them to customer accounts. Salesforce's Eric discusses their 100% renewable energy achievement since 2021 and public cloud transition strategy. Adobe's Rao shares their net zero by 2050 commitment, including 42% Scope 1-2 reduction and 52% supply chain emission reduction targets by 2030. Both customers emphasize needs for greater data granularity, API accessibility, and product-level carbon footprint visibility to meet stakeholder demands and enable informed sustainability decisions.

{% youtube https://www.youtube.com/watch?v=7xMMAKI1Q1o %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/0.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=0)

### Introduction: Sustainability at AWS re:Invent and Session Overview

 I think we are finally ready to get started. First off, thanks everyone for joining the session today. I know you have a lot of choices to pick from, so we're really happy for you to join us here to talk about sustainability. Before we dive right in, I wanted to just get a quick poll of the audience. Who here works in sustainability? Does anyone work in sustainability or touch sustainability data? Okay, we got a few. How about operations? We got some software development engineers in here? Okay, got a couple of those. How about FinOps? Okay. Also, this blinding light, so I see about two hands out there. All right, great.

Well, hopefully this talk will touch on all of those dimensions and have something a little bit interesting for our builders out there, our FinOps, our operations, and of course, hopefully you all walk away here wanting to get more into sustainability or at least thinking more about it. Last question before we dive right in. Who knows what the Customer Carbon Footprint Tool is? Oh, we got, all right, three. All right, by the end of this talk, you will all know what the Customer Carbon Footprint Tool is, and hopefully you will all go there and check it out and see what it is and what information it can provide you. All right, let's jump here.

All right, so just to recap, you are in session 332. In this session, I'm joined by some colleagues from Adobe and Salesforce to talk about how they're enabling their sustainability initiatives using the Customer Carbon Footprint Tool. My name is Alexis Bateman. I am at AWS, and I lead our sustainability tech team. We enable all of our sustainability objectives at AWS, whether that be on our net zero carbon objectives, our water positive, our renewable energy goals. We are sort of the backbone of those products and initiatives. Today I am joined by Eric from Salesforce, who's a director of technology sustainability, and Rao from Adobe, who's a senior program manager for Cloud FinOps. Thanks both for joining me today to talk about how you've leveraged the Customer Carbon Footprint Tool to meet some of your goals.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/140.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=140)

So we're going to get right in. Our agenda is pretty streamlined.  First, we're going to talk about AWS sustainability, what are some of the goals that we have, and then of course how you actually can get insights and visibility into some of those goals through the Customer Carbon Footprint Tool, which is available in the AWS billing console. Then I'll turn it over to Eric to talk about Salesforce sustainability goals, what they're doing, and then of course how we've partnered on some of those sustainability objectives and some of the reporting and insights that we provide to enable them. Then we will pass it off to Rao to talk about a similar set of initiatives at Adobe and how they're driving these initiatives together with us. Then we have a few targeted Q&A sessions. I've had some conversations while I've been here in the short time at re:Invent, and we kind of curated a few questions that we're going to talk about, which are some topic areas around generative AI and sustainability, and so we'll end with that. So that's our session for today.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/200.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=200)

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/210.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=210)

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/230.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=230)

### AWS Sustainability Goals and the Shared Responsibility Model

 We're going to dive right in. So AWS has sustainability  at the core of a lot of our operations and how we provide services to you and to our builders and partners. We are a Climate  Pledge signatory, which is Amazon's pan-Amazon Climate Pledge. This is to be net zero carbon by 2040. So Amazon co-founded this together with Global Optimism, and we are one of the signatories to the Climate Pledge and of course abide by that same set of objectives.

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/260.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=260)

But how do we actually operationalize that at AWS? We have a few different pillars that we work towards. Some of them are carbon-free energy, so how we actually supply our data centers with power, what sources of energy we use to actually power those across solar, wind, nuclear, achieving and striving to achieve renewable and carbon-free energy goals. Also, operating as efficiently as we can in our operations of our data centers.  We also have circularity at the heart of a lot of how we build things, so whether that be looking at our critical raw materials, critical rare earth materials, how we take those back, as well as circularity initiatives for our components. So just a quick factoid: last year we had over 23.5 million components recycled or sold on the secondary market since 2022, 2023, excuse me.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/290.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=290)

 We also have a water positive objective, so let me just spell that out. So we are seeking to be water positive by 2030, and we're about 53% of the way there. What does water positivity mean? It actually means that we're giving or replacing more water than we use in our operations of our data centers. So these are sort of like the three pillars that we are driving towards in AWS sustainability.

And we partner across our entire company to achieve these objectives.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/320.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=320)

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/350.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=350)

But of course, what does this look like at scale?  We're seeking to provide a sustainable infrastructure at scale across 120 availability zones, over 245 countries and territories, as well as over 38 geographic regions. We've also looked at some studies that compare that we actually operate more efficiently, about 4.1 times more efficient than on-premise. So we are actually providing that efficiency for our customers that are coming to the cloud.  But we know we can't do sustainability in isolation. That's why, of course, today I'm joined by some of our customers to tell their story as well.

So sustainability is a shared responsibility. We as AWS had just talked about some of our goals across carbon-free energy and water positivity. As the infrastructure that is providing these cloud services, we can support how we're building our servers and the components we put in them, how we're cooling them, where the water is coming from, how we handle our waste, and what energy we procure. So we as AWS can control those and provide those to our customers. But we also have the customer partnership where customers like yourself are leveraging our services. This is where we need to actually support our customers on how they use AWS. That could be through the services we offer or through the reporting and insights they're able to use to make decision making. This is something we call the shared responsibility model for sustainability.

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/430.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=430)

So if you're familiar with the Well-Architected Pillar, this is one of the fundamentals of the sustainability pillars in the Well-Architected framework. You can read a little bit more about it, but fundamentally this is the relationship that we share between AWS and our customers as well.  So with that in mind, we as sustainability really needed to understand and meet our customers where they were at as we were understanding what we were delivering, what goals we're taking, what kind of transparency, what kind of visibility they needed to actually meet some of their sustainability objectives. Again, not just the partnership model, but of course our customers are running directly on AWS, so we are a portion of their footprint. So how do we start to build some of that relationship?

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/460.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=460)

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/500.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=500)

### Evolution of the Customer Carbon Footprint Tool: From Launch to Verified Methodology

 It's been a longer-term initiative, but one of the key initiatives was launching a carbon footprint tool in 2022. Our carbon footprint tool, and I'll show you a little bit more later what that looks like and of course where you can learn a little bit more about it, actually shows our customer what portion of AWS's emissions they're responsible for based on their usage. So you log into the billing console and you're able to see that allocated portion. I'll talk about a little bit about how we built that and we provide that to our customers. But what we learned along the way was we weren't listening close enough to our customers. We needed to know really what  was blocking them from making some of their progress on their initiatives and how we could listen better.

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/540.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=540)

So in 2024, we opened up a customer survey to some of our biggest, our most vocal, our most sustainability progressive customers to really gather them together to get feedback. Through this mechanism we were able to say, hey, what can we be doing better? A lot of what we heard in the beginning was we need more transparency from AWS. We need to know how our footprint looks running on AWS. We need to know more visibility about your operations. So we've done a couple of things in response to that feedback.  One of which was releasing our Power Usage Effectiveness ratings in 2024 across our regions. That actually provides visibility into how we're actually using power on site in our data centers.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/560.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=560)

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/570.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=570)

We also released our Water Usage Effectiveness earlier this year.  Similar to power, this is also looking at how efficient we are in water usage in our data centers to cool things down. You're able to look at that at a regional level.  And in 2025, along with some of what we heard, was to level up our Customer Carbon Footprint Tool. Customers wanted to know how we calculated those numbers. Could you tell us the methodology? Is this third-party verified? So we released a verified methodology to say this is actually how we're computing these numbers, and then we've had a third party say that this is up to speed. We did that earlier this year, along with better functionality to export so customers could get their allocated portion more easily.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/620.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=620)

We released location-based emissions, which is a different type of methodology. So we have both our market-based emissions, which accounts for some of our energy attributes and other credits that we use in our total footprint, and then we have the location-based emissions, which is essentially the grid. This is our Scope 2 reporting.  And then just recently, just about a month ago, we released something called Scope 3 emissions. I'll talk more about what that looks like across scopes, but it now rounds out the complete pie of AWS's footprint.

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/660.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=660)

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/680.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=680)

So we have all Scope 1, Scope 2, and Scope 3 emissions included in the tools so that our customers can actually see where the sources of emissions are coming from, whether that be operations, whether it be our energy sources, or of course our Scope 3, so things like our hardware that go into our server racks. And then we have a lot more plans going into this year, so we kind of hit a measure of success to have complete verified reporting,  and in 2026 we're looking to increase the customer experience and additional granularity and keep serving our customers to be able to enable their sustainability initiatives. We'll touch a little bit more on that later, but really we'll continue to hear from our customers like Adobe, like Salesforce,  on what they need and really be responsive to that.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/710.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=710)

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/720.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=720)

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/730.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=730)

So I talked about what Scope 1, 2, and 3 are, right? Some of you may be like, okay, that's old news. I know what that means. Some of you might not know what those scopes are, so I'll just give a quick recap on what Scopes 1, 2, and 3 are. If we're thinking about carbon accounting, it's similar to financial accounting. This is actually how we account for the emissions that we generate as a business. So there's something called Scope 1 emissions. This is our operational emissions, so things like our  backup generators or others that we're fully responsible for. There's Scope 2, which is essentially where we're getting our energy from the grid,  and so the sources of energy that actually supply our data centers. And then there's Scope 3, which are basically all  the components, materials, hardware, cement, everything that goes into our actual physical data centers to actually power all the compute services that we're actually providing to our customers. So that's sort of a recap on all of those just to provide that visibility.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/750.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=750)

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/780.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=780)

So how do we actually compute this assembly  footprint, the carbon footprint? We have a variety of different data sources that come from around our business. So things like hardware components and processors, data center building materials and equipment, all the way down to the cement, the energy consumption, where the grid source, water usage, all of these different data sources we actually take in from all over our business to be able to compute this footprint and actually provide it out to our customers.  And so this all feeds into something I was talking about, the Customer Carbon Footprint Tool that we actually built to supply this to our customers.

So how do we do this? We go through a couple of different steps. Obviously we have to ingest millions of data points from around the company to be able to actually generate that number. We orchestrate that, so obviously we have numbers coming in that are on an hourly cadence, on a daily, weekly, monthly basis, right? So we have different orchestration of how we pull in and generate those numbers and actually tally them all up into our total footprint. We also, because it's carbon, we often need to either apply an emissions impact factor, or we have a team of scientists that actually model the carbon emissions of things like our components that feed into our server racks. And then of course we need to persist and govern that data because it is data from around our entire company, which feeds into some of our downstream analysis and actions. So how we actually plan and forecast what we're doing to meet our 2040 objectives, and of course how we're reporting monthly through the Customer Carbon Footprint Tool.

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/850.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=850)

And so I talked a little bit about  how we kind of progressively take all these different components down. This is sort of how we go step by step on breaking down our top-down allocation for carbon emissions. So we actually do look at across all our regions. We are looking down to the server racks, allocating to our foundational services, something like EC2 or S3, and then allocate those out to our non-foundational services. These are the ones that run directly on services like EC2, which are running on hardware. And then essentially we allocate that out to our customers. So across all of our AWS accounts, we actually then do an allocation model to say, okay, hey, Adobe, across all of your accounts, this is the portion of the emissions that your usage is generating based on AWS's footprint.

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/900.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=900)

### Navigating the Customer Carbon Footprint Tool in the AWS Billing Console

So this is a quick screenshot of  the Customer Carbon Footprint Tool.

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/930.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=930)

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/940.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=940)

I also want to give a quick callout that we have our product manager, Marta Fraga, here who actually supports the Customer Carbon Footprint Tool and will be at a demo. We'll segue to that later, but if you can see, this is the screenshot. You navigate to it in the billing console and you find the tab, and we can look at both market-based emissions and location-based emissions. Then you have different data exports in terms of the range that you can pull for your needs. This would be actually our market-based emissions of  a sample account. Of course, this is nobody in particular's account, but you can see the example here. And then here is our location-based emissions. 

This was just an overview on how we've actually got to the point where we are now across all of our sustainability objectives and getting down to the Customer Carbon Footprint Tool. So now I just want to turn it over to Eric to talk a little bit about how we partnered and worked on this.

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/970.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=970)

### Salesforce's Sustainability Journey: Emissions Reduction, Energy Transition, and Data-Driven Decision Making

All right, thanks Alexis. This is great. Appreciate everyone being here. It's definitely been a journey.  Salesforce has been a customer of AWS for a while, and kudos to you guys for consistently pushing the envelope and bringing more sophistication to a very complicated problem. So thank you everyone who's worked on it. Emery Katz and I lead our tech sustainability program at Salesforce. I want to give you guys a big picture of what we mean when we say Salesforce sustainability. I'll talk about four quick pillars and then we'll dive a little bit deeper into our infrastructure program.

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/1000.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=1000)

 Emissions reduction is probably one of the biggest first starting points that we focus on. We want to be net zero by 2040 as well, and that's looking at long-term Scope 1 and 2 targets, total carbon as well as our carbon intensity for our Scope 3 targets. You can see those reflected up there. To do this, we have to reduce our value chain emissions significantly. That's Scope 1, 2, and 3, and we want to be more efficient internally, the things we have control over for the shared responsibility model. But we also rely on and work with our main providers like AWS to continue pushing the envelope and decarbonizing.

We have Agentforce for Net Zero Cloud product, which is essentially a sustainability management console and helps our clients really track, analyze, report, and reduce their environmental and social impacts. So it's something if you're in that industry you should take a look. It's a great way to tally and footprint, and then we have agents. As you know, Salesforce is big on agents as many of you are, and there's a lot of internal and external agents that are doing a lot of great work with sustainability. If we want to talk about that near the end, we can.

As far as energy transition goes, we are pushing hard to continue moving the industry forward. For ourselves, we have been 100% renewable energy since 2021, and that is Scope 1 and 2. We are looking further than that. We're looking at location-based grid emissions. We're looking at our Scope 3. We're trying to look at, as the market is, market-based emissions by time and space and consider what the right thing is for the world and the economy. We're helping to innovate at scale certain technologies, whether that's efficiency, fuels, renewables, removals, pushing the industry where we can, where we don't necessarily have direct control but we have our influence.

We want to be nature positive. Nature positive doesn't always show up in everyone's sustainability reports, but it's important to us. We believe that the health of business relies on the health of nature. It's kind of a no-brainer. We all float around on this rock together. However, there have been studies from the World Economic Forum and others that say that there's a tremendous amount of economic value. In fact, $10 trillion worth of annual business value and almost 400 million jobs that could be developed by 2030 if we implement nature positive policies out there. So huge to think about.

We want to reduce our impact. We've talked a lot about carbon, but water is a huge focus. It's a hyperlocalized resource. We're thinking about it at our data centers, resilience of the power grids. Power grids use a lot of water, something we're very focused on going forward, and we're also thinking about investing in upstream watersheds, quite literally upstream, to make sure that we have resilient sources of water to continue powering our data centers and our offices.

We also have a $100 million nature and sustainability fund. We are looking at nature-based solutions for decarbonizing through nature solutions like the One Trillion Trees initiative. Many of you may have heard of that, OneT.org, very important initiative. And then we recognize that big companies are not going to do this alone.

[![Thumbnail 1220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/1220.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=1220)

We have to foster an environment where ecopreneurs, small mission-driven companies that are doing a lot of good sustainability work, can be successful. Through our philanthropy or ventures work, we're putting our full capital stack at play there to support them.  So when we talk about our infrastructure sustainability program, I'm focused on a few things over this year into next year on that right side. Keep an ear out for how data transparency and data feeds into each of these things, and I'll talk about that a little more in the next slide.

First and foremost, we're transitioning to public cloud. We have a lot of our co-location data centers currently, but we have been rapidly deploying on AWS over the last few years. That requires a lot of data to understand the right economic incentives, the right sustainability impacts, and then going forward, making the case for continued deployment. But to do that, we also have to right-size that utilization. We don't want to just willy-nilly throw all of our capacity without making sure we're doing it in the right, most mindful, thought-out way. So we have been right-sizing our utilization and getting much more efficient over the last few quarters and years really.

In the shared responsibility model, we have thought a lot about cost-driven optimizations. FinOps professionals, if you're in here, it sounds like there's a few, you think a lot about keeping costs low and thinking about the cost to serve and unit costs. We do the same thing with a carbon lens, and they're very similar pathways. It's okay to work together and one ride the coattail of the other, and oftentimes work together to double the impact and the incentive, the business drivers. But this includes things like selecting the right instances, which I think a lot of you are thinking about, using S3 Intelligent Tiering, upgrading to EBS GP3, things like that, suspending or releasing on-demand or reserved capacity, things that make you more efficient and drive costs down and carbon down.

We're also taking a view of our own infrastructure deploying to our customers in a more efficient and effective way. So we think about the efficiency, and we try to benchmark where they should be based on where we see similar customers and try to help give them tools to deploy the most effectively, which then of course makes us more efficient as well. But we're staying mindful of regions for carbon intensity but also water stress, big deal, and we're giving customers an option to actually deploy or migrate either from their co-location to clean regions or from AWS to AWS regions. If they have priorities to be in the greener regions, we're trying to develop software code that's more efficient, elegant, and avoids anti-patterns that really can drag resource utilization down.

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/1430.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=1430)

So it all starts with that underlying code, and it's both human-generated code and AI-generated code that we're really trying to think about going forward. We're tracking water, we're tracking e-waste, we're trying to set the performance expectations. When we're talking about WUE and PUE, we're thinking about climate, we're thinking about what is the right strategy, the trade-offs between carbon and water. Data is highly important for that, and then of course we're continuing to engage a variety of stakeholders, customers, regulators, investors, and data is super important for those purposes. 

So I wanted to kind of reiterate some of the things. Different types of data have different purposes, and we need a lot of it for a lot of different purposes, right? Location-based grid emissions is important for certain uses. Market-based, we require, and thank you guys for continuing to push to bring more types of data to us. Scope 1, 2, and 3, the supply chain is really important as we become more sophisticated with the decisions we make as customers, suppliers. Our customers, it's a big ecosystem where we have to all think and work together. That, as dissected by master payer accounts, sub-accounts, by regions, by service-level stuff, WUE we talked about by region, but all regions with withdrawals and potentially in the future a variety of other things which are kind of put there in my brain of things, and I know you guys have been thinking about those too.

So those four categories to the right are sort of where we use things. Like footprinting, right? We have to keep track of historicals. We do that internally to track where we are and where we're hitting our goals, but we also have to do external reporting either through our stakeholder impact report, the CDP, or the 10K filings.

All of that requires data from the Customer Carbon Footprint Tool and AWS to get that right. Forecasting and trend setting involves taking historicals and thinking into the future. We have science-based targets we have to hit in 2040, and we need to know what trends we're on and how we're doing with our current trajectory and forecasting going forward.

Internally, it sort of dovetails with this internal analysis, but it goes kind of a level deeper because we think about each of our business units. Many of you know Salesforce is a CRM company, which obviously it is, but we have a lot of other arms. We do marketing and commerce, we have Slack under our umbrella now, and Tableau, MuleSoft, and a lot of these products that people know and love but do have sort of different businesses and different metrics. As we think about that, we need AWS data to match with that to help us find patterns and opportunities.

Lastly is stakeholders. We talk about engaging customers, and that is hugely important for us. Our customers are constantly asking for what their usage of our cloud is, what our products are, and of course we rely on AWS in many cases to sort of pass through some of that data. Also, when we set expectations for suppliers and we engage our suppliers, we think about industry benchmarking. All of that stuff is going to be required, and we think a lot about how we can bring that data in the richest way possible.

[![Thumbnail 1610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/1610.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=1610)

### Adobe's Sustainability Strategy: Three Pillars and Partnership with AWS

With that, I will stop. I'm going to turn it over to Rao from Adobe to talk, and then looking forward to some discussion after that.  Exactly. Thank you, Alexis, for that nice context as well as the introduction. And thanks, Eric, for walking through the Salesforce sustainability journey. It's inspiring to see how this collaboration drives the collective progress in our industry. This is a perfect example of the collaboration.

Hello, good afternoon. My name is Rao Charahala. I'm a senior program manager at Adobe. I lead the initiatives in cloud FinOps as well as sustainability. One of the things is that at Adobe, sustainability is mindful and intentional. The thing that we wanted to do is that sustainability is not a destination. It is actually ingrained as a business DNA within Adobe.

[![Thumbnail 1700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/1700.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=1700)

One of the things that I wanted to focus on here is that  Adobe's sustainability journey stands on three pillars. One is the product pillar, and the product pillar is where we are developing the digital workflows for our customers to enable their process flows. The second one is operational sustainability. In operational sustainability, we are enabling our internal as well as external operations and efficiencies. The third one is that we collaborate, which is the advocacy. We collaborate with our partners, with our customers, so that we are actually going together as one partner model.

To enable this strategy, we have actually developed five guiding principles. One is empower our customers through our products. You are all probably aware of Adobe Acrobat. I'm sure most of you are using Adobe Acrobat and Adobe Document Cloud, through which we enable digital workflows and through which we save billions of papers from printing. Also, Adobe Sign, through which we are enabling the digital signatures, whereby we are reducing the logistical

The second area focuses on building sustainable, innovative workspaces for our employees. Eighty percent of our facilities are LEED certified. For example, one of our recent buildings at Adobe headquarters features on-site renewable energy as well as our own water circulation system, which reduces the load on the municipality.

The third area is reducing emissions and boosting productivity within our own data centers for Scope 1 and Scope 2 emissions by reducing the PUE and cutting down on emissions. The fourth area is employee engagement. We have a number of green teams that engage thousands of our own employees who actually handle ninety percent of our waste recycling as part of the green teams and their efforts.

The last one is what we call transparency, which we achieve through annual sustainability reports. The annual sustainability report not only provides all the environmental parameters but also addresses ethical AI, sustainable AI, and data transparency. These are the five guiding principles that drive our overall strategy. Obviously, it is not just Adobe, but it's a partnership with our partners as well as our customers.

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/1960.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=1960)

Now, when it comes to our commitments,  we have our North Star, which is to become net zero by 2050. This is very similar to most of your organizations. However, the milestones that you see here, the commitments that you see here, are not just aspirations but milestones to our progress. Two of the important milestones which you can see here are reducing our operations emissions, which is Scope 1 and Scope 2, by forty-two percent by 2030. The second target is reducing our supply chain emissions by fifty-two percent, which is where our cloud as well as our supply chain sit in this category.

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/2070.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=2070)

Apart from that, we have four different operational milestones: one hundred percent renewable electricity by 2025, ninety percent global waste recycling diversion, eighty percent of our buildings continuing to maintain LEED certification, and a twenty-five percent reduced water consumption goal both within our own data centers as well as overall per employee.  Driving sustainability is a shared responsibility. Obviously, partnership is a very important aspect.

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/2090.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=2090)

 There are three things to keep in mind. One is the data, deriving the insights from data. That's one important aspect. The second one is the reporting aspect.

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/2130.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=2130)

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/2140.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=2140)

Moving from data to reporting. We have several types of reports, including sustainability disclosures and deep dive internal reports to understand the carbon footprint for each of the products. The third thing is  how we are actually going to get that programmatic data access  from the Customer Carbon Footprint Tool standpoint.

Our partnership with AWS is that our inventory at Adobe is standards aligned as well as greenhouse gas protocol enabled, but where the Customer Carbon Footprint Tool data adds value is in enhancing the data. We do our own data in the form of spend-based estimates, so to bring the precision together, that is where the Scope 3 data plays an important role here. In summary, sustainability is a shared responsibility and a shared progress that we are making. Thank you for all the support from AWS and for bringing that information together. Let's make every pixel and every partnership count. Thank you.

[![Thumbnail 2230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/2230.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=2230)

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/2240.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=2240)

### Call to Action: Collaboration and Resources for Sustainability Initiatives

Awesome, thank you. Actually, the funny story is last year Rao and one of his colleagues Tridar said that this year  they wanted to be on stage talking about how they deliver Scope 3, and so we actually came full circle moment that we are here on stage presenting that. We just have a little bit more  to wrap up here.

As you saw in the headline of our talk, collaboration is key, right? In sustainability, we can't do this in isolation. Sitting up here with two of our customers, they're telling us how we need to meet our commitments and our goals and drive transparency, but also their part in driving their sustainability initiatives and the demand. There are things that you can do, like running Well-Architected Pillar reviews that have a sustainability dimension, so you can work with account managers and any of our technical field community to do those type of sessions.

Of course, as I started the talk today, I saw that only three of you have seen the Customer Carbon Footprint Tool, so that means by the end of this conference, I want all of you to have seen it. Please go see Marta and some of our good friends over in the showcase at the Venetian. We have a very large sign that says sustainability and there's a bunch of cool demos. Please go and check it out. It will be worth your time, I promise.

A shameless plug as I'm doing another talk tomorrow where we actually talk about how we built some of our core data infrastructure using AWS native services. Please also join me for that if you want to see a little bit more of the guts of how we built this using some of our core data and AI services that actually feed into the Customer Carbon Footprint Tool.

### Addressing Generative AI: Balancing Environmental Pressures with Sustainability Opportunities

With a few minutes left, I had heard some conversations in these early days about maybe some topics we just want to bring up. We've got about nine minutes left to just kind of bring some of these questions and topics I've heard, and so I did want to ask you guys a few questions while we're still here together. I think the elephant in the room that I've heard from some folks is generative AI. We're here as sustainability professionals, but I wanted to get your take a little bit on how large organizations like Adobe and Salesforce are starting to grapple with being in a sustainability position but also with the growth of AI, and we know there are more environmental pressures with that. Maybe I'll just ask Eric if you want to comment, or Rao.

Sure, I can start. I think a couple of things is that generative AI is really a force multiplier. Certainly, it has a double edge. One is the positive from the opportunities standpoint. There are several opportunities, one being how we can bring that unstructured data together and then move from data to insights.

That is certainly feasible from the sustainability data standpoint, and also to bring that all the way to the ESG disclosures that we publish. So that's something that we do and that has been happening. But on the other side, which is the challenges front, obviously there are a couple of things that are adding up.

One is that the more you bring in the GPUs, you are adding more energy and water consumption. So for an organization like AWS, the carbon footprint is growing, right. And the other area is that you are refreshing those GPUs so frequently. There is e-waste that is happening unless you are extending the inner life of those GPUs within that. So I think when you are looking at that accounting part of it, you are accounting that to the customers as well, from the hyperscalers to customers.

So this is where the opportunities and challenges exist. But I think it also depends on when we look at finally the ROI versus the cost. So I know sustainability is one of the pillars for the Well-Architected Framework, but what we were trying to do is to introduce sustainability as part of the region selection process. So that's something that is definitely helpful from that.

Yeah, I mean, obviously we're looking at a lot of scary things in the future that may come from AI, but I'm fairly bullish on it. I think that AI is actually going to help sustainability quite a bit. Yes, we have to be mindful of our data center energy usage, and we will, but I honestly think that AI has an opportunity to transform these infrastructure systems, right.

When you're talking about data centers, being able to do better intelligent automation really allows you to save costs, be more reliable, and be more efficient. And even upstream from that, like the power grids, there's a tremendous amount of opportunity, I think, to forecast supply and demand around time and space to get more benefits there, to integrate more renewables to the grid, things like that. So I'm excited, and also it's going to accelerate innovation.

The IEA, I think, said that 50% of the carbon reduction we're going to see in 2050 will come from technologies not even invented yet. Which means that we've got to get these technologies online, and the faster we can do that the better. And AI, I think, is really going to push that forward quite a bit. And I think just modeling, I think AI does this really great where we just take so much data and help us model things.

And when we think about mitigation, whether it's near-term stuff around flood risk or sea ice change, things like that, or longer-term things like drought or tree canopy growth and water retention, those are important. And then even policies like how certain programs will change behaviors, like carbon pricing, things like that. AI can help generate models around what's going to be the most impactful and help our policymakers and our businesses and other organizations do better.

So it's just an exciting time. I think just like for the rest of the world and industry, there's all opportunities, but I think sustainability really needs to leverage AI as much as possible. And I think the whole industry would be better for it.

Yeah, awesome, great comments. I mean, I will also say that as an engineering manager myself, we have our developers building products at a faster pace than ever before, leveraging some of our core developer products like Q Developer, and then of course tomorrow I'll talk a little about Bedrock where we're actually using that to make insight and actionability more accessible to all of our internal and eventually external stakeholders as well. So we're seeing, obviously, some of the daunting things that we need to continue to operate more efficiently, but then also how we're building and supplying more sustainably, transparency, and progress leveraging AI. So we're also seeing that here with AWS as well.

### Looking Ahead: Future Needs for Data Granularity, Accessibility, and Continued Partnership

So I wanted to ask just one more question. We may have time for a third one, but we obviously talked about our journey this year, right. And where we thought we were last year, we were like we're never going to get there, but we got here and we're on the stage saying,

we've kind of reached this moment. Do you think we're done? Have we checked the box? Are we ready to go home now? Or what else are Adobe and Salesforce needing from AWS in this space? Can you talk a little bit about some of the initiatives you're driving and how we're going to continue to collaborate?

Yeah, I mean, it's kind of a loaded question, right? We're never going to be done. I think as sustainability professionals we have to keep pushing the envelope, and again, tremendous amount of progress. It's not mission complete quite yet for any of us, but I do want to reiterate that customers really do care, and I think the world cares. I know we're in a time right now of very interesting political winds, and I think it's a time for business to lead, show leadership and be true to your values. The pendulums will continue swinging constantly for the next 100, 200, 500 years.

So I think the importance of sustainability, it's not just about hugging some trees. It's really truly about delivering social value, economic value, business value, longevity of your business, de-risking, improving a variety of different things that are associated with your business, driving down costs, et cetera. So for us, it is not done. Thank you for your partnership. We want you to keep going.

I think the things we would like to see is just continued accessibility, more access to our data and maybe even better tools, easier APIs, more recent data, right? No lag time, which I know is on the horizon, which is exciting. Granularity of data, right? We sometimes look at very high levels, but going forward we're going to go deeper and deeper and deeper in our stack and try to understand where the carbon footprint's coming so we can analyze that at a better level to help us make better decisions in our sustainability program.

And I think probably actionability or identifying opportunities within our deployments on AWS is an area where we'd love your support more and more and help us understand the places in that shared responsibility model. I know there's a very clear pink and blue category, but the reality is you know so much about your infrastructure. You know so much about running workloads on the cloud that the more you can help your customers figure out ways to be more efficient, I think would be great. So these are things we could talk about next year, right, and the year after and the year after. We'll stay busy.

Yes, yeah, I could just say ditto, but one of the things though, certainly more and more of our customers are asking about what's the carbon footprint for your product X. We are not there yet. I think we at this point in time are trying to bring that together to be able to dissect and then allocate it to the product level. It takes a little bit more effort. So that's one use case.

And the other one is that, again, mostly the customers from Europe, they started asking the questions about AI, generative AI footprint, and I think, again, that all points to what Eric you are mentioning about the data granularity and the data accessibility. I think that those are the top items on our agenda as well. And then we are actually excited about the Customer Carbon Footprint Tool console. It's so because of that, that gives them more access from that standpoint.

And of course, one of the exciting things that you were mentioning about in the roadmap is the monthly data, availability of monthly data. So yeah, no, I mean, I think for the last 12 months or so, I think there's a tremendous progress that's been made, but certainly continue to push the envelope for data accessibility and the granularity.

That's awesome. I mean, I always like to hear that there's going to be plenty of work for my engineers for a while to come. So sounds like we got a lot of things coming. So we have a lot of things planned, and as I look over at Marta, who will be one of the key orchestrators in 2026, so keep watching this place. Lots of interesting things in terms of where we're driving granularity so that partners like yourselves, like Adobe, like Salesforce can continue to innovate on AWS with sustainability in mind.

So really thankful for you guys joining today. Like I said, just one more shameless plug. Go to our demo desk in the showcase, big sustainability sign. I would love to see your faces again tomorrow at the other session where we're actually going to do a chalk talk building out sort of the infrastructure, how we built some of the underlying, I like to call it guts of the Customer Carbon Footprint Tool. So would definitely be happy to see you there as well.

[![Thumbnail 3040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/de8dff5d0d2b7d3b/3040.jpg)](https://www.youtube.com/watch?v=7xMMAKI1Q1o&t=3040)

Also, please rate our session 5 out of 5 stars. We are, you know, sustainability is still fundamental, right? It's still super important. So want to keep bringing back these insights and bringing back these perspectives for you. So I really appreciate your time today. Don't forget AWS SkillBuilder.  Here's my obligated slide that I need to pop up for you, but we'll also, I kind of did sort of a pre-curated set of questions, but we'll be around for a few minutes if you want to come up and chat to us after. So thank you again so much.


----

; This article is entirely auto-generated using Amazon Bedrock.
