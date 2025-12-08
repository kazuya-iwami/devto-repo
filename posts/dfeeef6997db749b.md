---
title: 'AWS re:Invent 2025 - How FSI Revolutionized HFT Analytics with Agentic AI (GBL302)'
published: true
description: 'In this video, AWS Solutions Architect David and digital asset researcher Wes explain how market makers use AWS to build an agentic news analysis platform for cryptocurrency trading. They describe market makers'' role in providing liquidity and managing inventory risks during volatile events like FOMC announcements or influential tweets from figures like Elon Musk. The presentation details their evolution from dictionary-based sentiment analysis to LLM-powered systems, achieving 180 output tokens per second using SGLang with speculative decoding on AWS. Their architecture uses DeepSeek for classification, fine-tuned BGE-M3 embeddings for deduplication (reducing the muddy middle overlap between 0.5-0.75 similarity scores), and Aurora PostgreSQL with OpenSearch for storage. The system processes news streams, eliminates duplicates, generates spread widening recommendations, and alerts traders via Slack within 10 seconds, maintaining human-in-the-loop oversight for final trading decisions.'
tags: ''
cover_image: https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/0.jpg
series: ''
canonical_url:
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - How FSI Revolutionized HFT Analytics with Agentic AI (GBL302)**

> In this video, AWS Solutions Architect David and digital asset researcher Wes explain how market makers use AWS to build an agentic news analysis platform for cryptocurrency trading. They describe market makers' role in providing liquidity and managing inventory risks during volatile events like FOMC announcements or influential tweets from figures like Elon Musk. The presentation details their evolution from dictionary-based sentiment analysis to LLM-powered systems, achieving 180 output tokens per second using SGLang with speculative decoding on AWS. Their architecture uses DeepSeek for classification, fine-tuned BGE-M3 embeddings for deduplication (reducing the muddy middle overlap between 0.5-0.75 similarity scores), and Aurora PostgreSQL with OpenSearch for storage. The system processes news streams, eliminates duplicates, generates spread widening recommendations, and alerts traders via Slack within 10 seconds, maintaining human-in-the-loop oversight for final trading decisions.

