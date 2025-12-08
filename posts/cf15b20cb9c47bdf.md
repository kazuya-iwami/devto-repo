---
title: 'AWS re:Invent 2025 - Accelerate development velocity with better code reviews (DVT344)'
published: true
description: 'In this video, Graphite presents their AI-powered code review platform designed for the era of agentic software development. The speaker explains how AI code generation has created a bottleneck in code review, with engineers drowning in PRs. Graphite addresses this by integrating AI throughout the pull request lifecycle, featuring an AI review agent, unified inbox, and self-healing CI. The platform delivers measurable results: Shopify saw 33% more PRs merged per developer, and Graphite Agent''s comments lead to code changes 55% of the time versus 49% for humans. Companies like Snowflake, Robinhood, and Duolingo use the platform, which handles hundreds of thousands of PRs and saves significant engineering time.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Accelerate development velocity with better code reviews (DVT344)**

> In this video, Graphite presents their AI-powered code review platform designed for the era of agentic software development. The speaker explains how AI code generation has created a bottleneck in code review, with engineers drowning in PRs. Graphite addresses this by integrating AI throughout the pull request lifecycle, featuring an AI review agent, unified inbox, and self-healing CI. The platform delivers measurable results: Shopify saw 33% more PRs merged per developer, and Graphite Agent's comments lead to code changes 55% of the time versus 49% for humans. Companies like Snowflake, Robinhood, and Duolingo use the platform, which handles hundreds of thousands of PRs and saves significant engineering time.

{% youtube https://www.youtube.com/watch?v=xWf6zkw2Ni0 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/0.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=0)

[![Thumbnail 10](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/10.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=10)

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/20.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=20)

### The Code Review Bottleneck: Why AI-Powered Platforms Are Essential for Modern Software Development

 Great. Good afternoon, re:Invent. I want to start by letting you in on a little secret. This is the best one. Many of the fastest  moving companies and engineering teams in the world don't use GitHub for code review anymore. Now what do I mean by that?  What I mean is that they've realized that this new era of agentic software development requires a whole new platform for code review that can keep up with the pace at which software can now be written. And that's exactly what we've built at Graphite. We are building the AI-powered code review platform for this new age of agentic software development.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/40.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=40)

 Well, let's take a quick look at Graphite before we dive in. We are privileged to work with some of the fastest and largest engineering teams in the world, teams like Shopify, Snowflake, Robinhood, and Duolingo. And it's not just a matter of hype. We've delivered real enterprise value for them. Shopify, for instance, after using Graphite has seen 33% more PRs merged per developer after adopting Graphite. Asana has shipped 21% more lines of code per engineer, and we've done this time and time again with hundreds of thousands of engineers across the industry.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/80.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=80)

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/100.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=100)

So before we get into how we do this exactly, I want to talk about why code review  is important right now and why you can't just do this with any AI model. So starting with the first one, you know, why should we even care about code review? Isn't it enough? Can't we just let the AI run with this and write all of our code for us today? And I think that the answer to this, if we think about it, is that writing code, first of all, how many  of you are engineers? Raise your hand if you're an engineer. Okay, pretty much everybody here. So you all know that writing code is only the first part of the story.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/120.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=120)

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/130.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=130)

You know, once we've written the code, we'll build it locally, we'll debug. But after that happens, we still have to go through the entire outer loop process of getting our code reviewed, testing it,  reviewing, getting it merged, and deploying it. And oftentimes at enterprise scale,  this is actually more of the bottleneck than writing it in the first place. Even before this age of AI, my co-founders and I all came from large companies like Meta and Google, where we'd often spend more time waiting for our co-workers to give us code reviews than we would actually building the feature in the first place.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/150.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=150)

