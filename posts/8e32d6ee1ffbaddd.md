---
title: 'AWS re:Invent 2025 - A leader''s guide to emerging technologies: From insights to rapid action-SNR203'
published: true
description: 'In this video, former CIO Arvind and NASA''s first CTO Tom Soderstrom address the dual mandate facing technology leaders: delivering operational excellence while championing innovation. They present eight major technology trends including generative AI, edge computing, robotics, medical impacts, data growth to quettabytes, crypto, quantum computing, and energy. The centerpiece is Tech Recon, an open-source multi-agent AI system they developed that automatically scans technology landscapes, generates reports, and scores technologies by impact, maturity, and momentum. They demonstrate how this agentic solution creates both broad landscape analyses and deep-dive position papers tailored to specific industries like pharmaceuticals. The speakers emphasize building a Center of Engagement rather than a traditional COE, creating fuel for innovation through cost savings, and measuring return on attention (ROA) instead of just ROI. They provide the GitHub code and a 90-day implementation roadmap to help CIOs systematically track emerging technologies and engage business leaders strategically.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/0.jpg'
series: ''
canonical_url: null
id: 3088152
date: '2025-12-06T05:36:22Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - A leader's guide to emerging technologies: From insights to rapid action-SNR203**

> In this video, former CIO Arvind and NASA's first CTO Tom Soderstrom address the dual mandate facing technology leaders: delivering operational excellence while championing innovation. They present eight major technology trends including generative AI, edge computing, robotics, medical impacts, data growth to quettabytes, crypto, quantum computing, and energy. The centerpiece is Tech Recon, an open-source multi-agent AI system they developed that automatically scans technology landscapes, generates reports, and scores technologies by impact, maturity, and momentum. They demonstrate how this agentic solution creates both broad landscape analyses and deep-dive position papers tailored to specific industries like pharmaceuticals. The speakers emphasize building a Center of Engagement rather than a traditional COE, creating fuel for innovation through cost savings, and measuring return on attention (ROA) instead of just ROI. They provide the GitHub code and a 90-day implementation roadmap to help CIOs systematically track emerging technologies and engage business leaders strategically.

{% youtube https://www.youtube.com/watch?v=NwkH_F3yUiY %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/0.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=0)

[![Thumbnail 30](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/30.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=30)

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/40.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=40)

[![Thumbnail 50](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/50.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=50)

### The Dual Mandate Challenge: When CEOs Ask About Emerging Technologies

 What's our AI strategy? I want efficient customer service.  I want automated customer onboarding. The board wants an update on our view and plans for impact of quantum on our business  for the board meeting tomorrow.  Show of hands, how many of you felt like that pilot in that scene we just saw? Right?

So as a former CIO, it's been more than a few times that my CEO has walked into the room asking, hey Arvind, what's going on with this new technology? And in that instant I shift from being a trusted adviser to a scrambling researcher. And now at AWS, as we talk to more and more customers and CIOs, this pattern is repeating more and more often.

[![Thumbnail 100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/100.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=100)

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/110.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=110)

The reasons are very clear. Over the last 25 years, a range of technologies have completely transformed how industries in every sector and every function have been transformed more and more by technology, and this is only accelerating  powered by a combination of technologies that are accelerating and creating more and more impact, whether it's AI  or robotics or biotechnology. The cool thing really is that it's not just that these technologies are accelerating independently in isolation, but they're actually feeding into each other, creating an exponential impact that's accelerating impact for businesses as well.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/150.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=150)

And the interesting thing is that of course this is not just something which is hidden somewhere. The media is talking about it, vendors are talking about it, there are industry events, and your business leaders are going into those events and learning about it there, and they're wondering why they're not hearing this internally in your own organizations.  And this is a major challenge that we've got to overcome.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/170.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=170)

Now the real challenge is that it's not that our core work of technology delivery is getting any easier, right? Our work is getting more and more critical, more and more central to the business, and delivering excellence  is absolutely non-negotiable. But at the same time, the technology leaders are also being expected to be the innovation champions, the scouts, the people who bring in and shape and reshape existing organizations to be more modern and digital. This dual mandate is making things more and more complex for our roles.

### Building a Digital Kampong: Arvind's Approach to Technology Innovation

Now I was a CIO, but I joined AWS a year and a half ago. Before that I was a CIO for Kellogg's based in Singapore, and before that with Prudential Insurance and Procter and Gamble. And over the last 10 to 15 years I've seen this pattern repeat again and again where business leaders are getting more and more interested in technology, and the way I've responded to that is I've leaned into it and created a network of people around me in the technology organization and in business that I used to call the digital kampong.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/240.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=240)

Kampong is a Malay word. I live in Singapore, so Malay is a language that's used there. Kampong is a Malay word for village,  and this is a situation where you want people to come together to address this challenge. What I did is worked with enthusiasts who were keen to learn new technology, had that learning culture in them, and work with them to do a lot of experiments to test out new and emerging technologies.

