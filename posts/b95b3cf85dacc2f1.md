---
title: 'AWS re:Invent 2025 - Using generative AI to accelerate database modernization with AWS DMS (DAT420)'
published: true
description: 'In this video, Mike Revitt demonstrates AWS Database Migration Service Schema Conversion and AWS Transform for migrating databases and applications. He showcases two major announcements: Sybase database conversion using generative AI with Amazon Bedrock, and full-stack .NET/SQL Server to PostgreSQL migration using agentic AI. The session includes live demonstrations of converting Sybase stored procedures and views that failed deterministic conversion, showing how generative AI successfully converts complex financial code with 66% initial conversion rate. He also demonstrates AWS Transform''s automated workflow that creates Schema Conversion projects, DMS endpoints, and modifies .NET application code in GitHub repositories, including adding PostgreSQL frameworks and connection strings. Key emphasis is placed on testing AI-generated code despite successful conversions, and using infrastructure-as-code practices with CloudFormation templates for repeatable deployments.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/0.jpg'
series: ''
canonical_url: null
id: 3088971
date: '2025-12-06T13:15:45Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Using generative AI to accelerate database modernization with AWS DMS (DAT420)**

> In this video, Mike Revitt demonstrates AWS Database Migration Service Schema Conversion and AWS Transform for migrating databases and applications. He showcases two major announcements: Sybase database conversion using generative AI with Amazon Bedrock, and full-stack .NET/SQL Server to PostgreSQL migration using agentic AI. The session includes live demonstrations of converting Sybase stored procedures and views that failed deterministic conversion, showing how generative AI successfully converts complex financial code with 66% initial conversion rate. He also demonstrates AWS Transform's automated workflow that creates Schema Conversion projects, DMS endpoints, and modifies .NET application code in GitHub repositories, including adding PostgreSQL frameworks and connection strings. Key emphasis is placed on testing AI-generated code despite successful conversions, and using infrastructure-as-code practices with CloudFormation templates for repeatable deployments.

{% youtube https://www.youtube.com/watch?v=sogq2AHaqJc %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/0.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=0)

### Introduction: Database Migration with AI-Powered Schema Conversion and AWS Transform

 OK, welcome, everybody. Hopefully you're all here to look at database schema conversion or DMS Schema Conversion. I'm also going to be covering the new Transform announcement that was made on Monday, if any of you saw that. So, it's all about new announcements. For those of you that saw my session last year, the flow's the same, but the content's completely different. It's all the stuff we've done this year.

So I'm Mike Revitt. I'm a database migration specialist. Actually, I work in the product team now, but I'm not a database migration specialist anymore. I've moved teams. I'm now in the product team, controlling how the Schema Conversion is integrated with AI. I'm joined by Vlad. Hi, I'm Vlad. I'm the tech leader for databases here. I'm here really to help him out, so I'm going to be running around with the mics. If you have any questions, raise your hands and I'll come to you.

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/50.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=50)

 Well, yeah, and please ask questions. It is level 400. I'm assuming that you're all reasonably comfortable with database terminology, so ask questions. Code talks are unusual. They're interactive. We encourage you to ask questions. As I'm going through the demos today, and I will be live coding, by the way, I've only got about four slides. The rest of it I'm going to be coding the entire time. There's three or four things I've got to show you. The rest of it, if we don't, we don't. Questions are more important. If there's things you want to know, please ask. Vlad's going to be running around with the mic and be able to take the questions. I'll be looking at my monitor, so I'm looking at all these bright lights. I can hardly see anybody anyway.

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/90.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=90)

 So, I've pretty much gone through what we're going to cover. I believe these slides are available for handouts. You can go back through these, but as I said, Sybase is a big one. Anybody using Sybase databases in the audience? There's a few of you. Good, good, yeah. So I don't need to tell you that it's quite a hot topic at the moment. We are about a week ago, Friday, just before the event, we launched Sybase with generative AI in Schema Conversion. So that's our big announcement this year, and I'm going to be showing you that, how that works. And the other announcement, as I said, was with Transform, and that's using the entire stack using Agentic AI converting .NET and SQL Server to PostgreSQL. So I'm going to be showing you that as well.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/140.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=140)

### Understanding Schema Conversion Reports and the Role of Deterministic vs. Probabilistic Engines

There we go, so I typically start with this  slide, and the reason I start with that, this is the database we're going to be going through, by the way, the Sybase database. But the reason I show this is more informational. Whenever you use Schema Conversion to do an analysis of your database, you get a report and it typically looks like this. The one that says 100%, that's your data objects, those are your tables. If it's not 100%, it's 99%. It's very rare that it's anything other than that. Your code objects, those are your challenging ones. This is where your business logic resides. And when you see something like this, 66%, does it say? Yeah, when you see something like this, your immediate reaction is, well, that's going to be too difficult, so I need to not do it.

There's a couple of things you need to understand with that. One, and we're looking to move away from this, by the way, percentage doesn't actually tell you anything. 66% of a database that's got 100,000 lines of code, that's what, 300 lines of code that have got to be converted. That's no big deal. But 95% of a database with a million lines of code, that's a big deal. So the percentage doesn't give you the whole story, so you need to dive deeper.

And the other thing about Schema Conversion is if it doesn't know what to do, it typically throws a 9997 or a 9996, a 999 something, and that basically says I couldn't work it out, and therefore it assumes it must be really difficult. So you get a report that says, I found 100 things and they're all really hard. One of the most common causes of that is dynamic SQL. It doesn't know how to interpret dynamic SQL because all it sees is a string. It doesn't know what's in it. When you actually analyze the code, more often than not, it's a straight conversion. It's actually really straightforward. It's not difficult, even though the report gives you this scary number. So all I say is, that's not a reason not to do it. Dive in, find out what's going on, find out what's really sitting behind it.

