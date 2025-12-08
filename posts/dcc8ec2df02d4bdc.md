---
title: 'AWS re:Invent 2025 - Peak Performance: IoT Innovation in Professional Sports (SPF301)'
published: true
description: 'In this video, Mike and Greg from AWS explore how Catapult Sports uses AWS IoT services to transform athlete performance monitoring through their Vector 8 wearable technology. The session covers Catapult''s data ingestion architecture, processing device health, hot data (10 Hz live tracking), and cold data (100 Hz post-game analysis) through AWS IoT Core, MSK, and S3. Key metrics like player load and sport-specific movements are explained, demonstrating how 36 million data points from a single NHL game enable AI-driven insights. The presentation details the AWS IoT Greengrass-based device architecture, highlighting how over-the-air updates are now 32x faster than Vector 7, device onboarding reduced from days to 10 minutes, and how Shadows, Secure Tunneling, and Log Manager enable remote configuration and diagnostics. Future directions include edge ML inference and real-time cloud streaming for remote coaching access.'
tags: ''
series: ''
canonical_url: null
id: 3085191
date: '2025-12-05T03:48:13Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Peak Performance: IoT Innovation in Professional Sports (SPF301)**

> In this video, Mike and Greg from AWS explore how Catapult Sports uses AWS IoT services to transform athlete performance monitoring through their Vector 8 wearable technology. The session covers Catapult's data ingestion architecture, processing device health, hot data (10 Hz live tracking), and cold data (100 Hz post-game analysis) through AWS IoT Core, MSK, and S3. Key metrics like player load and sport-specific movements are explained, demonstrating how 36 million data points from a single NHL game enable AI-driven insights. The presentation details the AWS IoT Greengrass-based device architecture, highlighting how over-the-air updates are now 32x faster than Vector 7, device onboarding reduced from days to 10 minutes, and how Shadows, Secure Tunneling, and Log Manager enable remote configuration and diagnostics. Future directions include edge ML inference and real-time cloud streaming for remote coaching access.

{% youtube https://www.youtube.com/watch?v=Plw5F5LKNnc %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/0.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=0)

### Introduction: Data-Driven Sports and Catapult Sports

 I normally like to start these sessions with a little bit of interaction, so indulge me for one second. Hands up if you knew that almost every professional or semi-professional football team, professional team, or individual athlete uses some form of wearable device to track their data either in-game or in practice. Significantly more than I thought there would be. You may have thought about what type of data they're ingesting or what type of analytics can be generated on that data, but maybe you haven't thought about who actually makes these devices and the market that they serve. That's what we're here to talk to you about today. For those that didn't raise their hand, I hope today you can see a different side of sport, one that's fueled by data.

[![Thumbnail 60](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/60.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=60)

My name's Mike.  I'm a sports solution architect based in Australia, and with me I have Greg, one of our senior IoT specialist solution architects. Today, we'll be talking about one of my customers, Catapult Sports. I'll refer to them as Catapult throughout the rest of the presentation. We'll be talking about who they are, what they do, and how they're transforming sport.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/80.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=80)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/90.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=90)

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/100.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=100)

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/110.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=110)

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/120.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=120)

 We'll talk about athlete data ingestion. What are they ingesting? How are they doing it? And then what can be done with that data once it's been ingested?  What type of analytics, metrics, and improvements to athlete performance can be given? We'll go a bit deeper into the device software architecture.  And how they've used AWS to build their wearable devices. We'll talk about device onboarding, how they manage updates, and how they can  roll out different types of configuration. And finally, we'll look to the future.  What can Catapult do next to continue evolving data in the sports world?

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/130.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=130)

Now I've mentioned Catapult, so let me introduce you to who they are.  Catapult exists to unleash the potential of every athlete and team in the world. Operating at the cross section of sports science and analytics, Catapult products are designed to optimize performance, improve recoverability, and quantify return to play. The business is listed on the Australian Securities Exchange and has over 400 staff based across 24 different locations worldwide. Rather than tell you throughout the presentation who they are and what they do, how about a little video to set the scene and contextualize this.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/170.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=170)

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/180.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=180)

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/190.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=190)

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/200.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=200)

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/210.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=210)

