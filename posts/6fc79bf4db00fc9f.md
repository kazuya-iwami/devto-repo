---
title: 'AWS re:Invent 2025 - From punched cards to AI: The developer’s new mindset (DEV205)'
published: true
description: 'In this video, Anda-Catalina Giraud traces software development evolution from punched cards and ENIAC to AI-assisted development, demonstrating how core soft skills like critical thinking, collaboration, and precision remain essential across eras. She contrasts "vibe coding" (quick prototyping with AI prompts) with spec-driven development, advocating for clear intent and specifications before coding. Using Kiro IDE, she demonstrates building a tic-tac-toe game through spec-driven development with requirements, design, and task definition phases. She emphasizes providing better context to AI through agent steering files, MCP servers, and best practices to ensure system maintainability and deep understanding.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - From punched cards to AI: The developer’s new mindset (DEV205)**

> In this video, Anda-Catalina Giraud traces software development evolution from punched cards and ENIAC to AI-assisted development, demonstrating how core soft skills like critical thinking, collaboration, and precision remain essential across eras. She contrasts "vibe coding" (quick prototyping with AI prompts) with spec-driven development, advocating for clear intent and specifications before coding. Using Kiro IDE, she demonstrates building a tic-tac-toe game through spec-driven development with requirements, design, and task definition phases. She emphasizes providing better context to AI through agent steering files, MCP servers, and best practices to ensure system maintainability and deep understanding.

{% youtube https://www.youtube.com/watch?v=uhrUS6vUYmg %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/0.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=0)

### From Punched Cards to Agile: A Journey Through Programming History

 Hi, everyone. Thank you very much for coming to the session. The title is From Punched Cards to AI: The Developer's New Mindset, because what we build and how we build has changed, but how we think about building, that's where things get interesting.

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/20.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=20)

 My name is Anda-Catalina Giraud, and I work at the intersection of software development, design, and system thinking. It's been a couple of years where I work at the edge where people and systems meet, and that edge actually is moving again. I'm also a community leader and I get to talk with community members about the impact on our jobs today, and they have questions like, how do we collaborate with AI, what are the skills that matter nowadays, and how do I stay relevant? Maybe those are some questions that you've been asking yourself.

[![Thumbnail 60](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/60.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=60)

 So, probably you've all received one of these. This is a gift that I handmade for the session, especially. I would invite you to open it and have a look at the cover. The question is, who knows what the cover is? Anyone? It's a punched card, yeah. So this one is a blank one. It has no holes, and a punched card with holes actually is the equivalent of a line of code of 80 characters in your GitHub repo nowadays. Please keep these notebooks close to you. We're going to use them later as a pause button. Of course, it's a design choice, but also a bit of history.

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/120.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=120)

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/130.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=130)

So I invite you to take a couple of minutes with me and see how our job has evolved through time. From punched cards to high-level languages to agile methodologies,  and now the AI-assisted development era.  Here we have an image of the first general-purpose electronic computer, which is ENIAC. As you can see in the picture, the way we programmed it was quite manual. We needed to set up switches, we needed to plug in cables, so we may understand that a lot of planning was involved. Documenting everything was needed, and if something wasn't working well, a systematic approach of debugging things was needed. People worked at the time in teams and we also understand that the way of communicating and aligning everyone on the project goals was very important.

[![Thumbnail 180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/180.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=180)

This machine had no memory. So how did we input information and where was the information stored? And then we go back to the punched card.  Here we have a punched card with holes. The holes represent presence of digital information. The way we initially punched the cards was, as we see in the image here, where the lady is using a machine manually punching them. One card, one line of code. One program, one stack of punched cards. Then we would go with that stack of punched cards to a machine, input them, and then we need to wait. Sometimes we need to queue because we only have one machine available, right, to see the output.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/240.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=240)

If we had an error, then it might take hours to find out that we had an error, and then we needed to find out where that error was, right? And we need to go back and look at everything that was done, the design, the documentation, and find the card that wasn't right, if it was only one or multiple. But eventually, things evolved.  And we actually had programs that finally had symbols instead of cables, but everything was still painfully close to the machine because now developers had to deal with registers and memory operations. Instruction sequences was a thing. Developers still used the punched cards to input the information, hand it over to the machine, but this time the programs were loaded into the logical memory, executed, and then we had the results.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/280.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=280)

Eventually things evolved, as you might know, and  we had the very first high-level programming languages. You might have heard of Fortran and COBOL. We still have systems running that were written in these programs today, especially in the finance industry. And what changed here with these programs is that the developers now work more with mathematical formulas and also business logic than the individual instructions as it was the case with assembly.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/320.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=320)

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/330.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=330)

