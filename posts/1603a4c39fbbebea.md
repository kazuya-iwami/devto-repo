---
title: 'AWS re:Invent 2025 - Data modeling core concepts for Amazon DynamoDB (DAT311)'
published: true
description: 'In this video, Jason Hunter, a Principal Solution Architect specializing in DynamoDB, explains data modeling techniques using a phone book library analogy to illustrate partition keys, sort keys, and indexes (LSI/GSI). He demonstrates practical schema design through a chatbot history application example, covering requirements like retrieving user threads and metadata. Advanced scaling topics include compression strategies, sparse GSIs for soft deletes, partition sharding to handle over 1000 writes/second, DynamoDB Streams with Lambda for cost aggregation, and zero-ETL integration with OpenSearch for text search. He emphasizes incremental export to S3 for cost-effective disaster recovery at $0.10/GB versus $150/TB for full table restore, and introduces the new multi-attribute partition key feature for GSIs that eliminates manual attribute concatenation.'
tags: ''
series: ''
canonical_url: null
id: 3085291
date: '2025-12-05T04:43:14Z'
---

**🦄 Making great presentations more accessible.**
This project enhances multilingual accessibility and discoverability while preserving the original content. Detailed transcriptions and keyframes capture the nuances and technical insights that convey the full value of each session.

**Note**: A comprehensive list of re:Invent 2025 transcribed articles is available in this [Spreadsheet](https://docs.google.com/spreadsheets/d/13fihyGeDoSuheATs_lSmcluvnX3tnffdsh16NX0M2iA/edit?usp=sharing)!

# Overview


📖 **AWS re:Invent 2025 - Data modeling core concepts for Amazon DynamoDB (DAT311)**

> In this video, Jason Hunter, a Principal Solution Architect specializing in DynamoDB, explains data modeling techniques using a phone book library analogy to illustrate partition keys, sort keys, and indexes (LSI/GSI). He demonstrates practical schema design through a chatbot history application example, covering requirements like retrieving user threads and metadata. Advanced scaling topics include compression strategies, sparse GSIs for soft deletes, partition sharding to handle over 1000 writes/second, DynamoDB Streams with Lambda for cost aggregation, and zero-ETL integration with OpenSearch for text search. He emphasizes incremental export to S3 for cost-effective disaster recovery at $0.10/GB versus $150/TB for full table restore, and introduces the new multi-attribute partition key feature for GSIs that eliminates manual attribute concatenation.

{% youtube https://www.youtube.com/watch?v=oMVRvg_8m_I %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
### Introduction: A 300-Level Session on DynamoDB Data Modeling

Thanks for starting off your Monday with some Dynamo. I'm Jason Hunter. I am a Principal Solution Architect with a specialty in DynamoDB. My job is to know DynamoDB thoroughly, and if you are using DynamoDB and you get stuck, you can talk to me or one of my peers and we try to help you out. If you need cost optimization, we'll look at your tables. If you say I'm having a big launch coming, am I going to be ready? Is it going to handle the traffic? That's what I do.

[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/0.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=0)

 Today I'm going to talk about data modeling and some of the techniques. This is a 300 level session, so I assume maybe some familiarity with DynamoDB but not extensive. One of the hardest parts of doing a talk like this is figuring out how to level set. How are we going to start? Do I assume that you're brand new? Do I assume that you're here pretty familiar and want to level up? I kind of have to assume both.

What I'm going to do is start with an analogy of how DynamoDB is kind of like phone books in a library. If you're new to DynamoDB, this should help. If you're an old hand, it might be fun to see just another way to think about DynamoDB's data modeling. Then we'll talk about some core modeling concepts using an app that we're going to build together. At the end, we'll discuss what to do when you need to scale bigger because it's pretty easy to write a low scale app on DynamoDB, but when you get big, there are a couple of techniques you want to know about. If there are questions, we'll do them at the end.

[![Thumbnail 60](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/60.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=60)

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/90.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=90)

### Understanding DynamoDB Through the Phone Book Analogy

 Let's start with a day in the library. This is an Alex DeBrie analogy originally, and I'm kind of stealing it and expanding on it. He said DynamoDB is a lot like phone books. Does anyone remember what phone books are? Are we old enough here? Yes, a few of us. Before your cellphone knew everybody, before you could Google a name, you had these actual physical books.  On the spine was a city. You'd say, alright, I'm going to be in Denver, and you'd open up the Denver book. In there would be a sorted list of the people in Denver. Yes, we all shared our numbers with everybody. They were usually sorted by last name, and you could find by the last name and by the first name. That's kind of a lot like what a base table is in DynamoDB.

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/110.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=110)

[![Thumbnail 130](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/130.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=130)

 The partition key, the thing that you use first to find your zone, your item collection we call it in DynamoDB, is the city. That's the partition key, the PK that we call it by short. Here we're in Las Vegas, so we're looking at the Las Vegas book.  Inside of there is a sort key which is sorted by last name or maybe with a company, just their regular name and sorted alphabetically. There's a payload with each item, a phone number, a zip, maybe an entity type, you name it. This is a lot like what the DynamoDB base table is because there's a partition key and a sort key. You specify the partition key, and then you specify the sort key to find an item.

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/170.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=170)

 But what kind of queries can we do with this? Well, given a partition key and sort key combo, I can get the payload. I can find people whose name starts with Hunter. I can find ones who start with H. Can I find Jasons? No. If someone is on Stack Overflow asking, I'd like to do my sort key but I want to do ends with, what's the answer to that? There are two answers to that really. One is no, the other one is, well, if you reversed your sort key, then you could do it. Sometimes in DynamoDB you do that kind of thing. If you really want ends with and if you had another copy of it reversed, then you could do it.

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/230.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=230)

