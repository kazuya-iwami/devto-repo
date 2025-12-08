---
title: 'AWS re:Invent 2025 - Deep dive into databases zero-ETL integrations (DAT445)'
published: true
description: 'In this video, Dave Gardner explores AWS Zero-ETL integrations for simplifying data pipelines between operational and analytical systems. He demonstrates implementations from DynamoDB to Redshift, S3 Data Lake, and OpenSearch, plus Aurora MySQL/PostgreSQL to Redshift integrations. Key topics include CQRS architecture, handling NoSQL to columnar database mapping, filtering capabilities, cross-account support, and automatic schema change propagation. He covers newly announced features like Oracle at AWS and self-managed database support, along with monitoring through CloudWatch metrics for pipeline health, row counts, and latency tracking.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/70.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Deep dive into databases zero-ETL integrations (DAT445)**

> In this video, Dave Gardner explores AWS Zero-ETL integrations for simplifying data pipelines between operational and analytical systems. He demonstrates implementations from DynamoDB to Redshift, S3 Data Lake, and OpenSearch, plus Aurora MySQL/PostgreSQL to Redshift integrations. Key topics include CQRS architecture, handling NoSQL to columnar database mapping, filtering capabilities, cross-account support, and automatic schema change propagation. He covers newly announced features like Oracle at AWS and self-managed database support, along with monitoring through CloudWatch metrics for pipeline health, row counts, and latency tracking.

{% youtube https://www.youtube.com/watch?v=rwSfnVd1Bgs %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
### Introduction: The Challenge of Bringing Operational Data into Analytics

So reinvent day four, last session before replay. Appreciate you all coming out to the northernmost area or place for reinvent. So making the trek up here. I'm actually down in the MGM. I was at Mandalay Bay, so it's fun traversing all these different locations. So definitely appreciate y'all being here. So how many folks here are data architects, data scientists, ETL programmers? All right, good, a big majority of you. That's great. We're going to cover some things in the session that's going to help you out, hopefully make things a little bit easier.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/70.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=70)

So in those types of roles you're going to worry about data pipelines. How do I get my analytics on my operational data? How do I bring together maybe multiple transactional systems into one analytical system and then normalize all that data together? And then the last thing is, aren't you tired of those late night pages, escalations, and things like that? So if that's the case, good news is we're going to take a deep dive into AWS's Zero-ETL integrations to try and help make your life  easier, simpler, better, so you can go focus on all those cool Gen AI things that we've been talking about all week long and those agents.

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/90.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=90)

My name's Dave Gardner. I'm a database specialist solution architect, been with AWS for about nine years out of Charlotte, North Carolina, and we're walking you through the session today. So first we're going to cover the business or use  case for bringing that operational data into our analytical systems, kind of why we're going to do that. We're going to cover the key features of Zero-ETL. Then we're going to walk through the steps of creating AWS Zero-ETL for DynamoDB and Aurora, which, a quick show of hands, folks using Aurora as their source system? That's pretty much everybody. That's good. And then DynamoDB? Okay, it's great.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/140.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=140)

I'll also cover there's lots of permutations on the Zero-ETL. We continue to expand it, so we'll also talk about Oracle RDS, and then also last week we announced some new things we'll cover that as well. So it's pretty broad, but we'll go deep on a couple of them which seem to be the most popular in the audience. That's good. Last we'll cover management and monitoring on that. So why do we bring our transactional data  over to analytic systems? It's to enable key insights and drive business value.

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/160.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=160)

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/170.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=170)

So a couple of examples: customer relationship management, fraud detection if you're in financial services, gamer leaderboards if you're a gaming company, inventory optimization, sentiment analysis, as well as product insights and sales, and then there's many more use  cases, right? So we have a lot of analytical needs we want to do on that transactional data and mine all that gold out of there, right? So let's, since this is a 400 level session,  let's take an example and let's dive deep on it.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/200.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=200)

### Understanding CQRS Architecture and the Complexity of Data Pipeline Management

