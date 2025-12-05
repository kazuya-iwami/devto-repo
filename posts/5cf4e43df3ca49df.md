---
title: 'AWS re:Invent 2025 - Automating enterprise-scale code modernization with agentic AI (MAM342)'
published: true
description: 'In this video, Edo DiMaggio and Morgan Lunt introduce AWS Transform Custom, a newly launched service that automates enterprise-scale code modernization using agentic AI. They explain how technical debt consumes 20% of IT budgets and demonstrate how the tool addresses runtime upgrades, framework migrations, and custom transformations through natural language prompts. Key features include autonomous learning from developer feedback, scaling across hundreds of repositories, and pay-as-you-go pricing at $3.05 per agent minute. Real customer results are highlighted: Air Canada achieved 90% efficacy with 80% cost reduction, Twitch saved 11 developer years across 913 repos, and QAD reduced project time from 2 weeks to 3 days. The presenters demonstrate the CLI installation, interactive transformation creation, and campaign management through both push and pull deployment models, emphasizing the importance of pilots, validation criteria, and progressive learning from simple to complex transformations.'
tags: ''
series: ''
canonical_url: null
id: 3085123
date: '2025-12-05T03:17:14Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Automating enterprise-scale code modernization with agentic AI (MAM342)**

> In this video, Edo DiMaggio and Morgan Lunt introduce AWS Transform Custom, a newly launched service that automates enterprise-scale code modernization using agentic AI. They explain how technical debt consumes 20% of IT budgets and demonstrate how the tool addresses runtime upgrades, framework migrations, and custom transformations through natural language prompts. Key features include autonomous learning from developer feedback, scaling across hundreds of repositories, and pay-as-you-go pricing at $3.05 per agent minute. Real customer results are highlighted: Air Canada achieved 90% efficacy with 80% cost reduction, Twitch saved 11 developer years across 913 repos, and QAD reduced project time from 2 weeks to 3 days. The presenters demonstrate the CLI installation, interactive transformation creation, and campaign management through both push and pull deployment models, emphasizing the importance of pilots, validation criteria, and progressive learning from simple to complex transformations.

{% youtube https://www.youtube.com/watch?v=S42NNd7U_HE %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/0.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=0)

### Introduction: Tackling Enterprise Technical Debt with AWS Transform Custom

 Good afternoon and welcome to the session on automating enterprise-scale code modernization with Agentic AI. My name is Edo DiMaggio, and I'm a senior manager of product in AWS Transform Custom. With me is Morgan Lunt, who is a senior product manager and main partner in crime for this product that we're going to talk about today. Unfortunately, Sriram Devanathan, our director, is not able to be with us today due to a press event that could not be moved.

Before we get started, how many people here have problems with tech debt or modernization? Well, what we're going to talk about is technical debt in this presentation. The challenge that we have is actually very pervasive. Everybody that we talk to has this issue. Morgan is going to talk a little bit about how to get started with AWS Transform Custom and how to perform transformations at scale. Transforming and modernizing a single repository is more of a proof of concept instead of actually addressing your tech debt challenge, which usually spans hundreds if not thousands of repositories.

After that, we're also going to talk about customization because every customer that we talk to has organization-specific challenges. Even things that are common, such as Java upgrades, Python upgrades, or dependency upgrades, tend to have specific challenges that are unique to each organization. Then we're going to talk about value and pricing.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/40.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=40)

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/120.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=120)

### The Pervasive Challenge of Technical Debt Across Organizations

  Let's talk about technical debt. Twenty percent of IT budget is currently spent trying to address technical debt. That is a lot of resources that would actually be spent on innovation, and that's probably a lower estimate. Twenty-three percent of developer time is lost because of tech debt. That is not the time spent to address tech debt, but time lost because you have tech debt, dealing with all the frameworks and dependencies, trying to figure out what to do. Accenture estimates $2.4 trillion of tech debt cost per year in the US alone.

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/160.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=160)

 Of course, tech debt comes in many forms. There is security and compliance risk. Your end-of-life runtimes are a major concern. Many customers have Lambda running on end-of-life runtimes that are going to be deprecated this coming year. But even without Lambda, you probably are still running applications on Java 8. There are lots of statistics showing that many customers are in that situation. Similar things apply for Python and Node.js. You may have vulnerable libraries with known vulnerabilities that are hard to migrate from. These are usually things that are harder than just going into a config file and bumping up a version.

There's also maintenance burden. You may have unsupported frameworks that you have to start supporting yourself. You may have legacy patterns that you have to support and skill your developers in patterns that you don't really want to use anymore. There are performance limitations. You may miss out on performance improvements and spend higher operational costs because you're using older versions of libraries and frameworks. You may also miss out on modern features. Finally, there's strategic misalignment. Usually what happens as a result of mergers and acquisitions or just organically is that your different business units and teams end up using different versions of tech stacks, and that is a drag on your innovation.

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/260.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=260)

