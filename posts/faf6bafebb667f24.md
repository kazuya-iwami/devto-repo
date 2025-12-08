---
title: 'AWS re:Invent 2025 - Another Axiom: Migrating the backend for hit game Gorilla Tag to AWS (IND396)'
published: true
description: 'In this video, Morgan, a Services Engineer at Another Axiom, shares the journey of migrating their VR game backends (Gorilla Tag and Orion Drift) to AWS. He discusses building Mothership, their backend-as-a-service platform, using ECS and Aurora with a monolithic architecture deployed like microservices. The team achieved remarkable results by migrating to Graviton ARM processors, reducing compute costs from $1,000 to $250 per day—a 60% savings and 30% reduction in total cloud spend. The migration was surprisingly simple, completed in under a day with zero downtime. Key lessons include revisiting assumptions after infrastructure changes, leveraging RDS Proxy to eliminate database connection bottlenecks, working with AWS account teams for expert guidance, and investing heavily in CI/CD pipelines with integration testing using Locust and their game SDK for deployment confidence.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/0.jpg'
series: ''
canonical_url: null
id: 3087772
date: '2025-12-05T22:43:41Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Another Axiom: Migrating the backend for hit game Gorilla Tag to AWS (IND396)**

> In this video, Morgan, a Services Engineer at Another Axiom, shares the journey of migrating their VR game backends (Gorilla Tag and Orion Drift) to AWS. He discusses building Mothership, their backend-as-a-service platform, using ECS and Aurora with a monolithic architecture deployed like microservices. The team achieved remarkable results by migrating to Graviton ARM processors, reducing compute costs from $1,000 to $250 per day—a 60% savings and 30% reduction in total cloud spend. The migration was surprisingly simple, completed in under a day with zero downtime. Key lessons include revisiting assumptions after infrastructure changes, leveraging RDS Proxy to eliminate database connection bottlenecks, working with AWS account teams for expert guidance, and investing heavily in CI/CD pipelines with integration testing using Locust and their game SDK for deployment confidence.

{% youtube https://www.youtube.com/watch?v=km6LSILANI8 %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/0.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=0)

### Another Axiom's VR Gaming Success and the Breaking Point with Third-Party Platforms

 Hi, my name is Morgan. I'm a Services Engineer at Another Axiom where I'm the tech lead for Mothership, which is Another Axiom's backend as a service platform. Today we're going to be talking about the journey that Another Axiom has taken migrating the backends for our popular VR games to AWS.

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/20.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=20)

 First, we're going to start off by talking about who we are as Another Axiom. Then we're going to talk about the state of the world before we started this project. We're going to briefly cover things that we built on AWS from an architecture and implementation standpoint, and then we're going to cover load testing, cost optimization, and our eventual migration to Graviton. Finally, we're going to cover where we are today, what we've learned, and where we're going soon.

[![Thumbnail 40](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/40.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=40)

 First off, if you're not familiar with Another Axiom, we're a leading VR game studio with a focus on social, plausible, diegetic worlds and innovative interaction and locomotion systems. We believe in the power of VR to bring people together and do everything we can to create plausible alternative spaces for you to inhabit with your friends. Our flagship titles are Gorilla Tag, which is a social sandbox game created around playing playground games like tag as gorillas, and Orion Drift, which is a massively multiplayer sci-fi social sports game.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/80.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=80)

 To give you an idea of the scale of the games we're talking about here, Gorilla Tag is the most popular game on the Meta Quest store. At the last time we measured, we had about 17 million lifetime unique players. At the time of writing this presentation, our all-time peak concurrent was about 100,000 CCU. Orion Drift is also very successful with peaks of tens of thousands of concurrent users. The VR market has some challenges that sound familiar if you're familiar with the general gaming landscape and some that are a little bit unique.