So our transactional data is in our Aurora MySQL database. We've got reviews data from customers that have purchased products from us. We're going to use some Gen AI functions to determine sentiment of those reviews of those products, and then we're going to use Gen AI again to generate a prompt based on the negative, neutral, or positive feedback of those particular sentiment analysis and reviews that we've got. So let's walk through what that looks like.  Sorry for the small text. I tried to squeeze it all in one.

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/230.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=230)

At the very top up here you've got the integration between Aurora Zero-ETL and our Redshift cluster. Then so that brings us over our transactional data. Then we're going to bring in our products. Our reviews were already on there. So then we're going to join these two together and build a table that has the reviews and the transactions together based on customer ID.  Now we're going to build an external model to call Claude and bring in our LLM to determine what the sentiment is of those particular reviews. And then we're going to basically build that out to a table.

[![Thumbnail 250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/250.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=250)

Then the fun part is we're going to proactively take that sentiment and we're going to use our LLM  or our agent to go build a response based on the customer's feedback. So if the customers had a great positive review, we may tell them, hey, this is the next best product that you should go with what you purchased. If it was a neutral, well, maybe we'll give them like, hey, you know, a different kind of response. But if it's negative, maybe we give them like a free shipping or a discount coupon off their next purchase to try and earn trust back for that customer or make things right that were there.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/290.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=290)

So this really just gives you an example of why getting that transactional data over to our analytical systems is important, and Zero-ETL is one of those ways that's going to make it easier for us to be able to do that.  Now we're going to dive deep into the architecture of both the transactional side and on the analytical side, and so the architecture that does this is called CQRS, which is Command Query Response Segregation.

The other analogy I like to use for this is "right horse for the right course." For our transactional systems, think about last week. Last week we had a major US holiday. There was lots of travel, a lot of people going to see loved ones, and then there was Black Friday and then Cyber Monday. From a retailer perspective, there were lots of transactions going on in the background for those. These transactional systems need that high availability, the performance, speed, and availability to support those peak workloads when they happen. They can't be bothered with analytical functions.

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/360.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=360)

We have DynamoDB, DocumentDB, and our RDS relational databases in Aurora as that data persistence layer in that architecture, and it may be your classic three-tier web app. This is a serverless example, but you kind of get the operational side. We've got to run the business. Don't mess with it, right? That's kind of the thing. Now, we need to extract that transactional data  out of those systems so that we can go mine that for gold and run those analytics that we want to on top of that data.

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/380.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=380)

For DynamoDB and DocumentDB, we'll turn on streams. We'll have a Lambda function that extracts that data out and ingests it. For your relational databases, we'll use replication, or maybe it's DMS, or maybe it's your favorite ETL tool to pull that data out of those relational systems.  Once we have it, we're going to load it into our analytical systems, and I got one more build out. So before you take pictures, you might want to hold on to that.

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/400.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=400)

We've got S3 Data Lake, Redshift, OpenSearch, and then maybe we just have another database we use for reporting purposes. Now here's the cool stuff. If you want to take your pictures now, it would be good. That's drawing out the full architecture,  right? Now we have the data over in the analytical systems, and we can really start to mine that data. We can do data enrichment processes, data value-added processing, SQL analytics. We can plug in those LLMs, all those agents, do data discovery, and do all those cool things we just talked about that really impact the business and earn trust with our customers and help drive us forward.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/440.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=440)

Again, on the operational side, it needs reliability. It needs to have independent scaling and drive the business. So this is the overall architecture of bringing those transactional and those analytical systems together. Now, this part in the middle is the fun part, right? And this is the part that, you know,  it gets complex, and we'll go into some of the reasons why it's complex. It's also fragile, right? And now with AWS Zero-ETL, in a lot of cases, that's undifferentiated heavy lifting that you can offload to a managed service. So let's get into how that managed service is going to help you.

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/460.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=460)

### DynamoDB Zero-ETL to Redshift and LakeHouse: Setup and Configuration