Can I look up by phone number? Not with this data model. By zip? No. By entity type? No. There are some things you can do, and there are some things you can't do. That's why DynamoDB is so efficient because when you issue your query, you go to the book, you find the sorted list, and then you either find the item or the starts with or the between or the range, and that's how you find it. But if you're thinking that's a limiting set of queries you can do,  that's why we have indexes. One type is called the local secondary index, or LSI. This is where you say, I want to have the same partition key but maybe a different sort key.

In phone books, that's what the yellow pages were. For those of us old enough to remember phone books, in the back, the pages were yellow, that's why they called it yellow pages. They were sorted by like plumbers and appliance repairmen or whatever that is. Here I'm doing a sort key of casino or hotel, and now I've expanded my ability to query because I can go say, hey, find me casinos in Vegas. It's not hard, or hotels also not hard. This is one way to think about what the base table is and that an LSI is an automatically updated thing. When you update the one, it kind of propagates into the other one. It gives you another way to query it.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/280.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=280)

[![Thumbnail 290](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/290.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=290)

What can we do and can't we do? I still can't look up by name or phone number  or zip here. I could do the name against the base table. There's another thing called the global secondary index.  Nothing to do with global tables, by the way, common naming issue we have there. In this case you say, you know, I might want a different partition key. Instead of looking by city as the book, maybe I want by zip code as the book. That would be a smaller book probably usually, and in there would be its own sorted list of names by that zip code if I have name as the sort key on my GSI.

[![Thumbnail 330](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/330.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=330)

So now I can say, well, I know he's in Denver, I know he's in the zip code, and I can pick which one I want to do as my access. Makes sense? So what can we do? We can do now a different partition key and possibly a different sort key. I've chosen here to do the name as a sort key. What else could I do? I could do a phone number as a sort key.  And then I could find a certain phone number and a zip code. What if I want to look at my phone number in general in the whole database? Then I would have a GSI with the phone number as the partition key. And that would have a different, that'd be very little phone books. It'd be like kind of like a little book for every phone number in the world, but then going to the partition key, I don't even need a sort key. Sort key is optional in DynamoDB. I could look up by phone number. So one copy of the data and then you use these indexes to pick the different ways that you want to access it.

[![Thumbnail 370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/370.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=370)

That's the basic way that DynamoDB works, always with a partition key and an optional sort key. Right, how are we going to want to update these things? The easiest way to do it is we're going to  update just the base table, and we're going to let the other ones propagate out. They're a little bit different between LSIs and GSIs. LSIs, because they're in the same phone book, you know, the white pages and the yellow pages together, that one's done strongly consistently. When you update the base table, the LSI is updated at the same time, because it went to the same place. GSIs are in a different kind of place. They're in like a whole separate books and a whole other set of shelves. That one is eventually consistent.

[![Thumbnail 420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/420.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=420)

People want strongly consistent. I look forward to the day when we get that. We don't have it today, but if you update the base table, it will usually in a millisecond, but it can be longer, propagate over to the global secondary index, and then you only have to update in one place. This is how it works. If we think about storing this stuff, I picture a library holding the books, and so,  you know, I'm going to have a library and on there are going to be a whole bunch of phone books that I have to go find, but do I want just one library? What if there's a fire? What if one of them's down, right? What if there's road construction? It's hard to get to one of them.

[![Thumbnail 440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/440.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=440)

[![Thumbnail 460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/460.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=460)

Well, let's keep three copies of the phone book and let's separate them by, you know, a meaningful distance, possibly in different availability zones.  And then we'll make DynamoDB into a regional service. So if even one availability zone is down, you've still got two other copies and life continues. It's great that you don't have to think about this. You don't have to think about subnets. You just talk to a public endpoint, and we've got three copies of your data across three different availability zones all the time. And how do we update if we have three copies?  Well, amongst the three nodes, they will elect a leader, and it'll rotate. You're the leader, then you're the leader. They come to do a vote, and one leader gets all the updates.

Now when you're doing DynamoDB calls, have you ever seen that you can do a strongly consistent or eventually consistent read? Strongly consistent says go to the leader. You know you always have the latest data. Eventually consistent says any of the three will do. And you've got a small chance of seeing something that has not had the full propagation. A write is durable as soon as it hits one and the other one acknowledges as well, so you have to hit the two. The third one, we don't wait for it. We acknowledge it after the two. And so if you do an eventually consistent read, you might get that third one that is not necessarily as caught up, but eventually consistent reads are half price and eventually consistent reads have three targets instead of one, which increases availability. Makes sense?

[![Thumbnail 520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/520.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=520)

[![Thumbnail 540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/540.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=540)

### How Partitions Work: From Shelves to Splits

Right, and then this is where I think it really gets interesting because most people with DynamoDB who've been doing it for a couple of months understand all that, but let's understand  how partitions work. I think there's a common misconception that a partition key makes its own partition and that's not how it works. Think of it that in the physical library we have to have our books on shelves. Right, and each of these shelves acts like a partition. They're only so big, so we have to figure out how many of them we want to do.  How many shall we start with? One seems like not enough, ten seems like quite a lot. Let's do four. And why did I pick four here? A default on-demand table actually creates four partitions on the back end. We had to pick a number that was enough to handle a decent amount of traffic, but not so much as to be wasteful. You don't want to have more partitions than you need. So by default it does four.

If you've heard about warm throughput, that's where you can hint to an on-demand table you're going to be a big table, start out big, and that would increase the number of partitions on the back end of a table, and that's a really nice feature. Now with CloudFormation you can in an on-demand table say start large. You say that you want a lot of reads and a lot of writes. I have seen enterprises that go live with an on-demand table and they didn't do warm throughput. And as a result, the first hour, they're sending more traffic than a four-partition table would want to handle. And so it splits, as I'll talk about here, but it takes a little bit of time to do the splitting.

[![Thumbnail 600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/600.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=600)

Alright, so first thing we have to do is figure out how do we assign data to shelves? How do we assign items  to partitions? I think the obvious one is alphabetically, right? If you go to a library, you're probably going to find the cities arranged alphabetically.

[![Thumbnail 620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/620.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=620)

That's pretty good. But we don't do that. Why not? We don't do that because city names are not evenly spread. I'm from the Bay Area. Everything starts with S. It's a Santa this or a SA that.  So if we did that and we didn't know about what our data was, all the data would kind of go to the same partition if it was alphabetical. Everything would end up on the last one here, Q through Z.

[![Thumbnail 640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/640.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=640)

So what else can we do? Well, if you want a nice even distribution, the easiest thing to do is hash the name, hash the partition key. Now you're going to get nice distribution, even if the  partition keys are very lexically similar. They'll hash all over the place, and now you're going to get a nice distribution where about one quarter of items naturally end up on each of our four shelves. That's why you'll see the partition key called the hash key internally, because we hash. I kind of like the hash key name more because the partition key is what leads people to thinking that a partition key means one partition, but what it is is that we hash the partition key to help determine where it goes. But it alone doesn't get a whole shelf for each book.

[![Thumbnail 670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/670.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=670)

[![Thumbnail 690](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/690.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=690)

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/710.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=710)

Each shelf is only so big,  so at some point we're going to fill it up. I start with a table of four partitions, I keep loading data, and at some point we're going to say, alright, that's enough. In DynamoDB, the enough is around ten gigabytes. So if you've loaded enough and a partition gets around ten gig, we're going to start thinking, hey, maybe we should do something with that. What we do is we split it. Generally when it gets big, we split it in half right in the middle.  Now we have two different shelves there. We split the 40 to 5A hex prefix in half with the 5B to 7F being the other half. And now we can add more space and so we do this all the time in the background. A table that's been around a long time is going to have a lot of partitions because as data grows we're going to be splitting it on the back end for you. 

Some edge cases: what if a city or maybe a zip, but especially a city, what if it gets bigger than a shelf? What do I do there? The answer is exactly what you do in New York City. If you ever saw a New York City phone book, it wasn't a book, it was several books where they had split it by the last name. This is A through H or something, and so on and so forth. They split by the sort key. And we do the same thing. We split by the sort key too. If we see that it gets really big, we can split that thing right in the middle of an item collection and split the partition.

With a caveat, this is a pro-level caveat: we can't do it if the table has an LSI. Because remember the LSI, the idea is that I have the white pages and the yellow pages together in the same physical place, so I can't start splitting the white pages in three because now I don't have a good place for the yellow pages to go. This is why if you have an LSI, you can't have an item collection bigger than ten gigabytes. Because of what's going on underneath the table, we are like, nope, an item collection has to be contiguous. A book has to be one book, which is a downside of LSIs. The upside is the strongly consistent read nature. The downside is you can never get rid of them. Once they're on a table, you can't remove them, so they're table creation. And also they limit how big an item collection can be. Ten gigabytes is a pretty big item collection. It doesn't hit a lot of people, but it is there as a rule.

[![Thumbnail 800](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/800.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=800)

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/820.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=820)

[![Thumbnail 830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/830.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=830)

Alright, something else that happens in the  physical world is that there's only so much traffic that can go to one of these shelves. Each partition has hard limits. Do we know what they are in DynamoDB? Each partition gets three thousand read capacity units and one thousand write capacity units. And that's a hard limit there. At some point it gets a lot of traffic. Too many people are coming to write or too many people are coming to read.  So what do we do about it? We split. So we can split the books across shelves if we think it would help. So before and after,  we've said this one book is so popular that we're just going to split it into its own item partition. This item collection, because it's a book, an item collection gets its own shelf, and now that item collection can have up to one thousand writes and three thousand read units per second.

[![Thumbnail 890](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/890.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=890)

What if everyone wants the same item in that book? We can actually split one item to its own partition. It goes down to that level. So you can in DynamoDB, if you're going to really bang on one item and you're going to update it one thousand times a second, you can update it. You can read it three thousand read units with eventually consistent reads, that would be six thousand calls, so you can read the same item six thousand times a second because of how we split it. Now, you don't get it the first second, but we will very quickly split when we see the traffic. We call it split for heat internally, and if you Google up split for heat DynamoDB, you'll find a blog that has me testing this under all kinds of circumstances  to see what happens under pressure, and you see really quick expansion on the order of minutes.

So hopefully now if you're new to DynamoDB, you kind of get a sense of how DynamoDB thinks and why it's a key value store but with a sort key as well, not just simple key value. And if you've seen DynamoDB before, maybe that was a little fun trip to the library.

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/910.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=910)

[![Thumbnail 960](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/960.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=960)

### Core Modeling Concepts: Partition Keys, Sort Keys, and Common Patterns

Now let's think about some real data modeling and make it real.  First, a table always has a partition key, and you get to pick the type: string, number, or binary. We like string a lot because it's very flexible. If you want to do it with an account number, you can. If you want to do binary, you can. We kind of like string, and we sometimes like to mangle our strings to say what they are. For example, a customer ID string might be cust_ID_123, with a hash being a common separator. We do that because later on this table, I might want to store something that's not just a customer ID. We often like to use the same table and put different kinds of things in there. If I have a partition key that's a string that self-describes what it is, I have that choice. You don't always have to, but a lot of times we'll name our partition key PK and our sort key SK.  That way we're very flexible about what's coming.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/990.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=990)

[![Thumbnail 1000](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1000.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1000)

[![Thumbnail 1020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1020.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1020)

I actually think if I were to create DynamoDB today, I might say I'm not even going to ask you for names and types. They're going to be PK and SK, they're going to be strings, and let's just get rid of the whole idea of schemas and then you tell me. But sort keys are optional, so I guess you'd have to at least tell me that. You can't change a key value if it is an indexed item—a partition key or sort key. You have to delete it and reinsert it. You can change a payload attribute, but you can't change one of the indexed attributes.  So what kind of partition keys do you see? A lot of times you see a descriptive one, like zip_89109, and this says this is a zip code. I know that because I put a zip hash in front of it.  Sometimes, as we'll see later when we start charting because we get so much traffic, we want to support more than 1000 writes a second. We might add a suffix at the end—0, 1, 2, 3, 4—and that way I'll have more partition keys to write to in order to handle an influx of data that's coming really quickly. That comes a little bit later. 

Sometimes we see multi-value partition keys. In multi-value, it's like I have a zip code and a type together, and you can do zip_89109_type_casino, or you just do 89109#casino. It gets awkward. Has anyone seen the announcement from about a week ago where we now let you do multi-attribute partition keys and sort keys on Global Secondary Indexes, not on base tables yet, but on Global Secondary Indexes? So this whole maneuver to have multi-values, you don't have to do it anymore for Global Secondary Indexes. Really nice. Why? Because when you do a Global Secondary Index, sometimes you're like, I want a Global Secondary Index lookup and I want to do a where clause where it's in this zip code and it's of this type. If you didn't previously on your base table have an attribute that was those two joined together, you now have to write to all your base table items to have something to project into the Global Secondary Index. That's a lot of writes. It's a pain on you. Now you just say, I'd like a Global Secondary Index. I'd like it to be synthetically created as if these were joined together, and we're like right away, boss. No base table writes, no ugliness in your base table, no increased storage in your base table. I love a good feature. That's a very nice feature that we just introduced about a week ago. Too recent for it to be on the slides.

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1100.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1100)

When you see sort keys, sometimes you see them typed,  like name_hash_hunter, whatever. We do that because a very common convention in DynamoDB is to have a partition key be the thing—it's a customer, it's a device—and then the sort key prefix is like the different things that you know about it. For online shopping, I might have address, and then I might have another one that's like your credit card, another one like your orders, and another one like your shopping cart. The sort key prefix is like a description of what we know about this. It might be like order_ID_1, order_ID_2, order_ID_3, and I'll remember the orders that this person previously did. We call this single table design because we do different entity types in the same table. It's popular because if I want to learn about you in one go, I can just go to the database and make one query call and bring all of it back really fast and cheaply, because one quick call and we price by the amount of data returned or at least scanned.

[![Thumbnail 1160](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1160.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1160)

[![Thumbnail 1170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1170.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1170)

[![Thumbnail 1200](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1200.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1200)

That's a common convention and we see the typing here when we're doing such things. Sometimes people put a timestamp.  It's a pretty common sort key. If you have a device and you have different measurements at different times, we see the timestamp there. Sometimes you see hierarchical, like country, state, location.  That way, because I'm a sort key, I can do a starts with. I can say starts with USA and I can read everything in the USA. USA_NV and I can read everything in Nevada. USA_NV_LAS and I can read everything in Las Vegas. So that single sort key gives me three different access patterns by doing this. Again, this is going to be nice with the new feature because it's a little bit more expressive than you having to think through it as a string and prefixing. Now you can just say where these three things are like this.  It's syntactic sugar and I got a sweet tooth. All right, so let's put it in action—a somewhat realistic example.

[![Thumbnail 1210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1210.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1210)

### Building a Chatbot History Schema: Requirements and Solutions

I have to dream up an example of something  we all know. Here's my only AI in the talk. We're going to design a schema which is a history of a chatbot. You ask your questions, you get your answers, and I need to remember this history so that I can feed it back to the chatbot to remember the state when you come back and things like that. So I'm going to try to scale this big with millions of users, threaded conversations, and each conversation has its own specific metadata.

[![Thumbnail 1240](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1240.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1240)

What are we going to do?  Here are some requirements. Given a user ID, I want to pull all the threads and thread metadata. Just give me everything about the user because I'm building the interface and I'm going to populate the left sidebar with everything. Given a user and a thread, pull just that thread. So not everything, but just like I expand a thread and I want to see what's in there. Given a user ID, pull a recent thread, so I pull the last stuff. It's always fun when you get requirements and you have to think of a schema.

[![Thumbnail 1270](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1270.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1270)

I think it's pretty obvious that the partition key is going  to be a user ID because every one of these requirements is given a user ID. One of the nice things about DynamoDB is that if I solve it for one user and that user works well, I know I can solve it for one trillion users because of how each partition key can be isolated in the database to its own partition if I have to, and even smaller sometimes. I know that it's going to scale up to any level. You can often think about your world as: did I do well for one partition key and the amount of reads and writes that it accepts? If so, it will scale to whatever I need, and you can sleep well at night without any surprises because there's no dependence on what one partition key is doing with another one if they've split to different partitions.

So the partition key is going to be a user ID. The sort key is going to be thread metadata and thread messages. This is a pretty common convention where you have one item in the item collection that is like the metadata about it, and then a bunch of other little payload ones, maybe by timestamp here with a create time, which is going to be each message and the time of that message. You can pick your thing; you can do IDs, but time is easy to model. Given this, given a user ID, I can pull everything by just saying no sort key constraint and give it everything. I can do a sort key starts with a create date and thread ID and I can learn everything about that thread as long as I know the date and the thread ID together.

[![Thumbnail 1370](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1370.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1370)

I can also pull recent threads by just saying start at the bottom and sort backward by time, maybe pulling in batches of 50 or 100 or whatever until I've read enough, and that will get you the last ones because it says recent threads and it isn't specific, so I'm just going to be able to go in the sort key starting at the bottom and go back up. NoSQL Workbench is a client-site  application that we often use to visualize our data, and here is a screenshot from it. This is all for one user ID 12345. We see a sort key, which is the timestamp down to the seconds, and then we have thread IDs, user IDs, all that. Notice that the bottom item is different than the first two. It is meta, so that bottom one has different attributes. It is metadata about this.

This is weird for relational people because they're like, wait, did you just switch attribute names in the middle of a table? I sure did. I can do that. It's NoSQL, and it's one of the things you can do. What's nice is I can for a given thread ID just pull all this stuff in one go, even though one's metadata and one's the details. So I've satisfied the requirements, but you know how the real world works. They give me more requirements.

[![Thumbnail 1420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1420.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1420)

[![Thumbnail 1440](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1440.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1440)

All right, what's the new requirement?  Given the user ID, I just want the thread metadata. You just want the meta objects across all threads. All right, that's harder, right? With my current model, I would have to scan all the detailed messages to get the metas. I just want the metas. I want to pluck out the metas. What do I do? I have indexes.  A lot of times when you have an issue about a different access pattern, you think I have indexes. So we make a Global Secondary Index. We're going to make a Global Secondary Index that's sparse, sparse meaning not every item in the base table is going into the Global Secondary Index.

When you do sparse ones, it's kind of nice because you pay by the storage. You don't have much storage, and you pay by the writes, and you don't have many writes. So a sparse Global Secondary Index is a very affordable construct to create. I'll have the same Global Secondary Index partition key, but I'm going to have to have a sort key. You know what, anything that I put on the meta-item, which only exists on the meta-item, will be my sort key. If it exists, it goes into the Global Secondary Index. If it doesn't exist, it's out of the Global Secondary Index, and that's how we make a sparse Global Secondary Index. You do it on an attribute that's not always there.

[![Thumbnail 1500](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1500.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1500)

If I do this, then I have a nice Global Secondary Index ready to query just for you. It looks kind of like this. This is two different users, 12345 and 67890, and they're metadata items. When you project into the Global Secondary Index, you can  choose which attributes you want to project.

[![Thumbnail 1530](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1530.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1530)

[![Thumbnail 1540](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1540.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1540)

You can project all of them. At minimum, you have to project the keys, which are the partition key and sort key of the base table. They get projected in, and so now I can read just the metadata. It's a nice optimization when you do this kind of work. You think about how often you want to do this, because you could brute force it by just reading the whole item collection. But if you're going to do this a lot, then you probably want to spend the effort doing the writes with the right cost to save your read costs later.  So there are a lot of read versus write trade-offs. But I said GSIs were eventually consistent, didn't I? What do we do about that? 

Well, one thing we can do is accept that the GSI is usually tens of milliseconds behind, and a lot of times it's one millisecond or ten milliseconds. Are we okay with that? I'm going to get the metadata, and how fast is this really anyway? A lot of times we just accept that. Sometimes you say, you know what, this is my time for an LSI. I'm going to create an LSI because I need the strong and consistent nature of an LSI. You just have to be careful, because it limits your item collection size. You can't delete an LSI after creation like you can a GSI.

Another thing you can do is dual-write. You could say, I'm going to manually insert into a thread table and into the main table. You can pick which one goes first because sometimes it's okay if it's in the metadata but not in the main table. If you read it, you're like, oh, it's all right, it'll come in a second, let me try again if you're doing this yourself. But you have the chance that your program crashes between the two writes, and now you've left a dangling index entry. So another thing you can do is a transact write call. We have transactions, and you just say insert these into both tables and it succeeds or fails as one unit. Transactions are twice the cost of non-transactions. So if you like that, how often are you going to do it? Do I want to pay double? Do I want the GSI that's eventually consistent? It's your choice.

[![Thumbnail 1620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1620.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1620)

[![Thumbnail 1640](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1640.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1640)

### Cost Optimization Techniques: Compression, Item Splitting, and Storage Strategies

Alright, so up to this point, this is pretty standard DynamoDB stuff. I feel like using DynamoDB for lower scale  work is like weightlifting with light weights. You can really do it with bad form and still succeed. But now we're going to lift the heavy stuff. We're going to deadlift 500 pounds. Alright, let's talk about form exactly. So the first thing is we were successful. This was great, reduced costs. You always get this call as a database person, right? You launched, it was good.  You're doing things with scans, right? Scans are where you just scan the table to find what you want, just like a relational database full table scan. You can do it on small tables, but it gets expensive for big tables.

[![Thumbnail 1660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1660.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1660)

Alright, so let's see what we can do. First thing I noticed is that user message and bot response, which are the big text fields, are just stored plain in there.  There's a cost with DynamoDB for storing things. There's the storage unit you pay by default in US East 1, 25 cents per gigabyte per month. There's the write units because writes are charged by the kilobytes written. There's the read units which are charged by the 4 kilobytes read. There's point in time recovery, which is a kind of backup where you keep a write-ahead log and it is proportional to the size of the table. And there's backups which are costly and proportional to the size of the table. So the smaller your data, the smaller your bill. If you ever get a chance to reduce the size, you should.

[![Thumbnail 1700](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1700.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1700)

So what do we do? Well, the easiest thing is we compress them.  We store the data as you hand it to us. If you want to hand it to us compressed, we'll store it for you in that way, and you'll pay less. You'll pay fewer writes. You'll pay less on all the storage aspects. So GZIP and LZ4 are two options. I did a little math with fake data here, and LZ4 was a little superior. So let me point you that way if you're going to pick between the two. But what are the pros and cons? The pro is way less space. The con is I can't do a filter against the text in the database now because the database doesn't know what the text is. The database is blind. I hold a payload. I don't know what it means.

[![Thumbnail 1750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1750.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1750)

But a lot of times you don't care. In this application, I don't think the database needed to be able to read that string. You weren't indexing it or anything. Maybe if you moved it to another system, but then the other system would have to do the decompression. Another thing that might not be as obvious is if you have a lot of little attributes,  model, model version, temperature, top P, top K, max tokens, all this stuff, sometimes if it's just payload, you're not going to index on them, you're not going to filter by them, you don't need the database to be aware of them. They just need to be handed to the client. You can make it into a JSON map in DynamoDB. You can compress that and store it as a binary in DynamoDB. Now you've got a smaller set of data. It also serializes a little faster, I think. So you might not always think about that.

You can also do just a string. Sometimes people have a lot of little attributes. You can make one attribute with a string. It's a little tighter as far as its internal representation. It's not a huge win, but it is a win. Again, if you don't need the database to be aware of what it is, it's a way to save money. Another thing you see is how much does it cost to update an item in DynamoDB? The answer is it's proportional to the size of the item. If you update one little value in a 6 kilobyte item, it costs 6 write units because it's the larger of the before or after.

[![Thumbnail 1830](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1830.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1830)

How much does it cost to do a delete? It costs the size of the item. So one thing that you see people do sometimes is they have a timestamp in the metadata, which is the last update time. The metadata is 6 kilobytes in my hypothetical example. If I'm doing a frequent update here, what I could do instead is break  it into two items: a static and a dynamic portion. I could only do my updates in the dynamic portion. Now my writes are 1 instead of 6. Is that good or bad? I think it feels good. It's bad if you never do the update, because the initial write needed to do two items, and maybe the first one was like 5.8, which rounds to 6, and then you also have to do the other one, so that's 7. So maybe the initial write is 7. And later on you get 1 plus 1 plus 1. So I'd say it's a win so long as your update is often enough that I'd rather pay 7 plus 1 plus 1 plus 1 than 6 plus 6 plus 6 plus 6.

This is a common thing. One of the fun things is when you're doing video games and you have to keep a list of things in your knapsack. Do you want to have one knapsack item which is just big, but every time you change your knapsack I have to rewrite the entire knapsack? Or do you want to have every item in the knapsack be its own item? It's a challenge. Do you want to have little knapsacks where every unit of update is a knapsack? What is the best way to store many items for a video game player in their inventory? This gets into the cost of writes and the cost of storage.

[![Thumbnail 1900](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1900.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1900)

[![Thumbnail 1950](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/1950.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=1950)

### Scaling to Enterprise Level: Soft Deletes, Sharding, Streams, and Search Integration

Now, new requirements are coming in. Users can now delete threads, and the threads might be really big. So what I probably don't want to do is have the user wait around while I'm actively deleting. What do we do? This is a classic soft delete situation.  I'm going to mark something as deleted, act like it's deleted from the user's point of view, and then later on in the background, do all the cleanup. I'll update an attribute on the metadata item and delete the conversation later. How do I know which threads are deleted? Almost every one of these has an answer as an index.  There's an index on the deleted threads. I can do that. So what I will do is create a Global Secondary Index. I'll make it a sparse GSI because it will only contain the deleted thread metadata.

Now if I delete a thread, I will create a new attribute. I don't care what it's called, but it will be the attribute name that gets projected into my sparse GSI. Then I can go to that GSI and I can see only items which are representing deleted threads. So maybe I will pick a GSI partition key of thread state to say it's deleted, and a GSI sort key of a timestamp of when it was deleted, so that I could clean up the oldest first or something. I go here and I do a query and I say find me in this GSI where partition key equals S deleted. What's the S? State. I'm doing that name mingling thing you see that a lot. You don't have to, but I'm doing it. T timestamp, T representing timestamp. And I can say, all right, find me all the deleted ones, forward, give me the first 10, and I'll go off and actually actively delete them.

[![Thumbnail 2020](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2020.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2020)

Is there anything wrong with this? We're in the scaling section where we try to say how do we do this when the scale gets to enterprise level.  One of the issues is that in that GSI, the partition key of deleted can only accept 1000 writes a second. Are you going to delete more than 1000 threads a second? Most people don't. Some people do. So what do I do about that? This is actually where you get a lot of hot partitions, as we call it. The base table has a beautiful high cardinality partition key, but the sort key has state as the partition key. I chose this here just because it's so common. I wanted to get it out there, and the state can be an issue.

[![Thumbnail 2060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2060.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2060)

So this is where you can shard.  If I was going to do 10,000 deletes a second, then instead of deleted, I would have deleted hash random number. Now I can accept as many items as 1000 times the unique number of partition keys, so maybe I should do 20 so that none of them are close to being hot. This is sharding. You'll hear it all the time. I'd love a built-in feature, but it's not there. But this is where you have to be aware that sometimes my partition, my one partition key can get more than 1000 writes a second. Now we will sometimes be able to split it for you, but if you do this, you're guaranteed it'll work. The details on when we can split hot partitions is kind of complex. I'm happy to talk about it, but I blogged it. If you do that split for hot partitions, the DynamoDB blog tests out various scenarios.

[![Thumbnail 2110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2110.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2110)

[![Thumbnail 2120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2120.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2120)

Let me show you how this works with various scenarios. What if later I have more values like archived?  I could do archived 0, 1, 2, 3, 4, 5, 6, something else, 0, 1, 2, 3, 4, 5. Another way I wanted to show you is that you can just pick  the partition key. You don't need to care as much about what the partition key is. Look at this now. My partition key is S, I don't know, S shard. S stands for shard, yeah, that works. Shard 0, 1, 2, as many as you want, and I've moved the value into the sort key. So now I can do a query which is like give me everything in the S0 bucket. S1 bucket, or I could say S1 starts with Sh deleted. And now I can say give me all the deleted ones from bucket one, and I could now find all the deleted threads by going to my 10 buckets and for each one saying give me the sort key starts with this.

I just want to show this because sometimes you put a value in the partition key and sometimes you put it as a prefix of the sort key. They both work just fine. I think this one is kind of elegant. I pick as many shards as I want because I'm thinking, well, I'm going to update this many times and I'm going to have X many shards, and then the value just becomes something that goes into the sort key as opposed to me having all these partition keys with all the different values. But there's no real difference from the database performance or the cost or anything. It's just a style thing.

[![Thumbnail 2190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2190.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2190)

These people keep coming up with requirements.  I want to count the total costs per model per day, and I want to count the total costs per model for each user to do some billing. This is one of those situations where I probably don't want to put it on the main write path because if I put it on the main write path, I slow down the user, and there's really no need for that just to do accounting. This feels like a backend thing. I want to scale to any usage and I want to not make the user wait.

[![Thumbnail 2220](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2220.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2220)

Have you heard about streams in DynamoDB?  Streams are DynamoDB's change data capture, where every mutation that happens to a table will be put into a stream. You can then watch that stream to do something with the data. Some people do this if you're doing a shopping cart checkout. They'll say, oh, the order placed, and then they proceed to propagate it down. Sometimes people do it to propagate to a downstream system. I can watch that and push it into some other database that's going to have a different index model, or I can write it to S3, or I can check for bad behavior, and you can also do event filters on it to say I only want to be notified about certain things.

There's a tight integration with streams and Lambda, where you can create a Lambda function that says I watch this stream. It's just called for you. You don't have to iterate streams yourself. There's a lot of work to go find the latest data out of the stream, but if you hook up with Lambda, it's all done for you. You create a Lambda function that updates the count and would probably buffer internally. A Lambda will be handed not just one item at a time, but some batch of items, and you can control that so you can make a bigger batch. Now your item, you can have 1000 items and your Lambda can be like, all right, let me aggregate this together and update once instead of update 1000 times. That saves write costs and it improves efficiency because probably the cloud model on one day across all users is going to be used more than 1000 times a second. So it's better to update with a buffered value.

[![Thumbnail 2310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2310.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2310)

It kind of depends on how accurate you need to be though.  Streams are really nice because they have exactly-once delivery. You don't find this often in any sort of streaming event. They're exactly-once, in-order delivery out of your database. That's pretty cool. But if the Lambda function has issues, what happens? If the Lambda crashes, we restart it with the same payload. So you have to be careful because the Lambda might increment once and then die and then be restarted and increment once again. You have to be careful when you're doing the Lambda stuff, and there's no simple straightforward push a button and it's all good.

You basically have to have some state remembered where the Lambda can know if it already did the work or not. One technique is that you make a message ID set in the item of recent updates. You're doing an update and you hash your payload and here's your hash, and you make a note that you did the work and here's a recent set of work so that if you crash and come back you can say, did I already do it? Did my previous incarnation do this work? If so, I'll skip it. You can also maybe use transactions with a client request token, which is a way to get idempotency out of it, as long as you can get a deterministic client request token out of your payload because the Lambda is usually invoked with the same payload unless you say split on error, in which case it will split it down to different payloads to try to isolate the error out.

But it's complex enough that I wrote a blog on it with I think 7 or 9 or some such different ways to do it, all of which are a little bit tricky. You can look up that if you want to see ways to handle this.

[![Thumbnail 2420](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2420.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2420)

As a technical term, when you're doing the lambda processing, the stream is exactly one in order perfect delivery, but your processing of it may not be.  These product managers keep making all their money giving me work. I want to be a PM someday and make other people solve my problems.

So, server side search. Right now you can do a client side search to search your past chats, but it's gotten a little slow because it's unindexed. So I'd really rather have a server side search that I maintain for everybody. What do I do with that? What's the query in DynamoDB that does tech search? There isn't one. Was anyone excited about that? There is one. No, there's not one.

[![Thumbnail 2460](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2460.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2460)

[![Thumbnail 2510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2510.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2510)

What we do though is we integrate with OpenSearch. There's zero ETL, which I think was introduced about two years ago.  You can do this without having your own servers. It's all serverless and managed for you. You define the configuration of how DynamoDB will propagate to OpenSearch, and you have a copy of your data in OpenSearch or a subset of your data if you want. Against OpenSearch you can do all the stuff that is advanced indexing because OpenSearch is really about index tools. You can do relevance ordered search. You can do an analytics type query. You can do RAG, and we have a workshop at three o'clock that I'm leading where we're going to do this actual integration where we're going to use OpenSearch and Bedrock and do natural language processing with RAG driven by OpenSearch and vectors. This is one way to do it with DynamoDB and OpenSearch. It's pretty straightforward. 

[![Thumbnail 2520](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2520.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2520)

### Disaster Recovery Made Affordable: Point-in-Time Recovery and Incremental Export to S3

So one last tip, and I just put this in all my talks because it's going to save you so much money the day that you need it.  This is your money saver. This pays for your re:Invent ticket. The new requirement is we want to be ready to recover from an application level corruption. Something went wrong. A bulk load happened. I deleted an item I didn't mean to. That level of corruption. It's our job as AWS to make sure that your database doesn't corrupt, but it doesn't mean that your application couldn't make a call that you didn't mean to make and actually do something bad with the data.

[![Thumbnail 2550](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2550.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2550)

[![Thumbnail 2570](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2570.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2570)

The thing you do with that is a push button. You can turn on point in time recovery, PITR, and PITR maintains the change log  for up to thirty-five days. You can configure it smaller if you want to forget things after a day, but we will remember up to thirty-five days for you. Against that you can then typically do a full table restore before the damage. That's the classic approach. This is you after doing the bad load, thinking I'm going to get fired. I can't believe it. You just ran a bad bulk load.  The full table restore is a way to solve the problem, but it costs one hundred fifty dollars a terabyte, or fifteen cents a gigabyte, a way to make it sound smaller, but it's one hundred fifty dollars a terabyte. You might get fired. How big is your table?

[![Thumbnail 2590](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2590.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2590)

[![Thumbnail 2600](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2600.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2600)

[![Thumbnail 2620](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2620.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2620)

So what else can you do? There's a cheaper way, and the cheaper way was introduced about two years ago.  Does anyone remember? It is called incremental export to S3. If you have point in time recovery on, you have access then to doing an incremental  export. You can do a full export too. We've had that for a long time. An incremental export though is where you say, I want the list of changes that happen between these two points. Give that to me as JSON on S3. So now, think about it. If I get this on disk, which tells me the time, the partition key, sort key, if there's a  key, the primary key of the item, the old image and the new image, I can see that at this time I changed Mary Grace to Mary Smith. I can now undo the damage without having to go make another table.

One approach is to restore a table. Now I've got two tables. I've got to delta them. I've got to figure out what happened and then put it back, or I've got to point to the new table, but that's awkward because you can't rename the table to be the old name. You've got to point to the new name. But here I can just do an incremental export. I can then process it and say, all right, items for this set of item collections, these partition keys, these are the ones that are bad, roll it back. Whatever is the old image, make that the new image, basically on the table.

[![Thumbnail 2670](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/1603a4c39fbbebea/2670.jpg)](https://www.youtube.com/watch?v=oMVRvg_8m_I&t=2670)

And the cost is what? Well, it was one hundred fifty dollars per table. Here, incremental export is ten cents per gigabyte of log size, and assuming that you had  like a gigabyte of logs during the time window of your bad load, it changes one hundred fifty dollars to ten cents. There you go. It's bigger if you have a larger table, and the recovery time goes from hours to restore a ten terabyte table to minutes to export and process the export and be able to do it. So I'm just putting that in my various talks. If you ever hit the PITR issue, be ready. You should probably dry run this, but that's a way to undo the damage if you ever make a mistake and not get fired.


----

; This article is entirely auto-generated using Amazon Bedrock.
