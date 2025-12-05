---
title: 'AWS re:Invent 2025 - Redefining Operations: Caterpillar''s Geospatial Intelligence Solution (IND322)'
published: true
description: 'In this video, AWS and Caterpillar present their geospatial intelligence solution built on AWS. Steve Blackwell discusses AWS''s smart products and services framework covering connectivity, analytics, AI/ML, and product-as-a-service models. The Caterpillar team explains Cat Helios, their digital data platform processing data from 1.5 million connected machines globally, built on AWS services including S3, ECS, and DynamoDB with 1.15 petabytes of storage. They demonstrate Cat Grade, an integrated machine control system using GPS for precision earthmoving, and VisionLink, the cloud-based back office solution. The technical deep-dive covers their geospatial architecture using AWS Step Functions, PostGIS, SNS-SQS fanout patterns, and CloudFront for processing elevation data, design file visualization, and surface exports. The solution addresses construction industry inefficiencies where 75% of projects run late and 10% of materials are wasted, enabling real-time jobsite monitoring and proof of work completion for contractors.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Redefining Operations: Caterpillar's Geospatial Intelligence Solution (IND322)**

> In this video, AWS and Caterpillar present their geospatial intelligence solution built on AWS. Steve Blackwell discusses AWS's smart products and services framework covering connectivity, analytics, AI/ML, and product-as-a-service models. The Caterpillar team explains Cat Helios, their digital data platform processing data from 1.5 million connected machines globally, built on AWS services including S3, ECS, and DynamoDB with 1.15 petabytes of storage. They demonstrate Cat Grade, an integrated machine control system using GPS for precision earthmoving, and VisionLink, the cloud-based back office solution. The technical deep-dive covers their geospatial architecture using AWS Step Functions, PostGIS, SNS-SQS fanout patterns, and CloudFront for processing elevation data, design file visualization, and surface exports. The solution addresses construction industry inefficiencies where 75% of projects run late and 10% of materials are wasted, enabling real-time jobsite monitoring and proof of work completion for contractors.

{% youtube https://www.youtube.com/watch?v=WSXDdRFUGiM %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/0.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=0)

### Introduction: Smart Products and Services at AWS

 Good afternoon, or good evening I should say. Thank you for coming to IND 322, Redefining Operations: Caterpillar's Geospatial Intelligence Solution. I'm Steve Blackwell and I lead our Product Engineering and Services team at AWS, which covers the solution areas of product engineering and smart products and services for automotive and manufacturing. Today it's a pleasure, having started my career as an engineer at Caterpillar, to be co-presenting with Ash, Alan, and Sriram, who will come up on stage later talking about their geospatial intelligent solution.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/40.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=40)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/90.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=90)

So what do we have for today?  I'm going to start off and give a brief background and overview of how we talk about smart products and services at AWS and how we help manufacturers build their smart products and services solutions on AWS for their connected products and the capabilities around that. At that point, I'll hand over to the Caterpillar team, who will go through an overview of Caterpillar Inc. themselves and then go into a deep dive around Cat Helios, Caterpillar's digital data platform. They'll talk about their construction customers, the problems they face, and how they take advantage of the capabilities that Cat Digital has been developing. Then we'll talk about Cat Grade, and finally we'll finish off talking about VisionLink. 

So with that, let's start and talk about smart products and services. When we work with manufacturers, smart products and services cover a broad area, and we break this down into three pillars: consumer, commercial, and industrial. Consumer includes anything from a robot vacuum cleaner in your home to a smart plug. Commercial covers anything from a printer in an office through to an HVAC system in an office building. Industrial includes everything from a Caterpillar mining truck right through to a robot on the shop floor. Across these three areas, you really see a common pattern of what manufacturers want to do with their products.

### The Journey from Connected Products to Product-as-a-Service

The first is to have it connected. Once that connectivity is there, it allows the manufacturer to start providing additional capabilities on top of that. The first capability, when you see this as a journey, is around data and analytics. Once that product is connected, they can understand how that product is actually operating and being used out in the field. From there, manufacturers can then say, now that they understand how that product is being used and have some data analytics around use cases like condition-based monitoring, they can start applying AI and ML on top of that dataset to start providing insights. A prime example is around predictive maintenance or condition-based monitoring.

But it's not just around the data. We also see manufacturers looking to say that now their product is connected, it allows them to really focus on the customer experience because it allows them to understand how their customers are using their product. They can start linking the telemetry data, the event data, and usage data of that product to their customers' insights, either through their service management portal, through their sales team, or through their field service teams that are actually out with the customer. They can start developing features and capabilities and improving customers' experience of using their products.

We're also seeing that products are evolving and becoming much more software defined. It's not just around the hardware and the IoT or connectivity platform around that, but really how they can start providing more features and capabilities as software on those actual products themselves. This enables manufacturers to add new capabilities to those products once they're out in the field, based on customer feedback or being able to uplift and up-level that product based on how it's actually being used, as part of new service packs for example.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/290.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=290)

