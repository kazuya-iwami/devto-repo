---
title: 'AWS re:Invent 2025 - Real-time insights for smart manufacturing with AWS Serverless (CNS375)'
published: true
description: 'In this video, Mohamed Salah, Senior Solutions Architect at AWS, addresses how unplanned downtime costs the top 500 manufacturers $1.4 trillion annually—equivalent to Spain''s GDP. He demonstrates a solution using AWS Serverless architecture and GenAI to solve three key manufacturing challenges: data silos, skill gaps, and delayed visibility. The solution implements the AWS Garnet framework to unify disparate data sources (IoT, SFTP, telemetry) into a single data lake, fine-tunes models using Amazon SageMaker and imports them into Amazon Bedrock, and deploys AI-powered chatbots with voice capabilities. A cookie production line example shows how vision language models detect defects and automatically notify operators with compliance-focused recommendations. The architecture includes continuous learning through operator feedback, enabling model improvements. The entire implementation was completed in seven days.'
tags: ''
series: ''
canonical_url: null
id: 3084349
date: '2025-12-04T16:17:21Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Real-time insights for smart manufacturing with AWS Serverless (CNS375)**

> In this video, Mohamed Salah, Senior Solutions Architect at AWS, addresses how unplanned downtime costs the top 500 manufacturers $1.4 trillion annually—equivalent to Spain's GDP. He demonstrates a solution using AWS Serverless architecture and GenAI to solve three key manufacturing challenges: data silos, skill gaps, and delayed visibility. The solution implements the AWS Garnet framework to unify disparate data sources (IoT, SFTP, telemetry) into a single data lake, fine-tunes models using Amazon SageMaker and imports them into Amazon Bedrock, and deploys AI-powered chatbots with voice capabilities. A cookie production line example shows how vision language models detect defects and automatically notify operators with compliance-focused recommendations. The architecture includes continuous learning through operator feedback, enabling model improvements. The entire implementation was completed in seven days.

{% youtube https://www.youtube.com/watch?v=Me14ltJ8WWE %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/0.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=0)

[![Thumbnail 10](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/10.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=10)

### From Burned Cookies to Manufacturing Crisis: The $1.4 Trillion Problem of Unplanned Downtime

 A few weeks ago I was baking cookies with my daughter. This is what we are always doing. She's mixing the dough  and I'm the one who is baking the cookies. I opened the oven and found a tray where half of the cookies were burned. We tried to figure out what happened, but my daughter was looking at me with her hand full of flour and asked, "What went wrong, Baba?" To be honest with you, I don't know because the recipe didn't tell me the butter was too soft, and the oven didn't mention whether we have half of the oven hotter than the other half or not.

This is what hits me: if this is happening inside any manufacturing facility, what will happen? You have the data, you have sensors doing notifications for your system, you have a system recording this, and you have machines reporting those kinds of information, but at the end they are always disconnected. This kind of silent moment is what we are trying to solve today. Today I'm going to show you how AWS Serverless architecture along with GenAI can build a connected manufacturer integrated with an AI system to have a solid understanding of your manufacturing operations. My name is Mohamed Salah, and I work as a Senior Solutions Architect at AWS, looking after the public sector in the Middle East.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/100.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=100)

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/110.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=110)

My job at AWS is helping different customers solve similar problems.  Picture this:  the same silence inside the factory. A machine stops, the production line pauses, and everyone starts running around. Every minute your products are not made, your orders get delayed, and costs pile up. This silence, this helpless pause, is what we call unplanned downtime. The top 500 manufacturers globally lose 1.4 trillion US dollars every year to this silence. They are losing around 11% of their revenues. To make it much clearer, this is equivalent to the GDP of a nation like Spain.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/170.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=170)

When we take a closer look at this, we found it has three different issues. First is data silo.  Your production line consists of four or five different machines, and each machine has its own data sources or different data information. You need to report those information, and those information are completely recorded in a separate database. It will end up that you have five machines as part of your production line, each machine has its own database, but you don't have this kind of correlation to understand what happened end to end. Yes, the data are there, but it is disconnected.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/210.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=210)