### Limitations of Existing Automation Approaches and Real Customer Challenges

 Now, of course, many companies address tech debt in a manual way. Fundamentally, they just treat it as any other software development. There are specs, development, and tracking in your issue tracker, and you just go at it. There are ways to make things better and to try to automate. The existing approaches are rule-based automation, think about AST grab and open rewrite tools like that. They usually require specialized expertise to run. They tend to be brittle and inflexible. You have a rule-based code modification recipe or rule, and that works for that specific case.

But when things are different, there's usually a lot of cleanup that has to happen afterward. There's typically a lot of startup cost, and it's really hard to customize them for your particular case. They also have a much more limited scope. There's only so much you can do with deterministic rules. In the end, you're basically writing a program that modifies a program in abstract, which is really, really hard.

There are general purpose coding AI tools, and there are many of them. These are usually focused on interactive execution with a developer using them as an assistant, which provides great acceleration and they are great products. However, they are less suited for autonomous execution, and in the end, we do not want your developers to spend time doing this kind of modernization. We want to automate it. There's also no easy way to organize reusable knowledge. Usually, what happens with this modernization is that it affects multiple teams in a similar way, but with general purpose coding agents, every team has to organize the knowledge to do the modernization by themselves, and there's no way to share the learnings across these teams.

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/400.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=400)

What we really want is a low barrier to entry. We want a tool that's designed for automation. We want to be able to learn once and use it in every situation where that transformation or modernization challenge happens, and we want the tool to be able to improve continually.  Let's look at some of the transformation challenges that our customers had. We've had our product in beta since April of this year, and we work with many customers internally and externally to tweak our product and make sure that we could provide value.

Air Canada had a problem with Lambda upgrades. Node.js 16 was basically coming end of life, and they had very tight security and tight deadlines to make sure that they got off of that. They also wanted to migrate to Graviton instead of x86. Internally, our friends at Twitch were still running on AWS SDK v1 on their Go repositories. They had more than 900 repositories running on it. There was an enormous task to basically upgrade across all of these repos. QAD is a manufacturing company. They wanted to migrate all of their customers with each one with their custom plugin from their previous platform to their new platform. So they wanted to translate their very specific progress plugin into their new DSL. This is a very complex modernization project and definitely very organization specific. We also work with MongoDB, which is working on how to help their customers migrate off of older versions of the MongoDB SDK and adopt new functionalities.

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/490.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=490)

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/510.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=510)

### AWS Transform Custom: Natural Language-Driven Code Modernization at Scale

 Let's take a step back. In May, we launched AWS Transform and supporting modernization workloads specifically for VMware migrations, full stack Windows modernization, and mainframe. Yesterday we launched AWS Transform Custom.  This is a product that allows you to transform any code pattern. It has built-in transformations or you can create your own. It supports diverse scenarios like version upgrades and runtime up to language transformations. It does have continual learning and improves, and it scales transformations to enterprise-wide scale.

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/530.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=530)

 What are the main components of AWS Transform Custom? You can define transformations with natural language. You do not need specialized expertise. You have natural language prompts, you can provide code samples of what you want to do, migration guides, documentation, whatever you have. Then you can execute them at scale, either through a command line, interactive, or integrated in your CI/CD pipelines. We support multiple ways to integrate it into your own flows using your own environments, and of course, it enables campaigns across multiple repos.

Finally, and very importantly, it learns from developer feedback. So when your developers are modernizing your repositories and they provide feedback to the agent, that feedback is captured and reused for different executions. Not only that, the agent also learns by applying the same transformation over and over on your repositories. So if it tries an avenue of transformation and finds out that the test doesn't work, it will not try that again later.

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/640.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=640)

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/690.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=690)

Of course, all of this learning can be controlled and access controlled. The idea is that you can define the transformation once and we use it across many teams and many repos. That is different than just reusing a migration guide or every team interactively using coding agents. We also provide AWS managed transformations for very common scenarios.  Java runtime upgrade, Python runtime upgrade, Node runtime upgrades, AWS SDK upgrades, and we also have a few early access ones: Tech debt analysis and x86 Graviton for Java. These transformations are zero setup, we validate them on very wide benchmark suites, and we're continuously growing the number of transformations that we provide natively. We also, and importantly, allow you to build your own. The common use cases where we see this being very useful are API and service migration, language upgrades for things that we don't support, framework upgrades, code refactoring, custom patterns, and things like that. The idea here is that you can instruct the agent to execute the patterns that are unique to your organization. 

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/720.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=720)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/740.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=740)

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/760.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=760)

At the end of the beta, Air Canada was able to achieve a 90% efficacy rate for the conversion of thousands of Lambda code bases and an 80% reduction in expected time and cost. They're basically making this product part of their operational internal standard. Quad was able to enable projects that would take 2 weeks into projects that took just 3 days, achieving 60 to 70% production gains and saving an estimated 7,500 developer hours annually. This is to convert all of their customers to their new platform.   Twitch achieved 70% acceleration on each application migration for 913 repos. They're projecting savings of nearly 11 developer years. Finally, MongoDB was able to demonstrate a big impact in modernizing and migrating Java applications specifically for the MongoDB APIs. 

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/790.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=790)