So code review has always been a bottleneck at enterprises, and this has always been important. But now, AI is making this 10 times  worse because every engineer has these incredible tools like Claude, Cursor, and Windsurf that they're able to use to build features faster than ever. But again, this is just shifting this bottleneck over to the right here. All of that code still needs to be reviewed. It still needs to be tested, merged, and deployed. And now what we're finding and hearing from many of our largest customers is that their engineers are just drowning in PRs. They can't find enough time in the day to review the PRs that their teams can now create.

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/180.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=180)

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/190.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=190)

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/200.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=200)

 I thought that this tweet really summarized it well. The new bottleneck is no longer doing the work, it's reviewing the work.  So this is our conclusion number one. In this new age of AI, code generation needs AI code review if we're going to truly unlock the power of LLMs for generating code.  Put another way, you can't just apply AI to the IDE. You need to apply it to your entire toolchain if you want to really get the most out of what LLMs can do for software development.

[![Thumbnail 220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/220.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=220)

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/230.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=230)

So that answers the first question, but now you're probably thinking, why can't we just rely on the base LLMs?  Isn't Claude and GPT-5 good enough on their own to be a reviewer? Can't we just hack something together, throw it in a GitHub action, and let it run?  Well, this was actually the first thing that we did when we tried to build this. So in the beginning, before AI was really powerful, we were just building a workflow tool, and the first approach that we had to this when we were experimenting was, let's just take the code diff and give it to Claude and see what happens. So we tried that.

[![Thumbnail 250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/250.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=250)

[![Thumbnail 260](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/260.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=260)

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/270.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=270)

 And these are some of the common responses we got. Things like, you know, you should update the code to just do what it's doing already. CSS doesn't work this way. You should add a comment here. This is a really common  one. Or even you should just revert this code to do exactly what it used to do. It's kind of like you just dropped an intern into your code base in their first week on the job and  expected them to do complicated refactors.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/280.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=280)

So this is the second major realization that we had, was that the LLMs are trained in RL to generate code in  seconds, but reviewing and understanding of code is a much harder problem that needs a lot of specialized engineering. It's an instance of what's called the generator validator gap in LLM research, where LLMs are fantastic at generating valid examples of something. But they're much worse at understanding and validating why something is valid. They can't exactly introspect and tell you why is this correct, why this is matching the pattern that we want it to.

[![Thumbnail 310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/310.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=310)

So  that was the second major realization, and that really brings us to this third point and the most interesting question: how is Graphite achieving this? How are we helping enterprises ship code faster in this new era of AI?

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/330.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=330)

### Graphite's Multi-Pronged Solution: Integrating AI Agents into the Entire Pull Request Lifecycle

It's a multi-pronged approach. It's not enough just to drop in an agent  that reviews code. You really need the whole platform for code review and reimagining the pull request experience completely end to end. And that's exactly what we've done. First of all, we have an AI review agent that scans every pull request in a few seconds. It's able to find bugs, security vulnerabilities, and any of the common issues that a developer would be looking for in a code review. But instead of waiting potentially hours or overnight for a coworker to give you a review, this will give you the review in a few seconds.

So you save hours of back and forth. What would have been many review cycles now can be condensed down into one. We've combined that though with a best-in-class code review experience. So we have an inbox that unifies your workflow and shows you exactly what PRs are waiting on your review and where your changes are in progress. It's amazing that in 10 years, GitHub has not built a way to just organize your workflow and see what you need to do. That's the first thing you see when you log into Graphite.

We also bring you smarter self-healing CI, so Graphite Agent will suggest if a test is failing. It'll suggest a fix and let you just rerun it, requeue it, and get that unblocked. We're about to introduce merge conflict resolution as well. So if something is failing in merge, Graphite Agent will fix it for you, get your quick sign-off, and be ready to keep going. We let you measure developer productivity and we bring it all together in a streamlined platform.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/410.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=410)

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/430.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=430)

 This is a quick view of what the new PR page looks like. Let me see if I can get this to play. So we basically have your PR, but you have a whole chat window right alongside it. In Graphite, I can ask it questions, I can understand this change, I can have it highlight a few issues, and then I can actually give  it an instruction and work with it. So in this case, I'm asking Graphite Agent to swap out the model. It's going to actually make that change in line and propose a code change.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/440.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=440)

 I can open up a whole editor and view the diff that it's generated, and then with one click, I can go ahead and apply that change. This previously would have taken you minutes or potentially hours to go back to your IDE, make this change, pull the changes in, make the change, push it back up, and get a review again. Now this is all just happening in a matter of seconds on Graphite's new PR page. So yeah, much like what Cursor did to VS Code, bringing AI right into the IDE, we're doing that with pull requests with Graphite, bringing an agent in real time that you can collaborate with right into the PR page.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/480.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=480)

