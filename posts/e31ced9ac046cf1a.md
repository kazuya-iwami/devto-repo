---
title: 'AWS re:Invent 2025 - AI Agents – the new face of privileged machine identities (SEC226)'
published: true
description: 'In this video, CyberArk presents their Secure AI Agents Solution, addressing AI agents as privileged machine identities requiring comprehensive security controls. Venu Shastri and Anat Eytan-Davidi explain how AI agents combine human-like reasoning with machine-scale operations, creating unprecedented risks as they access enterprise resources. They introduce the CyberArk AI Agent Gateway, which enforces dynamic context-aware policies, implements just-in-time least privilege access, and provides audit traceability. The session highlights OWASP''s top 10 AI risks, the Model Context Protocol evolution, and announces general availability by year-end. They also showcase open source security toolkits available on AWS Marketplace, including the CyberArk MCP server and Agent Guard, emphasizing the need to secure agents now while deployments are manageable.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - AI Agents – the new face of privileged machine identities (SEC226)**

> In this video, CyberArk presents their Secure AI Agents Solution, addressing AI agents as privileged machine identities requiring comprehensive security controls. Venu Shastri and Anat Eytan-Davidi explain how AI agents combine human-like reasoning with machine-scale operations, creating unprecedented risks as they access enterprise resources. They introduce the CyberArk AI Agent Gateway, which enforces dynamic context-aware policies, implements just-in-time least privilege access, and provides audit traceability. The session highlights OWASP's top 10 AI risks, the Model Context Protocol evolution, and announces general availability by year-end. They also showcase open source security toolkits available on AWS Marketplace, including the CyberArk MCP server and Agent Guard, emphasizing the need to secure agents now while deployments are manageable.

{% youtube https://www.youtube.com/watch?v=hcwZ-TpypfA %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/0.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=0)

### AI Agents as Privileged Machine Identities: Understanding the Potential and Rising Security Risks

 Hello, hello. How is everyone doing? I realize this is day three, and the only thing keeping between you and a happy hour is me, so I'll try and make it interesting. I know you've heard AI agents all through the conference, but trust me, this is going to be really interesting. We're going to be talking about AI agents, the new face of privileged machine identities. I'm Venu Shastri. I'm a Senior Director of Product Marketing for Platform and AI Solutions at CyberArk, and joining me for the session is my colleague Anat Eytan-Davidi, Senior Manager for Product Management, who's leading our Secure AI Agents Solution.

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/50.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=50)

All right, so I don't want to assume that everyone here knows about CyberArk, so just a quick intro.  We are an identity security leader that offers a comprehensive platform for securing all types of identities: humans, machines, and the new wave of autonomous AI agents, all to ensure that they have the right level of privilege control. Now some of you might recognize us as a Privileged Access Management leader, but we offer way more than that, including things like Identity and Access Management, Threat Detection and Response, Governance, comprehensive Machine Identity Security, and so on. And the idea is that we provide all these capabilities to every identity, human, machine, or AI, every time they are trying to access any kind of enterprise resource, whether it's an endpoint or infrastructure or cloud, to drive all kinds of value in terms of cyber risk reduction, enabling business resilience, ensuring compliance, and increasing efficiency. And in case you missed the news, recently Palo Alto announced its intent to acquire CyberArk as a part of its broader platformization strategy.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/120.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=120)

All right, so let's get started.  Today's focus is going to be on Agentic AI. Now AI agents are rapidly becoming the new face of privileged machine identities in the enterprise. You have heard a lot about it, so I'm just going to quickly skim through the immense potential they have to offer, but I'll quickly get into why it is important to think about the identity security aspect of AI agents. And then I'll have my colleague Anat talk to you through our Secure AI Agents Solution before I wrap it up with summary and next steps.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/150.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=150)

