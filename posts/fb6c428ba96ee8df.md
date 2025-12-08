---
title: 'AWS re:Invent 2025 - Deploy AI Agents with Confidence & Control with Rubrik and AWS (ISV320)'
published: true
description: 'In this video, Mike Gillespie from AWS and Devvret Rishi, General Manager for AI at Rubrik, discuss deploying agentic AI at scale with proper governance. Rishi presents the four stages of agentic maturity (experimentation, formalization, proliferation, autonomous) and reveals that over half of 180 organizations surveyed remain in experimentation phase. The main challenge isn''t building agents but managing risk and governance when moving from POC to production. Rubrik Agent Cloud is introduced as a solution offering three pillars: monitoring and observability through agent inventory, governance with enforceable policies, and AI Agent Rewind for undoing destructive actions. The platform integrates with AWS Bedrock Agent Core to provide visibility and control over agents operating in production environments.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/0.jpg'
series: ''
canonical_url: null
id: 3088721
date: '2025-12-06T11:18:45Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Deploy AI Agents with Confidence & Control with Rubrik and AWS (ISV320)**

> In this video, Mike Gillespie from AWS and Devvret Rishi, General Manager for AI at Rubrik, discuss deploying agentic AI at scale with proper governance. Rishi presents the four stages of agentic maturity (experimentation, formalization, proliferation, autonomous) and reveals that over half of 180 organizations surveyed remain in experimentation phase. The main challenge isn't building agents but managing risk and governance when moving from POC to production. Rubrik Agent Cloud is introduced as a solution offering three pillars: monitoring and observability through agent inventory, governance with enforceable policies, and AI Agent Rewind for undoing destructive actions. The platform integrates with AWS Bedrock Agent Core to provide visibility and control over agents operating in production environments.

{% youtube https://www.youtube.com/watch?v=LVH2mBpXshg %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/0.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=0)

[![Thumbnail 10](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/10.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=10)

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/20.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=20)

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/30.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=30)

### The Challenge of Scaling Agentic AI: From POC to Production

 Great. All right, thanks everyone for joining us today. My name is Mike Gillespie. I'm a Principal Solutions Architect with AWS. I've been with AWS a little over nine years.  Hi everyone, I'm Devvret Rishi. I'm General Manager for AI at Rubrik, a data cybersecurity and resilience company. So we're here today to talk about how Rubrik  can help you roll out agents at scale with confidence and control, but first I'll talk a little bit about how customers come to me and  oftentimes they will say we really want to take advantage of agents or we're having trouble with our organization. How do we get better control and visibility into the things that are happening within our account? Especially when you talk about agentic AI, and Devvret will talk about it shortly, that's a big concern for our customers and a real big roadblock from going from POC to production. It's probably the number one thing that's stopping people from doing that.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/70.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=70)

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/80.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=80)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/90.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=90)

So how do you get over that hump, and that's why Devvret is with us today, and also how do you do that cost effectively? And the important thing is if you're an AWS customer working with Rubrik, we work hand in hand very closely together, so  we're very good partners. They're very familiar with the AWS environment, and Devvret, why don't you take it from there? Yeah, absolutely, and thanks for the introduction, Mike.  I think just to give everybody a little bit of the background on myself, I'm Devvret. As I mentioned, I'm General Manager for AI at Rubrik, but that's actually a relatively new job title.  Until this summer I was co-founder and CEO of a generative AI infrastructure startup called Predibase. We were essentially the model backbone for a number of customers that were deploying high volume large language models into production. And so a lot of these organizations were really on the cutting edge for where AI was actually already being deployed, especially in really high throughput scenarios.

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/130.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=130)

And we joined forces with Rubrik this summer to think about what would it look like to bring some of the same classes of technology that we had built and piloted with some of the leading tech companies over to the Global 2000 Enterprise and beyond. And for those of you that may or may not be familiar with Rubrik, Rubrik's actually a little bit of a difficult company to define  when you join a company via an acquisition. You need to come up with your own framing for what that company does. The way I actually think about Rubrik is it's a company that's helped organizations accelerate different enterprise transformations in the past. What does that actually mean? Well, Rubrik started off in its core as a data backup and data resilience company around the same time that we were making that graduation from tape onto digital. Rubrik really started to revolutionize the world of backup and recovery.