AWS Zero-ETL  integrations, we want to make it simple to set up those pipelines of data for you. We also want to make it simple to manage those. Enable those powerful analytics that we talked about for the sentiment analysis and every other cool thing that you want to do with your agentic AI agents out there. The Zero-ETL integrations are going to enable that and make it easier for us to do so you can go focus on those other value-added processes and things. So again, simple, secure, easy way to enable analytics on your petabytes of transactional data.

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/500.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=500)

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/520.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=520)

Let's get into a little more detail on how the Zero-ETL works and operates and  things that you'll need to do for it. Now we talked about the complexity, right? So let's take a look at our DynamoDB table, which is the first source system that we're going to look at. It's a NoSQL database, key-value pair. It has a partition key and a sort key, and you have multiple attributes or varying attributes associated with that NoSQL database.  Here's an example. Here's my product description. The product description has a product ID. It's got some different attributes on it. And then I've got this nested JSON description field that I have baked in there.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/550.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=550)

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/570.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=570)

Getting that over to Redshift, our columnar database, will take some mapping, right? In your own ETL pipelines, you have to do that yourself. We're going to show you how Zero-ETL will help you with some of that. The other challenge with DynamoDB is single table design, right? So single table design basically means I  have one table, but I'm storing an item collection. I'm collecting data that has similar characteristics or keys, but it may not be the same row. Now, in this case, I have invoice and billing data in the same table. Now I'm going to have to ETL and extract that out and do all that logic to get it out. 

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/590.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=590)

So let's talk about our targets. Redshift, it's a columnar MPP database. It's got columns, distribution keys, sort keys, and the SUPER data type. And so now I've got to map this NoSQL database over to this columnar database. That's one of the things we're going to help with.  Now the LakeHouse S3 target from DynamoDB is similar, except it just doesn't have the SUPER data type. For even more fun, OpenSearch is also a target system that we can do from DynamoDB. Now we're talking documents, index IDs, and search criteria, right?

So it's a completely different data layout. This is part of that complexity in managing your data pipelines and figuring all this mapping out. This is where Zero-ETL is going to help you with that.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/620.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=620)

 So DynamoDB as a source for Zero-ETL. There are some upfront things that we have to do before we can start the process of walking through building that integration pipeline. First, it needs Point In Time Recovery, so you turn that on. The other thing that you need to do is it needs to have a table policy upfront that will enable Redshift and Glue to go scan our table and our export table.

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/650.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=650)

All right, so what does that policy look like? And there's examples of this in the documentation as well, but just kind of walk through.  Hey, we tell the table resource policy Glue and Redshift can get to this table. What can it do? Well, it can do the export table to point in time, describe table, describe export. Which table can it access? We then describe that, right? And so this basically is the policy that you set up, and then the rest of it, Zero-ETL will kind of map it out for you.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/670.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=670)

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/680.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=680)

 So just wanted to cover that upfront. So this is the Glue console again. It starts a little small on the screenshot.  But you see here we've got multiple source types. For this particular use case, we're going to focus on DynamoDB. Now you also have Salesforce and other things, so you can see that Zero-ETL is a big thing for us. We continue to invest based on customer feedback like y'all. We'll add things in here that we need to.

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/700.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=700)

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/710.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=710)

All right, so for this particular walkthrough, we're going to pick  DynamoDB as our source. And in this case, it's Redshift and LakeHouse or S3 as the target for this particular walkthrough.  So we define our DynamoDB table. One of the cool things that's here is that cross-account is out of the box, right? So maybe I have my transactional operations in one AWS account and my analytics are in another account. This will do that for you, right?

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/750.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=750)

So the other thing I'll define is whether it's going to go to Redshift or it's going to a data lake, and then what's the actual catalog here, and then what's the target database. And then what's the policy, and you'll see this little fix it for me here. This was done before AI agents kind of came about, but it's a similar concept, right? It says, hey, you don't have the right resource policy out there.  Would you mind if I go out and build it for you? And so that's a fix it for me feature that's there.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/760.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=760)

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/770.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=770)