### Getting Started: Simple Setup and Interactive Transformation Execution

Now I'm going to give it to Morgan to tell you how you can get started with AWS Transform Custom right away. Thanks Elio. The team that built Transform Custom was very deeply involved in the initial AWS Transformer release and in the release of Q Developer, and we learned some lessons along the way. One thing we learned was that onboarding on those services was harder than it had to be. Some of them required Identity Center setup and connection of your federated identity provider to AWS, which had all sorts of security approvals that would need to go through. 

You can get started with Builder ID for Q Developer, but then there wasn't a good way to graduate up to an enterprise subscription. So we tried to do away with all of that as much as we could with Transform Custom and really cut to the bone the bare minimum needed to set this thing up. There is no human identity. It's all IAM permissions just like any other AWS service meant to be machine driven. Of course humans can use it as well, and I'll show you how to set that up in a moment. We use standard AWS infrastructure, security, and access control—stuff that you already know how to use. We're not trying to reinvent the wheel here.

The setup is quick and easy, and I'll show it to you in a second. It's literally a one-line bash script to pull down our installation script and execute it. Then you configure your commit permissions just like you do with the AWS CLI. It works with your build system and your existing tools. The transformation execution is happening locally on your device, so you don't have to try and recreate your complex build environment in the cloud like some other services will have you do. It's super easy to compose this as part of your broader modernization workflow, so you can write a script to pull down your code from wherever it is, execute transformation, and push it wherever it needs to go. It's meant to be composable and flexible—an engine that you can stake and put in the car that you're trying to drive.

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/880.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=880)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/900.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=900)

So I said it's easy to install.  That's it. That's the command. You can go find it on our docs page that pulls down the package, executes it, and installs it. We have two dependencies. You need to have Node.js installed and you need to have Git installed, and that's about it. It works on Linux, Mac OS, and on Windows through WSL. We don't have native Windows support at this time, but we're working on it. 

This command line has human interactive mode as well as a machine drivable mode.

In human mode, you have a friendly conversational agent that asks you what you want to transform. It queries our registry to find if there's an existing transformation that does this, or it helps you build a new one. It has all of our best practices encoded in it. When you're trying to create a new transformation, you don't really know what you need to provide the agent. Let's say you want to go from API A to API B. The agent will extract from you information like whether you have documentation for those APIs, which would be helpful, or if you have a migration guide that you wrote for human users, that would be helpful too.

Based on several months of data we've run here, we've come to some crystallized best practices, and we put that right there to guide you the right way. As I mentioned, the command line can also be driven in machine mode with a totally deterministic syntax. You put all the arguments up front, hit go, and it executes your transformation with no human involvement needed at all. The benefits to this are that it's quick to start, quick to get feedback, and quick to figure out if this is going to work for you. Ideally, if it doesn't work right away, you can transform and customize it. It's meant for you to tweak it and add your own customizations. If you have organization-specific requirements, libraries, dependencies, or coding conventions, you can teach them to this tool and it will remember them and use them as it transforms your code.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/990.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=990)

As Elio mentioned, we have some out-of-the-box transformations that we provide. We have Java, Python, and Node.js runtime upgrades,  AWS SDK upgrades for those languages as well, and tech debt analysis, which I think is super cool. That's something we got squeezed in at the last minute. Basically, it does a complete thorough documentation of your code, including all of the relationships between the various functions, data flow diagrams, and entity relationships. Think of it like the most comprehensive code documentation you could imagine. We try to generate that, and then we use that information to distill out an analysis of technical debt and suggestions for things you may want to modernize. There may be libraries in there that you didn't know were deprecated, and we try to surface that to you.

Now, that's early access. It's not perfect yet. We're still working on it, but we're comfortable enough to give it to you to try out. We also have Java x86 to Graviton, and we're seeing really good success on our benchmarks internally on that. That one is hopefully going to leave early access relatively soon once we have a little more data. Of course, if you have to do something beyond this or in conjunction with these transformations, you can build your own. You can upgrade your Java and replace library A with library B, or you can stick these things together and mix and match to solve the problem that you're really trying to solve.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1100.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1100)

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1110.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1110)

There is not an official community thing set up yet. It's something we've really talked about, and when we talk about transformation definition portability in a minute, we'll come to that. But one of our tenets is that you spend the time and effort to build a custom transformation, and as far as I'm concerned, that's yours. You can keep it secret, you can share it with the world, that's up to you. We try to make these things importable and exportable as much as we can. Let's see how easy it is to get started. I put that command in, I run it, it's installed. That's it.  Now I hit type ATX, which is the three-letter command to kick this thing off. There's the help menu and there's our cool ASCII art that I designed. 

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1120.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1120)

[![Thumbnail 1130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1130.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1130)

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1140.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1140)

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1150.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1150)