The next big shift, I'd say in large part thanks to our friends at AWS, was the shift to cloud, and one of the key considerations as organizations are making that shift to cloud was really around security. And that's when Rubrik rolled out the Rubrik Security Cloud as a way for it to become a lot easier as you make that transition and have more cloud native opportunities. And now I think we're on the advent of the next transformation era, which is the AI transformation era. Within this, you've probably heard a lot about AI transformation at re:Invent already. Yeah, it's hard to walk past the booth without it saying AI. The underlying trend that we've really decided to be able to underwrite is what we call the rise of agents or agentic AI.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/200.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=200)

 Now, many different people have different definitions for what agents are, so I want to start off with my own. The way that we define agents are just LLMs with access to tools. So you can think about that as models that are capable of interacting in a production setting and taking action on your behalf or on an employee's behalf. Over the last four months since I've had a chance to join Rubrik, I've talked to over 180 different organizations, existing customers, as well as net new prospects in IT, in security, in engineering, and in AI organizations. And probably the most clear trend that I've seen is that agents are coming.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/240.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=240)

 In fact, in a lot of our slides that I've used, I've had this slide around agents are coming, and the enterprise has told me no, agents are already here. The challenge that I think that a lot of organizations have is no longer what it looks like to be able to build agents. Thanks to really great tools like AWS Bedrock Agent Core, as well as a number of other agent builder tools, it's actually never been easier to start to build your first agent today. Hooking an LLM prompt up to a few models may only take days or weeks in order to be able to really validate the end to end experiment. But what customers tell us is that their real concern is around what happens after that initial experiment. How do I actually operate and govern these agents really at scale?

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/290.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=290)

After the 180 conversations that we had, we created something that we call the four stages of agentic maturity. These are the four stages that  I see customers really go through as they go from ideation on we should do something with AI all the way to what I consider AI native enterprises. The first phase starts with experimentation. This is usually the area where you're starting to build maybe your first or your second agents.

The second stage is what we call formalization. I think that graduation from phase one to phase two is actually the most important one that enterprises go through, where the agent builder experience goes from cool prototype or demo to something that actually operates within a context that the enterprise can consume. We'll talk a lot more about what this actually requires in just a minute.

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/340.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=340)

The really interesting thing I think about the rise of agentic AI is then stage three, which is proliferation. One thing we've noticed is it can take weeks, it can take months or quarters, sometimes even a year to get to stage one or two.  To build your first agent, your second agent, even your third agent can take quite a bit of time. But by the time you've actually done that rinse and repeat process of building your first, second, or third, we've seen organizations scale up to hundreds quite quickly. That's what we call the proliferation stage.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/370.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=370)

Then finally we have autonomous, which is relatively uncommon, but this is when agents are actually operating with full write permissions and actually acting on behalf of organizations. Out of the 180 customers that we spoke to, this is how we classified the breakdown. Over half were still  in experimentation phases. About a quarter were in what we call formalization, and you can see the balance here. The ones that aren't represented, about 13%, had not yet started on the AI adoption lifecycle.

### Understanding AI Agent Risk: Why Governance Becomes the Bottleneck

Just out of curiosity, out of a show of hands for everyone in the breakout, who would classify themselves as still in the experimentation or not yet started phase? Okay, and I'll just assume that the balance are probably in the next phases or so beyond. As organizations start to make that shift from experimentation to formalization, one of the key concerns that we've heard reported is that it's no longer necessarily the difficulty of building these agents, but it's how to think about and manage risk.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/420.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=420)

Now this is a problem that is actually very close and near and dear to Rubrik's heart.  I mentioned a little bit of how Rubrik had evolved across time. Turns out the data backup market had evolved across time as well. Data backup used to be about protecting business continuity in the world of natural disaster, fire, flood, then it became about cybersecurity because the number one threat vector was no longer a flood, it started to become ransomware. And I think our view is that in the future, the real threat vector that organizations are predominantly concerned about now is going to be AI in nature, whether that's malicious or inadvertent, like an example of a coding agent earlier this summer that went viral because it got access to a production database and decided that the best way to optimize a subroutine would be to drop the database and delete it altogether.

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/470.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=470)

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/480.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=480)