[![Thumbnail 250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/250.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=250)

 I think this, oh no, I've got 22 slides to go, a couple of marketing slides, apologies. Schema Conversion. So this is the product that we use. It connects to a source, connects to a target. For generative AI, the source databases as of today are Sybase, SQL Server, and Oracle. Your target is PostgreSQL, so those are your generative AI options at the moment. The source and target can be anywhere where you can get an IP address, a connection to using JDBC. So EC2, on-premise, it doesn't really matter as long as Schema Conversion can see it.

Within the engine, we've got deterministic rules. They've been there for 15 plus years. They're pretty established now. Even Sybase was in there, so did anyone use Schema Conversion Tool? Which, yeah, a few of you, that's good. So Schema Conversion Tool is a product we created 15 odd years ago to convert databases.

And then four years ago we turned it into the service Schema Conversion, which we did so we could add things to it like generative AI. Sybase has been in that code stack for almost since it launched, I think about 14 or 13 years ago. We've had Sybase deterministic rules in the engine. So adding Sybase into Schema Conversion, actually we took a very well-established code stack and put it into our service, so we didn't have to create new stuff here. So it's a pretty solid conversion rule set that's in there, the deterministic rules.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/350.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=350)

The new bit that we've added, as I said, is the probabilistic engine using Bedrock. So the way it works is if the deterministic conversion doesn't work, we then give it to Bedrock to see if we can use probabilistic rules to do the conversion. And that's how it works, it's as simple as that. I went into that in a lot more detail last  year. I want to get into the coding this year.

### AWS Transform: Full-Stack Migration from .NET and SQL Server to PostgreSQL

The other one that I'm going to go through is the Transform announcement, the one that we made on Monday, and this is a full-stack conversion. Now, I'd love to show you the entire thing end to end, but unfortunately it takes an hour and a half, so I can't do that, so apologies for that. But actually if I step back, I'm actually not really apologizing for that if I think about it, because I'm doing a full-stack conversion of .NET and a SQL Server database from SQL Server to PostgreSQL, and I can do that entire process in an hour and a half. That's actually quite impressive. My problem is I've only got 60 minutes, so I can't show you the whole thing.

Now, I did run it last night. I've run it end to end last night. So what I'm going to do, and I'm going to have to bounce around a little bit because it's an agentic AI solution. And so you kick a job off and we can sit there and watch nothing happen for five or six minutes, or I can show you something else. So I'm going to be bouncing around a little bit just to show you what's going on, so we're not watching the paint dry. And so what I'm going to be showing you is we'll go through the agentic model, show you how it all works, and then when it's running, I will show you some of the things that it created last night, so I can show you what it's actually doing.

There's one job we're going to get to which runs for 20 minutes. When we get to that point, I'm going to switch in and show you the Sybase stuff. And then if I time it right, when we come back, we'll be at the next step in the agentic model. So that's what we're going to be doing. And I think that's it for my slides. Oh no, that's me.

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/450.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=450)

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/460.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=460)

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/470.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=470)

All you need to know about me. I've been doing this a long time. As people that work with me like to point out,  I started migrating databases before they were born, so that either makes me really experienced or really old. I prefer the former. I'll let you decide. But yeah, that's me.  Right, from that, we're going to do some coding. Yep, and we're on the right slide. I prefer to stand than sit down,  but I think I'm going to have to sit at some points to type, but I'll be up and down, but I can't see you if I'm sitting down.

### Live Demo Setup: Bob's Used Books Application Architecture and Prerequisites

So we're going to start with the agentic model, the Transform model. So what we're going to do, this application is freely available on the Amazon website. I've got some links at the end of the slide deck because I'm pretty certain they're downloadable, but if they're not, if you search for Bob's Used Books, it's a little .NET application. We use it for testing and training. All I've done is I've taken, so when you download it and you install it, it installs everything into one host. All I've done is move the SQL Server element out into an external SQL Server Enterprise database just so I can demonstrate it.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/520.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=520)

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/530.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=530)

So this is the application. I'll just very quickly show you what it does. If I log on, it should show you some stuff about me. There we go, I'm logged on.  So it's a running application. It's got pages. I created an order which I canceled. I can shop for some books. It's not a huge database, I'll be honest, but  it's got all the processes in it, and it gives me something that I can demo. I'm not going to bother ordering anything, it won't turn up anyway, it's only running on my laptop.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/550.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=550)

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/560.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=560)

Right, so let's go in and we'll start in Transform. Now, I've done this a few times and I've got so much to show you. If I don't go to my cheat sheet, we're going to get lost.  So we're going to start in the Transform. So this is the Transform engine. So I've created a workspace, I've pre-created the workspace and I've done that so I can create all the engines in it.  And within there I've got this one job that I ran yesterday, which took this end to end, that's successfully completed.

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/580.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=580)

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/590.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=590)

So the first thing I do is going to create a new job. And so what it's now doing is it's having a look to see what things are available to us, what things it can offer me that I can create. And so here's the list.  So there's a number in there, mainframe, et cetera, but the one we're interested in is Windows modernization. So if I click on Windows modernization, it now gives me two choices. You've got .NET modernization,  and what that does is it will modernize the .NET application into modern architectures. And you've got SQL Server modernization, which actually does a .NET modernization as well.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/620.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=620)

The distinction is that SQL Server modernization converts the SQL Server database into PostgreSQL. In fact, I'll click that so it's doing something while I'm talking. It'll convert the SQL Server database into a PostgreSQL database, and then it will modify the .NET application and make all of the  SQL Server code changes necessary to make it run against a PostgreSQL database. That's the bit I'm going to show you at the end.

Unfortunately, I have to wait for the Agentic AI to catch up with where I'm at. At the moment, it's looking at all the things that it's got to create, all of the privileges it needs, all of the access it needs in order to complete a SQL Server modernization program. One of the things it's got to do is this is running in a separate service available to me. It's not part of my AWS application, so it doesn't have privileges to connect to any of the things in my account. One of the first things it's going to come up and say, in fact the first thing it's going to come up and say, is it's going to give me a list of prerequisites, things that you have to do.

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/680.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=680)