So Graphite Agent, yeah, it's not just a matter of being there and working  with you. You know, we can really validate the results of this and look at the rate at which its comments are accepted. Less than 4% of Graphite Agent's comments are downvoted or marked as not accurate, and 55% of comments actually then lead to an underlying code change. This is how we're measuring and validating that every change we make to Graphite Agent or every change Graphite Agent suggests is a good one and that you want it to actually lead to that code being changed. For comparison, humans are about 49% here, so we've actually now exceeded that. Graphite Agent is now better than the vast majority of human code reviewers in terms of the percentage of their comments that actually create code changes in the end.

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/530.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=530)

So you know, going back to the enterprise  impact and talking about this, Shopify has been one of our larger customers and one that's really worked with us and pushed us to help their engineering team move as quickly as possible. You can imagine in the past month or two, they were in their full sprint getting ready for Black Friday and Cyber Monday. It's a huge code push for them. You know, they're doing tens of thousands of PRs in lead-up to this. And with Graphite, we saw real results in terms of developer efficiency.

So 33% more PRs shipped per engineer. We're now up to almost three-quarters of all PRs at Shopify are now being merged through Graphite instead of GitHub. Again, hundreds of thousands of PRs that this is covered. And you know, if you want to really dig into what is the enterprise impact of this, we get to hundreds of additional engineers' worth of developer time saved. You can imagine every single PR, even if you're saving an hour or two off of review time, off of time that they would have been blocked waiting for somebody to unblock them, that's a huge amount of time over an entire year and a huge amount of engineering hours that you're now giving back to the team and letting them spend less time in review and more time doing what matters and actually being able to build.

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/600.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=600)

 We're, you know, zooming out. We're really excited and we're proud to partner with some of the leading companies in the industry.

We talked about Shopify, but we also work with Robinhood, Snowflake, teams like Ramp, and some of the AI-native companies like Harvey, Replit, and Vercel. We're really seeing large adoption of this across the industry and excitement around what AI can do, not just in the IDE but in the outer loop as a whole.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/630.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=630)

Back to the graph I showed a moment ago, what we've built is the best-in-class code review platform on top of GitHub.  We're trusted by some of the very best developers and teams in the industry, and we're really bringing AI to the pull request process and transforming what code review looks like now in this new era of software development.

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/650.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=650)

### Enterprise Impact and Key Takeaways: Addressing the Surge in AI-Generated Code

There are a few key takeaways that I want to leave you with.  Number one, LLMs are really flooding enterprise engineering teams with more code than they can safely review. We now have not only more code being generated than ever, we see about 70% more code per engineer now than we saw back in 2023. It's just a huge amount of more changes that are being created with the help of LLMs now.

There's more code, and it's also potentially less valid, less safe code than ever before. Let's be real, is every single engineer scanning every line that's being generated? Probably not. They're in a rush, they're putting code up, and we almost need better tooling not only just to handle the volume of code, but the nature of AI-generated code necessitates us to have better tooling around making sure that it's reviewed, making sure that it's safe, and making sure that it's validated.

There's more code than ever before, and we need AI code review more than ever. Traditional tools and LLMs, we can't just take Claude or GPT-5 and throw it at this and expect it to do well. We really need a whole new platform for code review for this new era, and that's exactly what we've delivered.

Graphite has built that platform. We've integrated AI into the entire pull request lifecycle from the moment it's created to the moment that it's merged, and we're delivering really great measurable enterprise outcomes by doing that. Thank you very much. That's all I've got for you.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/cf15b20cb9c47bdf/750.jpg)](https://www.youtube.com/watch?v=xWf6zkw2Ni0&t=750)

You can find us at graphite.com if you want to check it out. You can try the platform out for free for 30 days, and we're right around the corner over at Booth 535 if you want to come check us out. We've got plenty of people who can tell you more about the platform and the details, and we're excited to chat with you. Thank you very much, everyone. 


----

; This article is entirely auto-generated using Amazon Bedrock.