[![Thumbnail 1160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1160.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1160)

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1170.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1170)

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1180.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1180)

I'm telling the agent that I want to create a new transformation. I want to upgrade my Python repos.  It's going to search through our registry and say, hey, it looks like there's an existing transformation built by AWS out of the box already to do this. Do you want to use that one?  I'll say sure, because it's a totally agentic chat, you just talk to it.  It's doing some prerequisite checking here, making sure Git is initialized on the repo that I pointed it to.  Finally, it's going to ask me if I have any special requests, like you're at a restaurant and they're asking if you want the burger but do you want onions. You can give it your extra little special request here, which it's going to incorporate during this particular transformation execution.  And it's off, it's running, it's doing its thing. It'll take 10 to 20 minutes, depending on the size of the code base, but that's how easy it is to get started: install the CLI, point it to an existing repo you have on disk, and just start transforming it.  

[![Thumbnail 1190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1190.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1190)

### Scaling Transformations: Centralized Planning and Knowledge Sharing

Scaling up. So I can transform one repo, but I've got 500 repos I need to transform. How do I do that?  Based on our beta experiences with our customers so far, we found certain patterns that seem to work well that map to different people with different jobs within a large organization.

The first thing you want to do is plan and figure out what you want to transform. Do you want to upgrade your job at one time? Do you want to migrate from Dropbox to Box because your company signed a new deal? Did you acquire another business and they use some API provider that differs from your standard, and you want to migrate their code to use that instead? Figure out what it is that you're trying to transform, and then you're going to identify some representative targets to start with. Choose a code base that's not too simple and not too complex, something that feels like a good representation of all the other code bases you're going to have to transform that are facing the same problem.

Then you try it. You sit down with our agent like I just showed you. It asks you what you want to transform. You tell it you want to go from one thing to another. Here's some documentation. Here's a how-to guide I wrote for human users. Here's a blog post. You can really feed it any text that should be helpful, code diffs if you've done it before in a few repos. Those are really helpful to actually show, not tell the agent what you're trying to do.

When you do this and create your initial transformation, you'll notice this is done by a centralized team. One pattern that we found works well, especially at large companies, is when there is a core central team in the organization who's responsible for coding standards, conventions, and practices. They tend to be the ones who go about creating these transformations so they can disseminate them out to the application owner teams who are actually going to run them.

After you've created your initial transformation, start a pilot with tens of apps, and you'll start to collect data on how long this takes to run and how good it is. Does it hit 70% efficacy? Does it hit 95% efficacy? It just depends on the complexity of what you're trying to do with it. You can also get a really good idea of what it's going to cost here. Elio is going to talk about pricing in a little bit, but it's purely pay as you go, totally usage based, with no subscription stuff at all. It's based on how much the agent is working. A more complex transformation means the agent works more, so it's going to cost a little more.

Through the pilot phase you'll collect data on that and you'll be able to budget. To do this campaign, it's going to cost us roughly this much money. Then you can start to scale it out. I'll talk in a minute about the push modality and the pull modality, which are the two different ways of doing these sorts of campaigns that we've seen. You integrate it with your source control if you would like, and you can monitor and make sure the progress is going as you would like.

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1370.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1370)

The most exciting part about all this in my opinion is the learning. Every time a transformation executes, our agent goes and checks all of its work, looks at everything that it did, and identifies knowledge items. These are things that it wishes it knew at the beginning of the transformation that will make it faster and better and do what the human actually intended for it to do. It's going to incorporate that information next time it runs. 

The benefits of doing this in a centralized approach like we're talking about here is that you only have to put in the effort of defining this transformation one time. The quality is going to be consistent. Everyone across your organization is going to have the same thing done in the same way. If I were to write a wiki page telling everyone in my company that we're migrating off this old ticketing API and you need to use this new ticketing API, and I send that to a bunch of application owner teams, getting them to do it is going to be hard unless there's a top-down mandate. But there's a decent chance that just because humans are humans, everyone's going to interpret the words in that wiki a little differently even if I try to be really explicit with it, and they may follow different conventions and do things differently.

[![Thumbnail 1450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1450.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1450)

With a centralized transformation approach where you define once and you run many times, you can be confident it's going to be done roughly the same way each time. Finally, we break down those knowledge silos. If you have a bunch of individual developers and a bunch of individual application teams that live in different cities and don't talk to each other a whole lot, as they make discoveries about using this function to save a lot of work or using that one where there's a bunch of problems and it's annoying, that information today is going to be kind of siloed. But if you do this centralized approach with the centralized transformation definition, these learnings are upstreamed and they're shared with everyone and saves everyone some effort. 

### Push vs. Pull Campaigns: Deployment Strategies and Automation Integration

So push campaigns versus pull campaigns. The way I think of a push campaign is you have this centralized team. They defined the transformation. Maybe they got to a really good efficacy, 80, 90%. They're very confident that this thing works rather well. They can directly pull down code that needs to be transformed, transform it, and then push a PR to the application owning team to review. This way you're putting less work on the application owning team. They don't actually even have to install the CLI or do anything. They just have a pull request that they need to go review and make sure it does what needs to be done.

This is especially useful for things with high efficacy. For things that are a little more complex or trickier, or if you just want to give the application owning team a little more ownership over it, you can do a pull campaign where you have a centralized team still create that consistent transformation definition and instruct the individual application owning teams.

We instruct the individual application-owning teams that we're running a campaign to migrate from one version to another, and here's the deadline and an important person says you have to do it. But instead of doing it manually, here's a command you can just plug into your command line after you install the ATX CLI and run it on your code base. It should get you most of the way there and take most of this work off of you. In our experience internally at Amazon, this has really helped accelerate internal migration projects because there's something to be said for human nature. When you ask someone to do something for them versus asking them to do something and saying, "Here's something I made. I put time and effort in to help you do it."

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1540.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1540)