Ten to fifteen years ago, a lot of this stuff was very new. So one of the first things you see on the top left is we built Alexa skills. Amazon had just launched Alexa then to figure out how consumers would engage with voice systems. We built IoT sensors that helped manufacturing teams figure out how automation can be accelerated, and a number of other such things. Over time we did more and more of this, eventually including the families of some of our employees, because nothing gets people excited more than having their kids learn new technology as well.

So there were a number of learnings I personally had that we will share about this now, and now I'll invite Tom to share his background on this space.

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/320.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=320)

### Tom Soderstrom's NASA Experience: Predicting Technology Trends That Matter

Thank you, Arvind. My name is Tom Soderstrom, and I was NASA's first CTO at NASA's Jet Propulsion Laboratory. The slide that you see here was the attempt at how can we spend less money on IT and more on science and engineering,  and we looked at cloud computing as one of the answers. And yes, it worked, and you can see the journey here from 2008 and beyond. The idea was that if we could use cloud and AWS saw a business for it, then they would build it and we would help launch it. That's how we came up with so many of the services that you use today.

So the secret here for you all is if you're interested in something that AWS is interested in, you have an extremely strong voice, not just for what is built, but how it's built. And of course, we're going to talk about some trends here. I was the futurist at Jet Propulsion Laboratory as the CTO, and who in here considers it their responsibility to keep understanding what the technology landscape looks like? Show of hands. Oh, good, good, then we have a gift for you because it's a damn hard job, honestly.

So what we did is it was fun to hear you talk about Glacier or Alexa, so she's not listening, is she, because one of the first things we saw is when we came up with GovCloud and AWS built it, it created psychological safety for people to innovate in all kinds of ways. Have you ever heard of a two-way door decision? That is the way of using the cloud. So we created the first NASA Alexa app, and it's called NASA Mars, so it still runs. But this is about technologies.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/420.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=420)

And what we're going to talk about is, so you have this dual mandate of figuring out what are the technologies that are coming and at the same time keep the plane flying.  So we're going to have this in two pieces. We're going to, we looked at the trends that are coming, we looked at it manually, and we're going to share what they are. And at the end of this, I'm going to ask you to bring your cell phones up and see what you care about, which ones you think are interesting. But then, we're going to show you how you can do this automatically. So this will be our gift to you all.

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/450.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=450)

So what are the major technology trends?  You are already looking at it, and the point is not to predict what's coming in the world. The point is to predict for your company how will it make you more productive, more profitable, how will it save money using these new technologies. In fact, if you build it, they will come if you guess right, or if you don't want to build it, then somebody else will build it, and that's how we worked at NASA.

[![Thumbnail 480](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/480.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=480)

So when you are looking at the future and you are wrong, it's easy to be ridiculed. And you can see some of these fun ones here.  I'm very happy that he was wrong about the airplane, otherwise we'd be speaking to a very empty room. So if you predict right, you can get a first mover advantage, you can excite your people to participate, you can impact the creators of what's being built, and you can also get a first mover advantage, or you can detect very early that you're way off and this trend doesn't matter.

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/520.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=520)

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/530.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=530)

### Cloud and Generative AI: From Experimentation to ROI Generation

So now we'll go a little deeper into each of these, but these are the eight that we saw were interesting, and  two of them are more questionable than the others, and I think you can guess which ones they are, but we'll find out. So, cloud and generative AI belong together.  You really cannot be successful in generative or agentic AI without using the cloud. It just needs that much horsepower.

In 2020, we were experimenting, we were trying to see what we could do with generative AI and beyond. In 2025, we're generating ROI, return on investment. Where are we? With a show of hands, how many people here have in production a generative AI that is generating an ROI? Raise your hands very proudly if you do. Okay, it looks to be about 5%. All right. What about agentic AI? Do you have something in production? One person, two, three, four. I feel like a salesman here. Is it generating ROI? Next year when we ask the same question, a lot of hands will go up. That's the nature of these technologies.

And $15.7 trillion estimate, that's a big number, and new jobs that will be created. We think about AI stealing our jobs, et cetera. It's actually going to create new jobs, and your opportunity is to create those jobs and staff them with your own people to see if they pay out. If they do, you're way ahead of the game, and if you're right, then you couldn't afford to hire those people later anyway.

So it's a good way to just brainstorm what are the new roles that will come.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/630.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=630)

Now when we think about it, and I thought about this long and hard, what is AI in reality? So remember hailing a taxi. Actually, is anybody here from New York? You probably do that today. You trusted that they would stop for you.  Then later on you trusted that if you clicked your smartphone, they would come and get you. What we're seeing is this judgment is now that the self-driving car, you will trust that the car will get you there safely. So it's all about judgment.

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/660.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=660)

This happens to be Zoox. Has anybody been in a Zoox? They're here in Las Vegas. It's fun. I highly recommend you do it, but it's this trust. And when we look at AI at scale, anything from, anybody use Rufus to help you shop?  It's awesome. Ask for a coffee maker that is easiest to clean, and it'll give you an example. The other thing that we don't see is regular machine learning is not going away. For instance, Rufus on the back end filters out 275 million fake reviews, so you don't have to look at them. It turns out the reviews is one of the most popular things. So generative AI, the benefit of this is that this is working at scale.

