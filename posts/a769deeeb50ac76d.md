---
title: 'AWS re:Invent 2025 - Transform digital experience: Accelerate AWS Partner & customer success(PEX208)'
published: true
description: 'In this video, AWS introduces Billing Transfer, a new service enabling channel partners and end customers to operate in separate organizations while partners manage billing exclusively. The service addresses three key benefits: enhanced security autonomy for both parties, simplified billing operations through consolidated management, and protection of partners'' proprietary discounts. Previously, partners controlled the management account to handle billing, creating conflicts over administrative access and operational inefficiencies across multiple organizations. Billing Transfer eliminates these trade-offs, allowing customers complete environment control and access to native AWS cost management tools like Cost Explorer. Partners can centralize billing delivery, automate operations through new APIs, and streamline channel program qualification via the redesigned AWS Partner Central experience. The service is now generally available through AWS Billing and Cost Management console.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/0.jpg'
series: ''
canonical_url: null
id: 3086480
date: '2025-12-05T12:48:24Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Transform digital experience: Accelerate AWS Partner & customer success(PEX208)**

> In this video, AWS introduces Billing Transfer, a new service enabling channel partners and end customers to operate in separate organizations while partners manage billing exclusively. The service addresses three key benefits: enhanced security autonomy for both parties, simplified billing operations through consolidated management, and protection of partners' proprietary discounts. Previously, partners controlled the management account to handle billing, creating conflicts over administrative access and operational inefficiencies across multiple organizations. Billing Transfer eliminates these trade-offs, allowing customers complete environment control and access to native AWS cost management tools like Cost Explorer. Partners can centralize billing delivery, automate operations through new APIs, and streamline channel program qualification via the redesigned AWS Partner Central experience. The service is now generally available through AWS Billing and Cost Management console.

{% youtube https://www.youtube.com/watch?v=lt27cRYUPT0 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/0.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=0)

### The Challenge: Why AWS Channel Partners Needed Billing Transfer

 Today we're going to talk about how AWS channel partners and their end customers can transform their business and deliver more joint success using a new AWS service called Billing Transfer. Billing Transfer introduces a new era for AWS channel partner billing and governance.

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/20.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=20)

 There are three key benefits that we'll cover today. First, channel partners and their end customers now will have enhanced security autonomy. End customers of channel partners can now operate in an entirely separate organization from their reseller. This allows both of these entities to self-control their own AWS organization. Second, Billing Transfer will simplify billing operations for channel partners because partners shared organizations with their end customers before using Billing Transfer. Partners had to proliferate their organizations and manage billing across hundreds of organizations. You can now consolidate billing in a single organization across all end customers. Lastly, using Billing Transfer, partners can now protect their proprietary discounts and rates away from their end customers. The bills and chargeable billing artifacts are delivered exclusively to the partner in their own organization, while customers maintain self-governance in a separate organization.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/100.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=100)

Before we dive into the problems that resulted in the need for Billing Transfer, we have to understand the existing AWS account construct.  Within AWS Organizations there are two types of accounts. There's a management account and a member account. The management account today has full administrative control over the organization. It can establish things like service control policies. It manages multi-account governance settings like GuardDuty, Control Tower, and landing zones, but that same account is responsible for billing in the entire organization.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/140.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=140)

This means that for channel partners to manage billing for end customers, they traditionally had to control this management account while end customers operated within the member accounts.  What this resulted in is various conflicts over administrative access control in the management account. Channel partners and end customers had to compromise over partner control of the management account in order for them to have billing access, while end customers then delegate away all of their governance controls as well and create a security risk for many end customers working under channel partners.

It also results in operational inefficiencies where partners now have to create dozens or hundreds of organizations, retrieve billing artifacts at the end of every month from those organizations, and make payments there just to manage their multi-account reselling business. Together these create scalability challenges for partners. It's harder to close deals because you have to convince every end customer why the channel partner needs to control access to the management account, and it's harder to manage your customer base because channel partners have to go into all of these separate accounts to manage billing and configure management account level settings for their end customers.

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/210.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=210)

### Introducing Billing Transfer: How It Works and What It Delivers

That's why we're excited to introduce  Billing Transfer, which is now generally available through AWS Billing and Cost Management today. Billing Transfer allows the management account of one organization to delegate its billing to a separate AWS organization. This allows channel partners and end customers to now operate in separate organizations while the partner exclusively manages billing and the end customer does not have to compromise on control of their own organization.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/240.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=240)

 Billing Transfer gives partners a few key benefits. First, centralized management, which allows you to manage multiple end customers from a single partner account which will receive bills. It allows you to have privacy of your margins without asking end customers to sacrifice control of their own management accounts as those chargeable artifacts are delivered exclusively to the partner. Together it gives you operational efficiency where you no longer have to manage hundreds of accounts for customers.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/280.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=280)

For end customers, they now can have complete environment control. There's no more trade-off when onboarding to a reseller through the channel programs. They also for the first time have cost visibility through native AWS cost management tooling.  This means your end customers can now also use Cost Explorer, cost and usage reports, and soon will also be able to use cost anomaly detection and Reserved Instance purchase recommendations within their own separate organization while procuring through the channel partner. Together these make those sales conversations easier.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/320.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=320)