Best practice number one is pilots. You're not going to know how well your transformation works unless you test it. You're not going to know how much it's going to cost unless you test it. You're not going to find the edge cases and weird little bugs and strange behaviors that your users may run into unless you test it. I highly recommend taking some time, maybe a couple of days, to create your initial transformation definition. Find some really good representative repos and run it on those first.

Then refine it, tweak it, and fix those edge cases. Add exceptions when needed. You may need to decompose a really complex transformation into multiple steps, which is perfectly fine. I've seen customers use a pattern where they use our CLI syntax to invoke one transformation and then immediately invoke another one right after. For a user, it's just as easy as one transformation, but for the agent, sometimes it makes things better if you give it smaller chunks of problems to focus on at a time.

Learning items and knowledge items that are discovered during execution are not automatically enabled by default. That's to prevent any sort of malicious knowledge item from being discovered and applied to code unwittingly. The centralized team that owns the transformation is always going to be able to look at those and say, "The agent thinks it discovered something smart here, but it's actually not smart. Let me not enable that one." You can measure time and cost and put together your automation mechanisms for using this within your organization, however you store code and do code reviews. It can slot into that. It's a really bare bones, easily buildable CLI script.

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1640.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1640)



Automation and integration is something I've touched on already, but here's an example of how simple the syntax can be. You get clone the repo, then ATX exec, and N is the name of the transformation that's stored in your registry and shared across your company. C is the build command, which is optional. Sometimes for Python and similar languages, people use a linter for that. It's just a way to tell the agent, "Here's how you check your work. If you run this, make sure you don't get 1000 errors. If you did, you probably did something wrong."

X is non-interactive mode, so it means don't wait for human input. Just go. Sometimes I call it YOLO mode because it means the agent is not going to ask for any help or approval. It's just going to go do the thing. T is to trust all tools. You can also configure tool permissions if there are certain tools you do or don't want your agent to use. Then it'll run and you tell it to push the PR somewhere and someone will review it.

You can stick this in a container. You can stick this in a pipeline. You can stick this in an AWS Batch job. You can stick this on a laptop in the basement. I don't particularly care. However you want to run your modernization campaign on whatever infrastructure you want to run it on, as long as it has Node and Git and you can get your code onto it and it's Linux-based, Mac OS, or WSL, you can put it there. This is intentionally making it really flexible for however you like to work.

GitHub Actions, GitLab CI, all sorts of places that you can put it. Obviously, first-class integrations would be nice if we could say this already exists in a GitHub action or if we had a pre-built container we can give you and say just use this. We're getting there.

[![Thumbnail 1740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1740.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1740)



### Campaign Management Through the AWS Transform Web Application

I also wanted to note the AWS Transform web application. If you've used AWS Transform in the past, you may be familiar with this web application. This is where our mainframe modernization, our VMware migration, and our .NET modernization experiences live, the parts of AWS Transform that shipped a few months ago. ATX Custom is mostly CLI-based, but it does have some connections into the web app, namely campaign management. If you are a program manager or a campaign manager or just someone whose boss is asking, "Hey, you said you're running this campaign. I want pretty charts and graphs. How's it going?" This is the place for that. You can set up your campaign and see all the repos that have been executed.

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1790.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1790)



I'm a campaign manager or program manager. I go into the web app and tell it, "Hey, I need to upgrade all of my Python lambdas. What do I do?"