One of the things we discovered, so this has obviously been through beta, we've had it out with pilots with lots of people, and  one of the things, oh it's come up already, one of the things that it's going to do is it's going to say you need to have done all these things before we start, or I'm going to fail. Now, we're all engineers. Once upon a time I was an engineer as well, and as an engineer, reading the manual is an admission of failure, so none of us read the manuals. I'm telling you, read the manual because if you don't, it's going to fail. We've got a whole list of things up here that you're supposed to do. Actually, I'll say I never read the manual. Anyone use K&R? Does that mean anything to anybody, or are you all too young? K&R is the C Bible. It's about that thick. It was written by the guys who wrote C initially. I used to live with that on my desk, but anyway.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/730.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=730)

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/750.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=750)

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/770.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=770)

Let's see what it's gone and done. So it's come up and it's said, well, I'll  explain this so you can see it. It said you need to do these things before I can start. I'm not going to succeed if you haven't done these things. This is all database stuff really. It's saying you've got to have a user in your database with permissions to read it. I've got to analyze your database. I've got to be able to connect to it. The other thing it's saying when we come down here is the credentials, so your database  credentials have to be put into an AWS secret so Transform knows how to log onto the database. But you've also got to put some tags on that secret so Transform knows how to find it. Otherwise, I mean, most accounts have got loads of secrets in it. Which one do you go to? So you've got to have a secret in there, and this is what your secret's got to look like. I'm not going to show you my secret because it's got my login credentials in it, but  this is the contents of your secret: username, password, what type of engine it is. The engine's actually required to tell Schema Conversion what it's doing, not for Transform. Hostname, IP address. Oh, here's an interesting one. During our early testing, we don't know why, but if you put a domain name in there, it may fail, not all the time, but it may fail. We're investigating that at the moment, so if you put an IP address in there, it works every time. It's something going on in Schema Conversion we haven't got to the bottom of yet. So if you're testing this for now, use an IP address. Domain name will be fixed because IP addresses are bad. Hopefully I don't have to tell any of you that.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/820.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=820)

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/830.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=830)

### Configuring Transform: Database Connectors and Authorization Process

So that's the database side of it, and then the other thing you've got to do is you've got to tell Transform how to connect to your GitHub repository, and so you've got  to have created the necessary stuff inside your account to connect to your GitHub repository where your .NET code lives. And I've really got to  stop talking while I'm doing this. I should click go so that it's actually doing something whilst I'm talking. Anyway, so we're now clicking off and now, so what it's done is I've confirmed that I've done all those prerequisites, and it's now going off and finding out if it can actually see everything. It's told me what it needs, and now it's going to say, can I get to the stuff you've told me you've already done? So it's going to try and get to the secret. It's going to try and get to various other things. What it's going to come up and say at some point, now it's still running at the moment, catches me out every time. You've got a yellow box up there saying I'm waiting for customer input, and to the right, I don't know if you can see it in the room, to the right of it, it says it's running. It's actually working. It's not waiting for customer input. It's just telling me it is just to confuse me.

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/880.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=880)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/900.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=900)

So what it's doing is it's trying to find out if it can connect to the things it needs, and it'll come back in a minute. What it's going to do is it's going to,  in fact, has it come back? Yes, it has. It's going to come back and it's going to say I need some privileges. So the first thing it says is I need a connector that knows how to connect to your database. Now I can't say create a new one because I've already got one. It knows there's already one there, so all I can do is choose the one I've already created. 

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/910.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=910)

I've had to do that because to connect to it, I've got to put all my connection details in there, and again I'm not allowed to do that. That's a bad thing  to do, to put up on a screen all my connection details to my Amazon account. Security would not be happy. So the next thing it's going to do is it's going to say, okay, so I know how to get to your database. How do I get to your application code?

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/930.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=930)

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/950.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=950)

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/960.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=960)

So the same thing, I've got to say I've already created a connector, and this connector gives you the privilege to talk to my account. So  when I say choose the connector, you see this. Does my mouse show up on there? We've got this approved, so what this means, so what is actually happening in the background, and I'll kick that off as well so we're not waiting. So what's happening in the background is I create a request against my Amazon account, and that request is saying my Transform  session wants the privilege to connect to resources inside your account. And so what happens inside my account is I get  a notification request to say this thing is trying to access your account. Do you allow that or not?

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/990.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=990)

When you say yes, what you've now done is you've given Transform permission to run those things in your account, and it creates some IAM roles and things like that that allow that to set it all up. So that's what I wasn't able to show you, but that's what's going on when I was doing all those authorizations. And so now what it's done is it knows how to connect to the database, it knows how to connect to my GitHub repository, so it's going to give that a go. And I'm just going to see if this is one of those eight-minute  jobs or not, because I've got a feeling it is. Oops, that's down on this one. So, yep, we've done that, right, so let's go back into here.

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1010.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1010)

### Infrastructure as Code and AWS Resource Configuration for Migration

So what I'm going to show you. One thing I always do, and this is more an educational thing than anything else, I am and have been for a very long time,  a huge advocate of infrastructure through code. If you're not currently doing this, I really, really strongly encourage you to learn how to do this. Everything you're looking at that you're running today, that I'm running today, is built through CloudFormation templates, every single thing. So when I finish doing a demo like this, I just delete my stack, it deletes everything, I build a stack, and it's back to exactly where this started. I cannot encourage you enough that you really should be doing that if you're not already. Hopefully you all are.

[![Thumbnail 1040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1040.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1040)

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1050.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1050)

So the  first thing I'm going to show you is the architecture we're running here today. So, as I said, Bob's Used Books. Is that big enough for you? Do you need me to make it bigger, by  the way? All good? Perfect. So we've got three EC2 hosts, I've got my bastion host. I use my bastion host because that lets me RDP into my hidden, into my SQL Server host. So one of the questions I get asked a lot is, how do I connect to my RDS databases? They're hidden on a private network. You know, I've got no access to it from the external environment, as it should be, by the way, you shouldn't have access to your databases.

But what we do is we create what's known as a bastion host and then using SSH tunneling, I can use all of my desktop tools, so DBeaver or whatever your tool of choice is to connect to your RDS databases. You can also use it to RDP into your SQL Server hosts, which is what I'm doing here. So my bastion host has only got SSH access. But through that, I can open up an RDP session to my SQL Server host to debug it. So that's what I create that for. And then we've got one that's called my Bob's Books application host, you can probably guess what's running on there. And I've got the SQL Server host. So that's what's running. It's a pretty small environment and that's what we're connecting to.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1120.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1120)

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1140.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1140)

