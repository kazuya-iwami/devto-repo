---
title: 'AWS re:Invent 2025-Meta''s insights on implementing AWS security controls at scale (SEC308)'
published: true
description: 'In this video, AWS Solutions Architects and Meta''s Senior Engineering Manager discuss securing cloud infrastructure at planetary scale, focusing on Meta''s 3.5 billion daily active users across 150 countries. The session covers AWS''s cloud security playbook with three key domains: identity (using Identity Center and Service Control Policies), network (zero trust access and VPC Lattice), and resources (KMS key management and data perimeters). Meta shares their hybrid cloud architecture, including IPv6-only infrastructure, Auth-D credential vending service, and privacy-aware infrastructure with data lineage tracking. A detailed case study examines securing AI clusters on AWS using EKS, Direct Connect with MACsec encryption, AWS Network Firewall for ingress/egress filtering supporting one million CIDRs, and storage solutions including S3 Express One Zone and FSx for Lustre. The presentation emphasizes five principles: automation at scale, security by design, federated innovation within guardrails, resilience through threat detection with GuardDuty and Wiz, and building a blame-free learning culture from incidents.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/0.jpg'
series: ''
canonical_url: null
id: 3093082
date: '2025-12-08T19:54:33Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025-Meta's insights on implementing AWS security controls at scale (SEC308)**

> In this video, AWS Solutions Architects and Meta's Senior Engineering Manager discuss securing cloud infrastructure at planetary scale, focusing on Meta's 3.5 billion daily active users across 150 countries. The session covers AWS's cloud security playbook with three key domains: identity (using Identity Center and Service Control Policies), network (zero trust access and VPC Lattice), and resources (KMS key management and data perimeters). Meta shares their hybrid cloud architecture, including IPv6-only infrastructure, Auth-D credential vending service, and privacy-aware infrastructure with data lineage tracking. A detailed case study examines securing AI clusters on AWS using EKS, Direct Connect with MACsec encryption, AWS Network Firewall for ingress/egress filtering supporting one million CIDRs, and storage solutions including S3 Express One Zone and FSx for Lustre. The presentation emphasizes five principles: automation at scale, security by design, federated innovation within guardrails, resilience through threat detection with GuardDuty and Wiz, and building a blame-free learning culture from incidents.

{% youtube https://www.youtube.com/watch?v=5rTuf6bsYlM %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/0.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=0)

### Securing Digital Experiences for 3.5 Billion Users: Introduction to Meta's Cloud Security Challenge

 Fantastic. Thank you for being here. I'd like you to imagine something with me for a minute. Picture yourself responsible for securing the digital experiences of 3.5 billion people. That's nearly half the world's population logging in every single day, sharing memories, connecting with loved ones, and building communities across 150 countries, each with their own regulatory requirements, compliance standards, and security expectations. Now here's the thing. That isn't a hypothetical scenario. That's a Wednesday afternoon at Meta.

I'm Robin Rodriguez, a Solutions Architect at AWS, and I'm thrilled to be here at re:Invent with you all today. On stage today we've got myself and some incredible colleagues: Peter Nieuwenhuizen, a Senior Engineering Manager at Meta, and Syed Shareef, a Security Solutions Architect at AWS. Syed and I have the privilege of working alongside Meta's team to help deliver secure experiences that support billions of users, and the scale of the challenges that these teams tackle is truly remarkable.

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/90.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=90)

 Today's session is about what we call fortifying the cloud, but it's really a lot more than that. It's about how one of the world's largest technology companies has reimagined security for the cloud era. It's about the hard lessons learned, the innovative solutions discovered, and the practical wisdom that can benefit organizations of any size. Whether you're managing thousands of workloads on AWS or just beginning your cloud journey, these principles that we're going to share today can transform how you think about your security.

Over the next hour we're going to talk about this AWS cloud security playbook. What are some of these key principles for fortifying your identities, your networks, and your resources in AWS? We'll discuss the unique challenges that Meta has faced with their hyperscale infrastructure and how they've evolved to a hybrid cloud. Peter's going to share how Meta has implemented security controls across those identity, network, and resource domains, as well as these real world lessons learned. We'll do a deep dive into how they secure AI clusters at massive scale and also what's next in this evolution of security. I'm going to have Syed join us up on stage and walk us through the AWS cloud security playbook.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/210.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=210)

### The AWS Cloud Security Playbook: Key Characteristics of Security at Scale

Thank you, Robin. Let's start with a playbook for AWS cloud security, and what does that look like?  I have the unique privilege of being a security specialist at AWS for more than six years now, working with some of our largest customers. Based on our collective understanding of security at scale, we found some of these key characteristics that apply to all customers when they're doing cloud security.

Security at scale is defined, especially for hyperscales like Meta, as the convergence of ephemeral workloads, distributed systems, and automation. It's absolutely necessary at that scale when you're trying to secure. Edward Deming, the father of quality management, once said you cannot inspect your way to quality. It has to be embedded in the process. Similarly, security at scale has to be embedded in the process all the way from the design phase, built into the development phase, and embedded in the deployment phase and operationalized in runtime.

