---
title: 'AWS re:Invent 2025 - Hyundai Motor Group: Transforming business w/ SAP & Pan-Amazon Services(MAM212)'
published: true
description: 'In this video, Beth Sharp from AWS and Sunwoo Kim from Hyundai AutoEver share Hyundai Motor Group''s digital transformation journey, consolidating over 30 separate SAP ERP systems into five regional instances on AWS. Kim details their strategic approach addressing operational complexity, preparing for SAP ECC end-of-service in 2027, and implementing cross-continental disaster recovery architecture. The session highlights innovative solutions including an 85% cost reduction in document management using Amazon S3, integrated monitoring with Amazon QuickSite, and AI-powered development using Q Developer. Sharp demonstrates how Pan-Amazon services like Buy with Prime, Multi-channel Fulfillment, and Amazon Business integrate with SAP to transform supply chain operations, leveraging Amazon''s global fulfillment network that delivers 10 billion packages annually.'
tags: ''
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Hyundai Motor Group: Transforming business w/ SAP & Pan-Amazon Services(MAM212)**

> In this video, Beth Sharp from AWS and Sunwoo Kim from Hyundai AutoEver share Hyundai Motor Group's digital transformation journey, consolidating over 30 separate SAP ERP systems into five regional instances on AWS. Kim details their strategic approach addressing operational complexity, preparing for SAP ECC end-of-service in 2027, and implementing cross-continental disaster recovery architecture. The session highlights innovative solutions including an 85% cost reduction in document management using Amazon S3, integrated monitoring with Amazon QuickSite, and AI-powered development using Q Developer. Sharp demonstrates how Pan-Amazon services like Buy with Prime, Multi-channel Fulfillment, and Amazon Business integrate with SAP to transform supply chain operations, leveraging Amazon's global fulfillment network that delivers 10 billion packages annually.

{% youtube https://www.youtube.com/watch?v=gmwoJ_Ly8x0 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
### Introduction: Transforming Business with SAP and Pan-Amazon Services

Thank you everyone for joining us this afternoon in our session. I hope you had a great lunch and you're fully energized for our session today. We're going to have a great session where we'll talk about how Hyundai Motor Group is transforming their business with SAP and Pan-Amazon Services. I want to give a special shout out and thank you to Sunwoo Kim from Hyundai Motor Group who is sharing their transformation story with us here today.

To get started, I want to share some insights that we're hearing from our customers and a shift that we're observing. They're no longer asking us how they can move their SAP systems to the cloud, but what we're hearing more often is how can you transform my business? That's what we're going to share with you today—how we can transform your business with AWS and our Pan-Amazon services.

My name is Beth Sharp, and I lead our global solution architect team that works with our global SI partners here at AWS. I've been here about three and a half years, and like many of you in the audience, prior to coming to AWS, I was actually an SAP customer myself for twenty-five years. Sunwoo Kim is going to kick us off and start with his journey that he's been going through and share some insights with us. Then I'm going to close to show you how we're helping other customers with their transformation journey and how we can be your partner for transformation here at AWS. I'm going to turn it over to Sunwoo Kim for his introduction and to get us started.

[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/0.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=0)

### Hyundai Motor Group's Vision: From Automotive Manufacturing to Future Mobility Leadership



Good afternoon, everyone. I'm Sunwoo Kim, Vice President and Head of Solution Business Division at Hyundai AutoEver. It's an honor to be here at AWS re:Invent, the place where innovation becomes reality. Today, I'm very excited to share how Hyundai Motor Group is transforming its business with global ERP systems designed for agility, scalability, and the future of mobility.

I would like to share our journey over the last four years in transforming Hyundai Motor Group's ERP system, leveraging SAP on AWS. Our story demonstrates how one of the world's largest automotive manufacturers is driving digital transformation at a global scale.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/140.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=140)



[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/150.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=150)



Hyundai AutoEver is the IT service and solution provider for Hyundai Motor Group, including Hyundai Motor Company and Kia. We deliver integrated IT solutions that support the entire automotive value chain. Right now, we are driving our group's digital transformation by moving to the next generation systems that improve efficiency, enable innovation, and enhance the customer experience. My role is to lead the enterprise solution initiatives that support and build a platform for agility and sustainable growth.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/200.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=200)

Let me walk you through today's agenda.  We will start with a brief introduction, followed by an overview of Hyundai Motor Group's business scope. Then I will share our SAP ERP landscape and the challenges we faced, which led us to this journey. We will discuss our transformation strategy and why we chose AWS as our cloud service provider. Finally, I will share our experience working with AWS and our future plans.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/240.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=240)