My parents are actually working in IT, and when they studied at university, they would code in Fortran. So they would design their code,  they would use this kind of code sheets and then feed this to a machine that would eventually punch the cards for them. And here we have  a line of code in Fortran. The whole process was still pretty intensive because they still needed to queue to do everything, and it still took ages. They needed to work in teams to collaborate, communicate, and debug.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/350.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=350)

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/370.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=370)

 Things changed a little bit more with the following programming languages that appeared, and eventually my parents went from programming in Fortran to programming in Pascal. And this is the very first programming language that I saw myself at home on the computer. And there are some of these programming languages that I used through my career.  But now applications grew in size and lifespan, and programming became more a collective, intense process. Teams adopted new practices such as version control, modular design, and code reviews.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/410.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=410)

Also, at this time, if you will look at the years, you also know that internet adoption grew at a high speed. So teams started to work together and collaborate, being at a distance and being online, and that actually introduced new methods of working. Who here already uses Scrum? Hands up.  I can see almost everyone. So that actually appeared in the 90s, and until the 2000s, there were multiple similar methods. Only in 2001, they were formalized under the Agile Manifesto, and we know them as Agile methods today.

And what was formalized was the fact that software development required constant adaptation. We had short iterations, continuous feedback, user involvement, incremental deliveries. It became standard practice. So Agile placed interpersonal skills in the center of the software practice because it was more about individuals' interactions than the process and tools themselves. So as I went through this quick overview, you might have noticed some of the soft skills that developers had to use through every age, and we are actually still using them nowadays, and they are very much needed with AI.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/480.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=480)

### The AI-Assisted Era: Vibe Coding vs. Spec-Driven Development

And some of these soft skills are critical thinking,  collaboration, communication, and precision. And if I take precision, developers needed precision when they manually programmed. They needed precision when they punched their cards. They needed precision when they were writing the instructions. And now we also need precision when we interact with the AI systems because we need to be quite vigilant when AI writes code that looks right but isn't.

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/510.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=510)

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/540.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=540)

Yeah, you heard that? So we can say that now we are  in the AI-assisted era or natural language programming era, and we're using things like prompts. Here I have one from a community library, prompt.dev. You might go and check that out. There are multiple community prompts. And the way we do it, pick up your favorite AI assistant, use a prompt like this, hit submit button, and wait for the result. And this is called vibe coding. Who already  here, who here already vibe coded? Hands up.

It's almost everyone, but I think maybe everyone. The thing is with vibe coding, having the first version is very quick, having a prototype, but once you want to change things, to go forward, to implement new features, then it becomes really time consuming and you need a different approach. And now a different approach would be going closer to the traditional way of designing your systems, building your specs, having a clear intent. And once you have your spec clear, then you move forward to coding. And on the other hand, we have the spec-driven development methodology.

[![Thumbnail 590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/590.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=590)

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/600.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=600)

 Now, let's see how we can apply this to our software development life cycle. So here is  this life cycle that you might have been aware of. It's the Agile one.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/620.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=620)

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/630.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=630)

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/640.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=640)

As we look through the different cycles, the question is where can we apply spec-driven development? Actually, we can apply it starting with the requirements and the design, and once our intent is clear,  we move forward to development, testing, and eventually deployment. When you're in the development phase, you might go  back and forth with your assistant or AI system and apply some of those vibe coding skills or prompt engineering skills, but this would be within the spec-driven development.  So let's say if you vibe code alone, great for prototyping, you can quickly make decisions. If you decided to go forward with the product, start by implementing your specs first, designing your intent, and once that is clear, move forward.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/670.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=670)

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/690.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=690)

I work myself with clients, and when we talk about AI, there is a similar question to this one that comes up. How do I deeply  understand the system that I'm building with AI in order to ensure maintainability? Because in the end, it's all about how do we build them to live through time. One way of doing it is providing better context. I have a picture that is from Anthropic's  website that is comparing prompt engineering and vibe coding with a system prompt on top of it, and on the other side, we have context engineering, providing better context.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/730.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=730)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/740.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=740)

Now, what's better context? We already developed, we know the context that we need. We need the best practices of our team, we need the best practices of a company, we need samples of similar projects, we need to tell the AI which languages to use and what not to use, we need to tell them which tools to use and which not. All of these we provide to the AI, and based on the context, we're going to move forward with our system. So  if you go back to the vibe coding and the spec-driven development, the question is, where are we doing this? And I think you already know the answer, right?  Kiro.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/760.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=760)

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/770.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=770)

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/780.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=780)