When we say security is federated, what we mean by that is security is no longer a gatekeeper or somebody who's giving a yes or no answer. It's rather woven into the fabric of your cloud deployments. The objective shifts from preventing change to making sure change occurs within the established safety parameters. Resiliency in cloud security is not merely the ability to prevent bad days. We all understand bad days happen. The idea is how well can you withstand and recover from them.

The goal is to be anti-fragile. If you've read Nicholas Taleb's book, you'll understand this concept. So what does that look like? Instead of having a firewall that breaks under a DDoS attack and fails open, you have automated rules that add the bad IPs to your web application firewall and then create honeypots so that you can actually withstand that attack and recover from it.

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/390.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=390)

When operating at scale, a thing that we see very often is companies have a virtuous loop that is created by learning from past events. We all agree that bad days are going to happen, and it can be Thursday morning. But this can only be possible when there is an iterative process and teams can create controls from yesterday's event for tomorrow's protection. This really relies on a culture that is blame-free, incident analysis that focuses on the technology and timeline of the attacks and not the people or human error, thus transforming every incident into a  valuable learning asset for the whole team.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/410.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=410)

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/430.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=430)

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/440.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=440)

### Identity Domain: Federated Access Control and Service Approval at Scale

As we now talk about security at scale, let's look at some of the primary domains. The first one, and one of the most important ones, is the identity domain. There are some key things we want to highlight here.  On the identity provider and single sign-on mechanism, the first one is using an identity provider and a single sign-on mechanism that allows your human users  to access AWS applications and accounts within AWS.  Again, I want to highlight that when you're using Identity Center, it can be used for accessing AWS applications or accessing AWS accounts. You can choose which feature you want to use or both.

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/450.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=450)

Let's look at some of the common policies when you're trying to do security at scale, especially in a federated  and automated manner. When you're federating, you block the bad at the organization level for the whole enterprise, but then you leverage your org structure to apply policies at the organizational unit and the account level in alignment with the security requirements of the workloads running there. For example, you can apply an organization-level Service Control Policy that prevents anybody in your organization from modifying your CloudTrail. Or you can apply a Resource Control Policy that prevents a certain type of encryption being used on your S3 bucket that is congruent with customers using it in ransomware attacks, for example.

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/530.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=530)

A key thing to highlight here is that there are also these management policies, and one of them is declarative policies. You can use them to basically prevent noncompliant actions. For example, you can block public internet access to your whole VPC and also create helpful error messages for your end users. Now when you're using AWS at scale,  you probably are not enabling every service in your production accounts. You're using the services that you want. Now, how do you effectively manage service approval at scale while maintaining your security and enabling developer innovation? Your developers are looking to try the latest service. I'm sure some of you are excited about trying out the new security agent and other amazing products we've launched yesterday.

So what you do is by implementing a tiered approach, you create sandbox environments where developers get the freedom to experiment in isolated environments from the rest of your workloads. A common pattern we see is a 30-day period for the sandbox account, after which it is gone, but also ensuring that sandbox environment has some baseline security controls built into it, like allowing certain types of data to come in and keeping data in, also allowing controls like who has access in there and not, and then usage metrics that you're collecting in that sandbox environment. Because guess what, somebody's going to experiment, they're going to find that this works, let's move it to the next stage, and at that point, instead of creating the whole workload somewhere else, you might have to make a choice to then migrate that account over into your main workload and production accounts.

So how do you then use metrics to make that transition easier? The whole idea is to use automated guardrails replacing your manual controls so that you can scale securely.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/630.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=630)

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/670.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=670)

### Network Domain: Zero Trust Access and Traffic Inspection Patterns

Let's talk about some of the network domain features that come in. One of the most important is zero trust access, right?  This is to enable that your actors, whether they're humans, machines, or devices, can access your resources that they should be using in a manner that does not depend on the proximity. Rather, it leverages identity and other security features to make that authorization. AWS provides you these types of zero trust mechanisms across the stack all the way from the edge to the cloud APIs.  You can use the shared authorization context and consistent authorization evaluation across, and then also collect telemetry all through the phases so you can then monitor your access pervasively.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/690.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=690)

Now obviously you're going to have to inspect your traffic as it goes through. We've seen three common patterns come up very frequently in how customers at  scale do them. The first one is the outbound pattern, right? You want to keep the safe things in and not let them go out. So this is about how do you keep a connection from your resources going out to the untrusted connection or destination. Some of the potential risks you're mitigating with this, using a network firewall for example to limit this, is sensitive data being exfiltrated or command and control type of activity and such.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/720.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=720)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/740.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=740)

 The corollary is the ingress pattern, right, where you have an untrusted destination sending traffic to your internal resources. Similarly here, you can use the same mechanisms to mitigate threats like SQL injections, vulnerability exploits, and such.  And then when you're operating at scale, you're going to have trust boundaries that are different across your organization, right? We talked about sandbox versus not, right? So similarly, you're going to have your east-west inspection. This can be VPCs within the region or VPCs across a different region, and you are going to inspect that traffic using the same mechanisms, and you'll see more in the later slides.

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/770.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=770)

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/780.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=780)

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/790.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=790)

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/800.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=800)

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/810.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=810)