Hyundai Motor Group is one of the world's top three automotive manufacturers and mobility service providers. In 2024, we sold 7.53 million vehicles worldwide. We operate in more than 200 countries and have over 15 global production bases. This scale enables us to serve our customers everywhere and respond quickly to market changes. Hyundai Motor Group is more than just a car company. We are transforming to become a future mobility leader. Our focus goes beyond traditional vehicles to areas such as robotics, electrification, and advanced air mobility.

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/360.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=360)

Robotics is helping us innovate in manufacturing and logistics, making operations smarter and more efficient. At the same time, a wide range of electric vehicles is preparing us for electrification and sustainable transportation. We are also investing heavily in software-defined vehicle platforms and building an ecosystem for advanced air mobility, which is creating new methods for people and products to move. This approach shows how Hyundai Motor Group is striving to lead the future of mobility and deliver smarter, safer, and more sustainable solutions for our customers around the world. 

### Addressing Critical Challenges: Hyundai's Strategic ERP Transformation and AWS Selection

Let me start by explaining where we began. As Hyundai Motor Group grew globally, we rolled out our ERP system using traditional methods. Over time, this led to more than 30 separate SAP ERP systems around the world. This worked in the past, but it is creating big challenges for our future. These challenges made it clear that incremental improvement would not be sufficient. Therefore, we need fundamental transformation.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/410.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=410)

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/470.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=470)

Let me explain in more detail what challenges we have faced.  Over the past few years, Hyundai Motor Group has been looking for a better way to manage global operations. From 2020 to early 2022, we conducted a company-wide process innovation program to make our processes more efficient and standardized across headquarters and overseas subsidiaries. There are three key major challenges we need to address. First, operational complexity and inefficiency. We have multiple ERP systems running in different regions and different programs, which increases complexity. Infrastructure and license costs have risen sharply to support global operations. 

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/500.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=500)

Second, preparing for our future business needs. Most of our ERP systems still run on ECC, which is going to reach end of service in 2027. Business requirements are growing rapidly, including massive data handling and AI-driven innovation and automation. Third, our current architecture  cannot ensure business continuity or disaster recovery at scale, which is the most critical for global operations. Our current ERP systems were built separately in each country, which makes it very hard to support global processes and keep up with fast changes in sales and manufacturing. We also need a system that can quickly adapt to new business models like mobility services.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/560.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=560)

These challenges made it very clear that we need a unified, future-ready ERP platform so that we can deliver agility, scalability, and resilience for Hyundai Motor Group's global operations. To solve the challenges of our current  ERP systems and prepare for the future, Hyundai Motor Group is working on five key strategic initiatives. These strategies focus on process standardization, master data integration, improving infrastructure, and building a global operations model. First, we reviewed the current processes and ERP functions across headquarters and overseas subsidiaries. Based on the results of our process innovation program, we created new standard processes and a global template. These templates help all entities work in the same way, improving consistency and efficiency. We also define business process rules and policies that everyone must follow, and we converted them into digital assets for easy use.

Second, we developed common global functions and added local features for each subsidiary. These are now being rolled out step by step across headquarters and all entities.

In the past, all subsidiaries had their own master data and managed it themselves, which created differences and delays in global reporting. Now, with an integrated master data system, all entities use the same standard codes. This makes data collection and analysis much faster and more accurate. We unified our previous separate SAP systems into one environment on AWS. This gives us flexibility to scale as our ERP system expands, plus strong security, compliance, and resilience.

Before, we had fragmented individual SAP systems, which increased the risk of outages. Now we have high availability and disaster recovery, so we have achieved a top level of stability and reliability. Finally, we are building an integrated global operation model. This will allow us to run the ERP system in a more efficient way and quickly respond to business needs.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/750.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=750)

One of our most important decisions  in our strategy was how much to consolidate our systems. We looked at two options: one is a global single instance, and another is separate instances for each region. After careful analysis of these options, we chose a five-regional approach: Korea, America, Europe, APAC, and China. Why did we make this choice? There are three main reasons.

First, we need system stability. A single global instance sounds efficient, but if something goes wrong, the impact is huge. A regional approach reduces that risk. Second, every country has its own rules, especially about data privacy and where data must be stored. We have to follow these rules. Third, there are technology limitations. At the time, SAP HANA could support only up to 24 terabytes plus, and network latency from users in different countries would have been a problem.

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/870.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=870)

So we created a detailed plan based on our strategic decision. This allows us to move step by step while keeping business running and avoiding any disruption. I think this approach has been a big part of our success. Today, I want to share one of our most important  architectural innovations: high availability combined with disaster recovery across continents. What does this mean? Simply, our system keeps running even if an entire AWS region goes down.