Second is a scale gap.  It's all about people. In each factory, you will have experts and seniors who can understand exactly what is happening on the ground. They can give you insights from the sound of the mixer—they can tell you there is over-mixing here. From the oven, they can tell you this oven is having a problem, this is hotter on the left side than the right side. This kind of knowledge, if those experts are not there, will end up that you are not able to operate your production line correctly because junior operators will stay in the line, receiving warnings but they will not be able to act. This means that the knowledge is there but is not shared.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/270.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=270)

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/300.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=300)

Third one is when this silence became visible.  Once this silence happened, the production line will get a product delayed and the orders will get delayed. Why? Because you have multiple data sources at the end, and at the same time, you don't have this kind of expertise that can analyze those data sources. We thought about how we can solve this. We started with a simple solution. Let's think of a data link.  As you can see here, we have multiple data sources. We have telemetry data and alarms, we have environmental data, and we have documents.

### Building a Connected Factory: AWS Garnet Framework, Data Lakes, and Fine-Tuning Models with Amazon Bedrock

By implementing these data sources and consolidating them in one data lake, we discovered another challenge: integrations. Each data source has its own integration methods. As you can see, some data sources use modern IoT integrations like MQTT, while others rely on legacy integrations such as SFTP for documents. We decided to implement the AWS Garnet framework to create a single unified API that can absorb these different types of integrations and convert your data into a modeled format. This allows you to establish relationships between your different IoT devices in one graph database, building connections between different things.

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/390.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=390)

Once you have this data lake, you can support different consumers for that data. One consumer is QuickSight for dashboards, and another is building a replica of your production line, which we call a digital twin. Let's take a closer look. On the left side,  you see your production line—what is actually happening on the ground. You can understand what the problem is. As you can see, the cookie inspector is telling you that cookies are cracked and the machine state has been down since five. With this dashboard, you achieve what we call connected data points. At the same time, you break down these data silos.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/420.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=420)

 The important part is the skill gap. Yes, you have experts, but you don't have this kind of knowledge sharing. We decided to implement a GenAI application deployed over Amazon Bedrock and let operators interact with it by asking questions and chatting with your data. As you can see, we are fine-tuning the model and importing it into Amazon Bedrock to expose text and voice applications. At the same time, you get real-time data streamed using the Garnet framework to your Bedrock engine. This bridges the skill gap.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/480.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=480)

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/490.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=490)

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/500.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=500)

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/510.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=510)

The second important aspect for different use cases is the automation needed. You need this kind of automation  every five minutes, understanding what is exactly happening under the hood, knowing the streams, and then getting recommendations  based on what is happening and what data is captured from different data sources, giving a notification to an operator to act accordingly.  We decided to implement full automation AI to act and call APIs with different machines to act accordingly. 

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/520.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=520)

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/530.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=530)

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/540.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=540)

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/550.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=550)

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/560.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=560)

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/570.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=570)

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/580.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=580)

This is how it works at a high level. You have a cookie inspector here after the former,  after the tunnel, and this inspector uses a small vision language model to inspect whether cookies are cracked,  misshaped, or have air pockets. Let's say we detect cracked cookies. You invoke Bedrock with a vision language model,  and Bedrock requests data from multiple sources: production line standard operating procedures,  machine alarms and data, telemetry data, and even machine manuals. Once you have this data,  you can first send automated notifications to your operator. At the same time, you can expose the text and voice chatting application,  and accordingly you can take actions.  This is what happens to solve the problem automatically without having a senior on the ground and without having fully integrated data points.

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/590.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=590)

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/600.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=600)

Here is the high-level architecture showing how it works.  Let me show you the details. This is where equipment measurements fit inside the AWS Garnet framework and AWS IoT Core, where you send the data. 

For AWS IoT Core, you will send the data using a Lambda function. NOSS will change the data model of your input to a different standard data model. Accordingly, you will have an SQS to decouple between your S3, which is acting as a data lake to consolidate all your data points in one place. The second important part is actions. Is it possible to have actions to update operations in your production line, to update the cookie mixer to reduce the temperature, reduce the torque, or perform different actions? This can be done using an API Gateway to execute those actions. This is the foundational part for data preparation.

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/660.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=660)