[![Thumbnail 1800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1800.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1800)



I want to upgrade all my Python lambdas. The web app has full knowledge of every transformation that's in the registry, so there's the AWS-provided ones as well as ones that people in your organization may have created. It's going to pull down the relevant transformation, create a campaign, and give you a command that you copy and paste and send to your developers. You tell them to go put this in your CLI, execute it, and as they execute it on their repo, it's going to save the results of that execution—whether it validated, succeeded, or failed—and give you that beautiful UI in the web app to see all this collective aggregated data.

[![Thumbnail 1870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1870.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1870)

Because it is AWS Transform, you've got the agentic chat interface where you can chat with it about that data. You can say, "Hey, how many of these over the last month validated? How many did not?" Any way you want to slice and dice the information, the agent can help you with that. 

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1900.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1900)

### Creating Custom Transformations: From Prompts to Organization-Specific Patterns

We looked at how you can transform a single repo, and we looked at how you can scale it to multiple repos. What ends up happening is that every customer that we talk to really has organization-specific customizations that have to be done. This is why we call the product AWS Transform Custom, even for things such as job upgrades. You have your own target versions, your own framework, your own ways that you want to do things. 

When you have an AWS-managed transformation such as the ones that come right out of the box, you actually still probably need to add some additional customization. Think about your organization's specific pattern, internal libraries, output style. You may want to have your own validation criteria like a special integration test or commands that you want the agent to run to make sure that things are working properly. As an example, when you're doing Python 3.8 to 3.11, you may want to add an internal login framework that you want to use. You may want to have specific error handling that you want done in your organization.

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/1970.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=1970)

This customization happens once, but then the agent still continues to learn your organization's specific things every time it is executed. Developers that run it interactively and provide feedback, or ways that the agent finds that don't work or that do work, the agent will still continue to learn. 

[![Thumbnail 2000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2000.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2000)

Sometimes though, you actually do need fully custom transformations. There's organization frameworks, maybe you want to have your own custom libraries or proprietary patterns. You have strategic migrations where you may want to change your authentication service, migrating from Auth0 to Cognito or vice versa, going from Splunk to CloudWatch or vice versa, and doing vendor changes. You may harmonize the tech stack across merger and acquisitions or do architecture changes. 

There are multiple ways you can create custom transformations using AWS Transform. These are some of the ways that we usually propose. One is the prompt and refine approach. You can start with a simple prompt, the agent will ask some questions about what you want to do, and after you have a first transformation definition document, you can iterate on it depending on some samples and your examples. You can also start with guidelines. You may have a migration guide that's already existing. If you have an internal framework, you may want to provide API specifications of the different versions, or very importantly, before and after code samples of how you want things done.

[![Thumbnail 2080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2080.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2080)

Finally, you also have a way to do it with me, which is a way in which you have a very simple transformation definition and you basically stop the agent while it's doing it, providing feedback. Very important is having references with before and after examples. After you're done, the idea is that you publish this new transformation definition on your organization registry that is associated with your AWS account. This can of course be access controlled through IAM. 

[![Thumbnail 2090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2090.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2090)

Let's look at the demo for this. Here, as Morgan said, we can start ATX here. It's already installed and let's say that I want to do something custom that is not available there. I want to add OpenTelemetry tracing to my Node.js backend apps. 

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2120.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2120)

[![Thumbnail 2130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2130.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2130)

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2140.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2140)

[![Thumbnail 2170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2170.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2170)

This seems to be a pretty common modernization thing. What the system is going to do now is go and look at all the transformations that are  existing in my registry. We have a ton because this is our own internal account, but one of the things that I want to show is at the beginning, the list  of the AWS managed ones with the description and what they are. So after that, the agent is basically going to say, "OK, there is no existing transformation  for adding OpenTelemetry. It will help us create a new one." And it starts asking some questions and says, "OK, what kind of backend do you have? Are you planning to use a specific backend and do you have some existing logging?" What I want to say is, "OK, these are mostly Express and I want to use AWS X-Ray. And if I do have existing logging, I want to remove it." 

[![Thumbnail 2180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2180.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2180)

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2210.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2210)

OK, this is also the moment where it will basically ask, "Do you have migration guide, documentation, a whole bunch of things,"  "and an example for doing like before and after, how do I want to do that thing?" For OpenTelemetry, what I can imagine is that you probably want to have your own specific attributes that you want to put, the kind of traces that you want to have, but probably you want those things to be consistent across all of the repos. That would be the moment to add all those informations. Now, it's going to go here and it's going to think for a little bit. 

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2220.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2220)

[![Thumbnail 2240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2240.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2240)

This is great. And it basically said, "OK, this is the first draft that it came up with. Add OpenTelemetry tracing to such and such. It has some entry criteria, it has some implementation steps."  The idea is that this is a TD file. You usually run this inside of an IDE so you can basically say, "OK, this is my transformation definition. You can open it, you can edit the markdown directly and say, 'Hey, I edited it.' Or you can ask to change something." There's implementation steps and one of the things that is really important are the validations  and exit criteria. It says, "OK, packages and has all of these things." Some of those may require you to customize exactly what you wanted to test. You may have some integration tests, you may have to provide your own way to actually test, for instance, X-Ray integration. Or maybe you want to run some specific integration tests that are specific to your work. The idea is that you can look at it and you can now apply it and see what happens. You can review or modify, you can do anything like that.

[![Thumbnail 2280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2280.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2280)

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2310.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2310)

[![Thumbnail 2320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2320.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2320)

One of the things that I already did is that I used one of these transformations earlier to actually create, to add telemetry to this Node REST API and I created a diff in this file here.  So what I'm going to do now, I'm going to go and say, "OK, I want to modify the transformation to use these samples as references." OK, and if the demo gods are with us,  they're basically going to say, "OK, this is a file, I'm going to take it, I'm going to add it to the demo and basically what it is doing is looking at these files that can be multiple files, could be APIs and what it is doing is analyzing, it's creating and ingesting it into its own knowledge base, and it's also going and updating the transformation definition document to include the learnings from those files." 

[![Thumbnail 2340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2340.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2340)

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2350.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2350)

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2370.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2370)

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2380.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2380)

Now, this is if you have things already available, you can, of course, run it interactively and basically saying, "OK, I would like to do, I would like to do, I would like to basically give feedback directly when it's running on a specific code base."   Now, this is going to create a TD. I'm not going to wait for it to finish. After it's finished, you can publish it and you can run it like Morgan showed before.  This can be working, it's adding summaries and it's doing a lot of other things. Now, let's say that I have this thing and we want to run it. 

Let me just let this thing run a little bit because this is going to take some time. Let's say that I run it on this specific application. And what we see here, we see the transformation definition, it has the entry criteria, and it has a set of validations. After I run it, what it does, the system provides a validation summary, given all the validation criteria that I provided. So it tells me, "OK, this is what I did, what happened, and the overall status was partial.

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2420.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2420)

It wasn't able to validate everything, but it tells me why. Eight out of ten fully passed. Two criteria were partially  met because I did not give it any information on how to actually test X-Ray. I wanted to go and see the things were in X-Ray, and of course I didn't give it any information to do it.

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2460.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2460)