The other thing that I wanted to show you in there is the secret, there we go. Where's the secret?  So, one of the prerequisites was you've got to create a secret that tells Transform how to log onto your database. So here's my secret, Bob's Books SQL Server, and the IP address. I actually build this up dynamically in my templates, so it actually is the name of the database at the end of it. And so  this is what it is. I'm not going to show you the contents, but I'm going to show you the tags.

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1150.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1150)

So remember I said we've got to have tags on the secret so that the Transform knows how to connect to it.  So there's the two tags, Project and the name. It is case sensitive, so make sure you're entering it like that, and the Owner as database-connector. So that allows Transform to connect to the secret, and the secret tells it how to connect to the database. It doesn't know anything else about the database.

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1170.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1170)

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1200.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1200)

The other thing that you've got to create is this one. So this is  in the Developer Tools, and this is the bridge between my Amazon account and my GitHub repository. So when Transform wants to modify my .NET code, it's going to use this link to get to my GitHub repository to make all the changes in there. And then what it will do at the end, it will ask me to create a new branch, is actually what it's going to do. So that's everything that it's doing at the moment. It's connecting to my secret and to this connector to ensure that it can connect to the things it's got to convert. And then  let's see where it transforms up to. This is where I don't, yeah, it's still running, right.

[![Thumbnail 1210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1210.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1210)

[![Thumbnail 1220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1220.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1220)

### Behind the Scenes: Schema Conversion Projects and Database Migration Service Tasks

So the other thing that I'm going to show you then, whilst we wait for that to finish doing something,  is I'm going to go in and show you some of the stuff it created last night. So one of the things, the first thing it's going to create is  it's going to go into the console, into the Database Migration Service. Actually I'll go back to the start so we know where we are.

[![Thumbnail 1250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1250.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1250)

So Database Migration Service is where all of the database conversion tools live. Schema Conversion, which converts the structure of your database, and Database Migration Service moves the data between databases. So the first thing that the transformer is going to do, once it's finished ensuring it's got connectivity, and I'm going to keep an eye on that because I want to kick that off so it starts doing the work,  it's going to create a profile, and what the profile does is it tells Schema Conversion what networks it can run on, what privileges it needs, and everything it needs necessary to connect to a database.

[![Thumbnail 1270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1270.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1270)

[![Thumbnail 1300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1300.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1300)

It's then going to create two data providers. It's going to create a data provider which connects to your source, and as I said, this is that bug that I've told you about,  use the IP address for now. For some reason that only affects the source, not the destination. So for my PostgreSQL database, I'm able to use the DNS name. And in fact, it would appear that this has already created it because something that it's just changed. So it actually looks like it's found the one I created last night actually, because I can see that it's changed the virtual mode to on. So let's have a quick look in there and see what's going on.  No, that one's not a virtual mode, that's bizarre.

[![Thumbnail 1330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1330.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1330)

So anyway, let's go back to where I was. So what it's going to do is it's going to create two data providers, one of them to connect to the SQL Server database, and the first one, when it does its analysis, it's going to use another new feature which we launched about two months ago now, where you can use a virtual target, so you don't actually have to have a PostgreSQL database in order to do your analysis. So it's going to create one of those and then it's going to create a project which connects to the two and it's going to do the analysis on the database to work out how hard it is to convert and  you get a report like the one I showed you at the beginning.

[![Thumbnail 1340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1340.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1340)

[![Thumbnail 1350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1350.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1350)

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1370.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1370)

Let's see where we're up to. We're still running, okay. So, oops, not that button.  So once it's done that, in fact, let's show I can show you what's going on inside there then, because we've got time if I'll get back into the right window. So this is what a migration project looks like, so we can see all the  connectivity details. When you go into Schema Conversion, we can now see the source database on the left, this is my SQL Server database, and here we go, Bob's Books, oops, and the only schema within there is DBO.  Eventually in its own time, there we go, and it's going to be doing some analysis on that, working out how difficult that is to convert.

[![Thumbnail 1380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1380.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1380)

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1410.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1410)

 It hasn't run its report yet because if it had, I'd be able to see it in the bottom, and it's going to say, well, guess what, one hundred percent, because this one's really easy to convert. So it's doing all that in the background. Now, I don't know how many of you have actually tried to use Schema Conversion. I think a few of you put your hands up earlier. It's, let's say it's got some nuances. It's not the easiest thing in the world to set up. Transform's doing it all for you. It's nothing you have to worry about. Normally you'd kick, you know, a couple of Transform jobs off and it would be running,  and I really wish it would finish running so I could go on to the next part of the demo, but anyway, I've got no control over the speed the agentic AI is running at, so. So that's the Schema Conversion part that it's running.

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1420.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1420)

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1430.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1430)

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1440.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1440)

 Now, the other thing that Transform is going to do if I go back into DMS is it doesn't just convert the schema, it also converts the  data as well. And the way that it does that is again, it creates two endpoints. These are definitely the endpoints I created yesterday because I recognize the names.  So again, you've got a destination, which is your PostgreSQL database. Now we don't have the same restriction on names. You don't have to use IP addresses, but Transform's picked up all its information from the secret. So because it picked up the IP address and the secret, that's what it's used. And again, we've got the same target.

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1470.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1470)

[![Thumbnail 1480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1480.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1480)

And then the other thing it does is it creates a task. And so the task moves the data between SQL Server and PostgreSQL, and as we can see here,  it has, if I go into the right tab, it has moved half a dozen tables. It's not the world's most complex application, but that's not the point of the demo. The demo is not  to show you I'm moving something complicated, it's to show you what it can actually do. So it's something that I want to succeed, let's be honest. But there is some data in there and it moves all across, and when you launch this in PostgreSQL, all your data will be there. And unfortunately I don't have time to deploy all that, so you're going to have to take my word for that one, I'm afraid.

### Transform Assessment: Analyzing Applications and Databases for Migration Complexity

But that's the other thing that Transform is creating in the background and again you don't have to do it. You can create as many of these jobs off at the same time as you can, so we can use this to migrate

[![Thumbnail 1520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1520.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1520)

[![Thumbnail 1530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1530.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1530)

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1540.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1540)

100 SQL Server databases, for instance, instead of having to do them one at a time. Right, finally, it's asking me for something to do, cool.  Right, so it's gone through and it's found everything, so let's bring this one up so we can see it. So it's found my database. It knows it's running on EC2 and it says this is the database  I found, do you want me to do anything with it? And it's also found the repository, and it's found that there's a branch in the repository. There's the main branch, which is the one I wanted to  convert, by the way, that's the SQL Server branch, and then I've got this reinvent 2025. This is the one I created yesterday. We're going to look at that later on, by the way. So this is the converted PostgreSQL version of my application. But I'm going to convert it into mine because it's not going to do anything if I choose the PostgreSQL version.

[![Thumbnail 1560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1560.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1560)

So once I click start assessment,  it's now going to connect to the repository, do analysis to find out how difficult that's going to be to convert to PostgreSQL. It's going to do the same with the database, run an assessment report and find out how difficult that is. Now, when it's doing the connection, it is quite possible that when it went to the SQL Server instance, it may well have found several databases, and in fact, the first time that I ran this through as a demo test, it found every single database in my SQL Server estate. And then what happens if you had a GitHub repository with multiple applications in it, you get a report for everything, and then you get to choose which ones you want it to convert. For simplicity's sake, I've stripped it down to one, because again, I want the thing to work.

So it's found the one database, the one application, and it's now going and running a report to see how difficult that will be to convert into PostgreSQL for me. So that's what it's doing, but it can do multiple applications, as I said, it will find every single database in your instance when it comes up. And what it's going to do now is based on complexity, it's going to put those into migration waves. It also looks at interoperability, so it looks at the dependency between your application code and your database, do they talk to one another? Do I have to put them into the same wave, or can I create waves based on complexity? So all that's going on in the background, and I hope we get to that point when we, when it's finished running.

[![Thumbnail 1660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1660.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1660)

[![Thumbnail 1670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1670.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1670)

### Sybase to PostgreSQL: Generative AI-Powered Schema Conversion in Action

Well, this is going to run for about 20 minutes now. So what I'm going to do now is I'm going to take you into the Sybase part of it. So we'll just double-check that that is indeed the one that's going through. We've done all that, we've done all that. Yes, so we go and we're now into  the assessment, so we've done all that. Let's go into, right, so.  I suspect you're all well versed in AWS at this stage, but I will point this out because it is one of the gotchas that if you've not been into the console before, this again is the cause of quite a few support calls.

[![Thumbnail 1690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1690.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1690)

[![Thumbnail 1700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1700.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1700)

I'm running out of North Virginia, US East One for all of my Transform stuff. My Sybase code is all running out of Oregon.  So one of the, I've been worried for a minute. All of my, so my  Oregon account has got a completely different set of project codes, and so we sometimes get it that when people go into the console and they go into their EC2 list or the database list, there's nothing there, and then there's a big panic. Where's all my, where's all my stuff gone? Make sure you're in the right region. So anyway, so I've gone into this region. I've got three projects in here. I'm only going to show you the Sybase one because I haven't got time to show the others, but these are the three database pairs I talked about earlier.

[![Thumbnail 1740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1740.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1740)

Do I need to make that bigger? It looks a bit small on mine. Is that big enough for everyone? It's all okay, good. So these are the three database pairs that we currently support for Generative AI. So we're going to Sybase. Again, you have a secret  which has got all the logon credentials. This is how DMS Schema Conversion knows to connect to your database. You have to create a role, and the role gives DMS Schema Conversion the permissions to read the secret, same for the target.

[![Thumbnail 1760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1760.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1760)

And then the other thing which is in here, so I'd say the DMS Schema Conversion bit. The other thing that's really important is you have to create an S3  bucket. I do know that this causes customers or organizations some challenges because of demarcation of roles. But these are all prerequisites for DMS Schema Conversion. Transform doesn't have the problem, by the way. Transform does all this in the background, all of the stuff you've got to pre-create, other than the secret I mentioned at the beginning. But when you're doing DMS Schema Conversion yourself, so when you're doing it for Sybase, you've got to create all of this.

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1790.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1790)

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1800.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1800)

So if we go into the console. On the left-hand side, this is the, the slide we started  with. We've got my demo database. My reports 100% on my data objects, 66% on my code objects. 

So the first thing I'm going to do, I've also, so again, this was all built with a CloudFormation template. I rebuilt it all last night to make sure it was all good to go today. As part of my build process, I've used the API calls into schema conversion to convert all of the tables. They're 100% anyway, and I've also moved all of the data because that clock is ticking alarmingly fast. So I want to get as far as I can to show you the important bits. DMS works, I don't need to show you that.

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1840.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1840)

[![Thumbnail 1850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1850.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1850)

So first thing I'm going to do is I'm going to go into, where's that window? I'm going to go in, I'm going to log on to my  Sybase database first, and we're just going to make sure all of the data's there. So that's, oops, it's in this one. Rather than you watch me make a load of typos, I'm  going to cut and paste it, so I've just connected to my Sybase database. Now what's interesting about my Sybase database is this isn't even in my account, it's not even in this region. I have connected to a Sybase database that's in somebody else's account in another region completely hidden. This is using, as I said earlier on, I'm using an SSH tunnel, which I've got in another window open up to allow me to connect to this database so you can still use all of your desktop tools to connect to these hidden databases.

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1900.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1900)

[![Thumbnail 1910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1910.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1910)

So again, it's not a huge database, but it's got a number of tables in it, some data in there. And if we have a look at the PostgreSQL database, I'm hoping, so this is the Aurora PostgreSQL database, it's got the same data in it,  which I'm really glad about because DMS copied it across and I'd be really annoyed if it was different. So we've got two databases with the same tables and the same data. Now the challenge you get is converting your business logic. It's the business logic  that causes the problem.

So the first thing I'm going to do, I'm going to look at a view, and I'm going to, from in here it tells me which view I'm looking at, the portfolio summary, right. So all of the code in this database is financial code. But Sybase, for those of you who aren't familiar with Sybase databases, it's really, really prevalent in the finance sector, especially in Europe. There's an awful lot of Sybase there. It's typically very old. It typically began life in either IBM or Sun as it was at the time, so it's all Unix-based typically, or it isn't anymore because they've ported it, but it's all typically old code. Customers I've spoken to, there's nobody left in the company who's written it. This is code that's been there 20, 30 years, so it's a mystery.

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/1970.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=1970)

In fact, I was chatting to a customer earlier on today who've got a number of Sybase databases that they've got to do something with, and the challenge they've got is, as I've just said, they don't know anything  about their code. Now, I was with a customer down in Spain earlier on this year, who exactly the same position, ton of Sybase financial code. This was a, I think this was a trading house I was with. And nobody in the company knew anything about it. So I can't remember how our conversation got around to it, but in the meeting I was at with the CTO and his chief architects, I used what at the time we called Q Developer. We've just rebranded it as Quiro. You must have seen all the stuff about Quiro now. It is really cool.

And I pointed Quiro at the directory where their source code was and said, what can you tell me about it? And what it told the CTO, it told them which company they bought it from, when they bought it, all the changes they'd made to it, all of the security patches they'd put in it. It gave them all this information about what the application did, what it was for, where it came from. Not one person in the room had a clue about any of that. The next thing I said is, so what other things does it talk to? What objects in the database does it use? So it again scanned the directory and then it gave me a report and said these are all the database objects it talks to, functions, procedures, tables, and this is how all the functions interact with one another. So it gave me all the interoperability points, and it did all that in about three minutes.

So those of you that are worried about or wondering how am I going to modernize my Sybase databases, the tools exist. Just use them, you'll be amazed at what it's doing. What we've done with schema conversion is turn it into a service for you, specifically for the database part of it. Anyway, I digress. I've forgotten the name of the view I was looking for now. I am looking for the view called portfolio summary.

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2080.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2080)

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2100.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2100)

### Converting Sybase Views and Stored Procedures Using Generative AI

So portfolio summary, there it is. So portfolio summary, it failed to convert. I've got a 3014, I've got a critical error.  So schema conversion using deterministic rules has said, I don't know how to convert this piece of code. So think hours, days of coding effort. So what you can do now is we've got two options. We can go through the convert button here or I can use convert using generative AI. They both go to exactly the same place.  I'm not entirely sure why we put an extra button on there. Comfort I guess. But what it does is it opens this pop-up window. The convert will do the same by the way. And we've got this toggle switch, and I'll kick that off running.

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2120.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2120)

What the toggle switch does is it says, once you finish your deterministic conversion, actually I use these terms all the time. This was pointed out  to me when I did my walkthrough. Do people know what I mean when I say probabilistic and deterministic? Yes, no? Yes, all familiar with that? Good. I use them because I know what they mean, and it's one of these things where you say things, but anyway, so it goes through and it does the deterministic rules, and if it can't convert it, and it hasn't done in this case, what it does is it packages up the context of the SQL and it sends it to our Amazon Bedrock engine where the large language model has a look at it and it understands the context of what's being done, understands what we're trying to do, and it will come up with a PostgreSQL alternative which will achieve the same outcome.

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2170.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2170)

Now when it does the conversion, it doesn't actually mark it as being fixed. So when you do a deterministic conversion, what  that means is I can determine what the correct outcome is. So in the case of, apologies, my background is Oracle, so I have to revert to Oracle syntax. But in the case of Oracle, if I'm doing, it says if this value is null, use this one, standard function. In PostgreSQL, the command for that is called coalesce. So coalesce, if this one's null, use that one. I know that if I replace NVL with coalesce, it's guaranteed to work. I can mark that as fixed. That's an example of a deterministic rule.

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2210.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2210)

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2220.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2220)

A probabilistic rule, and you can just see now that it is completed, it said I have successfully completed this conversion, 100% success rate, but it hasn't marked it as being successfully converted. It should, let me see if I can show you this,  if I scroll up. Action items, there we go. I don't know if you can read that, but it says this conversion  uses machine learning, that generates predictions based on patterns. Basically, I can't say this is fixed. I think it's fixed. This is why it's a probabilistic, it's probably right, but we don't know. You really do need to test this. I've got a session coming up, my next session. I'm actually going to go into this in a bit more detail, but I haven't got time here. But yeah, you really need to look at it, test it, it's probably right.

[![Thumbnail 2260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2260.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2260)

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2270.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2270)

[![Thumbnail 2280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2280.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2280)

Anyway, so we've deployed that. So one of the things you can now do, and I say this every time I do it, I'm not a fan of this feature, but apparently management love it. I can go into this now and I can say apply that to my PostgreSQL database.  Actually, before I do that though, what I'm going to do is I'm going to run it, so I'm going to execute the view in my Sybase database. So let's do that.  So when we run it in my Sybase database, so this is a view of my portfolio, so a list of  all the stuff we've got basically.

[![Thumbnail 2290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2290.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2290)

[![Thumbnail 2300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2300.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2300)

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2310.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2310)

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2320.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2320)

Now, because I didn't convert this earlier on, I sincerely hope,  if I run this in PostgreSQL, I should get an error because it's not there. Indeed it's not. So it's saying, you know, view's not there, it didn't compile, I couldn't deploy it.  So if I go back now to my console, one of the options that's available to me is I can apply this change directly to the database  now. I've been head of architecture, I've been head of development, and I would discourage very, very strongly anybody from my team ever doing that.  Like, where's the control? How do you know you can repeat what you've just done? For my next conversion, I'll show you what you should do, but it's there. Now, I suppose the argument's there that if I'm in a development environment, I want to just rapidly deploy it if I can, and so the ability to deploy it multiple times possibly works, I don't know. Not a fan. Anyway.

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2350.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2350)

[![Thumbnail 2360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2360.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2360)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2380.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2380)

Let's go and have a look and see what that's done. So if I now go back and rerun that query against my PostgreSQL database,  we can see that it has now successfully worked. So the code that generative AI has converted has worked, and I hopefully, if we have a look,  they're coming out in the same order. So William Garcia's got 100 whatever shares, worth 9800, and if we go down here, he's got 100 of them worth 9800. I'm not going to go through every line. But it's converted all the code, it's executing correctly. So there's a conversion piece that you don't have to worry about yourselves. 

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2390.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2390)

[![Thumbnail 2400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2400.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2400)

Now, the next one I want to show you is, because that's a view. Views, views are more straightforward, and nothing's easy when it comes to conversions, by the way, but views are more straightforward. So let's have a look at a package  and the package I'm, or procedure actually, I'm going to look at is generate trade report. So let's see if we can find that one, so in our procedures.  SP generate, generate trade report.

[![Thumbnail 2410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2410.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2410)

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2420.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2420)

Let me demonstrate the sp_generate_trade_report procedure. Let me move that down. So here's the generate procedure, and there's the error 9996  that I mentioned earlier. The generate trade report has failed to convert. You can see it shows 0% conversion. I'll start the conversion process  before I begin explaining it, otherwise we'll be here forever.

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2480.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2480)

This procedure is based on financial code. One of the ongoing challenges we face as a company is obtaining representative customer code. Everyone asks for customer references, but nobody wants to be a customer reference. Everyone suggests using customer code for testing, but no one wants to provide their actual code. It's an ongoing challenge. However, we sometimes work with early adopters and other customers where we do occasionally get access to code. The code shown here isn't actual customer code, I should clarify, but it is based on real examples. It uses the types of patterns we've encountered in struggling conversions. When we've performed conversions for customers in the past and had to manually convert the code, these are the patterns we've trained our model on  to enable it to perform the conversions, because that's how AI works. You have to train the model, show it the correct way to do things, and then repeatedly practice it. All of these examples are based on that training.

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2490.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2490)

Now, the other thing you'll notice,  it's finished already which surprised me, is that deterministic conversions are pretty quick. If I do a deterministic conversion on that function, it's almost instant. But when you run it through the AI model, it slows things down dramatically. This is something you need to take into account. Don't just blindly convert your entire database using AI for everything, because unless you can kick it off overnight, it's going to take you a lot longer. Anyway, we have now converted our procedure.

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2550.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2550)

