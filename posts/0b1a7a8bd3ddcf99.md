---
title: 'AWS re:Invent 2025 - Empowering communities - create fun events on AWS services (DEV344)'
published: true
description: 'In this video, Pri Santos and Dan Rezende from AWS User Groups in Brazil share how they built a centralized platform to manage community events using AWS services. They evolved from a simple MVP using Amplify, S3, and API Gateway to a production-ready architecture with Route 53, CloudFront, WAF, ECS Fargate, Aurora, and other services. The platform, called Party Community, helped increase event engagement by 27% in views and 28% in likes through WhatsApp messaging. It saves over 70 hours annually by consolidating 10+ platforms into one console, allowing organizers to focus on community engagement rather than administrative tasks.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/0.jpg'
series: ''
canonical_url: null
id: 3089094
date: '2025-12-06T14:37:27Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Empowering communities - create fun events on AWS services (DEV344)**

> In this video, Pri Santos and Dan Rezende from AWS User Groups in Brazil share how they built a centralized platform to manage community events using AWS services. They evolved from a simple MVP using Amplify, S3, and API Gateway to a production-ready architecture with Route 53, CloudFront, WAF, ECS Fargate, Aurora, and other services. The platform, called Party Community, helped increase event engagement by 27% in views and 28% in likes through WhatsApp messaging. It saves over 70 hours annually by consolidating 10+ platforms into one console, allowing organizers to focus on community engagement rather than administrative tasks.

{% youtube https://www.youtube.com/watch?v=9WJ21KPT1q8 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/0.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=0)

### Introduction: Streamlining AWS User Group Event Management in Brazil

 Hi everyone, welcome. Now we want to talk about something very special that we created in Brasília, the capital city of Brazil. We are part of the AWS User Groups in Brazil, and we face challenges in creating fun events with many tasks and processes to manage. The question is, how can we use AWS services to improve and centralize everything with diverse services, while keeping more energy focused on the things that are really special, like keeping people engaged? I'm Pri Santos, I'm a business developer at Claro Brazil, and today we have Dan Rezende. I'm a solutions architect and leader of the AWS community.

[![Thumbnail 60](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/60.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=60)

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/80.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=80)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/90.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=90)

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/120.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=120)

 So everyone, we are part of more than 400 AWS User Groups around the world, and in Brazil, we have more than 20 groups. We face the challenge of creating fun events with many tasks, processes, and platforms.  As you can see, we are defining event details, speakers, dates, registrations, design, and so many tasks, more than 20 in total.  How can we do this in one place using AWS? That's the challenge. We have some communities and deliveries. This year, we had a famous event from AWS called AWS Summit in São Paulo. In São Paulo, we did something special as an MVP where we sent a lot of messages to people around the event, calling them to engage in the community lounge. It was amazing, with so many messages that we sent to them, and they were interacting with us.  It was really cool.

[![Thumbnail 140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/140.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=140)

Now we have 22 events that we organized in Brasília, one without the platform and another with the platform, so we can check some details and data that show us that it really works.  Now we can see one message that we sent to people via WhatsApp, and we have some data that shows us that it works very well in our community. On one side, we have 119 views, as you can see on the 9th here, November 9th, in the first event. On the other side, we have 27% more views because we sent messages in the second event. On the other hand, we have a lot of likes. People liked 28% more of our posts on our networks after they received the message through WhatsApp.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/190.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=190)

 We are Brazilian and we love memes, so we sent memes to our community lounge attendees at the AWS Summit in São Paulo. They could check the QR code and link, and connect with us through this platform. So everyone, how do we use AWS to create fun? Now I'm asking Dan, what you did is the best part.

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/230.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=230)

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/240.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=240)

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/290.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=290)

### From MVP to Production: Evolving the Platform Architecture

 Thanks, Pri. I'm going to demonstrate now how we evolved  from a simple MVP into a production-ready architecture built on AWS. Our first MVP was intentionally simple. We used Amplify to speed up development, S3 for static content, and a basic API to manage simple actions like event triggers and messaging. Amplify enabled rapid application development on AWS, S3 for assets and messages, and API Gateway inside EC2. We used CloudWatch for monitoring, very simple. This helped us test and validate the idea fast, but I knew it wouldn't scale. This is a true MVP, very simple but fast to test.  But I had a challenge. As more communities joined, we needed better scalability, stronger security, and more flexible services to build new features faster.