And so we see that agents and AI have this capability to be able to do 10x the damage and potentially in one-tenth the time.  And the truth is agents are powerful for the same reason that they're difficult to actually govern. They are unpredictable, and oftentimes they're what I would consider  weakly supervised in a lot of organizations. There's a governance process before they could get rolled out where someone has to approve what it is they do, but rarely does that actually have hooks into what they're doing at production time in real time.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/520.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=520)

In addition, they're operating with non-human identities, which I think makes it really difficult to think about them at scale. And one of the most consistent questions that I think I hear from enterprise organizations is how am I supposed to rationalize all the different things that AI agents can do, the nondeterministic way that they might operate as well. They're probabilistic systems in nature with the fact that they have production systems, how do I actually,  how do I actually have a framework for thinking about what could, what do I do when something goes wrong?

And for better or for worse, these instances of where things can go wrong are not theoretical. We see them even in the very early days of AI agent adoption today on things like AI agents recommending competitor products, hallucinating things like refund rules, or as we showed earlier, potentially deciding that the fastest way really to optimize something might be to delete it altogether. If we think about the common refrain you've probably heard in the news or even here, it's about are enterprises seeing ROI from AI adoption. My view is that enterprises aren't quite seeing that ROI yet because AI is not even necessarily being able to be pointed in attacking the most important problems, because the most important problems are too risky to be able to place directly in these agents' hands.

[![Thumbnail 580](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/580.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=580)

And I sit on Rubrik's AI governance committee, so I actually feel very viscerally why some of these challenges are hard to be able to enforce in practice.  One of the things I told you by definition of how we described agents is that agents operate on production systems. You can think about this as Salesforce, Microsoft 365, your AWS environment, but they operate on these systems that weren't necessarily architected to be able to support an AI native workforce. And so what we typically hear from customers is this thought process that AI is moving quite quickly and so the proliferation of agents

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/610.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=610)

either is going to start, has already started, or is something  they see happening over the next 6 to 12 months. But they don't actually have a system that gives them the initial set of answers that we consider table stakes on the single pane of glass. It oftentimes starts as simply as do I have a good answer to be able to say what agents and AI are actively running inside of my ecosystem today? Can I quantify the risk on what tools and data they can access, all the way down to what do I do when something actually goes wrong and how do I recover?

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/650.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=650)

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/660.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=660)

I think a lot of these same types of concerns is what we saw at Rubrik as we were a first party building out agents and agentic platforms internally. What we saw was that building that initial proof  of concept was something we could typically do with IT or engineering in about 2 weeks. Where we got stuck was when we had the need for approvals on each marginal  project. I see a few people nodding their heads, which I assume means that people have been through this process before.

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/680.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=680)

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/700.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=700)

The frustration I think here really stems from the fact that we're asked to go very quickly, but we also are continuously needing to review and approve each individual project. What our team really asked for was rather than  having to go through the system where I need to then go resubmit new approvals every time I go, and my initial timeline goes from weeks to months, give us a framework that we can operate in. As long as we're operating within that framework, let us build the different types of agents that actually are sticking close to that so we can get to our end goal of really being able to deploy into  production.

So that's the long road that I think exists today, and to be frank, I don't think a large part of that road today is actually necessarily technical in nature. It is a lot of interorganizational challenges and scaffolding. But those challenges are very real because of the risk that AI agents can pose if not properly governed and if not properly operated.

### Rubrik Agent Cloud: A Three-Pillar Solution for Agent Operations at Scale

That's why Rubrik's interested in this space. As a company that's helped organizations and enterprises through business transformations, we just released our newest product, which we call the Rubrik Agent Cloud. The Rubrik Agent Cloud really takes some of Rubrik's core understanding on what we call data and metadata. If you remember, Rubrik started off as a company with a rich understanding of what was the data that needs to be backed up and metadata, which is essentially what's the application context for where that data goes.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/750.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=750)