[![Thumbnail 2560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2560.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2560)

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2570.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2570)

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2580.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2580)

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2590.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2590)

What I'm going to show you now is the approach I would encourage all developers to use. We can go back into actions now, and I can select save as SQL. What this is going to do is create the SQL code and write it to the S3 bucket that sits underneath this Schema Conversion project. There we go. One thing that annoys me  is that everything you're doing appears at the back, not the top, for whatever reason. I've just clicked to create the code, but it's not the one that's on top,  it's not that one either, or that one, it's this one at the bottom. What it's done is successfully converted the code into a SQL script and it's located in this S3 bucket location.  I promise you, you want to click this button. It is sitting  in an S3 bucket. I don't know if any of you have ever looked at the S3 bucket that sits underneath Schema Conversion, but it is a  maze of files. The chances of finding the file you're looking for in there are remote. We've given you this link that says this is the file, this is where the file is, and it's in a buried directory. From here I can download it. That's downloaded, yes, that's completed.

[![Thumbnail 2610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2610.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2610)

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2620.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2620)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2630.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2630)

I'll open up that directory, which is where I'm running everything from, and I'll open up that  download. My finger doesn't reach that far, there we go. I'll drop it into there. If we now go back to  here, there's the zip file. Let me unzip it.  PostgreSQL.zip. I've now got the SQL file that Schema Conversion converted and it's sitting on my laptop. I'll test it, and when it works, I'll put it into my GitHub repository. That's what I would normally do, and then for production you deploy from there.

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2660.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2660)