So what does that look like, right? So it says, hey, your data catalog  resource policy and S3 location need to have an IAM policy that allows Zero-ETL to work. And here's what the policy is. You say, hey, that looks great, go do it for me, right?  Now on the output settings, this is where we get into the mapping that we just talked through, the different data types and data layouts. So you'll see here it's got a couple of options. I can either unnest just the top-level fields, I can unnest all the fields, or don't unnest anything.

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/800.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=800)

And then from a partitioning perspective, you can either use the partition keys that are native to DynamoDB, or I can use custom keys. And then you also configure the table name that you can name it here.  All right, so the data mapping partitioning options. Now this does vary based on target, right? So if I've got Redshift, we'll see what it looks like. And if I have the data lake as a target, it's an Iceberg table, right? So they're going to operate slightly differently.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/820.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=820)

And here's what that looks like. So here's my products table again.  Here's what the definition looks like over in Redshift when I do the Zero-ETL to Redshift. And so you'll see I've got my key as my distribution key in Redshift. So it's going to distribute on the same keys. So that'll give us good spread, normal distribution, assuming our DynamoDB partition key is good. And then we're going to take the rest of the values and we're going to put them in the SUPER data type and value.

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/860.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=860)

And Redshift does have functions in it where you can pull data out of that SUPER data type and be able to use it. Now if you wanted to split those out into separate columns, you could certainly do a value-added process and do that as well. All right, LakeHouse or S3 as my target. So  I copied an excerpt of the Iceberg table. The Iceberg table definition is like this big, it wouldn't fit on the screen, but I did pull the description field out here just so you can get an example of that.

And then here's the Athena query of what that Iceberg table looks like. So you can see I've got the product ID, product name, the different attributes, and then the description field in the JSON format here. So similar capabilities, right? The good news is that Zero-ETL did all this for me, set up the tables. I can create it in just a couple of minutes.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/890.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=890)

 All right, so last couple of steps for setting up the Zero-ETL. We're going to configure the security integration. You can have a different KMS key for your pipeline, so you can have your customer encryption key or use the service key. You can also, there's a one-time move option.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/910.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=910)

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/930.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=930)

So maybe I just want to take a one-time snapshot of my DynamoDB table  or I can do ongoing replication. So as my products change and things happen, that'll percolate over from DynamoDB over to Redshift or the lake house. And then also just give it a name so that I can describe it. And so it's pretty much the DynamoDB to Redshift or the lake house  setup.

Once you activate this and create it, it takes probably 5 to 10 minutes. Under the covers we're doing all the setting up of Glue and all those other pipeline steps for it. It'll go from creating to active, and then once that's done, it'll start every 15 minutes basically doing a pull of that data and then sending it over to Redshift if you do the ongoing replication. Just kind of under the covers, what it uses here is it does leverage the DynamoDB S3 export as well as the point in time recovery. So your data freshness on your target system or analytic system is about 15 minutes. So if something happens in DynamoDB, about 15 minutes or so it'll show up in the data lake, and that typically works well for Redshift analytical type queries. You don't need that real-time response.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/990.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=990)

### DynamoDB to OpenSearch Integration: Real-Time Search Capabilities

Now the next target we'll talk about, you do want to have it much faster, and we'll talk about why. So DynamoDB is the source, and now we're going to use OpenSearch as the target.  Now this is going to use DynamoDB streams, and so you want to have new and old images turned on your DynamoDB streams and of course another IAM policy to allow the process to access your table. The reason that you want the use case here for using streams and getting that data updated in seconds, I cover travel and hospitality industry. So let's say that my either property management or my car fleet management is in DynamoDB, so I know what is under construction, what's available, et cetera, but I want to enable my customers to be able to search for I want a red Tesla in Vegas the first week of December. That's not a very easy thing to do in DynamoDB when those queries and access patterns change all the time, but OpenSearch can really rock that. And so that's really where, but I also want to make sure that that gets updated in seconds so that once the car comes off maintenance and becomes available, now it's available for that customer to be able to rent. So that's why the data freshness in seconds kind of matters on the OpenSearch side versus on the other two, the 15 minutes should cover most use cases.

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1070.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1070)