Finally, we're seeing manufacturers say, okay, now I have my product connected and I'm offering all these different capabilities around predictive maintenance or customer 360. Can I actually change my business model? Can I go from selling a product and offering services and maintenance support packages to a product as a service and generating new revenue streams or creating new business models? That's the capability, but we very much see this as a journey that manufacturers go through. Many manufacturers talk about product as a service, but they really have to start at level one that we describe, which is connectivity, understanding those KPIs, and being able to do provisioning and device management. 

Device lifecycle management is particularly important in cases where consumer or commercial products may go through multiple different owners or be in multiple different sites or buildings. Therefore, that whole lifecycle management has to be built into it. We talked about analytics, AI, and services, but this is really where we also see manufacturers expand beyond their IoT platform or their connected platform and start to integrate it into aftermarket parts and servicing.

For example, there is a lot of spare parts management as part of the actual product. How can you use the product's telemetry data to feed into the service team's management of spare parts? When there is a service event, either proactive or reactive, and a maintenance engineer goes out to site, they actually have the parts available in their van or truck to be able to do the work on that product. Finally, with product as a service, these new business models are coming in. Manufacturers have been able to offer SLAs around it and potentially offer capabilities like zero downtime and outcome-driven engagements.

As you go through that journey of one, two, and three, we see increased value being added to the customer of using those products. This is all really enabled by what we describe as telemetry-driven X, where X could be parts management or service management, for example. Really, it is taking the telemetry from the product and actually being able to use it across the extended value chain of a manufacturer's business.

### Conceptual Architecture and AI Integration in Smart Products

From a conceptual architecture point of view for smart products, on the left-hand side, we have the connected product, and that is going to need secure data ingestion of the data from the product into the AWS Cloud. In most cases, this is done via IoT streaming of the telemetry of the events. In some cases, depending on where the product is, if it is out in a remote location where connectivity may not be as reliable as in a city, there may be bulk uploads of that data directly into an S3 bucket or using APIs based on the product's condition.

Within the AWS Cloud, that data comes into a data lake, which is generally built on Amazon S3. On top of that data lake, we start to build microservices type capabilities. Fleet management is one key capability we see manufacturers build as part of smart products. Having remote control and remote management is a key one. Telemetry and event management is generally where most manufacturers start, as it is their main use case to drive and build out this platform. Then on top of that, they have analytics and AI insights, and finally, agents come into this, especially when you start to talk about maintenance.

When identifying an event or anomaly from the product, you bring that into the telemetry management part. The analytics and AI insights determine whether that is potentially a part failure or a service event. Those agents really come in to orchestrate creating the service management ticket for potentially an engineer or a customer support center to actually contact the customer to tell them that we would like to come out and service your appliance, vehicle, truck, or whatever that product may be.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/420.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=420)

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/600.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=600)