### Building with Kiro: Demonstrating Context Engineering in Practice

So I prepared a short demo of Kiro. Here we have the IDE. Kiro is an IDE which is a fork of Visual Studio Code, and on the right side you actually have something new. You have the way to interact with Kiro, and we have the vibe coding  mode where you are able to explore by testing and prototyping, and then the spec-driven development mode where you would first start by building your specs.  And then you also have the Kiro tools: specs, agent hooks, agent steering, and MCP servers that you can add in there. 

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/790.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=790)

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/800.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=800)

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/820.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=820)

When I talk about the context, by providing agent steering files, it's actually a way you can provide better context. Here you would say, hey, these are  the guidelines when we are committing our messages in GitHub. Hey, we don't write files that have more than 500 lines of code, or maybe less.  So you're providing all these guidelines to your AI system. Then you would need to add also if you have some MCP servers that you're using nowadays. These tools, these ones are standard from AWS Labs, it's a bunch of lists, so I added them here. 

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/840.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=840)

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/850.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=850)

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/860.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=860)

So what we're doing here is building a tic-tac-toe game with spec-driven development. As you can see, the first step that Kiro is doing is building up the requirements. For the demo purposes, I accepted everything that Kiro proposed, but normally I would go in, read  through, adjust my requirements, and when I'm ready, I'll move forward to the design. Once the design is proposed, same, go read through, work on your  design, and when you're ready with your design, you will then move forward to step number three in your spec-driven development cycle, which is defining the tasks. 

[![Thumbnail 870](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/870.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=870)

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/880.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=880)

[![Thumbnail 900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/900.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=900)

You can see that some of them are in white, which are the mandatory tasks that Kiro would need to execute, and some of them are in gray, which are optional.  Here the optional ones are the testing ones, and there is one that I actually put mandatory, and we're going to see that it had an impact. Okay, I'm  happy with my specs. Go build the system for me. As you can see, it's starting looping through the tasks, executing them. Actually, the very first task, which is set up project structure and dependencies, took most of the time. It was like back and forth.  I don't have the permission to execute, then you don't have this installed, I need to install this.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/910.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=910)

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/920.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=920)

[![Thumbnail 930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/930.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=930)

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/940.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=940)

[![Thumbnail 950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/950.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=950)

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/960.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=960)

[![Thumbnail 970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/970.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=970)

But once the first task was validated,  it would run through the different tasks quite quickly. We're going to see that eventually,  Kiro would stop at task 4.1.  So the task 4.1 is actually the testing task for task number 4. And it was interesting because  when running the test, there were a lot of errors. The code wasn't compliant with the requirements. So we had like four or five times to go back and forth  fixing the code, executing the test, not working, fixing the code, executing the test. Eventually it worked. It went up until the end, and then  I would eventually get the game running.  And here it is. I played a little bit with it. It was working. I was happy.

[![Thumbnail 980](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/980.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=980)

But you may imagine that using spec-driven development isn't just for building this kind of game.  This is nice for the demo. But now if I want to go back and modify my spec, it's easy because I have a place where I can go modify only the part in the spec that I want to modify the requirements and then ask my AI system to go modify the code that is associated with it. And if you compare that with vibe coding, you would eventually ask it in the chat. Eventually you run out of your context window, and then it starts to be quite time consuming and it's quite difficult to track actually how the specs evolve.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/1020.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=1020)

Okay, now is the time to take out  your small notebook and a pen. I had some pens actually to hand over, but I'm not allowed, so hopefully you all have a pen. But eventually you all got the notebooks and actually you're going to find a question in there. And I would invite you to note down a skill, soft or hard, that you intentionally want to grow in the next six months. And to go forward, what you can add is a calendar reminder six months from now to go back and check in where you are standing.

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/1090.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=1090)

[![Thumbnail 1110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/6fc79bf4db00fc9f/1110.jpg)](https://www.youtube.com/watch?v=uhrUS6vUYmg&t=1110)

Because in the end, our jobs are evolving, we have new strategies and tools, but it all starts with experimenting. Probably we don't have the perfect workflows for now, but things are going to come as they all did with the compilers. Developers didn't trust compilers back in the time, so I'm very eager to see where things will move forward. And if there's one thing to take out of the talk with you  when you leave, remember that you're going to design your intent, that you have to have clarity of what you want to achieve, and you're going to guide the machine because it's more like a collaboration than only you telling what instructions to execute. So thank you very much. 


----

; This article is entirely auto-generated using Amazon Bedrock.