### Catapult's Athlete Monitoring Solutions and the Vector 8 System

     Within that video, I'm going to refer to a couple of things that you might have seen. Catapult has a number of different solutions, and we don't have time to cover all of them, but I'll touch on three and we'll go into depth into one of them specifically today. First, we have athlete monitoring. You may have seen in the video there was a little device that was put into a vest that one of the players was wearing. This is designed to optimize performance, reduce the risk of injury, and improve recovery. This is what we'll dive deep into today.

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/260.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=260)

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/280.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=280)

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/290.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=290)

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/300.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=300)

The second solution is scouting and recruiting. This is designed to find, evaluate, and acquire the right players for your team. And finally, to bring context to  data, Catapult has a video suite offering, and this connects the data to video to transform strategy, performance, and training. Now before we dive deep into the athlete monitoring solution, let's take a little bit of a look at Catapult's market. Catapult operates in over 128 different countries.  It has customers in over 40 different sports,  and has over 5,000 elite teams as their customer. Catapult's  customers include some of the biggest clubs and franchises in the world, from the NFL to the NHL to the English Premier League. Here is an example of the top sports that Catapult have customers in.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/350.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=350)

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/380.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=380)

Now diving into the athlete monitoring capabilities that Catapult provides. Catapult tracks comprehensive performance metrics that give coaches actionable insights for training and competition. I want to specifically touch on a couple of these metrics. Player load is one that we'll dive deeper into later, but it quantifies the total physical stress. This is a primary tool for workload management and injury prevention across a season. Next, we have speed zones. Speed zones categorize athlete output across velocity thresholds, helping us understand game demands and prescribe appropriate training intensities. For those who work with contact sports, contact intensity measures collision forces in those sports.  This is important for managing player safety and recovery needs. Finally, something very important is that Catapult can identify sport-specific movements, such as jumps, tackles, and throws. These are automatically detected and quantified, providing tailored insights for the 40+ sports that I mentioned earlier. Together, these metrics enable data-driven decisions that optimize performance while keeping athletes healthy and ready to compete. 

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/400.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=400)

With that, I want to introduce Vector 8. Vector 8 is Catapult's next-generation solution for athlete monitoring designed for both elite and competitive sports environments. Vector 8 consists of four main components. First, we have the Vector 8 Tag, which is what was shown in the video. It is compact and rugged, packed with advanced features.  It uses GNSS and local positioning for precise location tracking, and it works seamlessly indoors and outdoors. It captures sports-specific events, monitors heart rate, processes data onboard, and supports integration with third-party Bluetooth and ultra-wideband, or UWB, sensors. It offers up to six hours of battery life. Ultra-wideband, or UWB for those unfamiliar, is a wireless communication technology that uses short-range, low-power radio signals to enable precise location tracking and secure data transfer.

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/450.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=450)

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/470.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=470)

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/490.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=490)

It's important to note that each athlete has their own Vector 8 Tag. The Vector 8 Dock is used post-session or post-match, and the tag gets placed back into the dock.  The Vector 8 Dock enables fast WiFi downloads and effortless data syncing directly to the cloud. It supports easy workflows, manages devices locally or remotely, and its onboard computing means you can use it anywhere without the need for additional equipment. Next, we have the Vector 8 Receiver.  This connects via both WiFi and Ethernet, and it streams live data to the cloud for instant analysis. It supports both online and offline workflows and provides real-time diagnostics for performance and troubleshooting. Finally, we have the Vector 8 Relay.  This increases coverage across larger venues, eliminating the need for multiple receivers. It boasts a 400-meter range in a compact form, making scalable deployments efficient and cost-effective. Together, these components make Vector 8 a powerful suite for collecting, streaming, and analyzing athlete data, advancing both training and in-game decision-making.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/520.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=520)

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/530.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=530)

### Data Ingestion Architecture: Device Health, Hot Data, and Cold Data Pathways