On the right-hand side, there are going to be multiple personas interacting with this platform. There is a mobile application which the service engineers or the end users are going to be using. There is probably going to be some business intelligence capability built on top of it to understand how the products are performing and typical usage of those products from a features and capabilities point of view. There is obviously going to be enterprise application integration, the CRM system, the MRO system, for example.  I just want to touch on one thing.  I talked about agents as part of that platform, but really we are starting to see manufacturers look at how they've got their products connected and how they can actually bring AI in to enhance that product and increase the customer's experience.

We see a couple of areas where AI is really helping in products. First is around product development, using AI in machine-based simulation to help with physics-based simulations or designing the product, either the hardware through finite element analysis or CFD, or in the software development, helping developers test, validate, and integrate their actual software.

We also see AI assistance coming into products and really helping differentiate and innovate on that actual product and how it's being used, starting to provide a more personalized experience with dynamic UIs based on how that person is using that actual product itself. That's a brief overview of how we see smart products and services, the types of use cases, and how AI is going to be used more and more in smart products and services. With that, I'll hand over to Ash.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/720.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=720)

### Caterpillar's Global Footprint and Connected Machine Fleet

Good evening. My name is Ash Shah. I have responsibilities for technology strategy and emerging technology engineering in our space. Primarily my team is responsible for determining what we need to deliver to meet customer needs. Before we get into the geospatial solution that you're all here to hear about, I want to give a little bit of background about Caterpillar. 

Most of you have probably seen Cat equipment as you've been driving on the highway or have gone past job sites. What you may not realize is that Caterpillar has a global footprint. We are a multinational company and a global leader in the equipment that is being built, which is necessary for building the infrastructure that powers our world. Our workforce is over 112,000 strong and exists across 24 different countries in approximately 140 different locations.

Combined with our extensive dealer network of 152 dealers operating in 190 countries, this allows us to bring together a broad range of expertise in construction, mining, engines, turbines, and locomotives. We also have millions of machines operating around the world on almost every continent. Of those machines, 1.5 million are connected, so from excavators to generators, they are all streaming data to our back office, and we then process that data.

How does Caterpillar, a company with this kind of breadth, harness that data, analyze it, and process it well? That is the beginning of our Cat Helios data platform journey, and for that I'm going to hand it over to Alan to go into more details on that.

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/830.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=830)

### Building Cat Helios: A Unified Data Platform at Scale

Thanks, Ash. I'm Alan Doty. As they introduced me earlier, I'm the Senior Director of Architecture for Cat Digital. Digital is obviously a big buzzword. What we mean by Digital at Caterpillar is our customer and dealer solutions that enable them to be very successful. 

We're going to talk about why we built Helios and we'll talk a little bit about what we built, how we built it, and then because I talk about the scale, I'm going to hand it off back to these guys to talk about one of the solutions built on top of it. Why we built it probably started around 2018 or so when we had a potpourri of technologies that were deployed across all our digital solutions. We had Teradata, we had Cognos, we had Hadoop, we had Azure. We had everything everywhere all at once.

We heard back from our customers and dealers that they were confused because all these different solutions gave a different answer to what their fleet was, what their customer data was. We didn't really have very good control of it. We also had another failed experiment with an outside company that we usually try to go to in order to solve some of our problems. So our Chief Digital Officer got involved and said we're going to bring in somebody that's going to help solve this for us and actually bring in digital talent.

I brought in our Senior Vice President, Ogi Redzic, and he brought in a huge amount of talent. We set off to build this Helios platform. We wanted to be confident in the data. We wanted all of our digital solutions to use it. We wanted to be secure. We wanted to get all the market faster on those solutions, and we also want to improve our customer experience.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/910.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=910)

Let me explain a bit about how we built this from a logical architecture perspective.  On the left-hand side, at point A, we have all the data we need to bring in from different sources. Enterprise sources like what we built and what we manufactured are relatively straightforward, but once we got to our dealers and customers, it became very complex. We have 160 different dealers, each running their own businesses with their own systems, so bringing in their service information about what they actually did to equipment was very challenging. Additionally, as Ash mentioned, we have 1.5 million connected assets out there, so we had to bring all of those in. Some of these assets and devices were probably made when I graduated college, which was a long time ago.

