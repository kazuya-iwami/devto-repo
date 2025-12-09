---
title: 'AWS re:Invent 2025 - Bronto: Logs at Scale Without Compromise (DVT224)'
published: true
description: 'In this video, Mike Neville-O''Neill from Bronto and Aodh O''Mahony from Teamwork.com discuss solving enterprise logging challenges at scale. Bronto is introduced as a logs-first observability platform offering 12-month hot retention at cents per gigabyte, breaking the "3C Flywheel of Compromises" (cost, coverage, complexity). Key features include AI-powered dashboard creation, automatic parser generation, Statement IDs for code correlation, and subsecond search across petabyte-scale datasets using AWS Lambda and proprietary S3 storage format. Teamwork.com shares their success story: managing 33TB monthly logs across three AWS regions, achieving 42% cost savings, extending retention from 2 weeks to 12 months, and eliminating maintenance overhead from self-hosted Graylog and OpenSearch solutions.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/0.jpg'
series: ''
canonical_url: null
id: 3092975
date: '2025-12-08T19:03:09Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Bronto: Logs at Scale Without Compromise (DVT224)**

> In this video, Mike Neville-O'Neill from Bronto and Aodh O'Mahony from Teamwork.com discuss solving enterprise logging challenges at scale. Bronto is introduced as a logs-first observability platform offering 12-month hot retention at cents per gigabyte, breaking the "3C Flywheel of Compromises" (cost, coverage, complexity). Key features include AI-powered dashboard creation, automatic parser generation, Statement IDs for code correlation, and subsecond search across petabyte-scale datasets using AWS Lambda and proprietary S3 storage format. Teamwork.com shares their success story: managing 33TB monthly logs across three AWS regions, achieving 42% cost savings, extending retention from 2 weeks to 12 months, and eliminating maintenance overhead from self-hosted Graylog and OpenSearch solutions.

{% youtube https://www.youtube.com/watch?v=gcGWZlsHHvQ %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/0.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=0)

### The 3C Flywheel of Compromises: How Legacy Logging Solutions Create Cost-Driven Trade-offs

 Good morning and welcome. My name is Mike Neville-O'Neill, and I'm the Head of Product at Bronto. Today I'll be joined by Aodh O'Mahony, Engineering Manager from Teamwork.com. Benoit is currently occupied with booth duty, so you can check him out over at booth 1757. Today we're going to be talking about logs at scale without compromise.

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/30.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=30)

So picture this: it's 3 a.m., something's on fire, and you need to figure out which of the seven logging systems your company uses the data resides in.  Is it in CloudWatch? Is it in Elastic or Graylog, or is it in the S3 bucket with all the Athena queries that nobody remembers how to run? If any of this sounds familiar to you, you're not alone, and it's a symptom of what we call the 3C Flywheel of Compromises, and it starts with cost.

Legacy logging solutions are prohibitively expensive at scale. Whether you're running a Datadog or a New Relic, or you're running something on-premises like a Graylog or Elastic, as soon as you hit scale, cost becomes the major constraint for every subsequent decision that you're able to make. The first thing that you're likely to do is to start cutting retention of your data sources from 30 days to 7 days, maybe then to 3 days. Once you've shortened retention as much as you can, you start cutting data sources: CDN data, debug logs, anything that's remotely considered noisy ends up on the cutting room floor.

This invariably leads to coverage gaps across your infrastructure, where it's essentially like trying to understand a novel with several chapters torn out of it. Teams notice these coverage gaps, and visibility is still required into this data. Just because it doesn't fit into your budget doesn't make it any less business critical, and so teams invariably will spin up workarounds in order to get visibility into this data, which leads to a proliferation of tools in your environments and increases cost. Not just in terms of the total cost of ownership of the products, but the cost of the friction as engineers are trying to move across different tools with different query languages, different retention policies, different access controls to stitch together a cohesive picture of what actually occurred in their environment. This is the exact cycle that Bronto was built to break.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/140.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=140)

### Introducing Bronto: A Logs-First Observability Platform with AI-Powered Features and Transparent Billing

So, what is Bronto?  Bronto is a logs-first observability platform where you can retain all of your data for 12 months hot by default. We've been tested at petabyte scale, and although most of our customers are operating in the multi-terabyte range, we're more than ready for what's coming down the pike. The entry point is cents, not dollars per gigabyte, and it's our efficient architecture that enables this cost efficiency that I'll be talking about a little bit down the line.