Now I want to talk about how they do this, and this is where I introduce our first AWS service of the day.  AWS IoT Core is a managed service that provides scalable and highly available endpoints in the cloud, meaning you have an endpoint there so you don't need to create one for every device.  It provides device authentication and authorization, encryption in transit, and device management at scale, which is crucial for Catapult's use case. Most importantly, there's the rules engine. The rules engine is how you connect the application to the data coming through IoT Core. The rules engine is the bridge between IoT Core and your application to manage a large fleet of devices, ranging from thousands to tens of thousands. That's what makes this a great service to underpin Catapult's Vector 8 technology, and Greg will go into more detail later on the extent that they use it.

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/600.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=600)

Before we dive deeper into how Catapult does this, I want to talk about the different ingestion paths and the different types of data that Catapult can collect. First, we have device health. This is the battery status and firmware version of the Vector 8 Tag itself. We then have hot data, which is live in-game or in-practice data that's sampled at 10 Hz.  This includes things like acceleration, velocity, player positioning, and heart rate monitoring.

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/610.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=610)

Finally, we have the cold data.  This is post-game or post-practice raw inertial data sampled at 100 hertz. This makes the basis of the machine learning inference that Catapult runs. They do it on the cold data in a batched approach rather than streaming it through the hot data. But what does this look like?

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/630.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=630)

 Let's start with device health during a game or practice. This slide illustrates the real-time device health monitoring architecture, which is a critical operational feature for maintaining fleet reliability during live games and practice. It starts with the player wearing a Vector 8 tag, and the data that has been ingested is transmitted by UWB to the Catapult receiver. The receiver is on the sideline, meaning there is low latency and high precision tracking.

From the receiver, this publishes device telemetry via MQTT to AWS IoT Core. This creates a secure, scalable ingestion pipeline that can handle data from hundreds of devices simultaneously. AWS IoT rules engine processes incoming messages and routes them to Amazon MSK, our managed Kafka service, for real-time streaming and buffering, ensuring we never lose critical device health data. From MSK, data flows to Amazon Elastic Container Service, or ECS, where Catapult's containerized analytics services run.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/720.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=720)

This processes battery levels, firmware versions, and other diagnostics in real time. Results are exposed through Amazon API Gateway and delivered to the Catapult web interface. This is where coaches and equipment managers can monitor device status at a glance. An example of a payload is there on the screen. We can see battery and the firmware version.  This enables proactive intervention before devices fail during critical moments.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/760.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=760)

Prior to AWS IoT, Catapult lacked the observability of device status, health, and usage. Now they have that in real time for every device. This AWS-powered architecture ensures high availability, scalability, and real-time visibility into the entire device fleet, minimizing downtime and maximizing performance reliability. Let's take a look at the hot data. This architecture delivers real-time athlete performance at 10 hertz, as previously mentioned, or 10 updates per second.  This enables coaches to make split-second decisions during games and practices.

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/780.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=780)

There are inertial sensors on the tags, as mentioned previously, for precise positioning and tracking. The tags transmit via UWB to the receiver and then stream data to the mobile devices and web applications.  The payloads are updated in real time, and here is a small example. As you can see, there's heart rate, position, velocity, and acceleration. Coaches can see instant visualizations on their iOS apps. They can see player workload comparisons, positioning heat maps, and individual metrics on these devices.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/820.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=820)

This enables in-the-moment interventions, substituting fatigued players, adjusting tactics based on movement patterns, and preventing injuries before they happen. Finally, we have the cold data, which is post-game, post-practice, sampled at 100 hertz.  This is what Catapult uses to run their machine learning inference on. To put some numbers in the air, a two-hour session with 30 athletes can upload to S3 in less than 30 seconds.

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/870.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=870)

Let's take an NHL game. Assume it's a 2.5-hour game with 20 athletes on each side, so 40 athletes, all wearing Vector 8 tags with inertial data sampled at 100 hertz. That's 900,000 data points per athlete in that 2.5-hour game, which means 36 million data points are captured in a 2.5-hour game. We've spoken about the ingestion of data and what this looks like and how it's done. Now I want to talk about what happens  after that data is ingested.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/890.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=890)

### From Raw Data to Actionable Insights: Performance Metrics and Video Integration

I'm going to mention some metrics, but we don't have the time for me to mention all of them, as Catapult can create 600 unique metrics per player per game. I'm going to put up on the screen a couple of different sports and a couple of the different sport-specific metrics that can be captured for each sport.  Vector 8 delivers AI-driven sports-specific live data so coaches can monitor athlete output in any environment.