Once we brought all that data in, we had to establish a single point of truth. What is an asset? What is a customer? What is a dealer? Since all these different pieces of information came from different sources, we had to consolidate them. Then we had to build services on top of the data. We didn't just want to create a data platform and make a data lake. We actually wanted to build APIs and services on top of it so that applications could use them without replicating the data. We also wanted to ensure we had data analytics capabilities. We knew the power of the data wasn't just making it available but actually putting insights on top of it, so we built large data analytics pipelines, execution, and training environments that could run and scale. That was always our problem before—all the vendors couldn't scale to our problem size.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1020.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1020)

We also wanted to make sure it was secure by design, and of course we wanted to get to market faster while controlling our costs.  From a functional perspective, the first thing we had to get control of was data. We had to establish a single source of truth for customers, dealers, service, parts, and all those different types. We have created and mastered over 325 data objects. These services then drive data on top of those objects, so we made sure we had drive services that could build APIs, events, and notifications. We built over 340 of those so that I had one single source API that would give me what my fleet list is across all my commerce apps, my customer equipment apps, and everything else.

For analytics, we deployed over 23,000 models, including high-frequency inference models and on-demand real-time models. Obviously, we also had to have our ML Ops training as well. For onboarding, we heard from our customers that onboarding a dealer or customer is very difficult. It's not like the auto manufacturing industry where we have a VIN number and you go to the state to get your license. We don't have that. Someone could just say they own a tractor, so we have to do much more work. It's very hard for us, so we've had to make a lot of investment in that area. However, we've got 1.7 million customers on the platform, so by the numbers we're talking about scale.

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1110.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1110)

Obviously, we wanted to make sure we were highly available, and that's one of the reasons why we chose AWS over some of the other cloud providers we had considered. We have 296 million pipelines running per month, 1.4 million API calls, and 50 billion data records per month as well.  This is all built on AWS services for the most part. Just as an example, we have 680 S3 buckets, 21,000 million there, 5.7 ECS, DynamoDB, and gateways. Of course, there are tons of other services I didn't list, but those are just the main scale. I want to show you that it wasn't just a single database that we spun up. It's a very complex environment that we had to build because our previous people who tried to solve this before said they would just spin up one ElastiCache cluster and one Elasticsearch cluster and be good to go. We said no, that's not going to work for the domain and size of data that we have.

[![Thumbnail 1190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1190.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1190)

### Cat Grade and VisionLink: Solving Construction Inefficiencies

We have 1.15 petabytes of storage, 265 million lines of code, and 3,000 pipelines. One of the happiest things we have is 125 applications on board, both from a data and BI perspective as well as a digital solutions perspective, including web applications and other things. That's why I'll turn it over back to Ash and Sriram to cover one of the solutions that they built on top of the platform. Thank you. All right, so we have Cat Helios as our foundation. Now we want to start thinking about how we can help our customers solve some of their toughest challenges. I'm not going to read everything on this slide for you,  but our customers have a ton of inefficiencies both in the construction and in the mining space. As you can see from the data on the slide, there is a ton of opportunity where we can remove inefficiencies and remove waste.

We want to use the Helios platform to get the data and analytics to understand how we can take what we have and help our customers solve some of these problems. Before we get into the geospatial solution, let's talk a little bit about this. Construction customers are wasting 10% of their materials, a third of work is redone, and eventually what that all leads to is that nearly three quarters of projects are late. Anyone who has built a house or gone through that process has probably felt that pain and seen the inefficiencies that exist in the construction space.

[![Thumbnail 1280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1280.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1280)

Now think about that on a much broader scale with some of our big infrastructure projects. How many of you have driven down the highway and just seen barrels forever, wondering how long it takes to update a road? It feels like it should be much faster. Some of this is a result of inefficiencies in the process. What we wanted to do is build a solution that can try to address some of these issues. One of our solutions is Cat® Grade. 

Cat® Grade is Caterpillar's integrated machine control system available in a host of different machines such as dozers, excavators, motor graders, and pavers. Think of it like a smart assistant for heavy equipment. It helps the operator do cut, fill, and pass work with precision. If you have modern cars with lane assist or auto braking, this is the same concept applied to Cat® Grade. At Grade uses GPS sensors and onboard computers to continuously track the position of the blade or the bucket. It provides near real-time location of the digital design loaded into the system so you know how you're operating based on what you need to do.

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1360.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1360)

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1370.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1370)

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1380.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1380)