[![Thumbnail 700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/700.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=700)

So it's AI, it's moving forward, we're already there, but what's coming? It's where AI is becoming a personal assistant. It's where the future of education.  Anybody have young children who ask why over and over and over? Well, AI doesn't get tired. AI will engage the student and it will help them learn the way they want to do. Amazon CTO Werner Vogels said, and I'll quote this, when you use a tool to engage curiosity instead of enforcing compliance, schools spring to life and that changes everything. I think that's really key.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/750.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=750)

And for the teachers, they can now use generative AI and agentic AI for the drudgery, grading the reports, et cetera. So it's really about how we use AI. So you'll hear a lot about generative AI and agentic AI at this conference because it is the future. If you were through the cloud evolution like I was,  this is at least as big, if not bigger, and it's really exciting. If you went through the cloud evolution, take those lessons learned, and you can now go through the agentic revolution.

### Productive at the Edge: IoT, Digital Twins, and Connectivity Solutions

So productive at the edge. Anybody heard the word IoT? You're tired of it, it didn't pay out, didn't have any ROI. Well, that's changing. Because we have 20 billion devices today, by 2030, we'll have 40 billion devices, and they will be connected, and they cannot send data all the time. So what we're going to see is we're going to see that the return on investment between business outcome and being productive at the edge is going to be key.

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/810.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=810)

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/820.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=820)

Today we talk about large language models. Tomorrow we're going to talk about nano-language models where the AI will run on the device. Better silicon, smaller models, and it will create a digital twin. I'll talk about that in a second. But what are some people who are doing this well?  KONE, anybody know who that is? Anybody ever ride in an elevator or an escalator? They keep the cities running.  They have 30,000 maintenance technicians. It's a Finnish company. They go on 80,000 customer calls per day, and 30,000 customer calls per month is filtered out because of agentic AI.

They have everything at their fingertips. When they go to fix an elevator, they know the version, they know the history, what usually goes wrong, and they're much more productive. So IoT at the edge is really powerful. We also have AI in the jungle. I just like saying that. AI in the jungle is Hexagon, a Swedish-Swiss company, has a subsidiary called R-Evolution, and it's about measuring biodiversity.

So they have a device that sits in the jungle and measures biodiversity over time. It's sitting at the edge and it reports back when it has connectivity. And that's nice because we do want to save the environment, but if you're a mining company and you want to do a mine, you have to prove that you are actually not disturbing biodiversity. So there's business reasons for AI and IoT at the edge.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/890.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=890)

So we're going to see this massive digital  scale as twins, digital twins. And what it means is that they're collecting the data at the edge, they're reporting it back into the cloud. And now you have a copy in the cloud of what is in the physical space. So now you can start doing what-if analysis. You can inject faulty data and have a conversation with all of that data in the digital twin using generative AI.

[![Thumbnail 920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/920.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=920)

Anybody who's been affected by a wildfire or knows somebody affected by a wildfire?  Yeah, I live in LA, so very affected, but it's coming faster and faster, and natural disasters are coming more and more. How do you deal with it?

This is where we see the rise of augmented reality again, where you can insert people into a difficult or dangerous or far remote place without them having to be there. They can interact with the digital twin to see how fast the fire is spreading, what it detected early. There's a company here called Voxalis.AI. They're on the third floor of the Venetian here, and they have a device that they put on helicopters. And now the helicopters can just measure how is there a fire spot coming up, or they're in the middle of fighting the fire, and it's a startup of about 20 people. And what we're seeing is we're seeing the benefit of having startups working with governments, working with universities, especially on things at the edge and AI. So if you're in that space, you're in luck. It's growing fast.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/990.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=990)

Now they will need access, and if you've heard of Kuiper,  forget that it's called LEO, low Earth orbit, and that is to be able to provide internet access to underserved places in the world. They're also here at re:Invent, and I highly recommend the conversation with them because it's much cheaper and much faster and lower latency than anything we've ever seen before. Countries can now get backup if the fiber was cut, and the devices at the edge can get internet access.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1020.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1020)

### Robots and Cobots at Scale: From Fulfillment Centers to Last-Mile Delivery

Number three here is robots  and robots at scale. What on earth is a cobot? Collaborative robots. We're seeing people and robots working together, and it's a $165 billion market in just a few years. That's a big number. All of these have a cumulative, we picked them because they're very large dollar amount, and they have a very large cumulative growth rate, annual growth rate.

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1050.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1050)

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1060.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1060)

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1070.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1070)

So these robots,  they are amazing. So they're everywhere. They're on the moon. Of course I care about that being from NASA, but  if you think about something unpleasant for a second, wars, they're being fought with drones. But what we're seeing is we're seeing much faster adoption in the civilian space from things that grew up in the defense space.  We're seeing people who fight fires use the same technology that people who fight wars.

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1080.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1080)

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1090.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1090)

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1100.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1100)