We have exceptionally transparent usage and billing. One of the biggest problems when a bill is spiked in a logging platform or an observability tool is understanding why that spike occurred. With Bronto, you always know why. We can integrate with any agent. We do not have a proprietary agent of our own, so any telemetry agent you care to name can be used to send data to Bronto, whether that's a proprietary agent like a New Relic or a Datadog, or it's an open source collector like OTEL or Fluent Bit. If it speaks HTTP, you can send logs up to Bronto. You can even curl up a payload in your terminal if you so desire.

In terms of AI-powered log management, the AI features we have are available in shipping and production today. This is not roadmap theater. Everything that we're going to be seeing in the presentation and the slides is on product and ready for you to use. Lastly, we're able to deliver subsecond search against multi-terabyte sized datasets, and our efficient search architecture enables that capability, which I'll be talking about in more detail in just a bit.

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/230.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=230)

 Here you can see a couple of aspects of our interface. On the left side of the screen, you'll see our dashboard, which is providing some visibility into CDN data where we can see cache hits by POP, requests by geo country, the sorts of operational visibility that you would expect from a tool like this. On the right, we see the Usage Explorer, and this is what I was talking about earlier with transparent billing and ensuring that there are no surprises in terms of the drivers of cost in your account.

The Usage Explorer is primarily designed to help you answer three questions. Number one, how much data am I ingesting? Number two, what teams are ingesting that data? And number three, is anybody actually using the data that we ingest? We give you visibility granularly into search and ingestion metrics so you can figure out, am I actually using the things that I'm sending to Bronto and make more informed decisions about the data that you're choosing to keep in the platform.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/290.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=290)

 Next, let's talk about features and capabilities. Here at Bronto, we are obsessed with reducing MTTR and removing toil. We think that there are a number of problems that LLMs can be applied to to reduce the amount of work that you have to do to make your data and your observability tool of value, starting

with AI dashboard creation where one of the biggest pain points of using or migrating to any new observability platform is creating new dashboards, creating new widgets, understanding the query language and how the platform works. We have a natural language dashboard creation feature that gives you the ability to specify a prompt to get what you need. So it's really just as simple as typing in, show me all of my 500 errors across all of my services and you're off to the races.

Speaking of reducing toil, we've also implemented AI generated parsers. We're able to automatically identify the format of your data, create a parser for it, and then apply it. So there's really no more need for you to be writing regular expressions or grok patterns, unless it's something that you want to do. With Bronto, it's no longer something that you have to do.

AI investigations are where we're bringing LLMs to bear along with our context engineering and domain knowledge to shorten the time that it takes to investigate errors in your log data. We go a lot further beyond than just explaining what a particular error is. We go towards understanding what the source format of that is, what the key fields are. For example, if we see a 500 error, then the key fields might be something like client IP, endpoint, customer ID, and more.

We'll automatically launch queries across your data set in order to build a coherent timeline of what occurred, suggest an RCA, and also generate additional queries for you to run to continue your investigation to understand what the blast radius is. So with the error, with the Brontoscope or the error explanation feature, you can cut down your investigation time from 15 to 30 minutes to 15 seconds.

Statement IDs is something that is brand new and is unique to Bronto, and it is a technology that allows us to associate a unique fingerprint with every log message that you send to the platform. This allows us to do a couple of different things. One of the things that we can do is give you the ability to go directly from a log line to the code that generated it in GitHub. This is incredibly powerful from going from a monitor notification directly to your code so that you can go straight from finding to fixing.

The other piece where this becomes interesting is if you're familiar with something called log patterns, where with Statement IDs we can understand which of those messages occur with the most frequency and perhaps more interestingly which happened with the least frequency. Where are my outliers? Where are the new messages that I've never seen before? Why are messages changing in a sequence or transaction, where it's almost always the same type of logs? All these types of outliers can be identified rapidly using Statement IDs.

This quarter we've also released distributed tracing, particularly around cross-service correlation. Although we are logs first, we are not logs only, we're not zealots, and we believe that there is a place for telemetry signals to live in our platform to kind of assist in getting to the log data. Traces are exceptional for understanding where something has broken, and with the log data automatically correlated, you can very easily understand why.