[![Thumbnail 1390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1390.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1390)

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1400.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1400)

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1410.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1410)

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1420.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1420)

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1430.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1430)

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1440.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1440)

[![Thumbnail 1450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1450.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1450)

[![Thumbnail 1460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1460.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1460)

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1470.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1470)

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1480.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1480)

[![Thumbnail 1490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1490.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1490)

For example, if you need to slope it by 3%, the machine can measure the ground terrain and tell you whether you're above or below, then adjust the blade as needed. In the cab, the operator gets this feedback, allowing them to make adjustments, and in fact the system will do some of that automatically. Every time the machine goes over a piece of terrain, that data is collected and sent to the back office where it can be processed. I explained that quite a bit, so this video will give you a quick two minute overview of Cat® Grade in action, which should make it clearer.              

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1500.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1500)

Hopefully that made a little bit more sense about what Cat® Grade is. A lot of that focus was on what we call onboard systems, right, helping the operator actually use the equipment. As was alluded to and you may have seen in the video, there was discussion about VisionLink® PRODUCTIVITY. 

It is now under the VisionLink® umbrella, but what VisionLink® does is serve as the back office web component that combines with the onboard data that we are collecting to really make the solution viable for our customers. It allows the back office to help the operator and ensure that they are working on the right information. One of the problems that we have seen with customers is that they work on old designs. Someone made an update, but that update did not get to the correct operator. The operator is now working and doing the wrong thing. This system with VisionLink® in the background allows us to ensure that all the machines have the latest designs and are able to execute them properly.

What CAT® Grade gives us is the ability to correctly measure our metrics and volumes, and make sure that the right amount of dirt has been moved. We can ensure that our surfaces are properly constructed, speeding up the job process, cutting down on waste, and cutting down on cost. Having the machines at the optimal blade settings and doing the right amount of work improves our machine performance so that the ground is optimized and the machine is doing the best job that it can. Because of all of that, because there is less waste, there is less damage to the environment. We have an environmental impact where the construction work has the minimal footprint possible for sustainability.

### Geospatial Intelligence Solution: Technical Architecture and Implementation

Now, what we need and what we are getting to the heart of our geospatial solution is that we have all of this data coming off the machine that now needs to be processed by our back office. The solution that we built on top of Cat® Helios is our geospatial solution. I will hand it over to Sriram, who is our solution architect, to provide the technical details.

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1620.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1620)



Caterpillar is a stronger company the stronger our digital solutions get. This was Ogi Redzic, our Chief Digital Officer, in a podcast. I am Sriram, Solution Architect with Caterpillar for the last ten years. We look at how Caterpillar, being a huge manufacturing company, has been building good digital solutions for our customers to use. Let me ask you this.

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1650.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1650)



What is the single important factor that is needed for a business to succeed? Cost efficiency, right? Anything else? Those are right answers. So it is not the revenue. It is not the market share. Yes, but it is not quality either. Not even innovation, right? But it is customer satisfaction, right? That is the most important thing. At Caterpillar, the customer is at the heart of everything that we build. So we look at how VisionLink® powered by Cat® Helios is helping our customers succeed every day.