The two big features to think about when capacity planning for VR are seasonality and virality. VR is extremely seasonal. At peak times during the winter holidays, it's possible to have an order of magnitude more peak concurrent players than at the lowest point of summer. That kind of seasonality is pretty common across the entire video game segment, but we find that in VR that difference is magnified. VR is also extremely social and viral.

If the broader VR creator community decides to run a big game meetup, you can end up with these massive semi-unpredictable load spikes. It's a great problem to have, but your infrastructure has to be able to scale to take these sudden hockey stick moments. This leads to the whole reason we're talking today. When I started at Another Axiom, everything had been built on a popular third party platform as a service product.

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/150.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=150)

 Gorilla Tag was bootstrapped from a single developer, so it didn't make sense for that person to go and build their own backend. As Gorilla Tag took off, we quickly outgrew the off-the-shelf solution. We're big for the VR market, but some of these off-the-shelf backend as a service platforms are dealing with absolutely massive gains, particularly on mobile. Our needs don't always line up with what their average game customer needs either, so we spend a lot of time extending that platform and bending it to our needs.

Plus, as a smaller fish in a big pond, we had a very limited ability to get support and even less ability to request features or give feedback. As an example, Another Axiom's games primarily target the Meta Quest ecosystem. For a large customer, our platform vendor might have been able to get Meta integration onto their roadmap. Instead, they gave us some guidance on how to build our own pieces and helped watch us as we bolted them on. They left us with a bunch of dev work to do, new services to support, and a new security service area that we had to deal with all on our own.

Once you start building a couple pieces of that size and scale, you start to wonder how much it makes sense to keep a dependency on the rest of that platform. The final straw came during one of the hockey stick viral events I alluded to earlier. We had advance warning of a large creator-driven community event, and we'd warned our partners well in advance, but we still managed to fall over. At that point, we knew we needed to move to an in-house solution so we could take support into our own hands.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/240.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=240)

### Building Mothership: A Custom Backend-as-a-Service Platform

 For those who may not be familiar with what a game backend platform provides, here's a short list of some of the most important features Gorilla Tag needed in order to run as a live service game. Identity and authentication: we need to know who you are and that you're actually allowed to play. Do you own the game? Have you been banned, and so on. Moderation goes hand in hand with identity. Players can submit reports of other players behaving badly, and our support team can sanction bad actors. We also do real-time voice moderation with an AI product.

Analytics: we need to measure what players are doing in-game and use that data to make the best experiences we can for our users. Per user storage: players need to save things like what they're wearing or what their score is or other data, and that data needs to follow them across their devices. Per title configuration: Gorilla Tag and Orion Drift need to be able to remotely host configuration so that they can tweak things on the backend without releasing client patches. And then commerce and IAP: we need to sell things. We need to pay for development. We need to make money.

Most games need these as sort of table stakes, and while we could have just built them piecemeal as Orion Drift or Gorilla Tag needed them, we decided that we would much rather build a central generic platform that we offered to the game teams as a fully managed service.

[![Thumbnail 310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/310.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=310)

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/320.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=320)

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/330.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=330)

So with that in mind, we started working on what I call Mothership.  Mothership has three pillars: a back-end service app, a developer dashboard, and a game SDK.  Today we're primarily going to focus on the Mothership app. In Mothership, we have a few models for hosting game tenants, but they all revolve around the concept  of a vertical. Another word you might use would be shard. Multiple smaller game tenants might share a vertical, but we can also spin out larger customers to their own dedicated verticals. This gives smaller games and incubation titles great cost savings, but allows us to scale our largest titles as far as they want to go.

Each vertical is a network isolated clone of the Mothership stack, which itself is an OJS app hosted in ECS along with some storage. The technology we chose here was primarily driven by the constraints of our dev team size. For the majority of Mothership's lifecycle, we've been a dev team of one to three, and so we needed to rely on as much managed product as we could in order to keep our development velocity going. One interesting thing about the Mothership app itself is that it's a monolith, but it's deployed like a microservice. We shard compute and database by feature area. So for example, the analytics compute and database scales independently of the user storage compute and database.