Also,  we're seeing people actually form relationships with the robots. If you have a Roomba, raise your hand if you have a Roomba. Raise your, keep your hand up if you named it.  Half people name their Roomba. In Sweden, where I grew up, they have automatic lawnmowers. 100% name them. It becomes a pet.  And we find that these robots in hospital, kids will interact with them and be much calmer. Aging people will have a companion and they need less medicine and less care. So it really becomes key.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1120.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1120)

[![Thumbnail 1160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1160.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1160)

Now, for us at Amazon, we've been in a fulfillment center.  If you haven't, I highly recommend it. It's a toy box for robotics. So if you look at the person on the top left, she is stuffing things that you bought, and the red boxes say don't put it here because it's close to something that looks similar, and so she doesn't have the cognitive load of having to decide where to pick it. It saves a lot of time. On the right is a robot doing the same thing, augmenting it, but going high and going low, so to save her back. In the lower left-hand corner, the picker is giving a right angle where to pick, picks it from there, picks it, scans it, done.  Fewer errors, much faster.

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1170.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1170)

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1180.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1180)

[![Thumbnail 1190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1190.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1190)

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1200.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1200)

And then we have the SLAM process where in those few seconds that you see there, this is actually in real time, it is taking  all the big data and it's putting it on the label where, who is it and where are they going. The box is already sealed, so there's complete privacy. Now,  how big is this? It's huge. The fulfillment centers, for instance, delivered 600 million packages the same day.  And that is going up. So the speed is about three hours from start to finish in the fulfillment center. The average delivery is 1.9 days from that you bought it. And the average  time to buy is about three minutes. So this is speeding up and going faster, and robotics is helping. But how do they deliver?