This is really important if you want to automate these things at scale, because one of the things you don't want to do is have agents fail silently, basically saying I did it, and then you have stuff that's not working. Because then that means all of the ones where it tells you that are completed, you have to go and recheck everything. You want to make sure that the validation is specified as precisely as you can, and that will make sure that you can automate things and really have the savings that  you can have.

[![Thumbnail 2480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2480.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2480)

This is the validation summary. After it's done, a really nice thing is we do require a gift in all of these for all of these transformations. If you go here, you see that it created different commits in its own branch. So you can have every step that is in your transformation definition in a specific plan that is being created to apply the transformation to this thing, with a different step that is committed here, and you can see and review exactly what happened. 

[![Thumbnail 2500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2500.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2500)

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2510.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2510)

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2520.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2520)

The first thing that it does, which by the way was in the transformation definition, was to add all the packages as you would imagine, and it created a new JavaScript file with the tracing configuration  for AWS X-Ray. This is the common file that is used by all the applications. Usually you probably want to provide something that is very organization specific.  You want to say what it did. 

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2530.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2530)

[![Thumbnail 2580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2580.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2580)

Then it went and said, OK, these are my route handlers, data access. Let's look at data access. You see here there's a lot of things that were added,  from like orders create. All this green is basically all the OpenTelemetry spans that add the various attributes like order user ID, order category, and all of this. These are probably things that you may take as a first stab as it proposes, but you probably want to provide something that aligns to your own patterns and organization-specific things. These are the usual things that are specified in how do we want to integrate OpenTelemetry in our code base. You give that to the agents, you're going to make sure that all of the things, all of the code base in your organization follow the same patterns. You can also instruct it to detect divergences. 

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2600.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2600)

[![Thumbnail 2630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2630.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2630)

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2620.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2620)

A nice thing here is that you can also, when we talk about this, OK, this added, I have my updated transformation, I can publish it. One thing that is really interesting here, let me start a different conversation. The agent knows what it is good at. So if I say something like, can you transform .NET applications into Java? This is a hard thing. The thing is that you want the agent to know the things that it knows how to do.  It will basically tell you that, well, OK, this is a very high complexity transformation  due to the significant differences. So you can still try to do it and it will basically tell you what kind of things you should think about when you want to do those things. If you want to separate it between extracting specifications, if you want to use strangler fake, but basically it knows the things that are easier or not. 