Rubrik layered on an additional set of offerings in identity.  The observation was that most production and ransomware attacks actually happen because an identity system is compromised. So Rubrik said we not only need to know where all the data is, but we need to know what are the identity systems that are underpinning this organization and who has access to that data. Rubrik started off with these two components, and then with the acquisition of my company this summer, we brought in an LLM and AI platform that users were using for agents and agentic deployments. We think that these three ingredients really position us well to be able to solve the challenges that exist around operating agents at scale.

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/800.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=800)

If you're like a number of folks that I've talked to, this slide sounds good, but then the first question is what does agent operations actually mean? To us, agent operations really means three key pillars.  The first is starting with monitoring and observability. You can't govern, you can't improve, you can't fix what you can't see. What Rubrik Agent Cloud does is it hooks into the environments that you have, including through our integration with Bedrock Agent Core, to automatically discover agents and populate what we call an agent inventory, providing you a software-defined, non-manual way that you can actually see what are the agents that are running inside your ecosystem, what are the identities associated with those agents, and based on those identities and agents, what are the tools and data they have access to.

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/860.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=860)

The second pillar is something we call governance. I asked who here has an organization where there is an AI governance committee as an example, or your organization has AI policies. I think most of the folks actually in the room are probably in that boat. One of the unfortunate realities is a lot of times governance includes a lot of policies that are very difficult to enforce in practice. We say things like AI agents should not be able to give financial advice on  behalf of the company, or AI agents should be read-only by default but not write. While this gets approved at a point in time, very few places offer essentially the teeth that are able to stick into any sort of agent flow and actually monitor to determine whether or not those governance policies are being appropriately followed. Our governance pillar really has the two abilities of being able to define policies and then enforce policies that agents or other systems need to be able to track.

The final capability that Rubrik Agent Cloud brings to the table is something we call AI Agent Rewind. Agent Rewind is a unique capability based on Rubrik's core DNA to be able to help with resilience and recovery. Agent Rewind allows you to undo a destructive action by an agent if it's taken on any sort of protected property that Rubrik already protects for your business.

So if your agent, for example, went off and deleted a production database by mistake or made the wrong edits to a column in Salesforce, Agent Rewind allows you to correlate those agent actions with the previous healthy snapshots that exist inside of Rubrik's backup and do a one-click undo, essentially using both of those components of our technology.

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/950.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=950)

Our view is that the Rubrik Agent Cloud helps solve one of the most compelling challenges that organizations face when it comes to responsible deployment of AI and really faster deployment of AI. And with that, we've really built the Rubrik Agent Cloud to  be able to unleash agents, not risk. Hopefully if you've been around Vegas and the Strip, you've seen that a few more times, but to be able to unleash agents and not risk, building on top of the same primitives as the Rubrik Security Cloud.

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/970.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=970)

We have been really in large part motivated to be able to solve this challenge based on the same types of experiences  that we saw with Rubrik as Customer Zero, as well as the customers that we've spoken to over the last four to five months. What we've seen is that even at Rubrik Customer Zero, we see this where we have hundreds of use cases for agents across organizations, complex sets of requirements because we need to balance Infosec, Legal, IT, and others, and policies that need to be made in the process and be auditable as well.

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/1030.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=1030)

So we've taken really, I would say, a mix of our experience first party as well as the experiences that we've had interacting with a lot of our customers and prospects over the last since this summer, and we've started to combine them into the actual platform that's now available in preview today. So with that, I just want to thank you for attending the talk. I'll skip through this animation, but for attending the talk that we're able to give here in terms of how to be able to actually accelerate AI adoption in the resilience and AI era. 

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/fb6c428ba96ee8df/1050.jpg)](https://www.youtube.com/watch?v=LVH2mBpXshg&t=1050)

And if you're thinking at all about how to be able to deploy AI and AI agents more effectively at scale, please do come check out our booth where we'll be able to show you some live demos of how the Rubrik Agent Cloud actually works in practice. It's difficult to miss on the expo floor, or please grab me right after this, and more than happy to talk through this further. Thank you everyone. 


----

; This article is entirely auto-generated using Amazon Bedrock.