If I go back now and go into my PostgreSQL command, and I run PostgreSQL.SQL, there we go. I now  deploy that into my PostgreSQL database. Again, running off of my laptop. I know I've said it a few times, but I'm running PostgreSQL on my MacBook and I'm connecting to an Aurora PostgreSQL database which is on a non-routable network hidden well inside my environment, and it works fine. I'm old school. I said at the beginning, I'm old. I work on the green screen. I'm not a GUI person. I get ribbed about that all the time. I work on the console. But if you're more comfortable in DBeaver, you can do exactly the same in DBeaver or any other GUI of your choice. Yes, we have a question. Can you open the file, that AI file, please? Can I what, sorry? Can you open the file? Oh yes, certainly.

[![Thumbnail 2730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2730.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2730)

I'll make it bigger so you've got a chance of being able to read it. Okay, so what it's done is it  drops the routine if it's there, creates a schema if it doesn't exist. I don't know why it does it in that order, because surely you, no, no, it's got to drop the routine before you can drop the, no, it creates a schema anyway. It creates a schema if it doesn't. They all exist in my environment, so those were the two that didn't happen.

[![Thumbnail 2750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2750.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2750)

[![Thumbnail 2770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2770.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2770)

And then here is the converted code. So there's the informational message.  Now one of the things that's really important in the informational message here, so this is the one that says make sure you test your program because it's probabilistically converted. And then it puts this marker in here and says this is the beginning of the code that I converted for you. So here's what it converted. I've got to be honest, I don't know exactly  what this code used to look like, so I can't tell you where the change is, but what it has done is it says this is the end of all the changes I made. So everything I changed is between those two markers. That is what it's done. And so, and then it's just a create or replace procedure. Was there anything specifically you were looking for?

[![Thumbnail 2820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2820.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2820)

[![Thumbnail 2840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2840.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2840)

What it looks like and what it has. Yeah, I want to see like it's adding the comment what is done, so we can use this one and see, okay, what happened there at least. That's a good thing. And also having a protection if it's an exist, not exist, so that I want to see in the script file also. Yeah, yeah, and the other thing you can do actually is I brought it out as a single file. What you can do, and let me show you one of the  other gotchas, features, I'll call it a feature. One of the other features in Schema Conversion, oops, that's not the one I want, I want a conversion. Where's schema, oh, there it is. So one of the other things you can do is, so let's assume I had done,  if I had, well, I've done that, so even though every one of those procedures is ticked, the assumption is because I've said deploy your code, you're going to assume that everything that's ticked is going to be exported.

[![Thumbnail 2880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2880.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2880)

So one of the features, yeah, I'll call it a feature of Schema Conversion, is it only runs from the location of your cursor and below. So the only code it exported was the code of the procedure because that's where my cursor was. Now if I had gone up to this level and I had say clicked that one, that one and that one, and I said from procedures, I want you to generate the code, what it would then do is create one file with three procedures in it.  Because it does everything that's marked below where the cursor is, and likewise, I can get one file with every single procedure in it by doing that.

Now, what we would typically do, what I'd recommend people do when they're coding is at the data object level, tables and indexes, because those are almost always one hundred percent, I would take them all out as a single file. But when you get down into the code objects, you know, the procedures and the functions, I would typically take those out as one object per file, because that's how you work on them. When you put those into code control, you typically manage them at an object level, whereas your tables, you tend to manage at a database level. But you've got that flexibility within here to take them out at the level you need to manage them.

[![Thumbnail 2940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2940.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2940)

[![Thumbnail 2950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2950.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2950)

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2960.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2960)

Yeah, okay. Thank you so much. Right, let's go back to my code and make sure it's worked. So I'll just make that smaller now so I can see it. So yeah,  so what we've done is we've converted that procedure. I'm just going to quickly have a look and see what that procedure did. Oops, I should have done that.  So this is the, this is the trade report, so it's come out and it's a report of all the trades that have occurred. I've got to be honest, the data doesn't mean a huge lot to me. I've  never been a trader, but you can see what's in there.

[![Thumbnail 2990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/2990.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=2990)

And so if we go to the PostgreSQL equivalent using the PostgreSQL function we've just converted, we should, fingers crossed, get exactly the same results. Let's pick one at random. So we go four down. Emily Davis, oh, she's using PLAY as well. The PLAY's popular. She's bought eighty of them at one thirty-five. If we go to this one, here we go, Emily Davis bought eighty of them at one thirty-five. So we've got the same data in both. So again,  we have successfully converted the code without you having to manually do it yourself. So that's that part of it.

### Transform Results: Automated Application Code Modifications in GitHub

Now, I'm watching that time clicking away, so I'm going to go back to Transform because there's one thing I really do want to show you.

[![Thumbnail 3010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3010.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3010)

One thing I really want to show you is what happens at the end, because I think that's the most important. I'm going to start with the endpoint.  So what's happened is Schema Conversion, or sorry, AWS Transform has executed schema conversion to convert the database. It's executed DMS to convert the data. Well, this one hasn't yet, but the one I ran last night has. I've already shown you the output of that. Other than showing you how to kick it all off, I think it's more important to show you what it did in the GitHub repository.

[![Thumbnail 3060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3060.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3060)

When I ran it last night, we've already kicked off the step that says I want you to do it, but it hasn't got there yet. So we're going to go into my GitHub repository, and I'm going to show you what it actually did last night. That's my GitHub repository, so we'll connect to that. I'll do that here because we've finished with Bob's Books. So if I go into Git, there we go.  So this is my GitHub repository where the application code for Bob's Books sits.

[![Thumbnail 3070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3070.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3070)

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3080.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3080)

[![Thumbnail 3090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3090.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3090)

If I go in and say create a pull request, and I want to, I'm going to have to look up how to do it. I can never remember how to do this. Click pull request, new pull, there we go. New pull, and then compare.   So I'm going to compare main, which is my SQL Server code, with the code that I converted last night. I'm going to make this, you definitely can't see that one. How's that? Is that big enough? 

[![Thumbnail 3130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3130.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3130)

So what this is now showing you is main, which is, actually I don't know which one's green and which one's red, but anyway, you can see all the changes that have been made by AWS Transform. So if I do a search for Postgres, you can see, so red is the new stuff. You can see what we've put in here. For example, we have added the Postgres framework that's been put in there by Transform. So somewhere there's going to be a corresponding removal of the SQL Server one, I suspect. 

[![Thumbnail 3140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3140.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3140)

[![Thumbnail 3150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3150.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3150)

If we go down, where's my, where's next? So the problem you get when you make your window bigger is  you lose all your buttons, you can't find them anymore. Somewhere in here is the next button. Oh, there it is, gone to the bottom. Right, so there's the Postgres framework again.  Another one for Postgres. What I'm looking for is the connection details, which I think is about three or four clicks down.

[![Thumbnail 3160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3160.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3160)

[![Thumbnail 3180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3180.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3180)

Here we go.  So this is the connectivity information. We've now changed it to say that we're using the Secrets Manager secrets, and we are connecting to our PostgreSQL database rather than SQL Server database. So where's the connectivity commands for Postgres? That's going to be up here, I suspect somewhere.  Now, here's the SQL Server one. So here's the SQL Server connection string, which has been removed and it's been replaced by the Postgres connection string. So it's done all that for me in all of the necessary library changes that are required to support that.

[![Thumbnail 3200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3200.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3200)

[![Thumbnail 3210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b95b3cf85dacc2f1/3210.jpg)](https://www.youtube.com/watch?v=sogq2AHaqJc&t=3210)

Now when I've looked at this previously, when I get to the end, if I go right to the bottom,  I don't know why there's three in here this time. These are brand new functions that didn't exist in the program before. Are they new files or are they new  new functions within a file? But AWS Transform has worked out that in order to run in PostgreSQL, I've got to have these brand new functions. You can see there's no corresponding red that I've removed, this is all green, this is all brand new, and it's done all this for me. It's stuff I don't have to do.

I fall back to what I said earlier. It's been done by generative AI. I'm actually contradicting what Swami said earlier on now about the fact we should trust AI. I don't, I'm sorry, I'm not going to trust that. I'm not going to trust that with financial information, I'm really not, despite what Swami might have said in his keynote this morning. Test it, please. But it's taken an awful lot of code off the table for you. It's stuff that you don't have to do. It's all the complexities of integrating schema conversion with Database Migration Service with all the application code modifications. I've just clicked a few things and made that work.

We've got five minutes to go. If anybody's got any burning questions to ask, I think we're done.


----

; This article is entirely auto-generated using Amazon Bedrock.