[![Thumbnail 2660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2660.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2660)

Now let me go back here to my PowerPoint.  What we have here is that OpenTelemetry is a code refactoring, is one of the transformations that is one of the sweet spots that we have. Code refactoring, library upgrades, runtime upgrades, we've been dealing with those kinds of transformations for the last couple of years in AWS Transform, and we know that the tool is designed to do it, but you will come up with new things. We've had customers come to us with very strange things and sometimes they were able to be done very quickly, and sometimes they required a little bit more complex setups.

### Improving Transformation Quality and Measuring Business Value

How do you improve the transformation quality? How should you think about it?

The quality of the result depends on two things. One is the complexity of the transformation. For example, .NET to Java transformations are really hard because they involve two different languages. It can be even harder, like C to Rust. The other thing is the input codebase. You may have a .NET to Java app that is 800 lines of code and be able to do it right away. But even an easy transformation, even a job upgrade, applied to a 5 million lines of code codebase with multiple modules and tons of dependencies may still struggle.

We continue to validate the transformations in our sweet spots. We have a ton of internal evaluations on internal codebases, open source, and we always make sure that we improve that quality. But you still need to have some improvement strategies. There are some things that you can do at definition time. Providing high quality documentation and code samples is important. Running pilots is super important. After the pilot, you'll usually tend to have a lot of before and after examples of things that you want, so the agent can really learn a ton about those things.

You also need to test it and continue learning in order to make it work better. You can decompose the transformation. For instance, if you want to translate across languages, you can first extract specifications, then do code generation. You may decide to decompose based on functional layers. Some of our customers said, "OK, let me first migrate the data layer, then I want to migrate another layer," and so on.

[![Thumbnail 2830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2830.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2830)

During refinement, we talked about pilots. You want to understand what kind of results you can have. If you're seeing that you're not getting the quality that you want, that is the moment where you have to reevaluate and say, "OK, this is a really complex transform, let me decompose either the transform or the targets." 

Let's talk about value and pricing. The customers that we work with in the last 6 months reported 3 things that they were really happy about. On one hand, there were the direct time savings. It was faster than doing things manually. It was faster than just using a general purpose coding agent. They were able to run it at scale. The other thing was that it enabled things that they thought they were deprioritizing because they were too expensive. The Twitch team was deprioritizing doing the AWS SDK migration for this year because they did not want to spend 11 dev years. Fundamentally, what it says is that we're going to work on a year for a team with 11 people. It's possible, but it's not something they really want to do.

The other thing that was there was quality and consistency. Having consistency across all those 900 repos, or in the case of Air Canada, thousands of Lambdas was really critical. Most of these things are driven from centralized teams, and consistency is really important. They're usually in charge to make sure that things are consistent across the codebase. And of course, they really saw continued learning improve the quality and compounding value.

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2910.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2910)

How do they measure it? There's of course efficiency—how fast did you go? Quality metrics were also important. How many things have to be reworked? When we talked about the efficacy rating, 90 plus percent of the codebase for Air Canada did not have to be touched by anybody. That code was reviewed and sent to production. 10 percent still has to be tweaked. But there's a very big difference between just taking a PR, testing it, and flying through your validation versus having to tweak and understand and figure things out. 

And then there's business impact. There's cost reduction. These are operating costs. You may have going to more modern frameworks may actually save you money, less resource utilized, better advantages, risk reduction. You may have the latest versions, more reliability, less security risks, and of course, strategic enablement. You want to be on certain frameworks, so it's usually a reason.

[![Thumbnail 2980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/2980.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=2980)

### Pricing, Availability, and Progressive Learning Path to Success

This is generally available. It's available in US East One today, and we're going to add all the other AWS Transform regions in the coming quarter. We are in 8 regions. We do have cross-region inference, but they are geo-related. So North America, Europe, and things like that. The pricing is pay as you go, as Morgan was talking about, at $3.05 per agent minute. An agent minute is basically a notion of the amount of work that the agent takes. 

You only pay for active agent work. So if the agent is waiting for your codebase to compile, you're not paying for anything. If you're waiting idle because the user is waiting for something, of course, you're not paying anything. However, multiple agents may collaborate to do your transformation. So 15 minutes of wall clock time on the service side may actually result in a charge of 20. We have some examples there, and that's why we suggest running pilots on your own specific transformations to have an idea. However, the cost is small enough that it should not be hard or prohibitive to try these things. You can always interrupt, and you're in full control of how much you spend.

[![Thumbnail 3060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/3060.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=3060)



What we suggest you do, and what I incite you to do, is to build confidence through progressive learning. It's hard to use AI to do these things autonomously. It's kind of a step level from using an agent interactively. What we suggest to do, and what we see in successful cases, is basically to start with things that are simple and already provided by AWS. One low risk and high value way is to start with documentation. You can learn the workflow, you have no code changes, so there's no risk of messing anything up, and you can build familiarity with the tools.

You can then go on to AWS-managed transformations. Every customer we talk to has problems with runtime upgrades, end of life, and SDK upgrades. This allows you to learn the end-to-end workflow, to experience continued learning in action, customize these things, tweaking them for your own specific organization, and you can start measuring savings and see what you can get with this technology. You can then move on to simple custom transformations, think about logging, adding OpenTelemetry, trying to modernize things from that perspective. In my experience, every company that I talked to has one of those backlogs hidden in their issue trackers.

And finally, you can move on to more complex custom transformations in the context of larger modernization, architectural changes, language changes, and things like that.

[![Thumbnail 3160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/5cf4e43df3ca49df/3160.jpg)](https://www.youtube.com/watch?v=S42NNd7U_HE&t=3160)



So there are some resources here. You can learn about AWS Transform Custom on our website. There's a handsome demo. It's super easy to get started with the CLI, no complex setup needed, runs on your laptop basically. Well, it ran on mine. I'm really happy that you guys chose to spend your time here with us today. I'm really looking forward to seeing what you guys can build with this technology and what kind of transformations you can do. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