This is essentially automated performance insights. Catapult runs its machine learning algorithms on top of the cold data and processes large datasets quickly and accurately to identify trends and patterns that might be missed by the human eye. These sports-specific movements are very important, but I want to think about it from the coach's perspective. I have some customers back in Australia that prior to using Catapult spent over eight hours a weekend looking through video just coding events. Post-implementation of Catapult, they've almost halved the time taken by being able to identify these events automatically, not through video, but just through tags. I'll get to it a bit later on what happens when you can intersect data that is collected from these devices and video.

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/960.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=960)

Now, one metric I spoke about earlier was player load, and you might be wondering why we've just entered a math session.  For anyone who has done math, the minuses are not meant to be subscript; they're meant to be subtractions. But let me define player load and then contextualize what this means. Player load is the sum of the accelerations across all axes of the internal tri-axial accelerometer during movement. This was originally developed by the Australian Institute of Sport with rugby union as the customer in mind. It's important to know that the output of this is just in arbitrary units. This is a volume measurement—it is how much work a player has done.

This can be used to compare players to each other, or it can be used to compare a player's workload over time to see how they're returning from injury or whether they are tracking towards an injury. It has advantages over using something like distance as an effort metric because it can accumulate during tackles, ruck work, and non-running activities or off-the-ball activities. To contextualize, I'm going to use an example. I love tennis and I'm a big tennis player. Let's assume I'm coming back from injury and my player load for a session is 200—arbitrary units. The effort that I had for that session was 200. Coach is happy, I'm happy, my performance looked pretty good.

The next session, my player load is now 300. So I'm working 50 percent harder than I was in the previous session. But my output has dropped a little bit. This can identify a couple of things: I'm working harder, but my performance is worse. Maybe I'm not ready to come back from injury, or maybe I've aggravated something as well. Now I want to be very clear—this is not a definitive metric. This is something that can be used as an input to other statistics. It can be used as an input metric with rhetoric: how did I feel during the game? Or it can be used with other stats.

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1140.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1140)

Now a customer of mine in Australia started implementing this, and they decided to delay the return of a player by a number of weeks because they noticed that their player load was continuously higher when they had come back from injury earlier, but their output was significantly less than normal. This is a volume measure and an example of how much effort a player has put into a session or a game. Speaking of effort, Catapult provides effort breakdown reports, and this consists of three main sections. We have effort profiles, which is each acceleration or velocity event per player that is tracked, and we'll see what this looks like in a second. We have effort location mapping, which is a visual representation, and we'll use soccer as an example in a second, showing around where exactly on a field or pitch that effort occurred. And finally, we have the velocity traces. 

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1150.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1150)

Coaches can visualize how speed builds and fades across each individual effort for each player. But let's see what this looks like.  Now there's a lot happening on the screen, but I want to focus on the bottom left area first. This is a detailed profile of every effort. Each acceleration or velocity event is analyzed by duration, peak intensity, and recovery time to help understand the physiological load and tactical purpose of each effort. To the right, you'll see a soccer pitch. This is the effort location mapping. Coaches can view exactly where each effort occurred on the field.

This brings context to high-speed movements and context to the effort profiles on the left. And finally, we have the high-resolution velocity trace. With access to a ten hertz raw velocity trace, practitioners can clearly see how speed builds and fades across each effort. This supports deeper quality control and a better understanding of the player's output.

