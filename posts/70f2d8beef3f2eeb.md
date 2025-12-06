---
title: 'AWS re:Invent 2025 - Scaling Pinterest: Iceberg solutions for petabyte-scale challenges (STG211)'
published: true
description: 'In this video, Ashi Singh, Principal Engineer at Pinterest, explains how Pinterest scaled to serve 600 million users with a 500-petabyte data lake using Apache Iceberg. She details their migration from Hive to Iceberg, covering three key use cases: user data deletion (achieving 10x scale improvement and 30% cost reduction through sorted deletion keys), bucket-based table sampling (enabling reproducible ML workflows with 90% speedup), and feature backfills (achieving 65% cost savings and 90x faster development using bucket joins). She shares critical learnings on operating Iceberg at scale on Amazon S3, including user agent-based access control for in-place migrations, leveraging S3 inventory reports to identify orphaned files, and resolving throttling issues by implementing hash-based object paths with early entropy introduction.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Scaling Pinterest: Iceberg solutions for petabyte-scale challenges (STG211)**

> In this video, Ashi Singh, Principal Engineer at Pinterest, explains how Pinterest scaled to serve 600 million users with a 500-petabyte data lake using Apache Iceberg. She details their migration from Hive to Iceberg, covering three key use cases: user data deletion (achieving 10x scale improvement and 30% cost reduction through sorted deletion keys), bucket-based table sampling (enabling reproducible ML workflows with 90% speedup), and feature backfills (achieving 65% cost savings and 90x faster development using bucket joins). She shares critical learnings on operating Iceberg at scale on Amazon S3, including user agent-based access control for in-place migrations, leveraging S3 inventory reports to identify orphaned files, and resolving throttling issues by implementing hash-based object paths with early entropy introduction.