All right, so now we have our blueprints in the OpenSearch and we're basically going to pick a blank pipeline here, and then we're going to have a lot of JSON mapping on the next slide. All right, so that 4:15  was a workshop that was on Monday. This is basically a screenshot of that exact workshop. So if when you get back home and you want to potentially have this as one of your use cases, you can set up an immersion day with your AWS account team. We can walk through this, and you get some hands-on experience with setting this up, and so definitely contact your AWS account team to do that.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1120.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1120)

All right, so let's kind of walk through the JSON here without it being too, and again I apologize for the small print, at least the screen's big. So first we're going to do the mapping. So we have our DynamoDB table, we're going to define that, we're going to say what's the new image, we're going to have an S3 bucket that's an intermediate kind of data landing zone from coming from the streams into S3. Then we're going to give that a name of the feature, and then we're going to do the sync side or the OpenSearch target side.  And if you want to do pictures, I'm going to go ahead and build it out.

[![Thumbnail 1130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1130.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1130)

So on the sync side we're going to define what's the OpenSearch cluster that we want this data to go to. What's the  index that we want this data to go to? Now you can have multiple pipelines going into the same cluster to different indexes or to the same index, and then the key thing is this mapping function. And so yes, this is a little bit of work on your part. You're going to have to write the mapping and the JSON here to get this kind of done and make it mapped the way that you want to. But it is, so it's an ETL with a little bit of a caveat. You're going to have to write the mapping piece for it, so but it'll do all the pipelining, moving it, and monitoring it for you.

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1170.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1170)

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1180.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1180)

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1200.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1200)

All right, so that sets it up. So I just talked about index type, fields, mapping, the actions configuration,  you know, start streaming changes on DynamoDB. All right, so once that's created all up and running, now on your OpenSearch cluster you can start to do your product searches  on that same data set, and this is updated in seconds on your OpenSearch cluster. So now you get that enabled searching on pretty much anything. You can also, you know, OpenSearch is really good for natural language processing, semantic searches, things like that. It really opens up a lot of possibilities there. 

All right, one caveat with OpenSearch is on that pipeline there are OpenSearch Compute Units, or OCUs, and each OCU can process 1 megabyte of data per second. This is a typo on my slide, I apologize for that, I didn't notice it, but that's actually equivalent to 10 WCUs on your DynamoDB table. So what you want to do here is you want to match your WCUs to your OCUs so that your pipeline can process your data and keep up with the changes. OCUs do support auto scaling, and you do want to keep that where it can scale up and down. It is serverless, but it'll scale up and down as your throughput to your DynamoDB table comes in. So just something to call out. The integration does support dead letter queues so if there's some type of error or something doesn't get processed, it'll handle all that with CloudWatch.