All right, so starting off with the potential.  Now I know that Agentic AI is really exciting for a lot of organizations. It has the potential to unlock trillions of dollars across the industry. This could be whether it is software engineering, sales, marketing, customer operations, support, and so on. And early experiments have shown that it's not all hype. There is real measurable gains that a lot of customers are getting even as they start off on this pilot. But what is important is that it uncovers unprecedented risk, especially as these AI agents are given elevated privileges to access enterprise resources. Now, by definition, AI agents means that you're giving agency to the LLM to access and do things. And that's where the risk is. So these risks not just stem from external threat actors. We need to realize that we are still in the early stages of AI agents and they are prone to hallucinations or they might misunderstand the context, so they might end up doing things that they were not intended to do.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/210.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=210)

 So further, what we need to realize is that as Agentic AI adoption grows, the risks are going to multiply because the agents are going to operate at different maturity levels. Now for a lot of folks, AI agents typically means an AI assistant. I'm doing something with Copilot and it's doing something on behalf of me. So that's where we are today in terms of a lot of the actual agent deployment in production. But as you go down this chain, it's only going to get more and more complex. You're going to have organizations that are experimenting with completely autonomous agents with much broader access to enterprise resources. This may get triggered based on, let's say, an event like a customer support call or a bank teller interaction and so on. And that's when you start getting more business value, but that's when the risk starts increasing.

And within a year or less, take my word for it, you're going to start seeing way more complex agentic systems. You heard on the keynote today on the way we are progressing here. So that's when you have multi-level orchestrated agents in turn invoking other agents and humans to do multi-step tasks across systems. That's when the complexity is going to increase, the access to these set of agents increases, and the risk goes through the roof unless you have already started planning towards that. And that's where we come in.

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/300.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=300)

 So now let's kind of talk about the AI agents, the fact that they are making decisions and they are able to reason. That the elements are able to reason makes them really useful in the enterprise context.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/350.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=350)

And then as we start giving them more and more agency to access enterprise resources like applications, machines, databases, and so on, a new standard has evolved. Anthropic came out with Model Context Protocol, and the first version of it did not really address anything around authentication or authorization. Again, things are moving so fast we quickly realized that that's not the way to go, and so we are focusing a lot on how this can be improved. So the next version of the standard is coming out, and we know that that is going to focus a lot on using OAuth as a standard to kind of access these resources and ensure we have a standardized way  for the agents to access those resources.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/410.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=410)

Now the other part is in terms of what are the new risks that come out of this. OWASP has done a pretty good job of trying to outline the top 10 risks, and each of these risks may be targeting a different part of the agentic system. Some of them are targeting the components like the applications that they are using. For example, the agent behavior one targets the LLM so that it can be manipulated and do things like jailbreaking and so on. Same way, agentic supply chain targets the applications that it does as a part of its supply chain. So there are different ways that threat actors can use it, but a common theme that you see across all of this is the fact that a lot of them are going to use the privileges that the agent has on those applications, and that's where the identity component comes in. That's where it becomes imperative. In fact, if you heard Swami talk about it today in the keynote, he specifically pointed out that identity is becoming a  critical control plane for this.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/420.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=420)

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/470.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=470)

So now let's talk about that identity security imperative. I'll try and make it quick because I want to give enough time for Anat to get into the details.  So what we see is that AI agents are really kind of this new identity class that shows some characteristics of human identity and some of machine. They are able to reason, make decisions, and they work more like AI assistants. A lot of immediate focus is that AI agents should be done like service accounts, but if you look at the autonomous agents, that's where the agents are behaving more like cloud native machine workloads. They're ephemeral in nature. Traditional MFA will not be an answer there. You need to introduce humans in the loop as a part of the process. So overall, AI agents should be treated as privileged machine identities because they are, at the end of the day, they're still machine identities. They're highly privileged because they have all access to enterprise resources, and they operate at machine scale and speed. 

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/500.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=500)

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/510.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=510)

So our belief is that the only way to solve this problem of securing AI agents is to combine these two, like securing human and machine aspects of human identity including credential management, zero standing privileges, session monitoring, isolation, and all of those, as well as securing machine identities which includes things like strong authentication, secrets rotation, certificates, PKI, workload access, SPIFFE, and so on. So we are really excited if you have missed the news that we will be announcing the general availability  of the CyberArk Secure AI Agent solution by the end of the year. Now the vision is to extend the same set of privilege control that we do for humans and machines  to the world of AI agents.