{% youtube https://www.youtube.com/watch?v=L2v8WdBGjvg %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/0.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=0)

### Pinterest's Data Scale and the Evolution from Hive to Apache Iceberg

 Good morning, everyone. Thanks for coming. My name is Sachin Hola. I'm a Principal Solutions Architect here at AWS. During my time here, I help customers achieve massive scale with AWS services, and this session is one I'm particularly excited about. We have here Ashi Singh, a Principal Engineer at Pinterest, who's going to talk us through some of the challenges that Pinterest faced with their data challenges, and part of it is how they serve 600 million users with their petabyte-scale data lakes. So I think you'll find this session exceptionally interesting. Thank you.

Yeah, thank you, Sachin. All right, so thank you all for showing up here. Today we'll be talking about how we scale Pinterest with the help of Iceberg. We'll talk about some of the use cases that are powered by Iceberg, the adoption journey, and we'll share some of the key learnings that we had while operating this on Amazon S3.

[![Thumbnail 80](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/80.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=80)

All right, with that, let me talk about the agenda.  We'll start with discussing the scale at Pinterest, a little bit of that you already know about. We'll look at how table format evolved at Pinterest and in the industry. We'll talk about the Iceberg adoption at Pinterest, and then we'll dive into some of the key use cases that are powered by Iceberg. The specific learnings is where we'll end the session with.

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/110.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=110)

So scale at Pinterest from a business perspective, we have over 600 million  monthly active users. Every week we save around 1.5 billion pins. We have over 10 billion boards. But more relevant to this audience, I would think, is going to be the data scale. So our data lake is about 500 petabytes on Amazon S3, 100,000 tables in Hive metastore. But when I say Hive metastore, these are actually Hive and Iceberg tables. We have over 20,000 nodes that are running Spark and over 1,000 nodes that are running Trino. In total, we run around 400,000 compute jobs in a day.

[![Thumbnail 160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/160.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=160)

The table format evolution at Pinterest  aligns very well with how the evolution happened in the industry. Until 2019, Hive table, like everywhere else, was the de facto data lake table format. Around 2020, the industry and within Pinterest, we started realizing that we have a lot of new use cases and Hive is just not cutting it. So in the industry there were alternatives like Hoodie, Delta Lake, Apache Iceberg that were forming. We were involved in those discussions early on, but it took us two years to get the approval, make sure the whole leadership team understands the challenges of Hive, and we started the buildout in 2022.

[![Thumbnail 220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/220.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=220)

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/230.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=230)

2023 is when we actually went to production.  The first use case and the key use case that we went to production with was user data deletion. Since then we have been ramping up.  That takes me to the next slide where we'll look at the current numbers we are operating at for Iceberg at Pinterest. Around 15,000 tables are on Iceberg. We have around 200 petabytes of data on Iceberg.

[![Thumbnail 240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/240.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=240)

And if you look at the growth numbers,  the table count on Iceberg has been growing over 300%. By data volume it's much, much less, which we want. We want the data to be not growing as quickly because it's expensive. One of the reasons why we are able to do that, that even though our table counts are growing, our data count is not growing as significantly, is because we are able to utilize compression much better with Iceberg.

[![Thumbnail 270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/270.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=270)

[![Thumbnail 300](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/300.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=300)

### User Data Deletion: Achieving 10x Scale Through Optimized File Rewrites

All right, so we have Apache  Iceberg at Pinterest. How do users use it? We enable users to use Iceberg from engines like Trino, Spark, Flink. From Python, we allow users to do metadata reads. We have some MapReduce-based frameworks that write to Iceberg tables, and then we also support Trino. So that was pretty much about how we adopted Iceberg at Pinterest, but now let's talk  about some of the key use cases that are driven by Iceberg. The use case that I just talked about, that was the first use case that we went after, is user data deletion.

[![Thumbnail 310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/310.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=310)

User deletion is not a new thing.  I'm sure you all have heard of it and maybe you do that at your companies too, but essentially in this example, if there are three users, they come in and say delete me, our deletion service used to look at all the tables that had user data and then perform deletion on them. In the Hive world, what that means is we are rewriting entire tables. That's super expensive, takes a lot of time, and because it takes a lot of time, it was not as reliable. So we thought that we would solve this problem.

[![Thumbnail 350](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/350.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=350)

[![Thumbnail 360](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/360.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=360)

[![Thumbnail 380](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/380.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=380)

By the way, the cost of the rewriting was one thing,  but then the fact that if there are downstream jobs and users reading those datasets at that time and we change the files on the fly, those jobs will likely fail  because at the query planning time or the job planning time they saw there is a file, but when they were actually going to read it, it didn't exist, so they would fail. So we tried to solve this problem with Iceberg. With Iceberg, you don't have to do the whole table rewrite. You can just focus on the files  that have the content or that have the data for these users and just rewrite those files.

[![Thumbnail 410](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/410.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=410)

The good thing with Iceberg is that you get snapshot isolation. So while you are rewriting the files, the users that are reading from the previous snapshot remain unaffected. So great, right? Maybe. The problem that we ran into is that our cost of deletion was still the same. And that was because  the cost of deletion is proportional to the number of files rewritten. If you are rewriting all the files, it's as bad as doing it on Hive. If you're rewriting just a few files, it's going to be super fast.

[![Thumbnail 450](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/450.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=450)

Now the problem with the way we were doing the user data deletion is that we would batch the deletion request and then perform the deletions, but the users that were asking for data to be deleted, their data could be spread across all the files, so that would mean we are rewriting the entire table, even with Iceberg. To solve that problem, an easy solution is we just sort it by the deletion key so that we make sure that only a few files have the data for a particular user who is asking to be deleted.  That sounds good. It does help.

So in this example we were able to reduce the number of files that get deleted. For example, in table one, it's just one file that is getting deleted now. Table two, just one file, much better than the previous scenario. But this requires sorting. Our tables are on the scale of 40 to 50 petabytes per table, and doing sorting, that's going to be super expensive. So how do we handle that? We added optimization where we only sort the files that are not sorted, and this required us to make some changes to Iceberg.

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/510.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=510)

Some of those changes we contributed upstream. It required us to make changes to Spark. Some of that is in-house. But with that we were finally able to meet our requirements where we were able to scale the deletion capability at Pinterest by 10 times, reduce the data storage cost by 30%,  and also reduce the data compute cost of those deletion jobs by 30% while increasing the reliability by 90%. So that was the first use case.

[![Thumbnail 530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/530.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=530)

### Bucket-Based Sampling and Feature Backfills: Accelerating ML Development by 90x

Let's talk about the second use case: table sampling. Table sampling is a very common thing. All the engines, they come with the sampling functions. Essentially you can take  sampled data from your large dataset, and it's very good for data exploration. Great. But then our machine learning engineers, our data scientists want reproducibility. If they are doing something on a sample dataset today, they want to be able to reproduce tomorrow.

Add on top of that, if you have two tables, two large tables, and you want to do a join between them and you want to do a sampled version on both the tables, you want to make sure that there are keys that exist in both the output of the sampling from both the tables. If not, the join would just be meaningless. So to do that, we introduced bucket-based sampling. With Iceberg we bucketed the Iceberg tables and then instead of using sampling algorithms like Bernoulli and system sampling, we started doing the bucket.

So for example, if you have 100 buckets in a table, we'll say if you are doing one percent sampling, we just pick one bucket and we read that entire bucket. Now the good thing with that is if you have two tables, you are doing one percent sampling on both the tables,

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/630.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=630)

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/640.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=640)

as long as your bucketing key is consistent between the two tables, you are guaranteed that the same keys are present in the sampling output from both tables. With that, our data scientists and engineers were able to use sampling for their data exploration, which required reproducibility or even joining between multiple tables. With that, of course, the developer velocity went  up. We saw speed ups of over 90% with our users, and the deviation from if you were to do the entire table scan was less than 1%. 

[![Thumbnail 650](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/650.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=650)

The third use case is feature backfills. At Pinterest, our mission is to inspire users to curate a life they love.  To achieve this, we rely on state-of-the-art recommendation and ads models trained on tens of petabytes of data over the span of many months of engagement logs. These models drive personalized recommendations, showing users content that resonates with their interests. These models show significantly better performance when trained on large datasets with events spanning over multiple months.

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/690.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=690)

Our ML models are trained on a wide range of features, including pin, user, advertiser level, and session-based features. Machine learning engineers are constantly experimenting with new features, and  then they do the AB testing, and based on that, they will decide which features are going to live for a longer period of time. The traditional approach of doing this is forward logging, so you add the feature in production, you wait for the data to accumulate. Usually that takes three to six months, and the worst part is you are doing experimentation in production.

[![Thumbnail 720](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/720.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=720)

[![Thumbnail 740](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/740.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=740)

A very easy way to solve this problem, and it's very popular too, is to rely on  feature backfills. We counterfactually compute historical feature values and join with production data. The benefit is we are not waiting for our data to accumulate, and we are isolating the production and experimentation environments. One may ask, okay, then why were we doing the forward logging in the first place? That's because of the joins. 

[![Thumbnail 770](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/770.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=770)

The joins are super expensive when you're doing a join between tables which have petabytes of data. Especially if you have petabytes of data, the most common join mechanism that works is sort-merge join, which requires a huge shuffle. The shuffle was so expensive that we couldn't do the feature backfill approach in the past. And every time you were to use it, every time we add a new feature, which happens multiple times in a day, the cost keeps going up. 

[![Thumbnail 780](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/780.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=780)

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/810.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=810)

So we went after the shuffle with Iceberg, and we used the bucket join, also commonly known as storage  partition join in the Iceberg community. And with that, we were able to avoid expensive shuffles, achieve 65% cost savings in these large joins, and we were actually able to enable our machine learning engineers, or feature engineering folks, to do feature backfill instead of forward logging and achieve 90 times better feature development speed up. We added the support on Trino and Ray. 

This was already on Spark 3.5. We backported it to 3.2, which is what we used to use. And on top of that, we also enabled users, for example, on Ray, people can just read these tables, these buckets, do the join on the fly on Ray itself without doing any materialization before. And yeah, that was the feature backfill use case.

[![Thumbnail 840](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/840.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=840)

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/850.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=850)

### Operating Iceberg at Scale on Amazon S3: Key Learnings and Solutions

Next, I'll share with you some of the learnings we had  on operating this at scale on Amazon S3. The first one was user agent-based access control. So  when we decided that we wanted to go from Hive to Iceberg, very commonly people do data rewrites. They'll read the entire table from a Hive table, convert that into Iceberg, and then start using from Iceberg. The problem with us was that we have 200 petabytes of data on Iceberg today, and we couldn't do that copy. That would be super expensive for us.

So what we did was we decided to do in-place migrations. On the Hive table, we would build a snapshot and then atomically swap the table definition to use Iceberg. Now to do that, we wanted to make sure that only Iceberg-capable or Iceberg clients or catalogs are manipulating the data,

because in the Hive world one can go write a new file in a partition and then that file will show up the next time you read the table, but in Iceberg that's not the case. So we wanted to prevent accidental writes to the Iceberg datasets by directly accessing the S3 location and not modifying the table definition or creating a new snapshot. To do so, there were various mechanisms, but the best mechanism that we thought would work for our scale was using user agent-based access control.

In Amazon S3 you can define, while making the request, you can add some information in the user agent of those requests and have policies on your bucket level or even prefix level that either allow or deny based on the information present in the user request. This wasn't intended to block malicious access. This was intended to block accidental access, people who didn't know that their table definition has now changed to Iceberg. So this was very instrumental for us. We were able to modify all of our Iceberg catalogs and clients to add additional information in the user agent and then have bucket level policies with which we were able to deny anything that wasn't coming from Iceberg ready surfaces like Trino, Spark, and Flink.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/990.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=990)

The second learning we had  was the power of S3 request logs and S3 inventory reports. In Iceberg, it's very common because every time we are generating a new snapshot whenever we do a write, right? And the promise that we make is that if a user has started reading a data or a table and we do the concurrent write at that time, the previous read is not going to get affected. But under the hood, the way we do it is we write the new data, we keep the old data, and there are multiple versions of datasets living together. And this very easily can lead to a problem where the old data is just sitting there, it's not referenced by any snapshot, commonly known as orphaned file or unreferenced files problem.

To figure out if and how much of unreferenced files or the orphan files are existing within our Iceberg datasets, we heavily rely on S3 inventory. So we can look at the S3 inventory and compare that with the files in the Iceberg table definition or the metadata and find out which files are unreferenced or orphan, and then get rid of them. The other place we use this is to identify the accesses that are being made to Iceberg tables and figure out, are these accesses legit? If they are not, again this wasn't meant to block or find out malicious users, but then to find out users who are not using Iceberg in the intended way. And we did uncover a lot of that with the help of the S3 request logs.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/70f2d8beef3f2eeb/1100.jpg)](https://www.youtube.com/watch?v=L2v8WdBGjvg&t=1100)

Okay, the third learning I would share about is the throttling. It's very common, and one of the promises that Iceberg made was that this will solve the problem.  But we at Pinterest, we chose to do the in-place migration. Now when we do in-place migration, what that means is that we are still keeping the old way of organizing data in a table or on S3. So in the Hive world typically you have the table name, the partition name, the partition values, and then it goes on. If you have multiple partitions, it goes on.

Amazon S3 recommends that you have high entropy in the object paths as early as possible, but because your table name, partition name, all those super long strings are pretty static, right, so we aren't able to introduce any entropy until we reach the file name. And what this essentially means in terms of throttling, we usually would, if the dataset is large, it is being written by a lot of concurrent executors, we would hit the 503s, which is expensive because it means wasted compute and wasted time for our users.

With Iceberg, even though we did the in-place migration slowly, once we made sure that everything is going through the Iceberg table definition, we were able to modify how these object paths are, the objects locations are decided, and slowly we were able to use the recommended approach of using entropy in the path as early as possible. One of the recent things that Amazon contributed to Iceberg was the writing objects on 20-bit base to hash locations. And we started using that. We rolled that out to 66% of our top 10 datasets and boom, we don't have any more user complaints on 503s. We don't see it in the request logs, and this was one of the top concerns we used to get from our users, not anymore, so I highly recommend using that. And with that I'm out of time. But thank you all.


----

; This article is entirely auto-generated using Amazon Bedrock.