[![Thumbnail 1260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1260.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1260)

### Aurora Zero-ETL: Storage-Level Integration with Filtering Capabilities

Alright, so that covers DynamoDB as our source. We took a deep dive in that. Now let's switch gears to Aurora, which a lot of you  use, so this should be very applicable to you. On the Aurora zero-ETL, same thing, we want to make it simple, performant, make it easier for you to get that transactional data over to your analytical systems to be able to mine that for the gold that's in there. One of the cool things that the engineering team did with this particular integration is it's at the storage level. So your compute Aurora cluster primary instance, no impact there. It's all done at the storage level. So we're going to automatically handle data seeding and the continuous replication at the storage level between Aurora and then also with Redshift.

[![Thumbnail 1310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1310.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1310)

[![Thumbnail 1330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1330.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1330)

There's also monitoring with CloudWatch in terms of lag, number of rows, performance, and then you also have some tables on the Redshift side if that's your target  that you can monitor the data coming over to it. So let's take a little bit deeper dive on how that works. As long as you're running MySQL 3.5.2 or above, which I think 3.5.2 is now also out of support, so hopefully nobody's running that, and then also Postgres 16.14 or above, you'll get the data. So we're going to use a parallel direct export out of the storage from the Aurora side  over into our Redshift storage. So we'll make that happen so the seed data cuts over. It's like a full move, right, if you use the DMS terminology.

[![Thumbnail 1350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1350.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1350)

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1360.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1360)

Then we're going to do CDC. So under the covers it's going to use the MySQL enhanced bin log to move the data over and then for Postgres it's going to use the PG logical replication.  So we set that up and CDC comes from storage. CDC streams run, and then we replicate it over to Redshift, and this is typically done in seconds. So from a data freshness perspective  it's usually up to date in a matter of seconds. Transaction happens over here, it's over in Redshift in just a matter of seconds so I can start to do analytics on top of that. That's the basic setup of that. And that's really distracting, I'm sorry.

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1380.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1380)

[![Thumbnail 1390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1390.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1390)

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1400.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1400)

Alright, so now let's walk through our data setup again. So  on Aurora, it's a relational database whether it's MySQL or Postgres. It has primary keys, indexes, and it has your columns in there just like your relational database would. Now going to Redshift,  Redshift is also kind of relational, but it is a columnar format of the data. And then you also have that SUPER data type like we talked about earlier. So we have to map that. And LakeHouse similar  just doesn't have the SUPER data type.

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1410.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1410)

Okay, for the Aurora zero-ETL, some of the key differences in it is that it does enable filtering.  Now this filtering is kind of an include-exclude type of filtering capabilities. It's not full blown ETL. So we talked about multiple transaction systems. One customer that I supported for several years, they had 8 different transactional systems that they would bring together into their data warehouse and then normalize all those customer codes, product codes, et cetera. That's not this. This would help you bring all 8 of those over into like a staging area or a raw area inside your data warehouse, and then you do your value-added processing to take all those customer codes, product codes, normalize them into something you have in your data warehouse.

[![Thumbnail 1450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1450.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1450)

But this is fun. You can do includes, excludes,  and this is straight out of the manual just for MySQL examples. So I have a book database, you can bring over just the mysteries if you're a mystery freak, or you can exclude those. If you like Stephen King, you can bring all his books over so you kind of get the idea of what the filtering capabilities are. Now one thing that I'll call out is when we initially released this 2 years ago filtering wasn't there, so feedback from customers like yourselves said, hey, we want to be able to filter this data and then the service team added this in. So if you do some experimentation with zero-ETL and you find like, hey, I really wish it did this, tell your AWS account team and we'll get that feedback to the service team and we'll try to bake those features in. So we're definitely not done in this area, continue to invest and expand. So again filtering logic include-exclude but it's not full blown ETL and that's really the key thing or difference with when you're setting up the zero-ETL with Aurora as the source.

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1510.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1510)

There are some "fix it for me" capabilities  here as well. A word of caution with this one: you can see that this says here "requires reboot." If I'm doing this on my production system, I don't necessarily want this to go rebooted in the middle of the day. This would probably be something that I would want to schedule. So just a little something to consider. The reason that this needs to reboot is, let's say that my Aurora MySQL database doesn't have replication service turned on, or PostgreSQL. So that does require a reboot when you set up the logical replication, and that is something to consider there when you're setting up Aurora as a source.

[![Thumbnail 1550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1550.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1550)

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1560.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1560)

On the Redshift side, there's also a "fix it for me" for case sensitivity. So  maybe when I set up my Redshift cluster, I didn't set it up with case sensitivity. This "fix it for me" will do that. And basically here's kind of, "Hey, do you want me to do that?" You can press continue.  So again, they're not AI agents, cool little bots, but at least it is a "fix it for me." Click it and it'll take care of all that for you. Don't be surprised if in the future Kyra will basically do all this for you at some point.

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1580.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1580)