[![Thumbnail 1720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1720.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1720)



Let us do something interesting. Can I ask all of you to close your eyes for a moment? Think of you being in a busy airport. Flights are taking off, landing, moving in every direction. There is something that is controlling everything and keeping everything safe and coordinated, right? What is that one thing? Any guesses? I think someone said the airport control tower, right? That is the one thing that is actually keeping everything safe and coordinated in an airport. Similarly, in a job site, whether it is a construction site or a mining site, it is VisionLink® that is keeping everything safe and coordinated, making sure all our machines are actually working smoothly and safely. VisionLink® is Caterpillar's cloud-based solution that provides real-time visibility to what is happening in a job site. You can track fuel levels and utilization of every machine. You can schedule maintenance from the application. You can actually track where your asset is and what it is actually doing.

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1830.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1830)

All of those things are possible with VisionLink®. With VisionLink®, it's all about making the most out of the data that you're getting back to the back office. We looked at the top three use cases that our customers are using in conjunction with the in-cab solution that Ash was talking about, which we have in our digital solutions that are helping our customers. 

We are sitting in this beautiful auditorium, and the surface that we're sitting on is flat. But if you look at a jobsite, whether it's a mining or construction site, you're going to have elevation changes. What we're doing is our in-cab solutions actually pick up the elevation as the machine is moving on the jobsite, and that data is transferred to the back office. We can then have an elevation profile that can be shown in the application so that customers can understand what the elevation profile of the jobsite is that the machine is working on. That's one thing.

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1920.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1920)

Our machines actually move the dirt from crest to trough and then move the dirt from troughs to crests as well. We call it cut and fill, and the back office solution can calculate the cut and fill volumes. These two are critical parameters in our application where you're able to understand the lay of the land, know how the surface is at a jobsite, and calculate the amount of dirt that is getting moved by our machines. 

[![Thumbnail 1930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/1930.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=1930)

This is the interesting stuff.  As you can see, our machines are sending the data to Helios, which is actually an upstream system for our service that is completely masking everything that is happening to the left of us. It's just an upstream system, a black box to us, and we're getting the data from Helios piped into us. Once we get it, we're actually using a step function to parse the data. We have proprietary data formats that are coming in from the field, and we're parsing the data using AWS Step Functions because we have multiple steps of parsing that need to happen.

Once we do that, we store it in Postgres with a PostGIS extension. We use that for storing our surface data. Apart from storing the data, we use the SNS-SQS fanout pattern as well to do parallel processing. We use it for calculating the volumes, and we also calculate elevation tiles and then push them to Amazon S3. While we do that, it's being served from CloudFront to the end user. We're actually invalidating the tiles as we get the surface data from the machines. For example, you have one-foot by one-foot data that is coming into the back office, and while we calculate if the elevation has changed, we would need to invalidate the tile and make sure that we're showing the right tile at the right location.

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/2100.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=2100)

The bottom portion is straightforward. You have the user talking through the API through the ELB and the Fargate task. From the browser, the user is able to download the tiles from CloudFront. Pretty straightforward stuff. Now that we know the lay of the land, we know that the surface has elevation changes and you're able to see the elevation profile, we move to the to-be state surface. For example, typically you will have a surface that has crests and troughs, and then there will be a surveyor who will survey and find out how the to-be state surface needs to be. Let's say, for example, you're constructing an airport. You will need a flat surface at a specific elevation. That data is actually created by the surveyor and then created as a design file, and that will be sent to the back office. From the back office to the machine, before that happens,  through Helios again from the application, you can upload the design file.

Then you will be able to send the file to the machines working at the job site. Before we send this to the job site, we are able to see a preview of the file. Let me explain the workflow: I could be a site manager working in Vegas, probably working remotely, while an operator is actually operating the machine in Peoria. The file transfer happens remotely between us. The surveyors use design tools to create these files, but we did not have a way for anyone to visualize them without those specialized software tools. What we have brought in is a visualization capability where you can look at a design file in 3D format before it is actually sent to the machines.