In each region, we use multiple availability zones. We keep an active database and a standby database that are synchronized within 50 kilometers.

This setup allows us to switch immediately if one zone fails. But we didn't stop there. We also established disaster recovery in another continent using synchronous replication. So even in extreme situations like natural disasters or political issues, we can keep our system and business running. For example, our Korea headquarters system runs in the Seoul region with the standby also in Seoul, and disaster recovery in the US. I know this design is a big investment, but for a global company operating 24/7, it is essential. It gives us confidence that our ERP system can survive almost any disaster. Honestly, with traditional on-premises systems, reaching this level of resilience would have caused a lot of effort or even been impossible.

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1010.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1010)

We considered five key factors when choosing our cloud service provider. First, support for SAP. Second, global region availability. All providers meet these basic requirements. The third factor was  high availability architecture. Here we saw some differences. Only two providers, including AWS, could support both zone-level high availability and cross-continental disaster recovery. This was a very important decision point. Next, we also looked at BTP service availability. AWS was the only provider who could deliver a wide range of BTP services available where we needed them in the right regions. Finally, we also checked existing partnership and operational experience. We have already deployed over 100 major systems on AWS, and our team has strong experience with AWS services and support. AWS also guarantees fast response and tight collaboration with the SAP ecosystem team if any issues related to SAP occur. For this reason, we selected AWS as our cloud service provider.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1100.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1100)

### Implementation Success: AWS Partnership, Cloud-Based Solutions, and AI-Driven Innovation

Our journey with Hyundai AutoEver on AWS  has truly gone beyond typical ERP implementation. In the early stage, we had many questions about network design between our existing ERP data center and the new Hyundai AutoEver AWS environment. We needed the best architectural considerations for a complex network environment. AWS brought network and SAP experts and provided guidance and support to help our platform team build the right skill set. During the build and loading phase, we saw far fewer system issues compared to before. Of course, issues can occur at any time, but I think the most important thing is how we respond and how we restore our system to normal. AWS worked closely with SAP for fast response and root cause analysis.

They also supported proper sizing, delivered instances on time, and improved network performance for our critical business processes. Beyond that, AWS introduced services like Amazon S3, QuickSuite, and AI, guiding us toward cost-effective digital transformation. The results of these efforts will be shared on the next slide.