[![Thumbnail 1590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1590.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1590)

### Expanding Zero-ETL: Oracle RDS, DocumentDB, and Self-Managed Databases

Okay, so now my Zero-ETL  from Aurora to Redshift is done, and here's just the example. It's the same screen that we showed earlier with the sentiment analysis. All right, so that covers Aurora. So now we're going to move  into other RDS engines, and one of the things you'll find is that those are similar to Aurora. So we're basically going to set up the Zero-ETL. We're going to have, in this case it's Oracle, so it's going to be redo archive log instead of the binlog or PostgreSQL logical replication. So it's a mechanism that the database has for replicating changes to us, and there's no storage level integration here. It's going to be EBS or the log miner out from RDS Oracle into our Redshift or our LakeHouse S3.

[![Thumbnail 1630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1630.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1630)

Now Oracle and AWS, so last year at Oracle World,  AWS and Oracle announced that we would support OCI Exadata inside AWS data centers. A reason that I mentioned this, some people may not know what Oracle and AWS is, so I just want to make sure everybody is familiar with that. So this is for customers that have critical business workloads that need the Exadata performance and availability, but they want to run it inside their AWS infrastructure with all their other key pieces. So we do have Zero-ETL from the Oracle at AWS. So you're basically connecting your ODB over to your AWS analytical services like Redshift and the data lake.

[![Thumbnail 1670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1670.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1670)

[![Thumbnail 1690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1690.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1690)

Some of the things that you'll need to do for that,  you'll need to enable Zero-ETL on your ODB network. The Secrets Manager, KMS, IAM policies will all need to be done. And then on your Oracle at AWS, you'll need to enable the redo log to be able to capture those changes, and then you'll need the SSL ASM username and password grants to allow the access to that.  All right, that covers Oracle at AWS, which I've got one customer that's moving into production shortly, but that is a relatively new thing.

DocumentDB to OpenSearch, so this was announced, I think last quarter. Zero-ETL DocumentDB to OpenSearch, very similar to DynamoDB. IAM permissions, you'll need an intermediate S3 bucket, you'll need the pipeline permissions, and then you'll also need to do that JSON pipeline mapping your document or collection to your OpenSearch index. A couple of things to call out for DocumentDB as a source, there are some limitations. If I have multiple collections on my DocumentDB cluster, it's only one DocumentDB collection per pipeline. Now you can have multiple pipelines going against that cluster, but it is a one-to-one relationship between the collection and the pipeline.

Now one thing to call out, the cool cross-region, cross-account features that were in the other ones is not quite there yet for DocumentDB. So if that's something that you need, definitely reach out to your AWS team and let them know, and we'll put that feature request in. Also, the elastic cluster flavor of DocumentDB doesn't support this yet. And then the OpenSearch pipeline setup is very similar to DynamoDB.

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1780.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1780)

Last week we announced  AWS Glue Zero-ETL for self-managed databases. So now if you've got a MySQL, SQL Server, Oracle, PostgreSQL database running on-premises or running on EC2, now you can use that same Zero-ETL magic to bring that data into your AWS analytic services. So I did this presentation, most of it a while back. This got announced last week.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1820.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1820)

But I did want to include and mention it here just so that you're aware of it as well. So to kind of summarize the options, you've got eight new Zero ETL connections.  This includes AWS managed services like Aurora and DynamoDB databases. This includes your databases that run on Amazon EC2, as well as your on-premises databases, as well as third party clouds. So as long as you have the networking set up between AWS and if you have a third party cloud or something running on-premises, you can use the Zero ETL services to keep that data in sync and add it to your analytical systems.

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1860.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1860)

### Monitoring, Management, and the Benefits of Automated Schema Changes

So lots of options again. We really want to make this simple, performant, and easy for customers to be able to get the value out of that transactional data.  So that kind of covers all the permutations in terms of sources and targets. Now let's talk a little bit about monitoring and also some of the benefits of Zero ETL. So I've got my pipeline all running. I'm happy go lucky and then the development team adds a column, tells no one. What happens to my ETL? Well, typically I'm either not going to get that column because I didn't know that it was there when I set up my ETL, or my ETL is probably going to break in the middle of the night the first time they deploy it because I have an extra column and my insert doesn't match my data that I've got in the raw layer. Not fun either way.

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1900.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1900)

[![Thumbnail 1930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1930.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1930)

 Now with Zero ETL, what happens in that case? Well, since we're using the binlog or PostgreSQL logical replication, that DDL or DML change operation of adding or altering the table, adding the column, percolates over across the Zero ETL, and now my Redshift table gets updated with that additional column. I get to sleep in at night. The data is also kept, so if they do kind of a big update that flows over as well and updates all my records. So  there's definitely benefit. You get to go focus on other things and sleep well at night.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1950.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1950)

And again this just kind of shows on the Redshift side what that looks like. So again, glad to do an immersion day and walk you all through this and get some hands on and test this out.  So I did want to just cover that it should help address the fragile complexity as the Zero ETL will take care of that. Is it going to take care of every possible scenario? Maybe not, but it definitely will cover the common use cases of altering a table, adding a column, or adding a new table. All that stuff will be covered in the Zero ETL.

[![Thumbnail 1980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/1980.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=1980)

Now let's talk about the monitoring and management features that are baked into the Zero ETL. First is the status. So you know we're creating our, you know, is one of the statuses that's there so  you can keep track of that, which that may not be that interesting. What might be interesting though is modifying or syncing or needs attention. That probably would be something I'd want to know about. So I would set up a CloudWatch alert that says, hey, if my status of my Zero ETL says needs attention or it failed, I probably want to take a look at that.

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/2000.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=2000)

 Just things, and that does plug in to the normal CloudWatch alerting mechanism and that's there. Now tracking how the data is flowing across Zero ETL, that comes from CloudWatch and our normal metrics. So you've got things like what's the insert, update, delete counts, and you'll see here on this since you've got CDC and you also have the seed data. So it'll tell you how many rows did I move over in my full move and how many rows am I CDCing over the course of the day or the hour, and you can also set up those CloudWatch alerts on that.

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/2060.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=2060)

