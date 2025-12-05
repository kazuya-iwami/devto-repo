---
title: 'AWS re:Invent 2025 - Kiro in action: Red Team tactics at scale (DEV336)'
published: true
description: 'In this video, Nick Gilbert and Damien from FICO demonstrate building an IAM scanner using Kiro, an AI-powered IDE that supports spec-driven development. They address the visibility gap in AWS security assessments across hundreds of accounts. The scanner identifies high-value attack paths, enumerates cross-account access, prioritizes roles by impact, and detects common mistakes like wildcards in trust policies. Using the PROMPT framework (Purpose, Requirements, Output, Method, Pro tip, Testability), they create markdown specification files that Kiro translates into working Python code with multi-threading capabilities. The tool analyzes AWS managed policies, IAM principals, role trust policies, unused roles, and privilege escalation paths. Development time is reduced by 70% compared to traditional coding—from 200-350 hours to just 25-35 hours. The scanner generates both JSON files for automation and markdown reports for leadership, demonstrating 40 unused roles with 21 dangerous ones in their example. The code is available on GitHub.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Kiro in action: Red Team tactics at scale (DEV336)**

> In this video, Nick Gilbert and Damien from FICO demonstrate building an IAM scanner using Kiro, an AI-powered IDE that supports spec-driven development. They address the visibility gap in AWS security assessments across hundreds of accounts. The scanner identifies high-value attack paths, enumerates cross-account access, prioritizes roles by impact, and detects common mistakes like wildcards in trust policies. Using the PROMPT framework (Purpose, Requirements, Output, Method, Pro tip, Testability), they create markdown specification files that Kiro translates into working Python code with multi-threading capabilities. The tool analyzes AWS managed policies, IAM principals, role trust policies, unused roles, and privilege escalation paths. Development time is reduced by 70% compared to traditional coding—from 200-350 hours to just 25-35 hours. The scanner generates both JSON files for automation and markdown reports for leadership, demonstrating 40 unused roles with 21 dangerous ones in their example. The code is available on GitHub.

{% youtube https://www.youtube.com/watch?v=IU6FOTCJ2Qs %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/0.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=0)

[![Thumbnail 10](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/10.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=10)

### Introducing the IAM Scanner: Closing the Visibility Gap with Spec-Driven Development and Kiro

 Welcome to DEV336 Kiro in action. Let's talk about the problem that we're trying to solve today.  That's the visibility gap. When you're doing a red team engagement or a penetration testing assessment for a large organization with hundreds of AWS accounts, you're going to have hundreds of thousands of resources and you're going to need to find the vulnerabilities within those. We're going to show you how to build a tool using Kiro to do that.

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/30.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=30)

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/40.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=40)

 Let's introduce ourselves. I'm Nick Gilbert, the Red Team Manager at FICO. I'm an AWS Community Builder on the Security and Identity team.  I'm also part of the AWS Subject Matter Experts program for the Security Specialty Exam and a Gen AI enthusiast.

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/50.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=50)

And what's going on, my name's Damien.  I am a Senior Cloud Security Engineer at FICO during the day and at night I'm the founder of the DevSecOps Blueprint, which is basically to help people get into DevSecOps and cloud security. I'm an AWS Community Builder, just like my buddy Nick over here. I also produce a lot of content as well as create courses on LinkedIn. And then for fun, when I have the time, I love anime, I like cars, I play video games. Those are just a summation of my hobbies.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/80.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=80)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/90.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=90)

 But let's quickly discuss how we intend to close that visibility gap. I'm going to turn it back to Nick so he can cover the IAM scanner and the high level requirements.  So we're going to build an IAM scanner that does the following. First, it's going to identify your high value attack paths. It's going to enumerate cross account access. You'll often start out in a dev account and then want to work your way to those crown jewel assets in the prod accounts. It's going to be able to prioritize roles by impact. It's going to spot common mistakes like wildcards and trust policies, missing external IDs. It'll give you specific actionable escalation paths. It'll provide repeatable results, so once you report your findings, the team will tell you they fixed the issue and then you can simply run the scanner again to confirm everything's fixed. And then finally, we're going to use Python with multi-threading to be able to scan hundreds of accounts all at once.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/140.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=140)

 So the question that we need to ask ourselves is how can we solve our solution quickly as security engineers? With the introduction of AI into the industry, we know for a fact that there are two kinds of strategies that we can use to make this happen. There's vibe coding and there's also spec-based design or spec-driven development. The question is, which one do we use? Let's talk through vibe coding at a high level. Vibe coding essentially is basically you and the AI just vibe. You open a chat, ChatGPT, Claude, or whatever you have. You throw some ideas at it, you brainstorm, you refine it, and you copy and paste the code, you keep tweaking it and you just keep doing the same thing over and over again until it actually works.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/170.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=170)

 It's great for proof of concepts, but it is not what we need for scalability. It doesn't scale. There's no way to understand, there's no shared understanding, no guarantees. And there's definitely no long-term maintainability for all of that. For us in our approach, that's not how we want to build a production scale security tool or even guardrails for that matter or any type of security automation.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/210.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=210)

 But with spec-based design and spec-driven development, it's quite different. When you look at it like this, you're basically the architect of your solution. Before you even ask the AI to write code, you basically have to think about everything from the beginning all the way to the end, from the requirements, have everything mapped out and how you actually want it to be implemented. Which in a sense, is going to help us build that scalable maintainable solution that's updated by changes to the spec itself.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/240.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=240)

 And that's pretty much where Kiro comes into the picture. Kiro is basically that AI-powered IDE that is used to enhance software development by using AI agents on the backend. One of the key features of Kiro is that it supports that spec-based or spec-driven development and spec-based design that we talked about. Not only that, but it's very user friendly and easy to use. If you're a developer and you use VS Code, it's very easy because it's basically a clone of VS Code. And it also supports various different programming languages. Python is our number one programming language of choice and what we use to build the IAM scanner.

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/300.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=300)