[![Thumbnail 550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/550.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=550)

Now our solution is built to address all the top concerns that CISOs bring up. Do I already have agents running in my organization? That's discovery and context. How can I ensure AI agents are operating securely? That's securing access and enforcing zero standing privileges. How can I quickly respond if my AI agent is going rogue or has gotten compromised? That's threat detection and response. And how can I govern AI agents to ensure compliance? That's governance and lifecycle. So our solution covers all of these aspects. Now I realize that this is a lightning session, so we're  not going to have time for all of this. So I'm going to get my colleague Anat to get a little more details into the secure access part. The quick shout out I wanted to have here is because you heard about AWS Bedrock. So in fact that is one of the target agentic ecosystems for our discovery and context, so that gives you a way to kind of start utilizing the solution in combination with AWS Bedrock. Over to you Anat.

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/590.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=590)

### CyberArk AI Agent Gateway: Securing Access Through Dynamic Controls and Least Privilege

Perfect, thank you so much Venu, and indeed we are going to review the solution and we are going to focus on the secure access piece and how we can ensure that we can secure the AI agent access. So let's start by understanding what it  actually means to secure an AI agent. So first of all, we need to control what the AI agent can do, and more importantly what the AI agent shouldn't be doing.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/630.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=630)

Now there are cases where a bad attacker or even the AI agent itself can bypass those controls and guardrails. So we also want to ensure that the AI agent cannot perform any unintended action. The third challenge, and from talking with a lot of customers they see this as their first challenge as well, is how we can monitor and audit who did what when working with  AI agents.

So before diving into the solution, let's understand the complexity that we see when granting access to an AI agent. We have an example of a reporting agent that can be used by different personas in the organization: HR, marketing, finance, sales, and so on. That reporting agent can have access to different databases within the organization. But we want to make sure that as an HR employee I can access only my HR databases, as a finance employee only my finance databases, and as a finance manager I might have different permissions on my finance database.

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/700.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=700)

On top of that, even as a finance manager, if my AI agent is trying to attempt some risky or sensitive action, I want to provide the approval for that access. So whenever we are looking at the access of the agent, we need to understand that we have the same agent, different users, different intents of what that agent might be doing, and therefore we need to grant different access. 

So as Venu mentioned, AI agent identity is privileged identity by definition. It has access to the most sensitive resources in the organization: our SaaS applications, the databases where we store our customers' data, our environment where we run our solution. So as an identity, we need to treat them as privileged identities. And again, AI identity at the end of the day is a combination of a machine identity that behaves like a human. That's the reason that we need to take the controls from both identities and combine them into a single solution that actually addresses the unique characteristics of an AI agent.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/750.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=750)

And with that, I'm happy to share that we are introducing  CyberArk AI Agent Gateway. Our gateway is an enforcement point between the AI agent and the tools and the targets that the AI agent can access. With the AI Agent Gateway, we can actually control the access, we can ensure to have human in the loop whenever needed, we also ensure that we have least privilege permissions, and we will talk about why it is so important to control the permissions. And lastly, we will be able to provide audit and traceability.

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/790.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=790)

So let's talk about the access. The traditional access controls  and policies that we have in place are too static for the dynamic and complex nature of AI agents. We need to move to a different access policy and access controls for AI agents. It needs to take into consideration different factors such as the user that is using the AI agent, the agent itself, the risk, the context of the request, and much more. That's the reason that we are introducing a dynamic context-aware policy to address the access and to control what are the actions and what are the tools that AI agents can do or shouldn't be doing when accessing their target resources. So this is the first piece.

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/840.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=840)

