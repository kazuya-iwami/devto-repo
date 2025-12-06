---
title: 'AWS re:Invent 2025 - Building the Quantum Future on AWS (CMP357)'
published: true
description: 'In this video, Michael Brett and Anna Sarnek from AWS present their quantum computing initiatives. Michael introduces Amazon Braket, a five-year-old service offering on-demand access to eight quantum computers from five providers, including the recently launched Alpine Quantum Technologies device. He discusses AWS''s superconducting qubit approach at their Caltech facility, highlighting the Ocelot chip''s error correction breakthrough. Anna focuses on cybersecurity startups like QSecure and Potero building post-quantum cryptography solutions on AWS to address future quantum threats.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Building the Quantum Future on AWS (CMP357)**

> In this video, Michael Brett and Anna Sarnek from AWS present their quantum computing initiatives. Michael introduces Amazon Braket, a five-year-old service offering on-demand access to eight quantum computers from five providers, including the recently launched Alpine Quantum Technologies device. He discusses AWS's superconducting qubit approach at their Caltech facility, highlighting the Ocelot chip's error correction breakthrough. Anna focuses on cybersecurity startups like QSecure and Potero building post-quantum cryptography solutions on AWS to address future quantum threats.

{% youtube https://www.youtube.com/watch?v=uOxMX1ZDZJI %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/0.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=0)

### AWS's Quantum Computing Vision: From Amazon Braket to the Ocelot Chip

 Good morning everybody and welcome to this lightning talk on Building the Quantum Future on AWS. My name is Michael Brett. I lead the go-to-market strategy team for quantum computing here at AWS, part of our worldwide specialist group, and I focus on quantum computing every day. I'll be joined on stage a little later by my colleague Anna Sarnek. Anna leads our startups program around cybersecurity, and she'll be talking to you more about what some of the cyber startups are doing to increase quantum computing and post-quantum cryptography technologies as they build on top of our services.

We wanted to share with you today a quick introduction to what we're doing in quantum technologies at AWS and then give you a couple of concrete examples where startups are getting really engaged in this program. We're delighted to have so many people here. Apparently, this is the biggest session they've had at this Lightning Talk theater all week, so thank you very much for coming and being a part of this.

Quantum technologies is really exciting. This is an emerging technology with a lot of potential future benefits. It's still very early days, but what we're excited about are some of the applications it could unlock for our customers as they build. Quantum computers are really good at simulating quantum physics. You use a quantum computer to simulate quantum physics, and that leads to applications in physics and chemistry around drug discovery and materials design. Being able to simulate molecules coming together on a quantum computer is really exciting for our customers.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/70.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=70)

 Also in cryptography, once we have really large quantum computers, they could pose a potential threat to future encryption standards. We need to start thinking about how we adopt new standards that are safe against the future potential threat of a quantum computer. There are also a lot of other good applications, particularly around optimization and machine learning. Being able to use quantum computers to solve really big computationally intensive problems that we traditionally use in high performance computing and incorporating quantum computing along the way is very promising.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/150.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=150)

That's the future potential space, and this is what customers are excited about and what we want to help you build on AWS. Let me share a little bit about what we're doing specifically today and what we're investing in.  We work in four different areas of quantum technologies at AWS. First, we have a service called Amazon Braket. Braket is just like any other service on AWS. Think EC2 for classical compute and Braket for quantum compute. It's a generally available service that we launched five years ago at re:Invent. We've had quantum computers in the cloud for over five years now.

We also have a team called the Advanced Solutions Lab. This is a professional services team at AWS who specialize in quantum algorithms. This is the team you could engage with to do joint R&D together and investigate a particular problem or opportunity you're looking at with quantum computing. The third area is the AWS Center for Quantum Computing. This is where we build our own hardware. AWS is investing in building its own quantum computers as part of our overall lineup of compute that we build ourselves, including Graviton, Inferentia, and Trainium, with quantum computers coming along as well.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/240.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=240)

Finally, we have a basket of activities around post-quantum cryptography and quantum networking. Anna will talk a lot more about how startups are getting engaged with that over time. AWS is investing in security and cryptography so we can help you protect your assets against a future potential threat from a quantum computer.  Here's a quick look at the Center for Quantum Computing. This is our facility where we build quantum computers. It's on campus at Caltech in Pasadena, California. This is an AWS building where everyone who works there is an AWS employee. We share some faculty and resources with the university as part of our effort to build quantum computing hardware.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/280.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=280)