[![Thumbnail 1210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1210.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1210)

Many companies use ERP systems  to manage their business operations. These systems store a huge number of documents. In a cloud environment, the more documents you store, the higher the cost for storage and management becomes. Keeping documents for a long time is not an option due to tax compliance and audit requirements. To solve these challenges, we built a cloud-based document management solution that works seamlessly with SAP ERP and also connects to non-SAP systems.

[![Thumbnail 1270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1270.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1270)

Our solution provides secure encrypted storage and easy version control. It reduces system operation costs and makes document search faster.  For example, if there is a document that needs to be stored for 10 years, we cut the cost by 85 percent. By moving from the traditional directory structure to a key-value approach, search speed improved by more than 2 times. Security is our top priority, so we use a private link between VPCs to ensure safe document transfer.

[![Thumbnail 1310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1310.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1310)

[![Thumbnail 1330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1330.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1330)

In the future, we plan to use  AWS AI services to add more smart features, such as intelligent search and automated summarization. This will make our document management solution smarter, faster, and more cost efficient.  ERP systems are the backbone of global business operations. They handle massive transaction data and connect countless interfaces across organizations. In a cloud environment, managing such a critical system becomes even more challenging. Traditional monitoring methods are no longer enough. They often fail to detect issues early, making it hard to prevent system failures or business risks.

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1380.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1380)

To solve this issue, we developed an integrated ERP monitoring solution powered by Amazon QuickSuite.  The solution provides real-time monitoring of ERP systems and uses predictive analysis to identify risks before they impact our operations. It is not just about system performance that we monitor. We also monitor business data so we can manage technical issues and business risks together. We also plan to leverage AWS AI services to make risk detection more proactive and accurate. This collaboration is helping us build a smarter, more resilient ERP environment.

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1430.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1430)

 Our partnership with AWS spans almost 10 years. We have used many solutions on AWS. Recently, we have accelerated our move toward agile business transformation. In 2023, Hyundai Motor Group signed a strategic agreement with Amazon covering initiatives like selling vehicles on Amazon.com. In 2024, we ran a major cloud enablement program with AWS, and Hyundai AutoEver became an AWS Premier Partner. Today, through this collaboration, cars are sold on Amazon.com, and Hyundai will supply low-carbon materials for Amazon data centers in APEC. This is not just a relationship with a vendor. It is truly a strategic partnership across cloud, business transformation, and beyond.

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1510.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1510)

 We can highlight three ways we will reimagine the automotive business with Amazon. First, expanded reach through the big crusade on Amazon.com, which creates an entirely new customer channel and revenue stream. Second, enhanced experience—embedding Alexa into next generation vehicles will drive a smarter, more connected driving experience. Third, modernized cooperation—transforming our SAP system with Amazon Web Services provides agility and efficiency. This is a true digital transformation, not just moving to the cloud, but reimagining the entire business model and the customer experience.

[![Thumbnail 1580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1580.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1580)

During our global ERP project, we experienced many technology changes.  One of the biggest challenges was the rise of AI. Our project is not just about developing and building a global standard. We faced challenges to develop a lot of programs to meet our local specific requirements. Another challenge was converting old legacy systems to ERP. Some applications used outdated development platforms, so we had to analyze the code and rebuild it for ERP. This was not an easy journey.

That is why from the early days of AI, we worked with AWS to try different ways to improve development productivity and quality. One example is using Q Developer and Kiro for code generation and quality checks. We have tested and seen great results. We plan to apply this technology to our future projects to improve both productivity and quality. I want to express my sincere appreciation to AWS for their true partnership. Together, we have created a foundation that will drive tomorrow's innovations across our entire mobility ecosystem. Thank you for listening. I hand over to Beth.

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1710.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1710)

[![Thumbnail 1720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1720.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1720)

### AWS as a Transformation Partner: Beyond Cloud Migration to Business Reimagination

Thank you, Mr. Kim, for that truly inspiring story and sharing with us Hyundai's transformation with SAP and AWS.  I want to show you some other ways that we are helping our customers transform and becoming that transformation partner for them for their business processes as well.  As customers like Hyundai embark on their journey, they are looking for something a little bit more meaningful in their relationship than that traditional IT customer relationship. For many years, as I mentioned earlier, customers would ask, "How can you help me run my SAP systems on the cloud with better security, resiliency, performance, and agility?"

But as customers start to move to AWS for their SAP systems, we have seen a shift to customers asking, "How can you help me transform my business operations and better serve my customers?" Customers do not just want another cloud service provider. They want that transformation partner. They want someone who can help them reimagine their business processes end to end in their operations, and they are letting those needs and those of their customers and their business drive the decision on their tech stack, not the other way around.

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1770.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1770)

So I want to show you how we can serve as a  transformation partner for our SAP customers. For most customers, that transformation begins with a migration. They are migrating to the cloud, either extending their current license with SAP or looking at SAP's business suite on AWS, and that is truly the foundation that they need to unlock the innovation and everything else that comes with it. So once that foundation is in place, customers turn to us to focus their activities on driving business value and business process change. They can do that by accelerating innovation with AI, where they can take advantage of our 200+ services, including our leading AI and agentic AI portfolio that allows them to deliver operational efficiency as well as new customer experiences.

They can unlock the value of their data, so their data might be sitting in silos, but now they can open up that data for real-time and predictive insights. They want to scale and transform like Amazon as well, going beyond the technology to improve the core business processes, and that's what we call Pan-Amazon. Programs such as Buy with Prime and multi-channel fulfillment are some of the items we're going to go through right now.

These outcomes that customers look at are presented in a circle because they're not in any fixed order. You can do them one by one, or some people do them concurrently. We will work with our customers to tailor that approach and our priorities because we want to become your business partner. We believe there are some unique capabilities that we provide to our customers on their SAP transformation journey.

These are some of the reasons people choose to move their SAP systems to AWS, but there are others as well. SAP customers trust AWS to run their mission-critical SAP systems because of our experience and our scale and credibility. We're the most comprehensive and broadly adopted cloud with over 200 services, 60% more services than other cloud service providers. We have millions of customers and lots of partners to help you on your journey as well.

[![Thumbnail 1870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1870.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1870)

We also have the most SAP experience with over 17 years of building with SAP in the cloud. SAP trusts AWS. The overwhelming majority of SAP's products run on AWS, including the most BTP services in the most regions. That combination of experience, scale, and mutual trust is why thousands of customers have moved their SAP systems onto AWS. 

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1920.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1920)

### Pan-Amazon Services Integration: Revolutionizing Supply Chain and Procurement with SAP

What truly sets AWS apart is not only that we are a cloud service provider, but we can help you transform, and that includes some of our Pan-Amazon solutions.  You can see on the slide there are a lot of Pan-Amazon solutions that you can choose from, and they're vast, spanning from supply chain to business procurement as well as Amazon Ads. By combining these business Pan-Amazon solutions with your SAP systems, you can reimagine your business processes, accelerate your outcomes, de-risk your transformations, and innovate together with us.

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/1950.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=1950)