[![Thumbnail 310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/310.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=310)

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/340.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=340)

And then I called Priscilla again to involve the community to solve this challenge.  We thought, we have a lot of talented developers in our community, so we can use this strong force to create something together. The reason that we collaborated with communities in Brasília, Goiânia, São Paulo, Belém, and other community users was to create together one thing that we call it. This collaboration  guided the architecture, and I understood what we needed and defined the next steps.

### Production-Ready Architecture: Building a Scalable Multi-Community Platform with AWS

Now I have this architecture, a production-ready architecture for the multi-community platform. We decided to use Route 53 to host the domain and manage DNS. CloudFront is for CDN delivery content and low latency anywhere in the world. WAF is for security, protecting the application by filtering malicious requests. The front-end layer is served by S3 and CloudFront for fast and high performance. The backend API runs in ECS Fargate deployed across multiple availability zones.

The backend API runs inside the ECS on Fargate. Application Load Balancer holds and distributes the requests around the Fargate tasks. For network and security, we use public and private subnets across availability zones for increased security, and NAT Gateway for connection to the internet. For the database layer, we use Amazon Aurora multi-AZ to improve its resiliency and availability.

ECR stores container images. For notifications, SNS handles HTTP and webhook notifications. SES is for sending transactional emails like registration confirmation, invites, new meetups, and others. Finally, for observability, CloudWatch provides metrics and alarms, and CloudWatch Logs for ingesting logs. This architecture provides a scalable, reliable, secured, and flexible architecture to support multiple AWS communities. And now I'm production-ready to scale. That's it, Priscilla.

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/490.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=490)

### Platform Benefits: Saving 70+ Hours Annually with Party Community

So let's go.  Let's talk about benefits. What kind of benefits do we have with this platform? Now we can forget about sending 20 or more messages to people. We can just use the console to send messages before or after the events and simplify the process. In the past, we used 10 or more inputs in different platforms. Now we can use one place to do the whole process and find new ways to connect people.

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/540.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=540)

Now we can use the platform to connect people more efficiently, and I estimate more than 70 hours per year saved to keep more energy for the people, to engage with the people, to have fun. That's the goal. Now we have the face of our console,  that's called Party Community because we have fun and we love so much to have great moments together. Through this, we can make management of all events, community days, meetups, and summits, whatever the community wants to organize, we can do through this platform.

[![Thumbnail 570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/570.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=570)

### Connect with Us: Join the AWS User Group Community in Brazil

So guys,  if you want to connect with us, we have this QR code. Feel free to connect with us. On the side, we have AWS User Group Brasília, the group that I'm in as a leader and member. So guys, thank you for your support. You're a rock. And we have AWS User Group Goiânia that Dan is in, and the other guys, and they are very partnered with us.

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/610.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=610)

If you want to connect with us and understand more about our platform and our challenges,  feel free to reach out. This is a special moment that I will share with you, highlighting the many faces that have helped us tremendously. We embrace diversity because we have many younger women working as developers on our team, volunteering their time for free. They contribute many commits to our GitHub repository, where we share our code and other resources. If you want to connect with them, feel free to follow them on their networks.

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0b1a7a8bd3ddcf99/680.jpg)](https://www.youtube.com/watch?v=9WJ21KPT1q8&t=680)

Thank you for your patience. Finally, we've reached the end, and it went by quickly. The final message is this: if you're interested in supporting us, if you have any ideas, or if you have coding skills to help increase the functionality of this platform, please connect with us. If you want to reach us,  we have our contact information available through these QR codes. You can connect with me or join us at events in Brazil. Feel free to reach out, and here is my contact information.

Thank you so much. You are all very talented architects. Thank you, and have fun at the rest of the event. That's it. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