[![Thumbnail 1230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1230.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1230)

Effort breakdown gives coaches the tools to move from assumption to precision. By turning raw movement into actionable insight, teams can design smarter drills, manage workload with greater control, and ensure players are executing exactly what the game demands. In the video earlier,  you may have seen a screen similar to this being played through. It is important to note that wearable data without any context or video is essentially just noise. It can be used for analytics, but it cannot really explain what happened on the pitch.

Vector 8 automatically pushes athlete performance data into Catapult's Pro Video suite. This allows users to align physical output with what actually happened on the pitch or field. Coaches and analysts can review, tag, and explain moments of interest immediately, improving athlete understanding and engagement. You can capture, synchronize, and live stream multi-angle video on any device from any location. This is a way of integrating player performance data to video to uncover new context to team and player performance.

The fusion of video and wearable athlete data is changing the way that people think about sport. It is helping back decision making, it is helping quantify return to play, and hopefully in the future it will be able to prevent injury. We have spoken about Catapult and the data that they can ingest, what type of metrics they can create on top of it. Now let us take a deeper look into how they leverage AWS to do so.

[![Thumbnail 1350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1350.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1350)

### Device Software Architecture with AWS IoT Greengrass and Streamlined Onboarding

We are going to dive a little bit deeper now into how Catapult is using AWS IoT services to build their Vector 8 product, the benefits they are driving, and the value it is helping them create for their customers. We will start by looking at their device software architecture.  AWS IoT Greengrass is a service that Catapult is using as the foundation for the Vector 8 dock and receiver. Greengrass is two things: it is an edge runtime and a cloud service that helps you build, deploy, and manage a multi-process IoT application at scale. The edge runtime is open source and available on GitHub.

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1380.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1380)

[![Thumbnail 1390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1390.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1390)

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1410.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1410)

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1420.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1420)

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1430.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1430)

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1440.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1440)

 The processes or services that you manage using Greengrass are modeled as so-called components.  Those components can be executables, binaries, scripts, container images, or pretty much any code that you could otherwise run on the device. You can run under the control of Greengrass. As soon as you have a multi-process application, you have dependencies. Greengrass helps Catapult with that by  supporting component versioning and component dependency resolution. It also supports logging for diagnostics.  We have a large collection of pre-built AWS components to solve common challenges. And it has inter-process communication,  IPC for sending messages between components and between processes, but also to access some advanced Greengrass features. 

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1470.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1470)

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1480.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1480)

Looking at the software architecture at a very high level, the architecture of the docks and the receivers for Vector 8 has an embedded Linux operating system at the bottom. The Greengrass edge runtime sits just above that and manages a multi-process application. This application is comprised of Catapult custom components complemented by AWS-provided components.  Moving from the general to the specific,  let us take a look inside the receiver software architecture. As you saw earlier with what was shown about the data ingestion, the tags communicate wirelessly using UWB to the receiver. Inside the receiver software, there is the data processor custom component developed by Catapult that is receiving that data, including the device health data and the live data, the 10 Hz in-game data.

The data processor component uses the inter-process communication feature of Greengrass to publish the device health messages to AWS IoT Core via the Greengrass nucleus, which is the heart of the Greengrass edge runtime. Greengrass includes a message spooler that is useful if there is any loss of connectivity. If the dock loses internet connection, Greengrass can buffer those device health messages and send them later when connectivity is restored. This saves Catapult from having to build that undifferentiated heavy lifting themselves just to do the buffering and retries.

Additionally, Catapult has custom components that deploy a database, a RESTful API, and a web server, and those are used to serve that 10 Hz live in-game or in-practice session data to the iOS apps on the sidelines that the coaches and sports scientists are using. Moving onto the dock, as you heard earlier, when the tags are used with the dock, they get placed back into the dock for recharging as well as for getting data off the tags. When the tags are placed in the dock, it is a wired connection, not wireless. On the dock, there is a custom component called the retriever component that is able to retrieve that cold data—that raw 100 Hz postgame data—and retrieve it as data files and write that to disk on the dock.

Then the raw file upload custom component retrieves those data files and publishes them to the Amazon S3 bucket. As you heard from Mike, this whole process to get all the data from all the athletes from one game or one training session typically takes just tens of seconds. Device onboarding, or device first-time setup, is a really big topic for Catapult. For the Vector 8, they had a goal that onboarding should be a self-service experience that enables the customer to go from unboxing to athlete tracking in less than 10 minutes.

To understand why they had that goal, let's rewind and talk about how their previous product generation, the Vector 7, worked for first-time setup. Vector 7 was not a connected product. It was not an Internet of Things device. For Catapult's customers, when they got Vector 7, they would unbox the device and they would need to install a desktop application on their laptops. They would then book an appointment with Catapult customer support, wait for that appointment to come around, and then work with customer support to configure the devices, apply any firmware updates that had been published since the devices shipped, and then test and make sure the devices were in a working state.