At the heart of these Pan-Amazon services is our global fulfillment network.  We spent over 20 years building this global network, and it allows us to deliver over 10 billion packages every year. That network is not just for ordering on Amazon.com, but many of our capabilities are included in these supply chain and customer service services through Pan-Amazon and can be leveraged by our customers in what we call these Pan-Amazon services, allowing them to be integrated into their SAP system, which is important because SAP is powering some of the biggest global supply chains for companies across the globe.

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/2000.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=2000)

I want to spend the next few minutes talking about two of those services that are being leveraged today by our SAP customers to help them transform their businesses. That's Amazon Buy with Prime as well as Amazon Multi-channel Fulfillment.  With Amazon Multi-channel Fulfillment, it's a third-party logistics solution. It allows you to leverage Amazon's fulfillment network and expertise to pick, pack, ship, and deliver your orders beyond Amazon.com. That could be other channels including your websites, your social media stores, or e-commerce marketplaces.

Amazon Buy with Prime allows companies to attract and convert Prime members, offering them the Amazon shopping experience through Prime that they've come to know and love. It allows them to connect that to their websites and their mobile apps, including the delivery speed, the tracking, and the customer service that you get from Amazon Buy with Prime. Beyond fulfillment, we're also revolutionizing the procurement processes for our customers.

[![Thumbnail 2050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/2050.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=2050)

With Amazon Business, it integrates into over 150 leading procurement solutions including SAP Ariba and SAP Concur.  With those integrations, you can easily and securely move to indirect procurement through Amazon Business, and it's a way for you to reduce your procurement spend as well as optimize your processes around procurement all while natively integrating into SAP. That's a powerful combination that's uniquely positioned to help you in your supply chain.

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/2080.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=2080)

That combination is bringing AWS, the world's most comprehensive cloud, with SAP, the global leader in ERP, with Amazon, the world's largest fulfillment network.  That combination allows you to integrate these services from Amazon into your SAP system, allowing you to extend your supply chain and build a more agile, flexible, and scalable supply chain. Customers don't have to sell their products on Amazon to benefit from this.

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/2130.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=2130)

These are all services that are consumable as stand-alone 3PL fulfillment and procurement solutions that you can use. We believe that we have a unique approach to helping our customers transform their supply chain, their business models, and their operations through these services. I want to give you a visual representation of how these services can come together, just some of the options that you can integrate into your SAP system. 

[![Thumbnail 2160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/2160.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=2160)

You can see there are many different ways that you can do this, and we have these products and services that integrate into SAP to help you transform. With the 20 years that we spent building our own largest fulfillment center, we're giving you that expertise through these services so you can integrate it into SAP and transform your supply chain. For example, we talked about Amazon as a 3PL for your logistics and transportation, Buy with Prime for improved customer experience, as well as Amazon Business for procurement. 

Let's look at some of the tangible business benefits of bringing these services together. This brings our unique operational expertise to our customers who want to scale and transform, and they'll gain access to these benefits that they can't find anywhere else. When you work with us, we can help you deliver a better shopping experience with the speed, predictability, and reliability of Amazon's fulfillment network, as well as our Prime shopping benefits.

You can also achieve operational efficiency at scale. You can scale your business and optimize your operations with access to the world's largest online marketplace and fulfillment network. You can grow with a network that delivers and boost your shopper conversion by expanding into new regions geographically and new lines of business and channels.

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/2220.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=2220)

To hear more about real world SAP transformation stories and to talk to Mr. Kim  and other SAP and AWS customers who have gone through transformation journeys, I would like to invite you to our Q&A session and customer roundtable that is happening tonight at Smith and Wollensky between 4 and 5. To close us out, I want to thank Mr. Kim for sharing their inspiring transformation story with us here today, and hopefully we gave you some insights on how SAP and AWS and Pan-Amazon services can help you transform your business as well.

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/f88cfb4bd9c73aea/2260.jpg)](https://www.youtube.com/watch?v=gmwoJ_Ly8x0&t=2260)

I was hoping to see you at our other sessions over the course of the next few days and maybe see you tonight at our customer roundtable. Again, I want to say thank you for joining us in this session.  I hope you have a great rest of your week here at Reinvent.


----

; This article is entirely auto-generated using Amazon Bedrock.