{% youtube https://www.youtube.com/watch?v=TCJPcVhlQns %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/0.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=0)

### Market Making in Digital Assets: The Challenge of Real-Time News Analysis

 Hi, everyone. Thanks for having us. I'm David, an AWS Solutions Architect specializing in financial services and digital asset trading systems. Today, I'm going to share a little bit on how digital asset market makers are using AWS macro services to build an agentic news analysis platform. Before we begin, I will pass the time over to Wes for a walkthrough of what market making is in the financial industry.

Thanks, David. I'm Wes. I'm an independent researcher in digital asset market microstructure. Quick disclaimer before we dive in: the views I express today are my own and don't necessarily reflect AWS's official position or any organization I may be affiliated with. This is for informational purposes and not financial or professional advice. So that's the disclaimer.

[![Thumbnail 70](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/70.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=70)

Let's start with the big picture: the trading ecosystem.  In the trading ecosystem, there are buyers and sellers in the financial market. The buyers and sellers will place their orders into an exchange, indicating they want to buy or sell something. But there may not always be enough liquidity in the market. That's where market makers come into the picture.

The responsibility of a market maker is to always be quoting buy or sell orders, making sure there is always enough liquidity in the market. There is always a counterparty in the market to trade against you. Whenever you walk into the market, you can always buy or sell to someone.

[![Thumbnail 110](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/110.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=110)

 This is an abstraction of an order book. On the left-hand side, it indicates the bid orders, and the right-hand side is the ask orders. The Y-axis is the cumulative size at that price level, and closer to the right-hand side is the higher price. You can imagine in the daily work of a market maker, they place multiple bid orders on one side and also multiple ask orders on the other side.

Imagine if there is a news announcement coming up. The market may move significantly, and then all the ask orders may be taken by some other market participants. This could be a risk to a market maker because as a market maker, we need to manage our inventory well to make sure we have enough inventory to place ask orders. If the market goes up suddenly and doesn't fall back, only the ask orders are executed without the bid.

Then the market maker may need to refill the inventory by buying the inventory back from the market at a higher price. This means the market maker may need to sell at a low price and buy at a high price, which may incur a loss in the market maker's portfolio.

[![Thumbnail 190](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/190.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=190)

 In an ideal situation for a market maker, the market should swing only a little bit within controlled volatility. In the ideal case, the bid orders and the ask orders are executed in a very short time. Even though the profit of each trade may be small, the turnover rate could be very high if the market keeps swinging.

One may ask, can the market maker increase the spread so large that every time the trades could get you a better profit? This is a trade-off between turnover and P&L. Usually, the market maker chooses to execute trades in a more high-frequency way, but each trade has a small profit. Let's say if you increase the spread too high, there could be chances that your orders cannot be executed even once in a day. To maximize the profit, market makers tend to execute in a high-frequency manner.

[![Thumbnail 280](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/280.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=280)

Given that volatility is a very important component in the pricing formula, if as a market maker I know the market is going to be very volatile, then I can increase the spread before the event happens.  There are a lot of factors affecting volatility in the markets. For example, the FOMC is going to discuss the fed funds rate next week. As a market maker, we would like to listen to the news and try to analyze how the news is going to impact the market as soon as possible.

If the fed funds rate cut is more than expected, then you know the market is going to go up. In that case, the market maker needs to modify the orders as soon as possible. Otherwise, there may be some other market participants or competitors taking all the ask orders and resulting in a loss for the market maker.

All the analysis has to be done as fast as possible, so we are talking about maybe just a few seconds to finish all the analysis, determine how the market is going to be impacted, and act in the market immediately.

[![Thumbnail 340](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/340.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=340)

According to Chris Beauchamp from IG Group,  the immediate effect of Trump's market-moving tweets typically lasts around 30 minutes. I'm sure you know about Trump making a lot of tweets related to tariffs a few months before, and actually, not only Trump's, Elon Musk is another very good example. A few years ago, there was a period of time when Elon Musk kept tweeting about cryptocurrency. Sometimes he would write a tweet about how he likes some dogs, and then the Dogecoin price would go up immediately.

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/390.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=390)

Compared to traditional market news like FOMC meeting results, Elon Musk's tweets are much more difficult to interpret. For the FOMC  meeting, we know when it is going to be held and when the results will be released. We know the result is just a number in the interest rate, so we know how to do the comparison. If the number is higher or lower than the market expectation, then the market direction will be up or down.

But in the digital asset world, the case may be a little bit different. Using Elon Musk as an example, the tweets could be just some words. Today, he may say that he likes a particular cryptocurrency so much, and on another day, he may express his opinion on another cryptocurrency, saying maybe this is not a really good one. Interpreting the key opinion leaders or influencers' tweets is one of the challenging tasks for market makers because this is something not foreseeable, and this is not something numeric. You need to interpret the tweets intelligently.

Sometimes even if you try to do manual judgment on the tweet, even human judgment may take some time to process it. If you're putting it into a program or algorithmic trading world, it is not acceptable to wait for human intervention to judge whether the tweets or the news are going to impact the market up or down. That's why we need some LLM to help us judge if the news is going to have an impact on the market and to judge if the news is going to have a positive impact or negative impact. I'm going to pass the time to David now.

[![Thumbnail 510](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/510.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=510)

### Evolution from Dictionary Matching to LLM-Based Sentiment Analysis with Optimized Inference

Thank you, West. Before we get into the solutions, let's understand what came before. In the industry, the first approach to tackling this news sentiment analysis,  we started beginning with the dictionary approach, with dictionary matchings. Usually we'll have some industry sentiment lexicons like bearish, bullish, crash, wordings like this, and we put it into a simple pattern matching algorithm. However, you can imagine the problem here is with the limited vocabulary, you can't handle context very well.

For example, if there's a news title like "Massive Short Liquidation Event," you probably get a negative sentiment. However, it should be a positive one. The industry started moving from dictionary matchings to statistical matching and machine learning. With the labeled data and supervised learning, we use models like Naive Bayes or FinBERT on financial corpora. We will get better generalizations in this case and understand the news context. However, this requires massive labeled data training, which means it costs you a lot in terms of spending and also the time to market for these solutions.

Nowadays, we all have the large language models, where we are using the transformer-based multimodal reasoning. Now, with this LLM, with LLMs like Claude and DeepSeek, you can do context awareness with minimal or even no fine-tuning needed.

[![Thumbnail 630](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/630.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=630)

You can reason it on a specific news title, even with domain-specific events. Maybe with a protocol exploit being exposed, you can ask the LLM with reasoning model whether this has caused cross-chain impact. So this is the foundation of the agentic approach. However,  with this LLM, the inference performance matters. Remember what Wes just mentioned, this is a new sentiment analysis on a dynamic moving market with very short time window. We need to react, so we need to do the inference optimization.

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/660.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=660)

Here's the journey. In earlier this year, February 2025, we started this deployment using SageMaker Jumpstart  on P5EN instances, where we resulted with 80 output tokens per second. Not good enough. Later on in April 2025, we replaced it with vLLM. For this, we can enable DeepSeek Multi-Token Prediction, mixed precision linear attentions, also combined with distributed parallelism. Then we had the result boosted to 140 output tokens per second. Still not good enough. In August, we replaced vLLM with SGLang, and with the help of speculative decoding, finally we achieved 180 output tokens per second.

So why does this matter? Imagine when you have 10,000 events per minute, every millisecond of inference latency will compound. Going from 80 to 180 means that we can double our news ingestion processing in the same time window. These optimizations enable the end-to-end latency under 10 seconds that avoids the adverse selection possible.

[![Thumbnail 750](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/750.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=750)

### Building an Agentic News Analysis Platform with Fine-Tuned Embeddings and Human-in-the-Loop

So let's talk about the implementation. Here's our day one architecture. From the bottom layer, you see we keep ingesting news streaming in with news streaming  API going directly into our S3 bucket and trigger the event which triggers the Lambda for doing classifications based on asset, urgency, and sentiment. With this Lambda function, it will trigger the DeepSeek model for tagging all these metadata into the database, including Aurora PostgreSQL and also OpenSearch. We also utilize the Q CLI for the terminal so that even traders and business analysts can interact with these news sources to ask the LLM, ask Claude in the backend, to figure out the latest news about, let's say, Trump announcements or whatever, let's say the last 10 hours what Elon Musk is speaking on or publishing in the X feed.

[![Thumbnail 820](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/820.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=820)

But here's the hidden challenge. In crypto, in digital assets, the same news gets reported  50 times. For example, Ethereum upgrade delayed appears on Twitter, Reddit, Discord, Telegram, all over it, and all within minutes. So if you process every duplicate through the expensive LLM, you will be burning money and killing the latency. So we have another deduplication pipeline to handle this problem.

[![Thumbnail 850](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/850.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=850)

First, step one, we want to calculate the embeddings  in the Lambda, calling our embedding model BGE-M3. And then we do the similarity check. If it is greater than 0.75, which means it is a duplicate, we insert that into the duplicated collections in OpenSearch, saying we stop here, don't waste the LLM tokens. If the similarity is under 0.5, it means this is likely a unique one, and then we will check against the unique collections to make sure that historically this is against the historical news stored in our OpenSearch. The third step will be going to the analysis where if this is truly unique, then the second Lambda will do near real-time predictions, and then we're trying to do the spread widening recommendation also asset impact assessment and price movement probability analysis.

The final step generates a prediction report and sends it to the trader desk via Slack channels, allowing humans to decide whether to act. But here's the subtlety: generic embedding models aren't great for crypto-specific duplicate detection. That's why fine-tuning the embeddings matters so much.

[![Thumbnail 940](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/940.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=940)

Here is a scatter plot  showing an out-of-the-box BGE-M3 model tested on thousands of query-document pairs from crypto news. The green dots represent news articles that are duplicates. You want high similarity scores, ideally greater than 0.75. The red dots reveal the negative pairs, meaning news articles that are not duplicates, and you want low scores, ideally below 0.5. The problem you see in the muddy middle between 0.5 and 0.75, shown in the orange-faced zone, is a massive overlap. So we need to do better fine-tuning on this one to have better understanding.

[![Thumbnail 990](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/990.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=990)

This is after fine-tuning the BGE-M3  on thousands of labeled crypto news pairs. You see now the green dots and the red dots are separated much more. The muddy middle is already gone. There's a clean separation between 0.3 and 0.6. This is beautiful. We have now fine-tuned a tiny embedding model with just 560 million parameters. For the language model, you don't need to do any fine-tuning, just prompt engineering. This is the power of agentic architectures.

[![Thumbnail 1030](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/1030.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=1030)

[![Thumbnail 1040](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/1040.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=1040)

[![Thumbnail 1050](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/1050.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=1050)

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/1060.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=1060)

[![Thumbnail 1070](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/1070.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=1070)

So here we have a very quick demo.  You see, on the right-hand side, this is where the trader desk  is getting the news channels, and on the left-hand side, we are streaming through the news and letting our pipeline analyze whether this is an  important, impactful news item. The first one you see is an SEC filing.  It doesn't send to the trader desk because, from the reasoning model, it contains no indication of market-moving information. It is just  not a major regulatory filing.

[![Thumbnail 1080](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/1080.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=1080)

[![Thumbnail 1090](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/1090.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=1090)

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/1100.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=1100)

Further on, it will keep ingesting the news and keep analyzing.  Still, we classify this as just routine news. When a new item comes in about Trump  saying China is becoming very hostile, this kind of information feeds into the Telegram feed. You see  our pipeline identified this as very impactful news and pumped it to the trader desk to let the trader decide whether to increase the spread to protect the portfolio.

[![Thumbnail 1120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/dfeeef6997db749b/1120.jpg)](https://www.youtube.com/watch?v=TCJPcVhlQns&t=1120)

This is our demo today, and the key takeaways we would like to share:  use agentic architectures over fine-tuning. This helps you use general reasoning models like Claude for hierarchical task decomposition. For specialized embedding layers, this approach gives you more cost-effective and faster time-to-market results. You can do bias elimination through the architecture. You teach the system how to reason about novel events. Human-in-the-loop is still non-negotiable, but now you can use the trader feedback loop to change the system over time, which results in 24/7 real-time coverage with human oversight. This is our sharing today. Hope you find it useful. Thank you.


----

; This article is entirely auto-generated using Amazon Bedrock.