One more thing I wanted to highlight is at scale, some customers use identity aware service authorization.  So we talked about how humans use this. This is an example of how services use it. We have a service that we provide that is called  VPC Lattice. What VPC Lattice allows you to do is  create a lattice network for your applications running in, say, your VPC connecting to a different application running in a different VPC.  And right now these applications do not have any access, but using VPC Lattice you can create service definitions, target groups, and action policies, rules that allow you to then make this access possible. But behind the scenes, it's leveraging  other constructs like AWS IAM to make this connection possible.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/820.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=820)

### Resource Domain: Data Governance, KMS Key Management, and Immutable Infrastructure

Now let's talk about data governance, especially when it talks about resources, right?  The foundation of this is understanding your data landscape. Your security here starts with data classification and protection measures that match the data sensitivity, right? You cannot protect everything the same way. You have to choose to apply your security resources to your most valuable assets. One of my mentors early in my security career told me do not put steel locks on plywood doors, and that has never left me.

So some of the critical decisions here are regions where your data can live because of some compliance reasons, or encryption requirements and backup requirements. Now one of the key things that comes up here that we've seen customers do at scale very successfully is managing your AWS KMS keys, right? When you're operating at scale, you're going to have a lot of KMS keys. AWS KMS keys are protected by FIPS 140-3 Level 3 hardware security modules. I know a lot of words, but what I'm trying to say is it gives you a lot of options. It gives you a lot of options in terms of how and where you create the keys, but also the way the key material comes from.

So some of the best practices we see is a lot of times central security teams love to keep the keys in their control. Well, a challenge we commonly come across with that pattern is now you're basically having all the KMS level quotas applying at that account that you have in your central management account for security.

Rather, you take the keys and move them closer, so data has gravity. Similarly, it's pulling the encryption key with it. But that doesn't mean your central team does not have control over it. You still get the same governance by using SCPs and RCPs that we talked about earlier to ensure that these keys are protected appropriately. But now the keys are closer to the data itself, and the quotas on key usage especially are from that account where the keys are being used. Similarly, when you're doing bring your own key material KMS keys, KMS allows you to do that, but the consideration there you have to make is what are the performance and operational overhead that I'm taking on for doing this. I understand there are always very good compliance reasons and regulatory reasons to do that, but understanding where they apply and then using it appropriately versus using it in blanket across your organization.

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/970.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=970)

Now the thing that we want to talk about is how organizations implement defense in depth  using validation for infrastructure as code. When you're doing automation, you're going to have all your infrastructure defined as code. Hopefully you're not clicking in the console. So what needs to happen is infrastructure validation needs to happen in multiple layers, from the development stage in your IDEs or your CI/CD pipelines all the way down through deployment. So what does that look like? Let's look at an example.

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1000.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1000)

[![Thumbnail 1010](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1010.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1010)

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1020.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1020)

You have a Terraform core calling a Terraform template being created using the AWSCC provider. This one calls the Cloud Control API,  which then, if you have come across CloudFormation hooks, which is a manner of basically intercepting these in the target environment and then checking them for compliance with the rules applied in that  target environment. So that would then take those CloudFormation hooks and they could evaluate it. So in this scenario, say if you are creating an S3 bucket  right now, you created a rule in this target environment, not for your whole organization which is scanning every template, but in the target environment where it says, well, here I actually want a very specific configuration. Say you're doing a federal type of project and then you need dual layer encryption, so you can create a rule that says all my S3 buckets here need to have dual layer encryption, and that hook will evaluate that using that. So then based on the pass or fail, you can have a helpful message that then goes back to the developer.

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1050.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1050)

 Immutable infrastructure means treating infrastructure as code that never changes after deployment. Basically no in-place updates, no infrastructure drift, and changes trigger new deployments. This eliminates unauthorized modifications and click ops, ensures consistent security posture, provides clear audit trails, and enables rapid security responses. The idea is when you're doing cloud security at scale, immutability transforms your infrastructure from a potential vulnerability to a reliable security boundary.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1100.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1100)

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1110.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1110)

[![Thumbnail 1130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1130.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1130)

### Data Perimeters: Establishing Trusted Boundaries for Identity, Network, and Resources

Now let's talk about data perimeters where a lot of these things come together and something  that we see a lot of customers at scale implement to reach their security objectives. So what is it? Before you build that firm boundary that I just put on the slide,  you need to define what that means for your organization, right? What are the approved data patterns, and what is it that you trust? Something you own, something that AWS owns on your behalf, or something a trusted third party owns for you. So in this case you have trusted identities,  resources, and networks.

[![Thumbnail 1140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1140.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1140)

So let's look at some of the key objectives for data perimeter. As you  can see here, these are the key objectives about who can access what from where, defined by the different perimeters of identity, network, and resource. The identity perimeter primarily mitigates unauthorized access by identities external to your organization. The resource perimeter mitigates unintended data movement to resources outside of your organization, and the network perimeter mitigates credential use outside of your corporate environment, for example.

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1170.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1170)

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1180.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1180)