So let's now talk about what happened and how we can prevent any unintended action.  So again, as mentioned, AI agent identities have access to the most sensitive environments and targets in the organization. Now we already know that excessive permissions is a huge problem both for human and machine, but we are not going to talk about that in the session right now. But because of those excessive permissions and taking into consideration the way that AI agents work, just imagine what will happen if the agent identity is being compromised or the underlying human identity is being compromised with those excessive permissions. The damage can scale immediately.

We can run thousands of privileged permissions in minutes and not in days because the AI agent operates at machine speed. That's the reason that we need to control the permissions an AI agent has. We need to ensure they always operate under the principle of least privilege.

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/930.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=930)

The permissions are granted just in time, only when needed, and revoked afterward in order to ensure that we don't have any excessive permissions that someone can abuse. Already today we have a solution for least privilege and just in time access for AI agents with our MCP server, which is available in AWS Marketplace. 

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/940.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=940)

So let's move to the last challenge.  We actually want to restore visibility because, again, the current audit trail that we have today is not sufficient for the AI domain. We need to understand if we have an AI agent operating on behalf of the user who actually ran the action. On the target, is it me as an admin or is it my AI agent that worked on behalf of me? What were the actions that were run on behalf of the user or as an autonomous AI agent, and most importantly, why?

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/980.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=980)

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/990.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=990)

With our AI agents sitting in the middle between the agent and the tools, we are able to restore that visibility again.  So before wrapping up, I do want to share something with our AI agent builders.  We know that there is a common practice, it is not a best practice, where developers are actually loading API keys and secrets into environment variables, and that introduces several security risks. Among them is memory exposure and unintended leakage because the information is available in those environment variables and we are exposing very sensitive information using those environments.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/1020.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=1020)

That's the reason that we are very happy  to share our open source security toolkit with you. With that security toolkit, you're able to reduce the risk and integrate the toolkit with your AI agents without code changes, ensuring that the credentials are injected just in time, removed when they are not needed anymore, and we are integrating with the most common security stores like AWS Secrets Manager and CyberArk Conjur. So this is available already today at GitHub and I'm inviting you to use it in order to reduce the risk that we already have today.

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/1060.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=1060)

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/1080.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=1080)

### Taking Action Today: Key Takeaways and Available Resources for Securing AI Agents

 So with that, let's summarize. Sure, thank you, Anat. So I hope that you've all found this session helpful. Again, just a quick takeaway, right? The fact is that AI agents are here to stay, right?  And you need to start thinking about securing those agents today while the numbers are still manageable and most of those agents are still not fully in production, right? So you need to start thinking about securing these agents today, right?

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/1120.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=1120)

The fact is that these are going to open up new attack vectors, and as the number of agents increase and as the scope of access increases because these agents get mature, it's going to be harder and harder to control them. So the time to think about securing those agents is today. The way to do that is using identity security as your foundation, right? You ensure that these agents from the get-go have least privilege. You're putting in tight privilege control. You're having just in time access and just enough access so that you are able to keep the attack surface minimal. 

All right, so what's next? Now our solution is going to be generally available at the end of this month, so feel free to swing by our booths if you want more details about it or you want to talk to one of our specialists. We also have some interesting white papers. We had done an interesting study with McKinsey to look at how organizations are really adopting agentic AI and what are the types of risk it is surfacing, right? And it talks about why identity is the foundation for the defense against these agents, right?

So do download that white paper. The other one is a more technical white paper which talks about the key requirements to securely manage these identities. So feel free to download this or scan those QR codes. Now if you're not exactly fond of reading and you want or are itching to try something, then we have some cool developer tools. Anat had said about why it is really important to shift left and start having those best practices as a part of building your agents, right?

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/e31ced9ac046cf1a/1200.jpg)](https://www.youtube.com/watch?v=hcwZ-TpypfA&t=1200)

So here are the QR codes for a couple of interesting tools, the CyberArk MCP server for our secure cloud access, as well as CyberArk Agent Guard. Both are available in the AWS Marketplace to download so you can try them out. Thank you for your time again. I hope you enjoy the rest of this conference. We will be around to take any questions. Thank you. Thank you. 


----

; This article is entirely auto-generated using Amazon Bedrock.