The particular approach we've taken is around superconducting qubits. There are many ways to build a quantum computer, roughly five or six different architectural approaches. The one we've selected is based on superconducting qubits, which is a reasonably common approach at this point, with many other companies looking at this technology.  The particular approach we've taken is to build error correction technologies directly into the chip itself. You can see on the slide here that the actual quantum chip is about the size of your thumbnail. It's a piece of silicon with some exotic materials on top, and we lay down a quantum computing circuit rather than a classical compute circuit. We're starting to embed the error correction technologies directly onto the chip itself. We had a really great announcement earlier this year around a chip that we call Ocelot.

[![Thumbnail 310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/310.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=310)

 The Ocelot chip was a demonstration chip to show some of those error correction technologies in action, and we got some extremely promising results from that. We were essentially able to reduce the resources required to do error correction on board the chip itself by several orders of magnitude. You can look up the AWS Ocelot quantum computing chip to read more about what we do.

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/340.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=340)

### Amazon Braket in Action: Accessing Quantum Computers as Cloud Services

So what can you do today? Well, you can access quantum computers through a service called Amazon Braket.  Braket is just like any other service on AWS. It is there to get access to pay-as-you-go, no upfront commitment, on-demand quantum compute services. The idea is to deliver a consistent experience for you, consistent in the sense that it looks and feels and operates just like AWS, but it is consistent across many different types of quantum computers.

Today we offer a range of different quantum computers and different modalities, and we provide a consistent experience across all of those different technologies. We have a single API, a single software development kit, and a whole bunch of different ways that you can access those. Today we have third-party quantum computers, and AWS-built quantum computers will be added to that lineup over time as well. We are not making any announcements at this re:Invent about when those computers will be available, but in good time we will make those available through Braket.

The customer workloads that we see today on Braket are mostly in research. Most of my customers are researchers at universities and national labs, and they are doing science on these computers. They are accessing the quantum computers to do physics experiments in the quantum computers themselves and do computer science experiments to design new algorithms. They are also looking at applications and trying to benchmark against classical approaches. We connect the available quantum computers to these customer workloads and provide a consistent experience on Braket, so you can access that today if you need.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/440.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=440)

So how do we build it up? At the infrastructure layer of Braket,  we have a range of quantum computers, QPUs, and we also have a range of simulators. The simulators run on classical compute on AWS and allow you to test and debug at relatively low cost before you deploy to the quantum computing hardware itself. Every quantum computing algorithm is a hybrid algorithm, which means we need both quantum and classical working together. We provision a bunch of CPUs and GPUs to run alongside the quantum computers as well and help you orchestrate that overall workflow.