However, rather than relying on intra-service communication via an API like you would with a traditional microservices deployment, each instance is running the same code. That means that if the moderation database needs some data from the analytics database, that can all happen within a single process and every node has access to all of the sharded data at rest in all the various databases. We believe that keeping the contracts in code makes them easier to reason about and having a single version of the app deployed at all times simplifies operations and on-call for our small team.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/420.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=420)

### CI/CD Pipeline Architecture and Load Testing at 100,000 Concurrent Users

The most important piece of the Mothership infrastructure outside of the app itself is our CI/CD pipeline.  This is not meant to be readable. This is just for scale about what we're doing here. Mothership is a fully continuous integration, fully continuous deployment product. We utilize trunk-based GitOps, meaning that when a pull request merges to main, we have a GitHub action build a new Docker image and push it to ECR, which starts our pipeline. This keeps changes small, isolated, and easy to roll back.

Of course, we're not just merging to main and hoping for the best. We use CodePipeline to serialize our changes to one vertical at a time. CodePipeline also gives us robust control over the CI process. For example, if we have a risky change or a change that can't be fully validated without working with other services in the cloud, we can pause the pipeline at a given point in time and disable transitions out of our pre-prod stages. CodePipeline also makes disaster recovery with tasks like rollback very simple.

To manage actually deploying new versions of the Mothership app, we use CodeDeploy blue-green deployments in a canary strategy. When a new version of the app makes its way to a specific ECS cluster from our CodePipeline, CodeDeploy spins up a new set of tasks with a new version of the code, runs an integration test suite which we run through Step Functions and if that passes, routes a fraction of traffic to the new app. We've set up some key reliability metrics with CloudWatch alarms and if any of those trip at any point during the canary process, the deployment is stopped, rolled back, and the pipeline stops and alerts us so we can address the issues.

I want to talk a little bit in depth about our integration test suite because I think it's a really critical piece of reliability engineering that often gets overlooked. Our integration test suite is a Python Locust test project. Locust is traditionally a load testing framework, but we like how easy it is to model realistic user workflows in its test runs. The other thing that's pretty unique about this integration suite is that it's actually driving the same SDK that our games use. Our game SDK is a C++ SDK, but we have projections up into Python, C, and all sorts of languages, so that higher level languages can take advantage of the same tools that the games are using.

This is pretty unique and it allows us to do some really interesting things. For example, we can run the test suite against every known version of the SDK that a game in the wild is using, so we can be confident before a change rolls out to a vertical that a customer is on that change has been validated against the same SDK that that game is on. This piece took some real work to build, and it takes some real care and feeding to keep current, but it's paid for itself many times over with the peace of mind that it gives us.

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/560.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=560)

 About a year into development, we worked with a load testing partner to fully load test Mothership. We targeted 100,000 concurrent users per vertical because that's what we grew and scaled Mothership to hit that goal. On a previous slide, we showed RDS Proxy as part of the architecture diagram, but I want to note that at the time we ran these load tests, that was not yet a product we were consuming due to an incompatibility with another tool in the services. Our main bottleneck through all of that load testing process was the cost of opening new database connections. That's really IO intensive on the app side and pretty CPU intensive on the database side.

If we were scaling like a hockey stick, we'd open a ton of database connections in fast succession, and then that would start to dominate the database CPU which would then start to slow down IO in the app,

causing the event queue in the app to back up, creating this kind of cycle of death. As we were tuning and tweaking this, we discovered that the best tuning for the client-side connection pooling we were doing was to use large instances with many cores that we shard across those cores. This gave us two big wins. The first was that the CPUs were more powerful and could handle more traffic before they started to fail. Because we were provisioning larger instances more slowly, the database could keep up better with those incoming connections.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/670.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=670)

### Cost Optimization Journey: From $1,000 to $250 Daily Through Graviton Migration