It was a long process, typically on the order of two days but at least many hours, and it was very hands-on for both Catapult customer support and also for Catapult's customers. Now with Vector 8, the first-time setup process is more akin to what you are used to in setting up any consumer-grade electronics device. The customer unboxes the device, installs the Catapult app on their phone, registers that device within the app, and that also provisions the Wi-Fi credentials on the device. Then the device is able to connect to the cloud and automatically pull down the configuration and any firmware updates, and finally publish its status at the end. From go to woe, it is a 10-minute process with no involvement of Catapult customer support and a much more seamless experience for the customer. So it is good for Catapult and good for their customers.

### Over-the-Air Updates, Configuration Management, and Innovation Acceleration

In terms of the value that AWS IoT brings, it is helping with the scale and the automation of device provisioning, configuration, updates, and status reporting. I mentioned device updates, but there is a lot of detail there, so let's unpack that. At AWS, security is job zero, but in the IoT domain, I like to say that over-the-air update is feature zero. It is the foundation on which you can build everything else. If you have over-the-air update, you can easily add new features and enhancements

[![Thumbnail 1810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1810.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1810)

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1820.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1820)

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1830.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1830)

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1840.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1840)

 to your devices even after they've shipped to the field. You're able to easily patch your devices at scale, resolving bugs and customer support issues, and helping to prevent other customers from reporting the same issues.  Over-the-air update also helps enable innovation because you're able to iterate more quickly on your device software and help build features that customers want in response to customer feedback.  

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1860.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1860)

[![Thumbnail 1880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1880.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1880)

You saw earlier that the docks and receivers are using AWS IoT Greengrass. Greengrass supports over-the-air update out of the box using a feature called Greengrass deployment. Greengrass deployments allow Catapult to define a set of components  that should be deployed to a dock or receiver. Some of the components are shared between the two, but they also each have some unique components because they do different things. With those deployments, it can target a single device, but it can also target a group of devices, so a deployment can operate  at scale by deploying to a large collection of devices.

[![Thumbnail 1890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1890.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1890)

[![Thumbnail 1920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1920.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1920)

[![Thumbnail 1930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1930.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1930)

It's resilient to intermittent connectivity. When Greengrass  receives a deployment, it needs to pull down those component artifacts, the program binaries, executables, and container images. If there's any interruption in the connectivity, Greengrass will handle all the retries, pausing, and resuming, and get all of those artifacts to the device, saving Catapult from having to build that boilerplate themselves. Updates are granular, meaning that Greengrass calculates the delta  between its current state and what the deployment demands, and it only downloads what it needs to make its state match what's specified in the deployment. 

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1950.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1950)

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/1970.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=1970)

Greengrass has rollout and rollback configuration. If Catapult makes a deployment that includes a faulty component that fails to start up and goes into a broken state, Greengrass will automatically revert to the previous deployment state, leaving the dock or receiver in a good working state, again saving Catapult from having to build that capability.  Finally, it integrates with operating system updates. Catapult can patch, update, and manage the embedded Linux operating system using Greengrass deployments. 

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2000.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2000)

For the docks and receivers, it's straightforward because it works out of the box using what Greengrass offers. With the tags, it's a little more complicated. The tags run FreeRTOS and are microcontroller-based devices, but the UWB interface they have is bespoke, a custom protocol, and so is the WiFi protocol when the devices go to the dock. Consequently, there's something a little more bespoke for updating the tags. 

[![Thumbnail 2010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2010.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2010)

[![Thumbnail 2040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2040.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2040)

Catapult begins by making a Greengrass deployment to the dock that transfers the tag firmware binary from the cloud to the dock. They then use an AWS IoT  feature called jobs to trigger the update process. The device manager custom component subscribes to the necessary MQTT topics to be notified of when a job comes in, and it does that subscription over the IPC feature that Greengrass offers. Then when the tags are returned to the dock, the device manager custom component disseminates that tag firmware binary into each of the tags. 

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2060.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2060)

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2070.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2070)

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2090.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2090)