So now I'm going to turn it back to Nick so he can talk about how to create a spec using the PROMPT framework. So to create your spec files, a good way to do that is using the PROMPT framework. PROMPT is an acronym.  The P stands for Purpose. This is a brief description of what you're trying to build. Then you want to give the Requirements, so a high level overview of your program. Think of this like a job description.

[![Thumbnail 310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/310.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=310)

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/320.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=320)

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/330.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=330)

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/340.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=340)

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/350.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=350)

Next is the output.  List how you want to receive the deliverables. Do you want it in one single file or do you want it modular in several files? Next up is the method. This is going to be the largest section of the prompt.  You want to enumerate the very specific details of your prompt. Then a pro tip, this is something I always like to do when using Kiro is at the end of the prompt.  I tell it to ask me clarifying questions before it begins. There's usually always something important that I forgot. And then finally testability.  One great thing about Kiro is it can create unit tests for you. So anytime you make a change to the software, simply run the unit test to confirm everything's working okay. 

When it comes down to the IAM scanner, the thing that we use and that Kiro expects is three markdown files. We have the requirements, which is basically what we are wanting. The design, which is what is needed, and then also the task, which is how we actually want this to work, the step by step execution. The beautiful thing about this is that as someone who may not necessarily have a lot of software development or engineering experience, you can essentially create your own products by just creating markdown files with a bunch of text. I think that's the key thing with Kiro is that it puts you in a different part of the SDLC by allowing you to write the specifications in English instead of in code.

[![Thumbnail 400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/400.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=400)

### How the IAM Scanner Works: Architecture, Results, and Dramatic Time Savings

So let's talk about how the IAM scanner works.  The first thing it's going to do is cache the AWS managed policies. There are over a thousand of them. When you're doing hundreds of accounts, you don't want to download them each time. So it's going to use a caching system from that. Next up is going to collect the IAM data. This is your users, groups, roles and customer managed policies. Then it's going to analyze all those managed policies using a policy engine, find out which ones have high level permissions.

Next is going to check all the principals. Both the managed policies as well as the inline policies and determine which principals are dangerous. Then it's going to look at the role trust policy. This is important for lateral movement in the current account, as well as cross account access. Then it's going to look at unused roles. A lot of times when doing red team assessments, the roles we end up using are unused ones that were created a long time ago with high privileges and never deleted. And then it's going to analyze privilege escalation paths. This will go through every role, determine if it has a path to admin access, and then tell you how you get from where you're at to admin access. For example, it may say assume a certain role, then use that role to create an EC2 instance with the high privileged role. We'll give you the code for this at the end.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/480.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=480)

Now that you understand how the IAM scanner works from an architectural standpoint, let's talk about the estimated time it would take if we attempted to do this without using AI versus with using Kiro.  If you take a look at the table, you can see that from planning and design to development, you have touchpoint meetings, testing, and also the rework rate. You're looking at between 200 to 350 hours for building out a tool as sophisticated as that with all those various different steps. But with Kiro, you can see that there's at least a 70 percent reduction across the board simply because we're essentially passing in all the parameters for it to be able to build out everything with AI. That includes unit test cases. The key thing is also that rework rate because it's generating the code and it's also going through and testing the code as well while it's generating it. So you're thinking about maybe 25 to 35 hours, if not a week, for us to be able to build this tool.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/550.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=550)

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/600.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=600)

So just so you know that I'm not telling you no lie, let's talk about some of the results so you can see everything that actually happens.  When you take a look at this, this is step five of the IAM scanner where it's basically iterating through and performing a role analysis in my account. Yes, this is my account ID please do not steal it or take it or do anything bad because I'm going to terminate the account anyway. But ultimately you can see that there are 40 unused roles, and 21 of them are considered to be dangerous. The beautiful thing about this is that it generates two files. It generates the JSON file, which can be used for some automation that you're writing, and it also generates that markdown report with all of that information in those analysis points. But the thing about this is that with that report that's being generated, if you need to convince your leadership to throw some extra money so  that you can have the additional support to secure some of those resources in the cloud, you can export this as a PDF and send it all on your way.

This will allow you to convince people by giving them an executive summary of what's going on in your account and why. All of this is maintained by those markdown files, so if we need to make a change to this, we essentially update the spec and have Kiro go through and update the code for us.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/630.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=630)



Some key takeaways: Use spec-driven design or Kiro to rapidly develop and scale your security solutions. Why? Because you can essentially pass in those requirements via text to make that happen. Also, use spec files to translate those product requirements into working code and software without writing code. The last thing is, if you are a security engineer, whether you're on the red team, blue team, purple team, or any other team, you want to make sure that you create automated tools to find those unlikely entry points into your cloud environment and any of those hidden vulnerabilities and risks so that you can mitigate all of those in an automated fashion.

As we approach the end, if you want to check out the code, please make sure you scan the QR code at the very top for the IAM Scanner on GitHub, which is available for use. Also, connect with us on social media, myself and Nick. Those are our QR codes to our LinkedIn profiles. Thank you so much for listening.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/13f6cefbeed9e646/690.jpg)](https://www.youtube.com/watch?v=IU6FOTCJ2Qs&t=690)




----

; This article is entirely auto-generated using Amazon Bedrock.