[![Thumbnail 1190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1190.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1190)

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1200.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1200)

 Now let's quickly look at how this all works together, right? So I have my trusted identity going to my trusted resource. Well, I'm fine, that's everything goes through.  But now I have untrusted identities. Like I work for a company, but I brought my own credentials from my AWS environment, personal environment, and I try to use it.  Guess what? The identity perimeter is going to block that. What about if I go home using my company credentials and try to access those resources? My network perimeter blocks that out. 

[![Thumbnail 1210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1210.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1210)

[![Thumbnail 1230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1230.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1230)

[![Thumbnail 1240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1240.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1240)

And then if I use my personal credentials trying to get to the trusted resource, it blocks by the identity perimeter again.  Now, say I'm working in my corporate environment and I'm trying to go to my own personal S3 bucket, an exfiltration type of scenario. How do these perimeters work? Well, if you're trying to use your corporate credentials and try to go to these non-corporate resources, the resource perimeter will block it.  Similarly, if you're trying to use your personal credentials but you're coming from outside the network and then trying to go to these resources, the network perimeter  will block it. In this table you can see where the different data perimeters are applied and the different appropriate policies that are used, and the primary IAM features that are used for achieving the objectives of that perimeter.

I hope this was helpful. I know this was a lot of information, but I wanted to set you up with some of the key characteristics we see of enterprise security at scale. Please welcome at this point Peter from Meta to come and talk about how they have built their cloud security at scale. Thank you.

[![Thumbnail 1310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1310.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1310)

### Meta's Scale Challenge: Building Hyperscale Infrastructure for Global Social Connection

Great, thank you, Syed. So today I would like to talk about the scale challenge with Meta's cloud security reality. I'm pretty sure you've all thought about building for scale, but what does that mean? You know, being able to handle traffic on New Year's Day, on Christmas Eve, when everyone wants to connect all at the same time to your service. Today we're going to talk about building at meta-scale.  And as you can imagine, operating at this scale comes with many challenges. So we'll start by digging into some of the interesting and unexpected things that happen with planet scale social media.

[![Thumbnail 1330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1330.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1330)

Before we go deep into Meta's cloud infrastructure, I want to take a moment to share Meta's mission,  which is to build the future of human connection and the technology that makes it possible. We've been a social technology company since day one, and our technology stack reflects this in many ways. Compared to much of the industry, we're contrarian, because the social connection tech problems we're trying to solve are unique. Our innovations give people new ways to connect, and we're committed to keep everyone safe and make a positive impact on the world.

[![Thumbnail 1360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1360.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1360)

 With 3.5 billion daily active users, the world relies on Meta to connect and communicate. We have a truly global user base and can't build with any certainty about who is connecting with whom. We were born a tech company and maintain an incredibly strong, bottoms up engineering culture. I don't really need to say that. We like to lead the development of emerging technologies such as mixed reality and open source AI. Today I'm happy to share some of the exciting work we're doing in expanding our infrastructure into AWS.

[![Thumbnail 1400](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1400.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1400)

 Supporting 3.5 billion daily active users, with many generating content, requires infrastructure at hyperscale. Social media workloads typically exhibit higher transaction rates than most large scale internet sites and systems. To me that's to be expected. With social media, the cost per transaction, such as liking a post or watching a video, doesn't require movement of physical goods or the exchange of money. So the real world transaction cost is very low. We're not limited by the number of widgets in our e-commerce warehouse. We're not limited by the number of planes or seats or airport capacity. This means we see orders of magnitude higher transaction rates than most other global distributed computing platforms. CAP theorem is definitely in play here, and eventual consistency is something we think about a lot.

[![Thumbnail 1460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1460.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1460)

 Without the constraints of the physical world, our global user base loves connecting in unexpected ways. Our systems support diverse security, data and privacy compliance requirements. We've made huge investments in developing a privacy aware infrastructure. It builds on each request's context and applies privacy policies in real time. This enables teams to programmatically define the who, what, when and where their service should be called and data should flow. Meta deals with these unique requirements by building a globally consistent graph database. There are not many workloads that need this kind of infrastructure, and you can find research papers about how we did this if you look up the TAO system.

[![Thumbnail 1510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1510.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1510)

[![Thumbnail 1530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1530.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1530)

 Meta is building innovative experiences that are at the forefront of reimagining the way we interact in community. Building systems that power experiences like mixed reality and open source foundational models require new approaches to scalability and the secure handling of data. We're working on solving these challenges at the infrastructure  level.

Meta's strong engineering culture means we continue to focus on building highly scalable, hyperscale infrastructure. We're already a hyper-scaler, and we continue investing in our own data centers, backbone cables, and edge connectivity. It's these kinds of investments that let us do things like launching the Threads app to 100 million users in the first week. Without this kind of infrastructure, it wouldn't be possible.

[![Thumbnail 1590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1590.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1590)

But even that's not enough for AI, so we're doing hybrid cloud on a big scale. We terminate direct connect fiber in our own premises, and we already have a security infrastructure available. This gives us confidence to push further into the cloud. A lot of the security assumptions we've already made help us work and keep our infrastructure secure across multiple clouds. 

Meta has been at the forefront of AI application research for many years. It started with ranking content on the news feed and Instagram reels. We then doubled down on GPU infrastructure buildouts. In 2022, that was the year of the large language models, with Meta entering the race with our Llama system. Successive Llama versions were trained on our own infrastructure, and as we've seen in the media, Mark has launched our Super Intelligence Labs initiative.

We're in the process of building out our class-leading Prometheus 1 gigawatt and Hyperion 5 gigawatt superclusters. Even with all these efforts, we have AI research workloads that still need more capacity and feature job sizes that are well suited to GPU capacity in the cloud. These jobs don't need the hyperscale contiguous clusters we are building in our own data centers. In this talk, we'll go deeper on what makes Meta's cloud deployments unique and how we set up our AWS architecture to securely scale.

[![Thumbnail 1670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1670.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1670)

### Cloud Foundation Team: Identity Management and Auth-D Credential Vending Service

A bit of background on Meta now. Most Meta software engineering teams  develop systems that run in our existing hyperscale container environments. These are well suited to the small web requests that we see from the family of apps. Meta does run significant compute workloads in the cloud as well.

The Cloud Foundation team is Meta's team of cloud experts who are responsible for low-level infrastructure such as transit accounts, direct connect links, and building systems that enable efficient enforcement of cloud security standards. We help facilitate cloud usage across the business, and we've built an infrastructure fabric layer that provides identity, authentication, public key infrastructure, metrics, monitoring, and a host of other support services. Our team accelerates efficient and conformant use of the cloud resources.

[![Thumbnail 1740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1740.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1740)

We provide building blocks such as prepackaged AMIs, Kubernetes images, and a build pipeline. This helps our software feature teams to automatically support our privacy-aware infrastructure constructs and stay secure and compliant while moving  quickly. As we've expanded beyond our own data centers, the network edge no longer provides a strong data boundary, and we increasingly rely on a sophisticated identity system to grant least privileged access to user and service roles.

We have a centralized IAM system that is integrated across all of our internal systems. We use role-based access control for granting only a minimal set of capabilities required to execute the workload or administrative task. We use machine attestation to ensure the integrity of the host and as input to generate short-lived role-specific credentials.

Short-lived credentials reduce the blast radius of lost or stale credentials and reduce the need for complex revocation and untimely renewal. We stream CloudTrail and other AWS logs into Meta's production infrastructure and scan for anomalies in real time.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1820.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1820)

A strong identity and public key infrastructure gives us the confidence to extend our network into the cloud and support these workloads on demand. Auth-D is our credential vending service. It abstracts away the different credential  and token retrieval mechanisms across different clouds. When a request comes in from a Metaprod user or system, Auth-D receives a service call with a cloud account ID, a role, and some associated identities.

[![Thumbnail 1840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1840.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1840)

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1860.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1860)

At the auth checking step, we validate that the requested resources exist  in the cloud account and check if the request is allowed by policies stored in our internal IAM system. If the checks fail, we return an authorization permissions error. If the request is approved, we retrieve the credentials from the cloud service provider.  This is the step where we translate between internal security constructs and different types of credentials you need in the cloud.

[![Thumbnail 1870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1870.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1870)

In the case of AWS, this might  take the form of STS tokens, Kubernetes tokens, or even SSH certificates. In many cases, we generate a set of credentials that are only used for that specific request. This gives us full end-to-end traceability of each user session or job creation, which is crucial for maintaining our data lineage. I'll cover data controls and share an example of how our privacy-aware architecture maintains context as data is processed.

[![Thumbnail 1910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1910.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1910)

If assembling the credential package fails, we return a service error to the user.  We have standardized error categories of permissions, service, or cloud errors, allowing exceptions to be handled correctly and, if necessary, routed to the correct service teams. Once the user has the correct credential package, they can assume the temporary role with the permissions needed to access their cloud resources. The user then calls assume role with SAML and starts their SSH session or executes their job.

[![Thumbnail 1960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/1960.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=1960)

### Network Architecture and Security Controls: IPv6 Infrastructure and Encryption at Meta

Cloud Foundation maintains building blocks to refresh credentials as required. This setup works well for us because we have highly technical teams using the cloud, and this extra layer of complexity pays off in the time saved in maintaining static credentials and doing on-demand rotation.  Meta has a fairly unique IPv6-only infrastructure, and what we save on NAT and IP address space exhaustion calculations, we spend on BGP infrastructure that optimizes routing tables and makes sure that they're not too big for the routers to support.

We do still have some networking controls at scale, and we do our best to enforce a one workload per cloud account policy. This automatically gives us VPC isolation between workloads, and Cloud Foundation maintains transit and common service VPCs in separate accounts. We pay a lot of attention to strict internet ingress and egress controls, which helps immensely with maintaining data lineage for our privacy-aware infrastructure.

[![Thumbnail 2030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2030.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2030)

We grant east-west connectivity between VPCs quite liberally, and this helps us enforce the one workload per account rule by not restricting those connections too seriously. As you'll see coming up, even with IPv6 everywhere, we still have quite a lot of subnet complexity.  Meta uses IPv6 everywhere. It starts with multiple public slash 32 ranges.

There's a lot of addresses, and I won't try to make sense of the number of zeros today, but it's more IP addresses than grains of sand on Earth. We don't need to worry about running out. With this many addresses, we can allocate a slash 62 per host, and we even allocate one IP per Tupperware task. This is our equivalent of a Kubernetes task.

This means anything running on the Meta infrastructure can route to any device or other task, and we have no more IP address conflicts or NAT running in the network. The trade-off here is a huge BGP infrastructure where we manage routing tables and make sure we don't exceed the capability of the physical routers. This is even more of a problem with some of the cloud providers who limit the number of static routes we can use per account or per VPC.

However, on the whole, being able to extend our layer 3 network all the way through to a cloud VPC is really nice. We can then let network engineering manage direct connect links and decide whether traffic is routed over direct connect, VPN, or over the internet into our cloud infrastructure.

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2110.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2110)

One last thing before we get to the interesting stuff,  I have a couple of infrastructure assumptions to go over. Cloud Foundation provides something of a managed experience to cloud consumer teams within Meta. Working with our security teams, we've set up some fairly strict rules about how they're allowed to use the cloud. Some of these come from protecting our digital assets and others from hard lessons learned when scaling up the number of people maintaining specific accounts.

We only allow our cloud consumers to view their accounts from the console. We're pretty strict about enforcing the use of infrastructure as code through Terraform modules and integration we built into Meta's main CI/CD system. Terraform code allows the same peer review processes as all other code going into our mono repository. We're making progress towards real continuous deployment of Terraform code, and several of our large scale systems call Terraform apply every two hours.

We use service gating on production accounts to ensure we've got internal support for new or complex AWS services before letting our customers go wild. When teams just need to experiment, we offer short-lived sandbox accounts which we automatically isolate at the two week mark. We also offer Meta Instance, which is a nicely prepackaged AMI that carries all the software necessary to connect back into Meta services, providing things like identity, logging, metrics, and service registration right out of the box for free.

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2210.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2210)

Robust encryption controls are a key  factor in building our confidence in moving workloads to the cloud. I won't go into too much detail here, but it's important to know that these exist within our infrastructure and are critical in enabling rapid build-out of our cloud capacity. Our internal public key infrastructure includes multiple root CAs and a cloud specific CA so we can track where certificates are used and prevent them from passing between infrastructure boundaries.

We use EC2 instance attestation to generate X.509 machine certificates that allow us to trust the hardware and operating system environments. By regularly regenerating these certificates, we can tell if anything changed on the hardware or firmware. We secure all connections with mutual TLS, ensuring both ends of the connection are verified and helping to mitigate man in the middle attacks. Our Direct Connect links are encrypted using MACsec at layer two.

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2270.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2270)

### Case Study: Building Secure AI Clusters on AWS with GPU, Storage, and Network Firewall Architecture

 With the building blocks covered, let's dive into our case study of building a secure AI cluster on AWS. There are several unique challenges with AI clusters due to the GPU scarcity and the large volumes of data transfer needed to run some of these mega-scale clusters. We also don't need the same level of resiliency and availability that come from serving online production traffic. It's a bit of an all or nothing situation here. Is there any point in having a data store available when the GPUs that are paired with it have gone offline? I don't think so.

We don't really get to dictate which availability zone the GPUs are delivered in due to physical space and power constraints. So we've set up our infrastructure for rapid deployment of AI clusters and we don't allow AI training jobs to span multiple clusters. Researchers also have unique needs. They often work via an interactive session on the same host as the physical hardware. They often schedule packs of Slurm jobs across large blocks of GPU capacity.

They collaborate directly with other researchers and frequently exchange code via GitHub. They may need to pull in datasets from the public internet, and in short, they need easy access to the internet while safeguarding against leaking weights and biases or finished models. Now I'll go into more detail about how we build out this secure infrastructure.

[![Thumbnail 2370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2370.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2370)

We'll start this picture with the Meta production data centers.  Reduced to two here for simplicity, and because my slide just isn't quite big enough. You'll hear more about that theme quite a bit as we go into the details here. These are quite monumental by themselves. You can consider these analogous to AWS regions with multiple physical data centers within those two purple boxes. We terminate our own fiber in the data centers and there's no co-location facilities in this left side of the diagram.

Then we have our shiny new GPU hosts delivered by Amazon as reserved instances in a particular availability zone. The location of the GPUs and accompanying storage hosts dictates which availability zone we build out the rest of the infrastructure. When the hosts land, our automated tooling sets up the EKS HPC cluster control plane and validates the GPU pods and login pods used by the AI researchers.

[![Thumbnail 2440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2440.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2440)

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2460.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2460)

[![Thumbnail 2470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2470.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2470)

The GPU hosts are not directly on the internet. Rather, we pair this VPC with a management VPC.  This hosts internet connectivity bits like routers, firewalls, Route 53 private hosted zone, bastions, and proxies, allowing access to other parts of the infrastructure and management services for the GPU cluster.  You might have seen this architecture at our previous re:Inforce talks, with a new bit today which is the storage VPC. 

GPUs might be the brains of AI, but the storage is the heart and the circulatory system. The latest GPUs demand very high bandwidth and low latency data access to maintain high utilization. This is no mean feat in a general purpose public cloud without access to the specialty hardware, storage, and networking tricks that we get to do within our own data centers. This is especially critical when running clusters of 10,000 or more GPUs on many petabytes of data spread across smallish files.

We've had a lot of challenges with the file system metadata performance, such as just listing the contents of directories. By creating a separate storage VPC and dedicating a team of engineers to the space, we've been able to test, benchmark, and tune many different flavors of S3, S3 Express One Zone, FSx for Lustre and OpenZFS offerings, and running our own instances of Hammerspace. We're doing all kinds of experimentation in this space, including on top of bare metal instances.

[![Thumbnail 2540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2540.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2540)

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2550.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2550)

 VPC peering comes to the rescue here to connect the GPU VPC to the storage VPC.  And we also store checkpoints, weights, and biases in the specialized storage systems running in the storage VPC. To keep everything secure, we use S3 bucket policies to block unauthorized access inside of the storage VPC.

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2570.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2570)

Now we layer in Direct Connect  from Meta's production data center to the storage VPC. This is where the heavy lifting happens. Super high redundant bandwidth is in place with LAGs and DIFs and multiple circuits to reduce the impact of individual fiber cuts or device and rack interruptions. These links are where we do bulk data transfers of the training datasets into and out of the cloud environment. Those GPUs are hungry beasts, so a lot of work goes into optimizing the data storage layers and connectivity to try to keep this side of things efficient.

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2620.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2620)

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2640.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2640)

The last piece of the high-level view is DNS. Remember we have IPv6 everywhere, so there is a Route 53  private hosted zone in the storage VPC providing DNS resolution for everything that's accessible over these Direct Connect links. With this setup, we can decide when research and login traffic, for example, runs via the internet or via the Direct Connect. 

AI research workloads often require some degree of internet connectivity. Incoming traffic is typically in the form of researcher SSH logins, service account connections, and engineers connecting to review and optimize cluster performance. To support this, we have a sophisticated setup using AWS Network Firewalls, Network Load Balancers, and NAT gateways.

On the internet ingress side, we do traffic inspection to prevent intrusions and unauthorized connectivity. The AWS Network Firewall lets us cover all kinds of application traffic in one place. Even if the traffic is encrypted, it can apply some controls beyond just the network and transport layers. We use Server Name Indication extension and enforce TLS versioning and fingerprinting at these Network Load Balancers. We also use support for AWS managed threat signatures, which helps us to detect threats and block attack patterns of known vulnerabilities without having to write and maintain individual rules.

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2720.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2720)

Our internet gateway setup is configured to only allow ingress traffic to the AWS Network Firewall in the ingress subnet, with access restricted via routing tables.  We use standard industry tooling to write Suricata rules for our IP filtering and have hundreds of entries for Meta CIDR blocks. Because we have so much control over where we allocate IPv6 CIDRs, we can reasonably allow incoming traffic to be allow-listed based on the source IP, so the connectivity gets all the way through to our GPU VPC.

[![Thumbnail 2750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2750.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2750)

 On the egress side of things, Meta again deploys AWS Network Firewall endpoints in their own subnet with access restricted via routing tables. We filter HTTP and HTTPS traffic based on domain name, and we configure domain filtering using an allow-listing approach. We configure security groups very coarsely with generally none, Meta CIDR blocks, or totally open. VPC routing is set up to only allow egress traffic via the egress subnet, and domain allow-listing happens in the AWS Network Firewall.

[![Thumbnail 2790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2790.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2790)

 We have a self-service system for allowing researchers to allocate domains for their cluster depending on researcher, partner, or vendor needs. Egress traffic follows the path of the pink arrows. Egress inspection protects against unauthorized outbound traffic and can provide a detection vector for malware communication patterns.

To sum up the internet connectivity part of this, using AWS Network Firewall allows us to scale in a way security groups don't, especially solving their CIDR rule limitations. Network Firewall's ability to support one million CIDRs allows us to get granular with Meta's IP space, even when it blows out the CIDR blocks from approximately 30 to more like 200 or 2,000. Standard Suricata rules integrate well with our internal firewall management tooling. Domain filtering allows us to go beyond IP rules to domain allow lists and block lists. AWS has some great white papers on approaching centralized ingress and egress filtering, and we've used this to scale our solution to what we have today.

[![Thumbnail 2870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2870.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2870)

### Privacy Aware Infrastructure, Threat Detection, and Key Takeaways for Cloud Security at Scale

 As we move beyond infrastructure into the application layer, we integrate with Meta's data privacy controls. This is a much harder space because a policy decision requires an understanding of the type and meaning of data and the request context before deciding if a request should be allowed or blocked. For example, different privacy policies may be applied depending on which of Meta's family of apps is being used for the request. Meta's Privacy Aware Architecture is a sophisticated system that automatically applies privacy policies whenever applications process data. I'm only going to touch on a few aspects of PAI today, and you can find out more on Meta's engineering blog.

PAI includes a central unified metadata store where table schemas and data understanding signal annotations are stored. Signal collection systems are responsible for identifying the semantic type and shape of the data found in columns and verifying that it matches the schema and the intended purpose. These annotations are used to enforce policies and establish lineage, allowing the full life cycle of the data to be understood and factored into policy decisions.

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/2960.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=2960)

I'll quickly run through the classic banana data example from the engineering blog.  Let's say Alice is a user of a Meta system and has stored some data about a hand of bananas. She might own this hand of bananas that she's bought from a store. These diagrams show how that data could be represented across a number of different data warehouse contexts. We might find a table context in the top left there where the data on individual bananas is stored, including their color, country, and flavor. We might consider this as the online or base representation of the data.

As we process this into a data warehouse, we might be interested in a column representation where instances of sundaes have portions of Alice's banana data in them. We might consider a row-based representation where we care about different fruits having a type, a ripeness, and a price. How about a cell-based representation where Alice's banana data is used to make a juice with a size and a freshness. These diagrams represent how Alice's banana data might propagate through a data warehouse, and the key takeaway here is that data lineage is difficult, and just like with real fruit, it's not always possible to unmix the sundae.

[![Thumbnail 3050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/3050.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=3050)

 Now let's say we want to model some user interactions where Alice's phone is interacting with the smoothie, fruit basket, and bread applications. In this case, we have set policies such that Alice's banana data can be used in the smoothie and the fruit basket use cases, but not in the bread application. To take the analogy full circle, perhaps we have a policy here that blocks the usage because we don't want to heat up bananas, which is necessary when baking bread. For some more details about how our privacy aware infrastructure works, check out our security and privacy blog posts, which show more about how Meta enforces what we call purpose limitation via our privacy aware infrastructure at scale.

[![Thumbnail 3110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/3110.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=3110)

 Right, to wrap this up, we will land the plane here with threat detection and incident response systems, and close the loop. These systems close the loop and give us confidence that our cloud AI environments are well secured. We stream VPC flow logs to our internal systems and run analysis in real time to detect anomalies. We stream S3 data events to monitor what's going on with our S3 buckets, and we make extensive use of third-party monitoring applications, including Wiz and also GuardDuty for threat detection. Automation beats out human response time, and by instrumenting everything and using a central account for hosting detection systems, we gain the ability to automate remediation and isolation if things go wrong.

[![Thumbnail 3180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/3180.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=3180)

With that, it's back to Robin to wrap things up. Thank you, Peter. Yep, so as we wrap up, I appreciate all of you spending the time in this journey that we've gone on today.  If you think back, remember we started out this journey of imagining what it took to secure those experiences for those 3.5 billion daily active users. I think Peter gave us some amazing insight into Meta's real world implementations and really seeing that security at scale is more than just more of the same controls. It's fundamentally rethinking how security works when you're operating at that planetary scale.

Coming back to the principles we talked about earlier, the ability to automate manual processes inevitably fail at scale. Automation isn't optional, and it's the only way to ensure consistent security enforcement across vast infrastructure. I think back to Peter talking about their infrastructure's cloud deployments, that automated credential vending through Active Directory, and how that enables both security and velocity for their developers. Proactive security by design, making sure that security is not an afterthought in your organization and building it from the ground up. Their AI approach and policy validation throughout the CI/CD pipelines ensure that those controls are baked in from the start.

When you federate and provide teams with that freedom to innovate within an organizational boundary through the multi-account strategy and the use of those Service Control Policies to establish those guardrails, you again enable that velocity while managing the risk. Final two pieces here, being able to rely on your resiliency, understanding your failure modes, and designing systems that can detect and recover from those events. At scale, it's not if something will fail, but when.

So Meta's approach to network segmentation and threat detection enables them to respond quickly to unanticipated events. All of that wraps up with a learning culture, taking advantage of these real world experiences where every event, every incident is an opportunity to learn and strengthen your security posture.

[![Thumbnail 3330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/3330.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=3330)

 So what I hope that you take away from today is Peter shared this staggering scale that Meta operates at, and I think early on I called it just another Wednesday afternoon, but we all have our Wednesday afternoons. No matter what your scale, the fundamentals of effective cloud security remain consistent. Know your critical assets, implement appropriate controls, and build security into your processes from the beginning. I'd encourage you to ask yourself which of these principles can you apply, where can I use automation to reduce my risk, and how can I build a learning culture around security.

Start with what you can do today, whether that's implementing your first Service Control Policy, adopting infrastructure as code, or adding automation to your existing security workflows. Working with Meta on these journeys has taught me that scaling security isn't just about protecting more resources. It's enabling your business to grow confidently, knowing that your security posture is growing with it.

[![Thumbnail 3420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/bcde1ed3fa1a9def/3420.jpg)](https://www.youtube.com/watch?v=5rTuf6bsYlM&t=3420)

 Thank you all for being here with us today. Thank you, Peter, for sharing Meta's incredible journey, and Syed for going deep on some of those AWS security services powering at scale deployments. Please complete the session survey in your mobile app. Your feedback helps us keep delivering valuable content. Enjoy the rest of re:Invent and do remember security at scale starts with the right principles. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