This is a smaller thing, but maybe less small if you're trying to understand a monitor definition at 3 a.m. with bleary eyes, which is that we'll use AI to automatically name things like alerts, parsers, dashboards, et cetera, so that you're not required to come up with those conventions yourself.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/520.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=520)

### Building Bronto on AWS: Decoupling Storage from Compute for Subsecond Search at Petabyte Scale

Next, let's talk a little bit about how we built Bronto. The first thing that's worth noting is that we built Bronto entirely on AWS and that our architecture decouples storage from compute, which is really the key to our cost efficiency.  I'll kind of take you through the diagram starting with ingest, and we can see that data comes in over HTTP via any of the agents that you care to name that I discussed earlier, before streaming to EC2 instances behind load balancers.

From there we stream the data to MSK where it's written to S3 in a proprietary format. I'll discuss in more detail in just a bit. In terms of the web UI and API, we're using a combination of EC2 and CloudFront to make those things available to our customers.

In terms of storage, we're obviously using S3, but the way in which we're using it is where we really start to diverge from the other solutions that you see in the space. We actually write to S3 in a proprietary format that's been optimized specifically for log data, and this format makes use of all of the techniques that you would expect from a modern analytics engine: aggressive compression, columnar storage, bloom filters, data partitions, all built from the ground up to return your logs to you as fast as they possibly can. It's kind of this emphasis on the proprietary format that allows us to deliver speed in a way that other tools cannot.

But storage isn't quite where it stops. When we're talking about search, we have to mention Lambda as well, which in many ways is the backbone of our query experience in the sense that it allows us to deliver incredible power but also maintain exceptional cost efficiency by avoiding overprovisioning. So for example, when you run a query that needs to scan a very large volume of data, we have the ability to spin up Lambdas in parallel to process it.

On the other hand, we are only paying to use that compute when it is actually being used or when the Lambda is actually active, and that gives us the ability to be exceptionally cost efficient as well.

Essentially, if you're running a query with a large volume of data, we can spin up hundreds of Lambdas if needed to query S3 concurrently and get you those results as fast as we can. Because we're only paying for the compute that we're using, we can run a cost efficient platform and maintain that cost advantage and pass it on to you. In terms of security, we're using Cognito for authentication, Verified Permissions for our backend, and on the vulnerability management side of things we're using Inspector as well as AWS GuardDuty.

In terms of the cost model that we've come up with, it's going to be a bit challenging we think for other tools in this space to replicate that because the cost models inherently assume an expensive indexing cost for every gigabyte that you send to them. We're built fundamentally differently to assume that the price for that should be cents rather than dollars. So what does this ultimately unlock and what does it do for our customers?

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/690.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=690)

I want to draw your attention to the lines that you see on  the graph here where the X axis represents the size of the data set being queried in gigabytes and the Y axis represents the duration of the query in milliseconds. Just check out all of these lines. It really doesn't matter what you're doing, whether you're trying to count log events, detect a SQL injection attack, find a rare UUID, that classic needle in a haystack use case, or list all the events with slow response times. Ultimately all these queries complete in under 500 milliseconds against a 5 terabyte size data set. This is the kind of performance and experience that our architecture unlocks for you.

### Teamwork.com's Logging Challenges: Managing 33 Terabytes Monthly Across Complex Infrastructure

But given where we are in the talk, I think it's probably better to hear directly from what the benefits of bringing Bronto to Teamwork have been for him. Thank you, Mike. Morning, everybody. My name's Aodh O'Mahony. I'm an engineering manager at Teamwork.com. We're based and founded and headquartered in Cork in Ireland, founded in 2007.

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/770.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=770)

Our founders, Peter and Dan, ran an agency for several years before that, so they saw the need in the market for a project management solution. They built out the Teamwork suite, which is easy to use project management with streamlined operations. We were very proud of being self-funded for years, but the last few years we took on a big $70 million investment to help us  scale the growth of the product and add really cool new features into it. As of recently, we have over 20,000 customers worldwide, and having that many daily users is the reason why we have logging challenges which Bronto have been very helpful to help us solve.

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/790.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=790)

Just another quick  rundown of what Teamwork is. We're a project and resource management product powered by AI built for service teams. It's to help teams adopt quickly and get value from the work that they do in their business and make sure that managers have key insights into their financial performance. So it's all about getting work done and getting paid to get the work done and leaving the friction of the product to one side.