The other thing we did was provision larger database instances to keep them pre-warmed to avoid these potential spikes. Configured like that, this was our ECS run rate from mid-summer, which is the lowest point of the year for VR gaming in general. We were looking at about $1,000 a day of purely compute spend, and that seemed pretty expensive. 

[![Thumbnail 680](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/680.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=680)

In June, we started a broad effort to understand and reduce our cloud spending. In parallel to that, we happened to get rid of the piece that was keeping us from using RDS Proxy.  With that back into the infrastructure, we realized we could scale down all of our pre-warmed large provisioned instances and move almost exclusively to serverless Aurora compute. In places where we were already on Aurora Serverless, we were able to reduce our minimums across the board, although specific numbers vary by feature.

This didn't address our steady-state compute costs though, because we never went back and examined those provisions. Cost Explorer showed that we'd see a small cost improvement by switching to Graviton. At first it seemed a little inconsequential. Depending on the view, we were showing between 4 and 6% savings. That seemed low because we'd heard stories of teams realizing much larger savings. We reached out to our account team who put us in touch with some experts, and the experts projected a much larger savings of about 20 to 30%.

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/740.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=740)

We decided that was worth exploring, and so we did. Like many teams, we had a vague idea that running on ARM was probably cheaper or faster, but it wasn't a priority for us. Maybe it would be difficult. After all, sometimes porting games between different consoles  running the same architecture is hard. Porting between different architectures may be similar. Now that we were serious about cutting costs, we needed to take a closer look.

At the time, nothing in Another Axiom's backend ran on ARM. We were worried that there might be native Node packages, potentially even transitive includes that we didn't plan for that might not have good ARM support. We didn't know if there'd be impacts to CI tooling or monitoring, particularly the image build. We were looking at other broader cost-cutting measures and wondered if we were doing too much too fast. Plus, we had a bunch of different estimates coming in, and we didn't know if the effort was going to be worth it in the end.

[![Thumbnail 790](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/790.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=790)

With those concerns in mind, we scheduled a few meetings to start to de-risk a potential Graviton migration just to see what it would get us. It turned out that migrating to Graviton was a lot simpler than we anticipated. In the time it took us to finish scheduling meetings, I had a working proof of concept done on my PC.  By the end of that day, we were essentially ready to go to production.

It turns out Node apps are fairly simple to migrate to ARM. ARM is popular enough that the majority of common NPM packages just work out of the box. We didn't run into any cases where we had some native dependency that just wouldn't work. Docker Buildx makes cross-architecture images basically trivial. In about an hour, most of which I spent doing research, I had local cross-architecture builds, and with a little bit more work, I had a local Docker Compose stack working cross-architecture using QEMU.

I was able to validate startup, run some tests locally, and it all just worked. Finally, Mothership has extensive CI tooling and test coverage. So I knew when it was time to go live, we'd have our bases covered. With some local proof of concept and a bit of tailwind, I started the process of getting us officially migrated to ARM. Because we have a fairly robust CI/CD pipeline, we knew we'd be able to build a ton of confidence in the migration before it impacted players.

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/850.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=850)

Here's how we took this change to production. First, I went into Code Pipeline and I disabled two transitions: the transition from our staging  pre-production stage to our first production vertical, which we used for testing and incubation. Then I disabled the transition from that vertical to the first vertical running an actual game, in this case, Gorilla Tag. That ensured I had a safe setup where we could experiment in staging all we wanted and never impact a paying customer.

Then I built a multi-architecture image of the Mothership app on my personal desktop and pushed it directly to ECR with a custom tag for testing. I manually created a task definition that targeted the ARM processor family and deployed that to a staging ECS cluster. Essentially, I was just looking for app startup and health checks to work. I merged the changes to GitHub that updated our builder action to produce multi-architecture builds, as well as the necessary task definition changes to use smaller ARM instances, and we watched it roll out through our normal CI process.

When it got to staging, the new ARM instances started up, ran the integration tests, and performed canary traffic routing like normal. The next day, once I was satisfied that our metrics looked good in staging, I enabled the transition to our first production vertical.