This is powerful for Catapult because after they create a deployment in the cloud, essentially the next time the tags get returned to the dock, they will be updated. The really big benefit that Catapult is getting from AWS IoT and over-the-air updates is that compared to their Vector 7 product generation, it's 32 times faster based  on their own data.  A big part of why it's faster is that previously with Vector 7, the customer was in the loop. They needed to use their laptops and desktop application to get tags updated. Now it's an automated process. As soon as the tags go back into the dock, they're updated. 

Another benefit that comes from that same reason is that essentially almost every tag in the fleet is always running the latest firmware. If the tag goes back into a dock after an update's published, it gets the latest firmware.

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2120.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2120)

The only tags not running the latest firmware are a small percentage that haven't been returned to a dock for whatever reason—they're misplaced, they're lost, and so on. This aspect is really powerful because in their previous product generation, according to Catapult's own historical data,  more than 50% of customer support issues were resolved just by updating their devices to the latest firmware. Now in Vector 8, that happens automatically, and there's a big collapse in the number of customer support cases. That's obviously good for Catapult and good for their customers.

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2150.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2150)

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2170.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2170)

Over-the-air updates also help underpin innovation.  If you want to innovate, it helps to be able to rapidly iterate your ideation, execution, and delivery, and to have an efficient development process. Normally you'll ideate and plan, you'll build new value, you'll deploy your new increment, and in an IoT sense, that means  getting it onto physical devices. You test, you measure, you learn, you gather your customer feedback, and then you begin the cycle anew. The quicker you can get around this flywheel, the more enabled you are to innovate.

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2190.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2190)

[![Thumbnail 2200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2200.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2200)

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2220.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2220)

As you just saw, Catapult now with Vector 8 are much faster on the deployment  part. With that 97% penetration rate, when they do make a deployment, it's getting into the hands of a lot more customers much more quickly.  There's more feedback and more learnings and more testing happening. Half of this flywheel is much more performant because of the Vector 8 over-the-air updates, and this puts pressure on the other two parts to be faster. In particular, it puts pressure on Catapult's firmware team, so much so that they changed the way they  worked. They've swapped from a kanban process to sprints, their release cadence has moved from months to weeks, and in some cases even days, particularly when developing new features and doing beta testing.

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2250.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2250)

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2260.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2260)

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2270.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2270)

When they're working with their beta customers, they're able to iterate at very high velocity to help develop the feature that the customer actually wants. That's good for Catapult and good for their customers.  Every wearable device has quite a large collection of configuration parameters, and now with the  cloud connectivity available to Catapult, they're interested in having the tags also being configured from the cloud, not needing applications at the edge.  Configuration settings include things like individual athlete settings, metric settings relating to sports-specific movements, and also specific requirements of different teams, different coaches, and different sports scientists. They also include radio frequency settings to help tune that wireless communication between tags and the receivers in different environments.