This is a really critical feature. We have had this feature in production for about 12 months, and prior to that we had a lot of customer feedback where operators were actually getting wrong design files because the ecosystem was broken. We had to fill in that gap and make sure we are sending the right design file to the machines. With this feature, you will be able to rotate it into 3D and then verify that you have the right design file that is actually being sent across. This capability is available both on the application and on the ICAP solution that we have, so you can make sure that this is the right design file before it is actually sent to the machine.

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/2250.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=2250)

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/2260.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=2260)

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/2270.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=2270)

As I mentioned, sending to the machine happens through Cat Helios. We use Helios to send the files, and there were issues that we worked with the AWS team to resolve. We had to work around problems that IoT Core has because these files are huge and IoT Core cannot support these big files. We were able to circumvent those situations and ensure these files are actually transferred to the machines.  This is a microservice that we have for processing the 3D preview feature.  The user uploads a design file  into an S3 bucket using a pre-signed URL, and then it kicks off an SNS-SQS pattern into parallel processing. One path does heavy lifting, processing the design file completely and storing the data into the PostGIS database. The second path is a lightweight solution where we are doing preview generation, so the preview of the file and the actual processing are separated out.

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/2330.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=2330)

One is heavy lifting and the other is a lightweight implementation that we have. The preview is served from CloudFront, so we generate the file and it is served from CloudFront, and then from the UI it is actually pulled from CloudFront. We do have just-in-time target tasks that are actually getting created for processing the files.  Now that we have seen the current state and the to-be state design, and the operator has got it and worked on it, what is the proof of completion of the work? In certain geographies like Europe and Japan, you do have mandates from government agencies for submission of proof of completion of work so that contractors are getting paid for their work. Our customers, who are the contractors, would be needing this for generating revenue for their company.

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/2400.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=2400)

After they complete the work, you will be able to generate an export of the surface and it is sent to the agencies. We do have multiple formats of files that we can generate from the surfaces, and we will be able to allow the users to download them so that it can be submitted as proof of completion of work. This is the surface export. 

This process takes a considerable amount of time because we need to parse a huge amount of data from the PostGIS database, parse it, and create the file format that the user is requesting. For this reason, we implemented an asynchronous process. When a user creates an APA request, it kicks off an asynchronous process where we generate the file using a Fargate task. The file is then stored in an S3 bucket, and we use a Lambda function to trigger an email notification to the user.

Because this is an asynchronous process, the file generation happens behind the scenes after the user creates the APA request and receives an email. Depending on the surface area being selected, the process could take anywhere between 5 to 10 minutes. For example, if you're processing 10 square miles of quarry or mining data from a surface standpoint, the duration depends on how long the surface has been stored. Once the file is generated and stored in S3, the Lambda function sends a pre-signed URL as part of the email, allowing users to download the file.

We are also leveraging the email complaint delivery tracking capabilities of Amazon SES. Previously, we did not have these capabilities and struggled to capture metrics like how many users were actually downloading the files. Now that we have these capabilities in SES, we are able to track and manage those metrics effectively. I will now hand it over to Steve for concluding the session. Thank you.

Hopefully, that provided some good insights into how Caterpillar and the Cat Digital team are building capabilities on top of AWS for our customers around our products. I really appreciate them coming here and sharing their architectures, best practices, and how they have gone through this journey. This is just one example of a customer doing this on AWS. Other examples include Eunuchs, an Italian manufacturer of kitchen equipment, Heisenberg, a German manufacturer of printing presses, and HP with their industrial print presses. We have multiple examples of how customers are building their connected product platforms on AWS and not just providing insights into how those products are being used, but also building services on top of that, as you saw with Cat Helios and VisionLink, to provide better experiences and better usage of those products to their customers.

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70a2bbc184bcebcc/2590.jpg)](https://www.youtube.com/watch?v=WSXDdRFUGiM&t=2590)



With that, I want to say thank you. I really appreciate you staying with us.


----

; This article is entirely auto-generated using Amazon Bedrock.