The main areas that we cover, project management was always the core of the product when launched, but over the years we've also added in resource management and financial management, which when put together give real value to customers. Then we have some supporting features to the side. We have reporting, we have integrations, deep automations. More recently, AI is a fundamental part of our product stack, and we're shortly releasing a new generative AI feature called Teammates, which will really help give customers extra value with the product.

Dashboarding as well then for insights into their usage. And then as well we have Teamwork Desk, which is our help desk product and it supports client communications, B2B communications. Spaces is our knowledge base and help and documentation store. A recent product we added as well is Teamwork Connect, which is a way for customers to integrate their own BI tooling directly into our Teamwork data again to get better insights.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/890.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=890)

The overall aim is to give insights into increased billable utilization and improved profitability, helping customers to get the work done, to get the team's work billed, and then to iterate on that each month through the rest of the year. So,  our logging challenges before Bronto came along, we have a complex infrastructure. It's a mix of self-hosted open source, cloud-based and custom solutions with a lot of difficult query, structured and unstructured data, indexing issues.

We roughly have about 33 terabytes of logs a day, sorry, a month being ingested, billions of logs from different systems.

That requires, that causes operational overheads. We operate in three AWS regions, so we're a big AWS customer, two production regions in the US and the EU region, and then a replicated staging setup for testing. That requires a lot of ongoing maintenance when you have a lot of self-hosted systems. So things like Graylog, having to replicate Graylog configurations into all these different accounts is time consuming.

We had limited coverage. So the two main systems we were using were for our customer traffic that was coming in, traffic logs. We were using CloudWatch, but we had retention rules in CloudWatch which only retained about two weeks' worth of data for cost reasons, which is a challenge because you just don't have a long-term view. On the developer's side, everything was going into a giant Graylog store which was backed up by OpenSearch. Again, because of costs, you have to set a limit on your OpenSearch node sizes, which effectively turns it into a first in, first out log store. Very noisy teams would end up expiring logs, so you could never really rely on having a long-term view of your logs. And you just had no real insight into which team was using the log allocation.

Cost inefficiency then, operating all this stuff was very expensive, even though it wasn't very heavily customer facing, so it wasn't giving sort of direct value to customers, but it was costing a lot of money to run it. So it was good for operating the business but not for customer features. And it was inefficient and it was just brittle. So that's where Bronto came along.

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/1010.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=1010)

### Achieving 42% Cost Savings and Simplified Operations: Teamwork.com's Experience with Bronto

So what we have now with Bronto, simplicity. The unified logging solution has replaced a ton of legacy tools like Graylog  and the CloudWatch logs, custom solutions. We've no index management. We don't have breaking OpenSearch indexes anymore. We have a much longer retention, so standard 90 day retention, 12 month retention for our customer traffic logs, so we get really good detail of our long-term customer behavior.

We've been able to save 42% just in software costs by switching to Bronto. So that doesn't include the maintenance effort, but freeing up our SysOps engineers who have to maintain these custom solutions. And when we were doing comparisons of Bronto to other SaaS solutions, some of the other providers, their ingest cost alone was going to cost more than the entire operational cost of Bronto for the year. So from a price point, it was a very positive experience.

And then some of the new features we have now with Bronto we never had before, so the Team Explorer, the usage Explorer, it gives us very good detail into what teams are actually creating the logs and where the usage actually is in the system. And the bigger one as well is we can actually put the logging UX in front of teams and staff that are not very technically experienced or confident, so support teams, product managers can now interrogate logs in a much easier way and it reduces the burden on engineers to have to do all that preparatory work to provide logging outputs. So all in all, it's a great experience.

And as well, the other quick thing is just the customer experience of dealing with the guys. Any changes, we've asked for feature additions. They've been very responsive to make them. They've just been a dream to deal with, so it's been a really great experience. Back to Mike.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/d9efec9d2164bac5/1120.jpg)](https://www.youtube.com/watch?v=gcGWZlsHHvQ&t=1120)

Thanks very much, Aodh. Appreciate your time and attention today. If you've liked what you've heard, come over and talk to us over at booth 1757. You can also sign up for a free trial at Bronto.io. No credit card required. Thanks again. Have a good day. 


----

; This article is entirely auto-generated using Amazon Bedrock.