We are performing data preparation because we need the model itself to understand how to respond to queries. The response can be structured in a very detailed way to guide the operator on how to solve the problem, and instructions should be compliant, safe, and should categorize this kind of safety. That is why we decided to fine-tune the model to achieve this kind of skill and tone needed for your model. We started with multiple documents and multiple data sources. We are converting and transferring this to S3, and accordingly, we have a Step Function. This Step Function acts as a data pipeline. You are reading documents, PDF files, CSV files, and text files, understanding what is happening on the ground, and then invoking a model to generate structured data.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/750.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=750)

Structured data is needed because if you are going to fine-tune a model, you need very structured data. If you do not have structured data, it becomes a kind of fine-tuning hallucination machine. Accordingly, you will have an output of structured data  and another output of structured data only for testing. This output feeds into Amazon SageMaker AI Foundation Model Operations, where I will fine-tune the model using this data and using what we call instructed fine-tuning. Once I have these artifacts or updated weights for my model, I can accordingly import the model inside Amazon Bedrock.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/820.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=820)

This step can be done in different ways. We prefer here to use a serverless implementation by Bedrock. Instead of deploying this over an EC2 instance, you have to calculate GPU utilization and be aware of exactly what is happening under the hood to do correct inferencing. However, just importing this over Bedrock will give you a unified API to call different versions of your imported model. Once you have this Amazon Bedrock, we can go  to the web application. The significance of using the web application is to expose this kind of chatting for voice and data with your model.

### Deploying Intelligence on the Factory Floor: Converse API, Automated Notifications, and Continuous Learning

We have two different things here. First is the chatting application with React, the normal React over S3 with Lambda functions to invoke the model. Here we are using something called the Converse API. This Converse API gives you flexibility and scalability to invoke any model versions from your fine-tuned model. Let's say you got feedback, thumbs up, about one of your models and this model is doing great. However, you found an issue in production after ten days. You can roll back a model by just changing the model ID using the Converse API. The significance here is that the Converse API maintains the same signature while changing different model IDs.

Second, which is the important part, is the notification part. The notification here allows you to feed the model with data captured every five minutes and then start digesting that information, comparing that information with the fine-tuned model.

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/940.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=940)

By comparing this information with the fine-tuned model, you can provide clear recommendations considering compliance, severity, and the necessary steps to be taken on the ground. By doing this, you can have Amazon SNS notifications sent to your operators to notify them. You can act accordingly in this event to change the needed actions. At the same time, you can avail both options in the same place. 

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/970.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=970)

This is a high-level view of the architecture where we started by digesting the information using a Garnet framework. This is the foundational part where you perform data preparation and then fine-tune the model. You then have a web application that connects with a custom model imported over Amazon Bedrock. 

At this point, you have an AI application that makes every single machine sensor and system speak the same language. You are able to have connected data points and connected machines in one single place by implementing the Garnet framework to absorb these different types of integrations in one place. Second, you will be able to fine-tune the model and import it inside Amazon Bedrock, giving you this kind of intelligence. Now we understand what is happening on the ground, what changes, what will be done next, and what recommendations are needed.

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/1070.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=1070)

By implementing this, you are still generating much more data. This is what we call continuous learning, and this is your gold. Once you have updated information, you still have room to improve your model and enhance its accuracy, getting much more accurate information. This continuous learning is much needed because as part of this implementation, there is a feedback mechanism implemented by the operator based on the response from Amazon Bedrock. 

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/1090.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=1090)

If this feedback sums up or sums down, I can act accordingly and provide an updated, fine-tuned model based on this information. You can scan those QR codes. This is a Git repository for the Garnet framework to have this kind of conversion of your connected devices. The second QR code shows how to implement a small language model and import the custom model over Amazon Bedrock. 

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bf3bcba9cec17da6/1120.jpg)](https://www.youtube.com/watch?v=Me14ltJ8WWE&t=1120)

The last one shows how to continue updating your information, fine-tuning the model, and implementing a machine learning pipeline end to end. Now it is your turn. You can start with a small process and small data, and you can understand exactly what is happening on the ground. You can start with one production line with multiple data sources and implement this kind of fine-tuning implementation. We did this full implementation in just seven days. 

After implementing this and importing the model, you will be able to gain more insights and enhance your model. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