Again, this vertical is mainly used for testing and incubation purposes, so it was pretty safe to do. The task started successfully, the integration tests passed, and the traffic routing looked fine.

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/940.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=940)

At that point, we had a high degree of confidence in the process, so I enabled the final transition and we began rolling changes out to the game team verticals. There was zero downtime and zero noticed impact on players. Here's our run rate when we made the switch. This pricing trend has held to date. We went from about a thousand dollars a day spend to about two hundred fifty dollars a day, which is massive. 

To use some round numbers, that's roughly a sixty percent compute cost savings, and we saw around a thirty percent reduction in our total cloud spend with just this change. I want to note that this is not opting into any compute savings plans or any other pricing agreements. This is not using spot instances. This is pure on-demand usage.

We've seen other side benefits as well. One confounding change we made was that we went from large multi-core instances down to two vCPU instances at the same time because we no longer had that database proxy dependency. The instances themselves are cheaper, and because we can scale much more granularly with these smaller instance sizes, we're allocating about twenty-five percent fewer vCPUs. We believe that's due to both being able to allocate much more granularly and also because there seems to be some performance benefit just from moving to ARM.

The other side benefit we got is that on the large instances, if we were hovering right around an auto scale boundary, we'd get this fluttering effect where a large instance would come on and off repeatedly as we were right around a scale boundary. That could lead to some instability in our system. Because we can now scale so much more granularly, we're much more stable across all traffic levels.

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/1020.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=1020)

### Key Lessons Learned and Future Directions for Mothership

So what did we learn? First, it's important to update your assumptions as things change. We ran in a configuration with a ton of unnecessarily expensive compute  for much longer than we needed because we never revisited our priors about how the system worked and how the database connections worked. It's important that as you make any change to a larger system, you're thinking about the rest of the system and what the knock-on impacts could be.

Secondly, some changes that sound big are actually pretty small. It sounds like a huge shift to change processor architectures, but in our case it was trivial. It's worth going beyond blog posts, marketing, and gut level estimation and getting your hands dirty, even a little, just to understand the work involved in any change.

The next two points are linked. Cost Explorer tends to way underestimate your savings, and you should work with your account team to find experts. When we started trying to cut costs, the most valuable step we took was bringing in our account team and sharing our plans and concerns with them. They were able to get us in touch with experts from Graviton, RDS, ElastiCache, and a few other services to find places where we were being inefficient. They were able to walk us through potential savings and help us wrap our heads around the effort required. They saved tons of research and it helped us focus on the tweaks that would really move the needle.

The last and most important thing, I think, is that any investment in your CI/CD and test infrastructure pays huge dividends. I've been very fortunate at Another Axiom in having complete support from our engineering leadership to make large investments in our tooling here. Now we have incredible confidence that when we deploy code, it'll land in a working state, that it won't break any live game clients, and that we always know what is where and how it got there. It's Mothership's secret weapon for stability.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/1120.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=1120)

So where does that take us going forward? Orion Drift and all future games are one hundred percent on Mothership. Gorilla Tag is using Mothership for tons of critical workloads, including authentication and analytics, but there are still a few holdout pieces we're working on migrating. We're exploring different hosting models, including  a larger emphasis on multi-tenant verticals to enable low-cost rapid prototyping. We're exploring new platforms, new opportunities, and new services we can offer, and we're working with our game teams to really evolve the platform.

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/faf6bafebb667f24/1150.jpg)](https://www.youtube.com/watch?v=km6LSILANI8&t=1150)

The last thing, as a call to action, is to come try our games. If you have a headset somewhere, these links will take you to the Meta product details pages for our games. I highly encourage you to check them out. I think they're a lot of fun and I hope you will too. 

That's all I have for you today. Thank you so much. If anybody has any questions or wants to go deeper on anything, I'll be available over there. Otherwise, thank you so much.


----

; This article is entirely auto-generated using Amazon Bedrock.