We're making it an even playing field for partners to compete to deliver services to end customers. So how does this work in the billing and cost management experience? 

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/330.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=330)

You can see now the partner and customer are in two separate organizations. It's no longer a shared organization where member accounts are separated to customers. Within the customer organization, they  will not receive any invoices or bills from AWS. They will be able to view costs at the rates that are determined by the solution provider. So using AWS Billing Conductor, solution providers will be able to show custom rates to that end customer or public pricing rates.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/350.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=350)

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/370.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=370)

The partner will receive chargeable  AWS invoices with the actual price they pay, and they can view chargeable or pro forma costs. So that means the partner can actually view the cost explorer experience that the end customer has in their management account. This allows you to toggle between those two, and you'll do that through AWS Cost Explorer. 

Billing transfer also allows you to centralize billing delivery to a single management account across multiple customers. So in this example, the partner here would receive four invoices, one for the consumption within each of your customers and one for the partner's own consumption in their internal organization. This allows you to maintain segregation of those bills, use your existing integrations with third party billing tools to determine the re-rating to your end customers, all while customers maintain their separate organizations.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/410.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=410)

Billing transfer is available  through the Billing and Cost Management console in AWS. Within the Billing and Cost Management console, you can send invitations to invite end customers to transfer their billing to you. You can also centralize and manage your centralized invoice delivery and payments. You can switch between the views your end customer has and the views the partner has of their chargeable rates, and you can set custom pricing to show to your end customers.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/440.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=440)

Along with this, we've also launched  an entirely new AWS Partner Central experience. So in AWS Partner Central, we've made it easier for you to now qualify for channel benefits and channel discounts on your end customers' consumption. You can manage all of your accounts that you use against your channel programs in one place. You can self-activate these accounts with no more waiting for any manual approvals from AWS, and you can establish relationships with customers once per organization.

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/490.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=490)

You no longer have to complete end user reporting on every single customer account. Tell us who the customer is once at their management account level, and that will be inherited by all of the member accounts in the organization. This new Partner Central experience is also fully enabled by public  APIs that you can integrate with to report on your channel ecosystem and customer base from your own CRM or first party tooling. There are also new APIs in Billing and Cost Management that will allow you to automate the billing operations and onboarding of end customers through Billing Transfer.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/520.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=520)

### Partner Benefits and Getting Started with Billing Transfer

Let's hear from our partners what some of the benefits of this new experience are. There are three key areas that I want to highlight.  First, you can now grow faster by offering your end customers control. End customers never want to make any trade-offs in their AWS experience based on their procurement channel. You can now leave your root access to the customer. Let them retain their own organizational control and self-governance while you still manage their billing and deliver value-added services as a reseller. It simplifies your pre-sales and sales conversations, and makes it easier for you to close deals.

Second, it simplifies your billing and sales operations. You're now receiving all of those bills in a single AWS account. You're able to re-rate to the customer within AWS Billing Conductor. And you no longer have to rely on manual processes to report accounts to the channel programs in order to get channel discounts applied. You can do all of that to automate your FinOps workflows, rebuild natively, and simplify your third party tooling stack.

Together these should help partners scale more profitably with AWS. You can now simplify the workflows and conversations with your end customers to grow faster and reduce the operational costs you have to put in in order to deliver services to every single customer account.

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/610.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=610)

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/630.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=630)

We want to thank all of our launch partners  that helped us in this launch. We had an extensive beta and took a lot of feedback from partners who are experts in channel programs to get us here. Thank you to all of these partners: Software One, SHI, TD CINex, Reddington, and Commit. 

As we wrap up, let me emphasize what makes billing transfer transformational. For partners, you can now scale more efficiently and grow faster. You can simplify sales conversations and open new opportunities that were previously closed because of the channel program's root access requirements. Second, your customers retain their own control.

This means two things. First, you're going to have happier customers that can take control of their own AWS environment. Second, you're also going to be able to spend less time configuring settings on behalf of the customer just because they don't have the right management account level access in order to set up their own organizations. It will make it easier for you to onboard customers, close deals, and grow those customers.

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/710.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=710)

AWS will continue supporting mutual growth across the ecosystem as we continue into 2026. We will continue to invest in these new billing capabilities to make sure that there's no trade-off that customers ever experience based on their decision to procure through a channel partner. This truly represents the future of channel partner billing built on a single platform of trust and shared success. 

To get started with billing transfer, you can start today. Billing transfer is officially generally available as well as the new Partner Central channel management experience. You can access it through AWS Partner Central. AWS Partner Central is now available through the AWS console as well, so you can access this entire experience through the console or in your existing AWS Partner Central experience.

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/770.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=770)

From there you can start adding customers and initiate billing relationships to ask your customers to transfer billing to you while they retain control of their own organization. Here you'll find our technical guides, which will help you understand how you can add new customers to billing transfer and how you can migrate some of your existing customers who have root access conflict into billing transfer so that they're able to take back control of their organizations. 

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/a769deeeb50ac76d/780.jpg)](https://www.youtube.com/watch?v=lt27cRYUPT0&t=780)

With that I'd like to say thank you for joining us.  Let me know if you have any questions. We're happy to help and will support you in your journey either as an end customer or a channel partner or a partner yourself. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