[![Thumbnail 2300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2300.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2300)

Catapult are using an AWS IoT  feature called Shadows to help them with configuration. Shadows are JSON documents that AWS IoT helps you synchronize between the cloud and your device. On Greengrass, there is an AWS-provided component called Shadow Manager that implements the device-side part of this feature. You can use it off the shelf to get the shadow functionality onto your devices, and that's what Catapult have done with their docks.

[![Thumbnail 2330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2330.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2330)

[![Thumbnail 2340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2340.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2340)

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2350.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2350)

When a Vector Web user modifies the configuration, the Shadow Manager sees that the shadow document in the cloud has changed.  It automatically synchronizes  that to a local shadow database. The device manager custom component can use the inter-process communication feature to retrieve those  shadow documents, and the next time the tags come back to the dock, they automatically receive the latest configuration. Like the over-the-air update, you make the change in the cloud, and the next time the tags come back into the dock, they get the change.

[![Thumbnail 2360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2360.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2360)

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2370.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2370)

The value here for Catapult  is that this is quite decoupled now. Configuration changes can be made in the cloud, safe in the knowledge that even when the dock is offline, even when the tags are in use, the next time the tags are returned to the dock, they'll get that latest configuration.  There's no customer in the loop anymore. Previously, for configuration updates, the customer was involved, using their laptop and desktop application. They're out of the loop now, and it's less burdensome for them. They can also make configuration changes for some of the configuration parameters in the cloud. It's good for Catapult, and it's good for their customers.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2410.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2410)

### Remote Device Diagnostics, Future Outlook, and Conclusion

 Device diagnostics. Rewinding again to Vector 7 product generation, Catapult was relying heavily on TeamViewer to be able to reach their devices for diagnostic purposes. They would ask their customers to run TeamViewer on their laptops so their customer support people could have a remote session. Now with AWS IoT, they have the ability to access their devices much more directly.

[![Thumbnail 2440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2440.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2440)

 Secure tunneling is an AWS IoT feature that Catapult is using for remote SSH sessions into the docks and the receivers. There's an AWS provided component, a secure tunneling component, that makes it easy to implement that on your devices. Catapult is using that on their docks and receivers, and it's included in their Greengrass deployments. When a customer support engineer needs a remote session with a dock or a receiver, they go to the AWS console and create a tunnel.

[![Thumbnail 2470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2470.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2470)

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2480.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2480)

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2500.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2500)

 A notification with a secure access token is sent, and the secure tunneling component creates a tunnel  between the cloud and the SSH daemon running on the dock or the receiver. Then the customer support engineer has an SSH session in place. Previously, they had to have the customer involved, and TeamViewer was involved.  There's no customer appointment anymore, and there's no burden on the customer anymore. They just need to consent to Catapult accessing the devices.

Catapult has easy access, but also secure tunneling doesn't require any inbound open ports. It makes a connection using port 443 outbound and ephemeral inbound ports, and this also works behind firewalls. All of this together, it's good for Catapult, and it's good for their customers. Lastly, Log Manager. Log Manager is another AWS provided component. Previously, if Catapult needed to get to device logs with their Vector 7, they would again be relying on TeamViewer and customer involvement.

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2560.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2560)

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2570.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2570)

Now with Log Manager, it's able to grab the log files from the custom components and also from Greengrass, and publish those to Amazon CloudWatch. Customer support is able to access  those device logs at any time in CloudWatch. So it's near real-time logs in the cloud all the time, always accessible to Catapult.  Again, no customer appointment is needed, and there's no burden on the customer anymore when Catapult needs device logs. That's good for Catapult, and that's good for their customers.

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2590.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2590)

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2610.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2610)

 One last slide on future outlook. We've covered a lot of benefits and a lot of functionality that Catapult is getting now by having their devices being cloud connected and being Internet of Things devices, but there are maybe some other things that they can do as well. You saw earlier that the 10 hertz live data streams only from the device  to the iOS apps that are on the sideline, but something that Catapult is interested in doing is streaming that data live to the cloud. That can be used for in-game analytics during matches and during practice sessions in the cloud, but it would also enable remote access for coaches and sports scientists who are not on the sidelines.

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2640.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2640)

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2670.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2670)

With what Mike showed you earlier, the ML inference was happening on the post-processing after games and after practice sessions and happening in the cloud.  Catapult does have a small amount of ML inference happening on the device, but not a great deal, and they're interested in perhaps shifting more of that machine learning inference to the edge on either the receiver or even the tags, so that these insights can be performed during games and during practice sessions. Part of why they're interested in that now  is the OTA update mechanism makes this much more feasible. Previously with Vector 7, with the inference models on the edge on the device, Catapult didn't really have a good ability to update those, to iterate them, to enhance them, and to innovate over time.

[![Thumbnail 2710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2710.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2710)

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dcc8ec2df02d4bdc/2720.jpg)](https://www.youtube.com/watch?v=Plw5F5LKNnc&t=2720)

Now because they have OTA update, they're able to consider deploying more models to the edge, because they will be able to manage them and iterate them and enhance them. So they can have a true ML Ops pipeline deploying all the way to the device. They're looking also into AI analysis  of team-wide tag data in regards to team movements, players moving as a group, and team tactics. And also AI for natural language  search of video, for example, models like multi-modal embeddings models, which are powerful for that use case.

That's it, we've arrived at the end. Mike and I thank you very much for your attendance and attention. We'd like to please ask you to give us your feedback in the app. We'd really appreciate it if you could take the time to do that, and if you'd like to connect with us on LinkedIn, we would be more than happy to do so.


----

; This article is entirely auto-generated using Amazon Bedrock.
