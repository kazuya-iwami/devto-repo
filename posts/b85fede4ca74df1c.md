---
title: 'AWS re:Invent 2025 - Developing AI Solutions: What Every Developer Should Know (TNC207)'
published: true
description: 'In this video, Satabdi, a Senior Solutions Architect at AWS, explores essential skills for generative AI developers. She addresses the AI readiness gap where organizations struggle to find qualified talent despite high demand. The presentation covers five core competencies: prompt engineering, Retrieval Augmented Generation (RAG), agentic systems, fine-tuning, and retraining. She emphasizes building AI applications on ethical foundations including fairness, explainability, privacy, security, controllability, veracity, robustness, governance, and transparency. The session highlights AWS certifications like AI Practitioner, Associate Data Engineer, and the new beta Generative AI Developer certification as trust signals for verifiable skills. Actionable steps include accessing free resources on AWS Skill Builder and pursuing AWS certifications to advance generative AI careers.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Developing AI Solutions: What Every Developer Should Know (TNC207)**

> In this video, Satabdi, a Senior Solutions Architect at AWS, explores essential skills for generative AI developers. She addresses the AI readiness gap where organizations struggle to find qualified talent despite high demand. The presentation covers five core competencies: prompt engineering, Retrieval Augmented Generation (RAG), agentic systems, fine-tuning, and retraining. She emphasizes building AI applications on ethical foundations including fairness, explainability, privacy, security, controllability, veracity, robustness, governance, and transparency. The session highlights AWS certifications like AI Practitioner, Associate Data Engineer, and the new beta Generative AI Developer certification as trust signals for verifiable skills. Actionable steps include accessing free resources on AWS Skill Builder and pursuing AWS certifications to advance generative AI careers.

{% youtube https://www.youtube.com/watch?v=obKSKTQgCRA %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/0.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=0)

### Introduction: Building Essential Skills for Generative AI Developers

 Hello everyone, I'm Satabdi. I'm a Senior Solutions Architect with AWS for the last four years now, and today we are going to explore the skills that make you exceptional as a generative AI developer.

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/20.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=20)

 Alright, so let's take a look at the agenda. First, we will see the current state of AI readiness, where organizations are, what skills are missing, and why the demand for generative AI developers is growing so rapidly. Then we will look at the essential competencies every generative AI developer should have, and that includes the technical competencies like model integration and prompt engineering, and also the applied skills like responsible AI and solution design. Then you will see how AWS Training and Certification can help you build and validate those skills that make you stand apart in this area. And finally, we will see the small actionable steps that you can take today to build and advance your generative AI career.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/70.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=70)

### The AI Readiness Gap: Talent Shortage in a Rapidly Evolving Market

 Okay, so what we are seeing in the market right now is not a lack of interest in AI, but it's the readiness gap. Almost all the companies want to add AI into their solutions, but they are not finding the talent of the people who can make that happen for them. AI skills and technology are not the challenge here, it's the talent. AI tools are growing faster than the workforce skill set, creating a real friction between ambition and execution.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/100.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=100)

 But for developers, it's a real opportunity. For those who can blend their AI knowledge with hands-on skills, they can make real transformation in their organization. And AI is not only creating new jobs and skills, but it is also transforming the existing jobs. So what skill set made somebody exceptional five years ago will not be enough five years from now. That's why AI literacy, the way you can use your AI skills with hands-on knowledge and build applications, is becoming a critical baseline for career growth and development.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/150.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=150)

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/160.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=160)

### Five Core Technical Competencies: From Prompt Engineering to Model Retraining

 Now we will see, oh sorry, I'm pressing the wrong way.  Now we'll see the core competencies that every generative AI developer should build, and it starts with prompt engineering. It's the way you guide your LLM model with proper context, examples, and output cues. Next is Retrieval Augmented Generation, also known as RAG, and this is how you ground the model to your data so that it provides accurate and trustworthy responses. And next come the agentic systems. This is where LLMs stop responding and start gathering information and taking actions.

[![Thumbnail 200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/200.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=200)

 Once you have the three core competencies, you move on to fine-tuning, where you customize a model with your own data so that it speaks to your company's domain. And finally, it's retraining, where you build or adapt a model from the ground up for specialized use cases. And together, these five skills transform generative AI from the tools you use to the capabilities you own.

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/230.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=230)

### Building Responsibly: Ethical Foundations and Operational Principles for Generative AI

 So now we have seen the core competencies, and they define the generative AI applications you can build. But when we talk about building generative AI applications here at AWS, they are built on some strong ethical foundations, and next we will see how we can build generative AI applications ethically, responsibly, and at scale. And it starts with fairness, so that the models and applications we build don't disadvantage any group. Next is explainability that lets the team evaluate and understand why the model reached a particular conclusion. Then comes privacy and security. We put high emphasis on that, protecting the data, protecting the model data, and the people behind these models. And then finally comes privacy that allows unintended system behavior and hurtful system usage.

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/290.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=290)

 This now, this is the part of the operational side where controllability allows you to guide the AI model. The veracity and robustness make sure the model output is correct even under stress.

Governance puts accountability in every step of the AI lifecycle, and transparency enables stakeholders to make confident choices with conscious decisions.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/320.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=320)

### AWS Certifications: Validating Skills Through Trust Signals

 Now that we have seen what developers are learning, developing the skills, and responsibly building the AI context side, we will see what organizations are looking for. Employers are looking for trust signals that somebody has real verifiable skills they can use. It's nearly impossible to verify every skill somebody has during promotion or hiring, but certification can close the gap. Certification sets a clear benchmark that somebody has verifiable skills, and that is why we are seeing certifications showing up in more and more job requirements.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/370.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=370)

 Here are the key AWS certifications that we have available at this moment. We start with the foundational AI Practitioner, then the Associate Data Engineer, and we also introduced a new certification, Generative AI Developer, which is in beta mode right now. This is the progression you can have with your AI skills. We also had a 50% off voucher in your re:Invent portal when you signed up for re:Invent, and you can use that voucher to take any of the certifications.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/410.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=410)

 Here is feedback from one of our practitioners who took the exam, and I will give you some time to read the feedback. This is exactly the kind of feedback we had in mind when we built the certification suite we have for generative AI builders.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/440.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=440)

### Taking Action: Free Resources and Next Steps on AWS Skill Builder

 Now moving on to the next slide, the actionable steps. Generative AI is already here, and we have to build the skills to build the future. This is the QR code that you can scan. It will take you to AWS Skill Builder, where there are some free resources available for you this month. You can take the first AWS certifications, Cloud Practitioner and AI Practitioner. This also has labs and all the skills and resources that we talked about in this presentation that you can take at your own time to build the skills you need to build the future.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b85fede4ca74df1c/480.jpg)](https://www.youtube.com/watch?v=obKSKTQgCRA&t=480)

 Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