[![Thumbnail 1220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1220.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1220)

In the last mile, you have the driver now being able to use the same technology. You can see there it's going to shine a red light, don't pick that one, and then a green light, that's the one to deliver. That saves a minute per delivery,  which is a lot. Now you add augmented reality glasses, and they can now do the same thing with hands-free. So everything builds on everything else here.

[![Thumbnail 1240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1240.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1240)

### Medical Impacts and Data Revolution: Genomics, Digital Twins, and Synthetic Data

What we're seeing is medical impacts at scale is another one. In 2020, we never went to a remote doctor visit. Well then came COVID.  In one month, it increased 157% or 157 times. And now we're looking at it, yeah, you can actually have a doctor's visit. We're wearing wearables, we're counting ourselves, can we sleep better, et cetera. And that is going towards its personal AI guardian. Genomics is an area that uses a lot of data, a lot of AI, and you can even in the future be able to get a prescription before you even know that you have the disease because it looks at the genomic data and it tells you what you need to do. So it becomes your guardian, and it's a big business.

[![Thumbnail 1290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1290.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1290)

And this genomics is not just good for people, it's good for koalas. I want to tell this story about a researcher in Sydney, Australia, who had a lot of genomics data,  but she didn't have the people and the skills to analyze it enough. So she put it in Amazon's open data exchange, and a researcher in Sweden was able to solve it in just a few days. And do you know how you save the koala? You vaccinate them from chlamydia. Who would have known? Well, I'm happy to say that two months ago that was actually approved, the vaccine in Australia, so there are big hopes for the koala. The point of that is, if you share the data and you share how you can work faster, you can do miracles.

[![Thumbnail 1330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1330.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1330)

[![Thumbnail 1350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1350.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1350)

And digital twins is not just for things, it's for humans.  So if I have a disease, they can have a digital twin of me because everything is being measured, and they can see where they should operate before they do it. And now they can bring in robotics and have somebody from outside actually do the surgery. So the medical impacts are fantastic, and we're going to need them. We lived till  about 47 years old in 1900, 78 in 2000. And in 2100, it'll be, I don't have a clue. But we're going to live longer. And we will have implants, and AI is going to be what keeps us healthy. So it's just not a new thing, it's very personal and very important.

[![Thumbnail 1390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1390.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1390)

[![Thumbnail 1410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1410.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1410)

The fifth here is data. There's no surprise to any of you that data is going faster. Anybody know what the biggest number we measure is? Yottabyte. And yottabyte was impossibly big. Well, we're going to get there in 2030. So now you need to learn a new word, quettabytes.  So a quettabyte is 10 with 30 zeros behind it. So if you learn kampong, and now quettabyte. So you learn so much here. But how are you going to analyze all this data? We're seeing agentic AI as being an ability here  to analyze the data that's coming and to deal with it differently. And the data is not just structured, it's unstructured 90% of it. And how do you get your arms around all of this data in your company?

[![Thumbnail 1430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1430.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1430)

The answer, as uncomfortable as it is, is you won't.  You won't get your arm around all the data, but it's okay. Just think work backwards on the problem and think about the problem you're trying to solve, and then use generative AI to tie into partner's data. What we're thinking here is flipping the script. That's the easiest thing we have seen successful companies do. Instead of you having to fill out a form to use my data, I have to fill out a form to protect the data, and it changes everything because now you can experiment using AI and Internet of Things, it creates a lot of data, and now you can start analyzing it.

[![Thumbnail 1470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1470.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1470)

You can also use synthetic data. Is anybody using synthetic data yet? Okay, very few.  So this is the hand scanner called Amazon One. I'm not on a run, I go into an Amazon Fresh, pick up a bottle of water, just hold my hand over and run out. That is because they trained it with synthetic data, and it's 99.9999%. It's very accurate. It's more accurate than an iris or a fingerprint, but it was the synthetic data that made it happen. So that's the opportunity here with data.

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1500.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1500)

### Crypto, Quantum Computing, and Energy: The Final Technology Frontiers

Sixth out of eight is crypto,  and crypto revolution.

We may or may not be right, but if you had invested in Bitcoin 10 years ago, it would be the single biggest investment you could have made. It's up and down, so it's worrisome, but now we have regulations coming and there is growth, and blockchain keeps going. The token economy means that you will have digital tokens for physical things, and that changes everything. Have you ever thought about how an agent will pay another agent, software agents? They'll probably use stablecoin, and they'll probably use micropayments, a fraction of a fraction of a fraction of a penny. So all of a sudden it creates a new economy, and decentralized finance may be one of them.

[![Thumbnail 1550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1550.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1550)

Then quantum computing.  So quantum is, we worried about building the hardware, and in the left-hand corner is Amazon's quantum lab at Caltech kept in Los Angeles. Now we've actually made error correcting qubits. So a qubit is not a qubit if it's error correcting. It's 90% faster and better than a non-error correcting qubit, and it's a large market. Where will it go? You can see it's a big impact. It's gonna have a lot of jobs. Where will it go as far as business applications? That's gonna be the question and when. So we think this is one of the key trends because of the growth rate and the amount of money, but what applications will we see, and which quantum computer?

[![Thumbnail 1600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1600.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1600)

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1620.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1620)

So these are four completely different quantum computers. We don't know which one will win  the ring that rules them all, but Amazon provides choice. So you can use Braket to try and to see which one makes the biggest impact. The important part here is to get your programmers to try to use quantum computing to see what you can do. Then we have Airbus and BMW looking at  being able to do logistics, being able to soundproof it, a lot of near-term opportunities. The biggest one of all, quantum safe encryption. That's where you worry about the future quantum computing decrypting your data, and so that's being worked on. That's the single biggest opportunity right now, and you don't have to wait. Use AWS's quantum safe encryption algorithms. Others do too. The key here is to use it.

[![Thumbnail 1650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1650.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1650)

[![Thumbnail 1680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1680.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1680)

[![Thumbnail 1700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1700.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1700)

The last one is, do we have enough energy for this?  We use fossil fuels, then we use renewables, and Amazon is the biggest producer of renewables and also purchaser of renewables. And now we're looking at nuclear power. It doesn't produce fossil fuel, but we're gonna need all of this energy. And it isn't just about energy to power things like windmills, it's about your energy to be able to handle this and having this dual mandate.  So if you pull out your smartphones, which of these emerging technologies will matter the most to you? Sorry, there's the QR code. If you do that, you'll be able to see  which ones of these do you care about. Click on all of them, click on none of them, and we're gonna see the results in just a few minutes.

[![Thumbnail 1710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1710.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1710)

[![Thumbnail 1740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1740.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1740)

### Tech Recon: An Agentic AI Solution for Automated Technology Reconnaissance

 So it looks like generative AI and data and energy, that's a surprise. I didn't expect that. Crypto, not so much. Quantum somewhat, and otherwise it's an even split. I think surprising not more people on edge computing, but this is why what matters to your company and having to do this on your own is hard work. So isn't there a better way?  Arvind, absolutely. So in the last 20 minutes we've done a kind of whirlwind tour of eight technology trends that we felt were important, but they may or may not be important for you, and this thing is moving so fast that you've got to build your own capability to stay in touch with technology trends.

[![Thumbnail 1770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1770.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1770)

[![Thumbnail 1780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1780.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1780)

[![Thumbnail 1790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1790.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1790)

What you need is to upgrade your cockpit with a radar system that can do this reconnaissance for you. What you need is a radar  that can sweep the landscape on a periodic basis and identify what technologies are starting to appear on the landscape.  And more importantly you need a systemic way that can then identify those technologies, understand how far are they from impact  with you, how fast they're moving, what's the business impact potential, and how mature are they to make an impact for your business. So that's something you need to continuously do and be able to share that with your business leadership to stay ahead in this dual mandate.

[![Thumbnail 1820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1820.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1820)

And very importantly, based on this data, you also need to then be able to tell if this is a technology that I should be deploying in my business right now because it's making an impact in my industry, in my peers and competitors,  or is this something which is close to making business impact and therefore we should be starting pilots or maybe experiments, or if it's something which is less mature but moving fast and potentially big impact, this may be something that requires monitoring. This is a lot of work, and Tom and I in our past have done this manually using that kampong to help us in this journey, but things are much easier now.

[![Thumbnail 1860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1860.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1860)

This is 2025. This is the year where agents can help you solve this problem. And we're not just suggesting that agents can solve this problem for you. We actually have a solution for you. So  a quick backstory here is when we were doing the research for this talk, Tom and I, as often happens in Amazon, we wrote a paper on this topic saying what CIOs need today is a systemic automated way to scan for technology landscape and do these assessments and understand what's important. And as again happens in Amazon, this paper gets distributed and people add to that, provide inputs, and some of them say, hey, I'll build this for you. So I want to introduce Jiyun Park, who's part of our Solution Architects team, and she raised her hand and volunteered and said, I'll build this for you. So what we're going to show you today and give you today, in fact, is an agentic system that can automate this entire reconnaissance work.

[![Thumbnail 1940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1940.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1940)

Thanks Arvind. Hi everyone, I'm Jiyun. Today I'm going to walk you through what is the Tech Recon powered by Agentic AI. So before I show you the result, I'm going to very briefly explain the architecture of the Tech Recon behind the Agentic AI. So the Tech Recon is a multi-agent system that researches public sources  like newspapers, consulting companies, and IT vendors and generates a series of documents to IT leaders so they can understand the fast-changing emerging technologies and deep dive where they need. So we have a supervisor agent here which orchestrates the entire workflow across the sub-agents.

[![Thumbnail 1970](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1970.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1970)

[![Thumbnail 1990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/1990.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=1990)

And we define the Tech Recon by a prompt  so it's going to support the CIO of the XYZ company in researching the emerging technologies. We iterated with this with many different prompts and finalized this with the best one. Inside the system, each agent has a specialized role which  orchestrates their respective tasks. So the planner agents create tasks for the other agents, researcher agents gather the information and they actually research on behalf of humans. Coder agents read, save, and create files, and reporter agents synthesize everything and make it as a final output. And these agents work in a sequential way.

[![Thumbnail 2030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2030.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2030)

So based on the user request, there are going to be two major outputs  which is the part one, Emerging Tech Landscape Analysis, which is going to cover the broad landscapes of the technologies, and part two, Position Papers on the specific technology, which dives deep on one specific technology and creates a report of your company's position papers. This is the strength of agentic AI systems with multiple agents which have their specialized roles, they're working together using the tools they need and making the final outputs autonomously.

[![Thumbnail 2070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2070.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2070)

[![Thumbnail 2100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2100.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2100)

 And as we were working together on this, what we realized is not only can it do research for you, which is gather all of these insights and convert that into summarized documents, but it could even do the analysis we talked about earlier. So if you build a slide, what the agents were able to do using coding capabilities and so on, just click this, yeah, is we created three measures, three scores for every technology that it found. We calculated  an impact score, and this is based on the work that actually was done at NASA on technology readiness scoring.

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2110.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2110)

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2120.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2120)

[![Thumbnail 2140](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2140.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2140)

The way this happens is very  specific to your company. June mentioned earlier that it's designed for your company. Let's say you are the CIO of a pharmaceutical company. It'll do the analysis and the impact analysis for  your industry, right? Similarly, maturity scoring measures how ready this technology is for use today, and finally momentum, which is how fast is this moving. Is this accelerating or slowing down? For every technology, we were able to create these three scores. Next slide. 

[![Thumbnail 2150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2150.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2150)

What we were able to do once you have those scores is build one more layer. Based on those scores, you could actually  reach some sort of judgment on whether this is a technology which is high impact and highly mature and is being used right now. If there are examples of use cases of this making an impact in your industry, then that's something you should deploy right now. But if it's earlier in maturity, then that may be something you want to pilot or experiment with. Whereas if there's something else which is very early stages, very immature but moving fast and potentially big impact, that's something you want to monitor. Now these were some parameters we chose as we were experimenting with this, but you can pick your own parameters and create how you want to assess and decide what to do with each of these technologies. Now June will show an actual demo of how the system works.

[![Thumbnail 2210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2210.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2210)

[![Thumbnail 2250](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2250.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2250)

Okay, now I'll show you the demo, but before I play the demo, I'm going to briefly explain the front end of the Tech Recon. As you can see in the middle, you can see the real-time logs as they are being streamed.  They're going to be the outputs of each agent and displayed in the middle of the screen. On the right-hand side, you can see the control panel and you can see the status of each agent. Below down there, there's a button so that you can execute and run to create the reports, which is the Part One Landscape Analysis Report and Part Two Technology Position Papers. Once all those tasks are completed, you can download all the files in the bottom right corner.  That's all right. I think we can just move forward, yeah.

[![Thumbnail 2270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2270.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2270)

[![Thumbnail 2350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2350.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2350)

After the demo is completed, you can see the result, which is the  Emerging Technologies Reconnaissance Report, which is Part One. This is the real result that you can utilize in your real world. This is the example Part One report for the CIO of the Pfizer company. If there's anyone here from Pfizer or the pharmaceutical industry company, congrats, you can get this report today. But otherwise, you can run this code in your local system and then just replace the variables with your industry and your company name. This Part One is going to cover the overview of the technology landscape, strategy recommendations based on the score assessment which Arvind explained before, and the industry-specific context as well. Because the Tech Recon follows a strict fixed template, you can run it monthly, quarterly, or whenever you want to catch up on the new emerging technology trends. 

[![Thumbnail 2380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2380.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2380)

[![Thumbnail 2390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2390.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2390)

After creating the Landscape Analysis Report, you can create the Technology Position Paper, which dives deeper into one specific technology. Here you can see Generative AI. As you can see, it includes the key findings, strategy recommendations, technology deep dives, and also the real-world use cases you can utilize. The Tech Recon is not only just covering the technical things, it covers the business as well. You can see  why this matters for Pfizer in this report as well. The Tech Recon is not a new  service. It's a blueprint so that your teams can deploy within a few weeks. On average, maybe it's going to cost less than two thousand dollars per year, but it depends on how much you're going to run.

The implementation timeline can vary depending on your organization. It can take less time or more time than expected. Many companies don't know where to start. They actually want to build the agentic AI, but they don't know where to begin. This is why the Tech Recon is so powerful, and you can start it today. We're going to give you the code architecture and the implementation guide today so you can run this and build your own strategic advisor with the Tech Recon.

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2460.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2460)

### From Center of Excellence to Center of Engagement: Building the Innovation Flywheel

So that's the technical side. Then what about the human side? Tom, I'm going to introduce it. I'm not sure it was clear, but the system actually scans the technology for you and writes those reports. That's what you're going to be asked for. That's what the dual mandate is. So you get a lot of help from what you and I have built, but it's not about technology alone. It's about people. So I want you on three to yell out what COE means.  One, two, three. But it shouldn't. So I created a Center of Excellence for cloud at NASA, and we've seen many companies do that. What do you do? You create a center of people who think they're excellent, and everybody else hates them. And then they create their own Center of Excellence. You now don't have just shadow IT, you have shadow COEs. So isn't there a better way?

[![Thumbnail 2490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2490.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2490)

There is with all of these new technologies that are coming.  What you really want is you want to work backwards on the business problem, and you want a Center of Engagement. You want people to try it, experiment with it, and be able to very scrappy test it. And if it pays out business dividends, you double down. So you think big, start small with these experiments, then scale fast. But they're business experiments, not technology experiments. And trust me, the companies we talked to, this works and the people who do it get promoted. So this is the way to actually test and try this out.

[![Thumbnail 2530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2530.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2530)

Now the other thing that it does is it engages teams and people beyond your organization.  This means you're all sitting here and hopefully you're talking to each other because those are the future collaborators that you're going to have. So the Tech Recon Center of Excellence can now reach out to business units, as many as you can, and then to external partners and the entire ecosystem intelligence. And you will start feeding each other information that you're looking for and perhaps even create new partnerships with these agents. And it all starts with the Tech Recon COE, or I'm sorry, the technology reconnaissance that we and you have built.

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2590.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2590)

But how do you fund it? How do you prove that this is worthwhile? Arvind, how do you fund it? Yeah, so I would say that the human side of this is probably the most important investment needed, but you do need some dollars as well. And in my experience as we got this thing going, one of the most important things was to create the fuel for innovation.  We all hope we'll go to our CFO and CEO and say, hey, we need to stay in touch with technology, emerging technologies, and please fund it. It's very hard to get funding for that. The way I've always done that is to start the innovation process, the new technology application process in areas that generate some savings first. And then I have the agreement with the CEO and the CFO that instead of taking that saving to the bottom line, we'll reinvest this into more innovation. And that's the reason I call it fuel for innovation.

[![Thumbnail 2640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2640.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2640)

And once you have the technical capability, the human side of it, and this little bit of funding, then the way you want to think about this is to get your flywheel of emerging technology and innovation going. The way Amazon likes to look at flywheels is  you think of this as a flywheel. You need to give it a start by getting some business leadership engagement, attracting talent into it. You get the right talent, the right ideas for innovation come up, and you make them successful. You invest in making the early ones successful, and that gets your flywheel going. And as you gather momentum, more and more ideas are generated and more impact on business happens. And as someone who's trying to get this Tech Recon capability going, unless you want to do this all by yourself, you've got to think about how the flywheel is running and how you can accelerate it further.

[![Thumbnail 2690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2690.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2690)

So that was one of the areas that we focused on. And a common learning that both Tom and I had was oftentimes the question that gets asked is ROI. And what I have done,  sometimes successfully, sometimes not very successfully, is kind of shift that conversation to ROA, which is a return on attention. What truly matters when you're looking at emerging technologies and early stages is, are there ideas that actually business cares about?

[![Thumbnail 2720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2720.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2720)

Are they paying attention to this? Are they putting effort into exploring this and figuring out how this can create value for them? The way we measure that is through how much engagement are we getting.  Are people showing up? Are they volunteering to be part of this ecosystem? Is the velocity of the ideas that are moving from early stage to actual experimentation growing? And finally, most importantly, is this leading to strategic conversations in the executive room saying, look, here are the ten technologies which are coming up, these have potential, we want to do experiments with these three, watch these five, and so on. That is the sign that this COE is working for us or not.

[![Thumbnail 2760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2760.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2760)

Now there are many challenges to making it work, of course. This is not straightforward.  The common issue in the earlier stages is you launch this thing and nobody shows up. No one has time. Everyone's very busy. This is a classic issue, and again, investing in the early few projects in a way that saves dollars as well as capacity is very useful. I found that always very helpful. But more importantly, it's about creating that culture. Leadership has to convey the message that learning is important, not just for this job and this company today, but for your careers in the future.

The second thing that often comes in the way is, once you get people excited, everyone wants to work on the same hot technology. And if you truly want to look at a portfolio of emerging technologies, you want more people to be interested in different things. There are many ways to do that by making sure that the portfolio is very clear and you move people from one idea to the other, and not let everyone get focused on the same thing. And finally, very importantly, your business has to feel like it's their problem to solve. If that's not happening, you have to work backwards from where the business opportunities are, right?

[![Thumbnail 2830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2830.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2830)

So with that, hopefully we've given you a lot of ideas on how to do this. We have seen that it's possible in a ninety-day period to get this whole engine and this flywheel going.  In the first month or so, using now what's possible with Tech Recon, the agentic tool, you can actually set the foundation in place and have the conversation with your CEO and CFO that this is something as an organization you want to put more attention on. In the second month, you can start looking at some of these position papers, get people to own it, build more detail into it. And by month three, you should be going into your executive boardrooms and having a conversation about what are the technologies you want to keep an eye on and potentially do experiments with.

[![Thumbnail 2890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2890.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2890)

[![Thumbnail 2910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2910.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2910)

[![Thumbnail 2920](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2920.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2920)

[![Thumbnail 2930](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2930.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2930)

Now I make it sound very simple. This is not like a silver bullet, of course there are challenges.  But this is becoming so important for us in our roles as CIOs that this is not something that can be ignored. We have to figure out a way to manage our operational delivery responsibilities while also building the muscle to do technology reconnaissance. So hopefully with that, what you should be able to do is this future.  What's our position on the new AI regulations? Already briefed the board last week.  Position papers sent to your tablet. How will quantum computing affect our encryption? We've been monitoring quantum developments.  Recommend moving to quantum-resistant encryption in quarter two. Full analysis ready for review.

[![Thumbnail 2960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/2960.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=2960)

### Getting Started: Implementing Tech Recon and Conquering the Dual Mandate

All right, so with that, we want you to go out and conquer the dual mandate. You have a lot of resources here.  You will have access to actually the source code for Tech Recon is posted. You can access that. We'll be hanging around for a few minutes. We'll actually redo the demo as well since that didn't work earlier. But very importantly, this week is an amazing set of folks who are around here. There are people, everyone who's in this gathering, hopefully are folks who want to build this capability, so talk to each other, connect with them. Go to the expo floor. Go to the replay. There's a lot of new technology on demo. Get to see that and learn how that works.

So with that, one more point. When I asked you how many had an agentic something running, like two people raised their hands.

This is an opportunity to have an agentic system running that is very low risk and very low cost, but it gets you started. While she's setting up the demo, it's two minutes where you can actually see what you would get. It's worth it because people will get interested. As we started doing this, we realized it was not at all what we thought. In the beginning, you may think an agent is like an intern where you can just tell them to go do something. You can't do that with an agent, so that prompt that she showed is very, very telling. It came from a lot of iteration, so it gives you a chance to get hands on. So two minutes and you'll see this. Just do the demo again.

[![Thumbnail 3050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/3050.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=3050)

[![Thumbnail 3060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/3060.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=3060)

[![Thumbnail 3070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/3070.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=3070)

[![Thumbnail 3080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/3080.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=3080)

[![Thumbnail 3090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/3090.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=3090)

Yeah, sorry. So as soon as you run the blue crystal run button, the supervisor agent takes all the controls  of the entire workflow, and the planner agent begins the planning. The researcher, coder, and research report agent  are going to run. Actually, you can see the real-time logs as they're being streamed from each agent, which is specifically the planner agent  right now. Basically, the planner identifies what needs to be done. So once the planner finishes generating the task list,  the researcher, coder, and reporter agents begin executing their respective tasks and start filling in their information. 

[![Thumbnail 3100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/3100.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=3100)

[![Thumbnail 3110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/3110.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=3110)

When all the tasks are completed, you can see on the right-hand side the control plane, so all the agent statuses are  going to change to completed and the generated files become available. At the bottom right  corner, you can download all the files. After you create part one, you can proceed to part two. It's going to generate the specific technology position papers, which are going to be based on the part one assessment scores and the higher-ranked assessment scores.

If you go to the last slide, just so you can see where to download it, it's open source and it's a blueprint. It gives you a chance to have this dual mandate. It's hard, but once you've set it up, let's say you have a board meeting tomorrow and they ask you to come in and talk to the board of directors, which will happen with Agentic AI, you can just run this and have the latest report. It creates it for you. That's kind of the key. It saves you time.

[![Thumbnail 3170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/8e32d6ee1ffbaddd/3170.jpg)](https://www.youtube.com/watch?v=NwkH_F3yUiY&t=3170)

So thank you. We'll hang around here if anyone wants to actually implement this. Jiyun is here, and through that GitHub link, you can of course get access to the code and the instructions there. You can also reach out to us for any support or help needed to make it work. Thank you. 


----

; This article is entirely auto-generated using Amazon Bedrock.