[![Thumbnail 470](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/470.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=470)

[![Thumbnail 500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/500.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=500)

Sitting on top of that infrastructure layer,  we have all of our capabilities around software. We have our AWS standard API and our own software development kit. We have a whole bunch of notebooks in Jupyter Labs so that you can start to explore and try things out. We also connect into a range of third-party software libraries so you can connect into programming languages like OpenQASM or Qiskit, if you have worked with the Qiskit quantum computing software libraries, and Cuda-Q from NVIDIA.  This allows you to bring any software development environment that you like in quantum computing onto Braket as well.

Finally, up in the top right there, we have a whole bunch of companies that we work with that build on top of Braket and provide value-added services on top of that. These are AWS partners that have built software that helps these quantum computers run better. I saw one of our friends in the crowd today from Q-CTRL. Q-CTRL builds error correction software for quantum computers, and that helps our computers run faster, get more accurate results, and get better performance. We are really excited to build with some of these startups that are looking at extending what we make available through the infrastructure layer, so we really appreciate them.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/560.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=560)

So how should you think about quantum computing in the cloud?  Well, it is just another computer, right? It is just another instance type that you can work with. Already today we have a range of accelerated compute that you can work with: CPUs and GPUs and other special purpose accelerators like Inferentia and Trainium. Quantum computing is just one more instance type across that portfolio. When you think about where quantum computing fits into your overall strategy, think about it in that context of high performance computing, having a portfolio approach, and allocating the right algorithms and the right workloads to the right processors to do that work. Quantum computing is not going to replace what you already do; it is more going to augment and be able to provide an accelerator to some of the workloads that you are most interested in.

[![Thumbnail 610](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/610.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=610)

So where are we today?  Quantum computing today is more about investigating quantum computing itself. The number one application we have for quantum computers today is to study quantum computers. We've essentially built a tool and we're studying the tool. But in the future, we're going to start using that hammer to look at some of the applications I mentioned on the very first slide around chemistry, machine learning, and optimization.

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/640.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=640)

We're at the point where we've gone from characterizing hardware and understanding it  to now starting to research the quantum algorithms that get better and better and allow us to start to take on some of the potentially useful customer applications. We're right at this cusp. The next set of use cases are going to be in science, looking at how we use quantum computers to accelerate scientific discovery itself. Particularly in areas like high energy physics or chemistry simulation, these are value-added workloads that national labs, the Department of Energy, and NASA are going to be using quantum computers to do large-scale work with, and then eventually getting up to solving industry-relevant problems.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/690.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=690)

Our big enterprise customers will start to use that work. What we have available today on Braket is eight different computers from five different providers.  These are all different quantum computers from different brands of quantum compute. We have two different kinds of ion trap-based quantum computers. On Braket, we just launched two weeks ago a new computer from a company called Alpine Quantum Technologies, or AQT. They're a European-based quantum computing provider, and this just went on Braket two weeks ago as our latest device available there.

[![Thumbnail 730](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/730.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=730)

Then we have two superconducting-based computers, one from Rigetti and one from IQM, and finally a neutral atom-based quantum computer which does some really interesting work around analog programming.  What we have for you is a wide selection of different quantum computing technologies. You can access all of these on demand, explore them, and see why they're different. You can start to get a sense for the differences between them and the potential future opportunities there. These are all available on Braket today, and you can access them on demand, start to program them, and get a sense of why this is different and why it's such a different technology.

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/760.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=760)

Before I hand over to Anna, just a couple of last points about the overall  pathway we're on right now to do research and development together to get ready for quantum computing. We're right at the cusp of being able to do some of these large-scale scientific workloads. What we do together with customers is discover use cases and consult with you about the potential future applications of quantum computing. Then we research and design algorithms and do benchmarking work so you can understand how this is going to impact your business.

Then we've got to push the boundaries, make these computers go faster and faster, make the algorithms more efficient, integrate them with our classical high-performance compute, and lay that groundwork together so you've got the IP, the supply chain, and the workforce in place that can start to adopt this technology over time. I'm going to hand over to Anna now, who's going to share a little bit more about how startups are getting engaged with AWS generally and then share a couple of stories specifically about how quantum security companies are starting to make use of this technology. Over to you, Anna.

### Cybersecurity Startups Building Post-Quantum Cryptography Solutions on AWS

Thank you, Michael. So just to give you a little bit of an overview for our startup program, we have several thousand cybersecurity startups globally building on AWS today. The way my team engages is we're responsible for holding relationships with the top investors investing in cybersecurity innovation. We're collecting feedback from security practitioners where post-quantum crypto is one of the top five topics that has been coming up this year. We're also responsible for identifying the upcoming top founders that are going to be building on AWS and supporting them in the right way of resourcing, whether it's connecting to resources like Braket or engaging with our product leaders and leadership to help ideate on what direction AWS should be innovating down the line in the future.

[![Thumbnail 880](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/880.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=880)

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/890.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=890)

I just want to build on that story and share a couple of slides  about the companies that are earlier on in their journey with Amazon as a partner, as startups that are building in the post-quantum crypto  space. The first one is QSecure. This is one of the companies that is providing end-to-end protection for post-quantum cryptography. The great thing about these technologies that are popping up today is that they can be deployed in any hybrid environment.

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/940.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=940)

They're available on-premises and available on cloud, and enterprises are able to start adopting these technologies in their existing infrastructure to be ready for when the post-quantum cryptography problem becomes more prominent today. So what we're doing here is bringing in startups and enabling our enterprise customers to access them and engage with them so that they're able to be more forward-looking and proactive in their security problems. 

The second example here is Potero. This is another company that is helping to develop post-quantum cryptography technology that is going to help address this "decrypt later" risk. It's going to help develop that zero trust architecture for endpoints as we're thinking about how to embed the future security technology. As quantum computers become more and more prominent in the technology landscape, new cybersecurity threats are emerging, and the best part is that it can be deployed across existing environments today. So these startups are thinking about how this technology can be ready for the existing infrastructure as we also continue to think about the problems that are developing down the line.

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/1030.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=1030)

These are just two early examples, but this is really folding into, as Michael said, a lot of the third parties that we work with. We work a lot with, for example, NVIDIA as well. I run a cybersecurity accelerator for startups where we think about how to develop more useful and prominent required technology for enterprise customers and partner with innovators and startups like many of you in the room likely are today. These are the type of companies that we're helping to support and helping to surface within the enterprise so that we can continue to solve the problems of the future. 

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/0bc0fca94b6cbcd9/1060.jpg)](https://www.youtube.com/watch?v=uOxMX1ZDZJI&t=1060)

I'm going to wrap it up with some resources that you can access around the topics that Michael discussed for Braket, and we're happy to answer any questions that you have around the startup program or any of the quantum computing services that we offer today and down the line. Thank you for attending. There are also additional quantum computing sessions that are available for you at re:Invent. If you scan the QR codes, there will be additional technical sessions that Michael and the team that he works with are working on. Thank you. 


----

; This article is entirely auto-generated using Amazon Bedrock.