So if you know that I typically see, you know, a thousand rows per minute on CDC, I can set up an alert that says, hey, if this drops below 500 for more than 10 minutes, I probably need to take a look and see what's happening on that. So all that monitoring and alerting mechanisms that you're used to with CloudWatch can be applied here with Zero ETL as well. OpenSearch, so OpenSearch  pipeline does have some additional monitoring metrics. You can set up a dashboard through CloudWatch. It's also on the metrics tab in the pipeline, and it's going to track for you what's the CPU utilization, what's your OCU utilization, memory. It's going to also tell you how many records it dropped into S3, which is the intermediate piece, and then the record counts and if there's any errors.

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/2090.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=2090)

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/2110.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=2110)

And then on your sync it's going to give you details around how many bytes have transferred, how many documents are written, what's the latency,  et cetera, so you can kind of keep track of all your Zero ETL. And that'll also help with your troubleshooting. So all this goes into the CloudWatch logs and so if you did need to troubleshoot it, you can go mine the CloudWatch logs to get that. So in summary,  you know, AWS Zero ETL integrations, again we're trying to make it simple, easy for you to manage to get that transactional data over to your analytical systems to enable all those cool generative AI and analytical capabilities that we've talked about.

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/ec2a0c3cc4ea129a/2130.jpg)](https://www.youtube.com/watch?v=rwSfnVd1Bgs&t=2130)

So that's pretty much our presentation. So other resources to consider, just some URLs here,  the documentation, and if you've got any other questions I'll be glad to sit up here in front and answer those. So thanks again for coming up to the end for the last session before re:Play. Hope you have a great re:Play experience tonight and thank you for being here and don't forget to fill out your survey. So thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
